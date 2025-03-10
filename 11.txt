import requests
from bs4 import BeautifulSoup
import json

url = "https://www.imdb.com/chart/top"
headers = {"User-Agent": "Mozilla/5.0"}

response = requests.get(url, headers=headers)
soup = BeautifulSoup(response.text, "html.parser")
script = soup.find("script", {"type": "application/ld+json"})
data = json.loads(script.string)

for movie in data["itemListElement"][:10]:
    name = movie["item"]["name"]
    year = "N/A"
    summary = movie["item"]["description"]
    print(f"Name: {name}")
    print(f"Year: {year}")
    print(f"Summary: {summary}")
    print("--------------------")











import requests
from bs4 import BeautifulSoup

url = "https://www.imdb.com/chart/top"
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3"
}

response = requests.get(url, headers=headers)

if response.status_code == 200:
    print("Page fetched successfully!")
    soup = BeautifulSoup(response.content, "html.parser")

    movies = soup.select("div.ipc-title a.ipc-title-link-wrapper h3.ipc-title__text")

    print(f"Found {len(movies)} movies.")

    for movie in movies[:10]:

        link = "https://www.imdb.com" + movie.parent.get("href")
        print(f"Fetching details for: {link}")

        movie_response = requests.get(link, headers=headers)

        if movie_response.ok:
            movie_soup = BeautifulSoup(movie_response.content, "html.parser")

            try:
                movie_name = movie_soup.select_one("span.hero__primary-text").get_text(strip=True)
            except AttributeError:
                movie_name = "N/A"

            try:
                movie_year = movie_soup.select_one("a[href*='releaseinfo']").get_text(strip=True)
            except AttributeError:
                movie_year = "N/A"

            try:
                movie_summary = movie_soup.select_one("span.sc-42125d72-0.gKbnVu").get_text(strip=True)
            except AttributeError:
                movie_summary = "N/A"

            print(f"Movie Name: {movie_name}")
            print(f"Movie Year: {movie_year}")
            print(f"Movie Summary: {movie_summary}")
            print("--------------------")
else:
    print(f"Failed to fetch the IMDb Top 250 page. Status code: {response.status_code}")