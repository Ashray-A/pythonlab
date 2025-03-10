from mrjob.job import MRJob


class MovieSimilarities(MRJob):
    def mapper(self, _, line):
        twitter_id, movie_name, genre = line.split("::")
        yield genre, movie_name

    def reducer(self, genre, movies):
        movie_list = list(movies)
        for i in range(len(movie_list)):
            for j in range(i + 1, len(movie_list)):
                similarity_score = self.calculate_similarity(movie_list[i], movie_list[j])
                yield (movie_list[i], movie_list[j]), similarity_score

    def calculate_similarity(self, movie1, movie2):

        movie1 = movie1.lower()
        movie2 = movie2.lower()

        common_chars = set(movie1) & set(movie2)
        similarity_score = len(common_chars)
        return similarity_score


if __name__ == '__main__':
    MovieSimilarities.run()