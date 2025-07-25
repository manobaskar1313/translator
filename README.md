# Install with: pip install googletrans==4.0.0-rc1
from tkinter import *
from tkinter import ttk
from googletrans import Translator, LANGUAGES

# Create GUI window
root = Tk()
root.title("Google Translator")
root.geometry("700x300")
root.configure(bg="yellow")

# Function to translate text
def translate_text():
    translator = Translator()
    input_text = text_input.get("1.0", END).strip()
    
    if not input_text:
        text_output.delete("1.0", END)
        text_output.insert(END, "Please enter text to translate.")
        return

    src_lang_code = lang_dict.get(from_lang.get().lower(), 'en')
    dest_lang_code = lang_dict.get(to_lang.get().lower(), 'en')
    
    try:
        translated = translator.translate(input_text, src=src_lang_code, dest=dest_lang_code)
        text_output.delete("1.0", END)
        text_output.insert(END, translated.text)
    except Exception as e:
        text_output.delete("1.0", END)
        text_output.insert(END, f"Translation error:\n{str(e)}")

# Dropdown options
language_list = list(LANGUAGES.keys())
language_names = list(LANGUAGES.values())
lang_dict = {v.lower(): k for k, v in LANGUAGES.items()}  # Case insensitive

# From Language
from_lang = StringVar()
from_lang.set("English")
from_menu = ttk.Combobox(root, values=language_names, textvariable=from_lang, state='readonly')
from_menu.place(x=100, y=20, width=200)

# To Language
to_lang = StringVar()
to_lang.set("Hindi")
to_menu = ttk.Combobox(root, values=language_names, textvariable=to_lang, state='readonly')
to_menu.place(x=400, y=20, width=200)

# Input Text Box
text_input = Text(root, height=8, width=40, font=("Helvetica", 12))
text_input.place(x=20, y=60)

# Output Text Box
text_output = Text(root, height=8, width=40, font=("Helvetica", 12))
text_output.place(x=360, y=60)

# Translate Button
translate_button = Button(root, text="Translate", bg="red", fg="white", font=("Arial", 12, "bold"), command=translate_text)
translate_button.place(x=300, y=140)

# Run GUI
root.mainloop()
