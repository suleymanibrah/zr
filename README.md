# zr
import tkinter as tk
import requests
import json

# Create the window
window = tk.Tk()
window.title("Dictionary and Translator App")
window.geometry("400x400")
window.config(bg="#ff6666")

# Create the top frame with icon and title
top_frame = tk.Frame(window, bg="#ff6666")
top_frame.pack(side=tk.TOP, fill=tk.X)

icon = tk.PhotoImage(file="icon.png")
title_label = tk.Label(top_frame, text="Dictionary and Translator App", font=("Arial", 20), bg="#ff6666", fg="white", compound=tk.LEFT, padx=10, pady=10, image=icon)
title_label.pack(side=tk.LEFT)

# Create the middle frame with search bar and search button
middle_frame = tk.Frame(window, bg="#ffffff")
middle_frame.pack(side=tk.TOP, fill=tk.X)

word_entry = tk.Entry(middle_frame, font=("Arial", 16), width=30)
word_entry.pack(side=tk.LEFT, padx=10, pady=10)

def search():
    word = word_entry.get()
    url = "https://api.dictionaryapi.dev/api/v2/entries/en_US/" + word
    response = requests.get(url)
    data = json.loads(response.text)
    if "title" in data:
        result_text.set(data["title"])
    elif "meaning" in data[0]["meanings"][0]:
        result_text.set(data[0]["meanings"][0]["definitions"][0]["definition"])
    else:
        result_text.set("Word not found")

search_button = tk.Button(middle_frame, text="Search", font=("Arial", 16), command=search)
search_button.pack(side=tk.LEFT, padx=10, pady=10)

# Create the bottom frame with translation entry and add button
bottom_frame = tk.Frame(window, bg="#ffffff")
bottom_frame.pack(side=tk.TOP, fill=tk.X)

translation_entry = tk.Entry(bottom_frame, font=("Arial", 16), width=30)
translation_entry.pack(side=tk.LEFT, padx=10, pady=10)

def add_word():
    word = word_entry.get()
    translation = translation_entry.get()
    # Add the word and translation to a database or file
    # (not implemented in this example)

add_button = tk.Button(bottom_frame, text="Add", font=("Arial", 16), command=add_word)
add_button.pack(side=tk.LEFT, padx=10, pady=10)

# Create a variable to store the translation result text
result_text = tk.StringVar()
result_text.set("Translation:")

# Create a label to display the translation result
result_label = tk.Label(window, textvariable=result_text, font=("Arial", 16), bg="#ffffff")
result_label.pack(side=tk.TOP, padx=10, pady=10)

# Start the main loop
window.mainloop()
