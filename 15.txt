import tkinter as tk


def calculate_bmi():
   try:
       bmi = round(float(weight_entry.get()) / (float(height_entry.get()) ** 2), 2)
       bmi_label.config(text=f"BMI: {bmi}")
   except ValueError:
       bmi_label.config(text="Invalid Input")


root = tk.Tk()
root.title("BMI Calculator")


tk.Label(root, text="Weight (kg):").grid(row=0, column=0)
weight_entry = tk.Entry(root)
weight_entry.grid(row=0, column=1)


tk.Label(root, text="Height (m):").grid(row=1, column=0)
height_entry = tk.Entry(root)
height_entry.grid(row=1, column=1)


tk.Button(root, text="Calculate BMI", command=calculate_bmi).grid(row=2, column=0, columnspan=2)
bmi_label = tk.Label(root, text="BMI:")
bmi_label.grid(row=3, column=0, columnspan=2)


root.mainloop()
