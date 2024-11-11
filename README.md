import tkinter as tk
from PIL import Image, ImageTk
import os
import random

# Fungsi untuk menambahkan ekspresi
def append_to_expression(key):
    expression_field.set(expression_field.get() + key)

# Warna acak untuk hasil
result_colors = ["#80ff80", "#80dfff", "#ffb380", "#ff80bf", "#b380ff", "#ff8080"]

# Fungsi untuk menghitung hasil ekspresi
def calculate_result():
    try:
        # Konversi simbol pangkat dari ^ ke **
        expression = expression_field.get().replace("^", "**")  
        result = str(eval(expression))  # Evaluasi ekspresi
        expression_field.set(result)  # Tampilkan hasil
        entry.config(bg=random.choice(result_colors))  # Beri warna latar acak
    except ZeroDivisionError:
        expression_field.set("Error: Division by Zero")  # Penanganan pembagian oleh nol
    except Exception:
        expression_field.set("Error")  # Penanganan kesalahan lainnya

# Fungsi untuk menambahkan simbol pangkat (^)
def calculate_power():
    append_to_expression("^")  # Tambah simbol ^ ke ekspresi

# Fungsi untuk membersihkan ekspresi
def clear_expression():
    expression_field.set("")  # Kosongkan ekspresi
    entry.config(bg='#333333')  # Reset warna latar

# Fungsi untuk mengaktifkan/mematikan layar penuh
def toggle_fullscreen(event=None):
    root.attributes("-fullscreen", not root.attributes("-fullscreen"))
    resize_background()

# Fungsi untuk keluar dari layar penuh
def exit_fullscreen(event=None):
    root.attributes("-fullscreen", False)
    resize_background()

# Fungsi untuk menyesuaikan gambar latar belakang
def resize_background():
    if bg_image:
        width = root.winfo_width()
        height = root.winfo_height()
        resized_bg = bg_image.resize((width, height), Image.LANCZOS)
        bg_photo_resized = ImageTk.PhotoImage(resized_bg)
        canvas.image = bg_photo_resized  # Simpan referensi untuk mencegah penghapusan otomatis
        canvas.create_image(0, 0, image=bg_photo_resized, anchor="nw")

# Inisialisasi jendela utama
root = tk.Tk()
root.title("Calculator by Subhan")
root.geometry("600x800")
root.configure(bg='#1a1a1a')

# Bind untuk layar penuh
root.bind("<F11>", toggle_fullscreen)
root.bind("<Escape>", exit_fullscreen)

# Memuat gambar latar belakang jika tersedia
try:
    image_path = r"C:/Users/User/Pictures/Acer/2022_Acer_Commercial_Option_03_3840x2400.jpg"
    if os.path.exists(image_path):
        bg_image = Image.open(image_path)
        bg_photo = ImageTk.PhotoImage(bg_image)
    else:
        raise FileNotFoundError(f"Image not found: {image_path}")
except Exception as e:
    print(f"Error loading background image: {e}")
    bg_image = None

# Kanvas untuk gambar latar belakang
canvas = tk.Canvas(root, highlightthickness=0)
canvas.grid(row=0, column=0, columnspan=4, rowspan=7, sticky="nsew")

# Set gambar latar belakang jika dimuat
if bg_image:
    canvas.create_image(0, 0, image=bg_photo, anchor="nw")

# Atur grid
root.grid_rowconfigure(0, weight=1)
root.grid_columnconfigure(0, weight=1)

# Label judul
title_label = tk.Label(root, text="Calculator by Subhan", font=('Arial', 16, 'bold'), bg="#ffcc99", fg="#4d4d4d", relief="ridge", bd=5, padx=5, pady=3)
title_label.grid(row=0, column=0, columnspan=4, pady=(10, 0))

# Bidang entri untuk ekspresi
expression_field = tk.StringVar()
entry = tk.Entry(root, font=('Arial', 28), bd=10, insertwidth=2, width=20, borderwidth=4, textvariable=expression_field)
entry.grid(row=1, column=0, columnspan=4, padx=10, pady=10, sticky="nsew")
entry.config(bg='#333333', fg='white')

# Konfigurasi tombol
buttons = [
    ('C', 2, 0, '#ff6666'), ('^', 2, 1, '#6699ff'), ('%', 2, 2, '#6699ff'), ('/', 2, 3, '#6699ff'),
    ('7', 3, 0, '#80bfff'), ('8', 3, 1, '#80bfff'), ('9', 3, 2, '#80bfff'), ('*', 3, 3, '#6699ff'),
    ('4', 4, 0, '#80bfff'), ('5', 4, 1, '#80bfff'), ('6', 4, 2, '#80bfff'), ('-', 4, 3, '#6699ff'),
    ('1', 5, 0, '#80bfff'), ('2', 5, 1, '#80bfff'), ('3', 5, 2, '#80bfff'), ('+', 5, 3, '#6699ff'),
    ('0', 6, 0, '#80bfff'), ('.', 6, 1, '#80bfff'), (',', 6, 2, '#80bfff'), ('=', 6, 3, '#ff8000'),
]

button_fg = 'white'
button_font = ('Arial', 18, 'bold')

# Membuat tombol
for (text, row, col, color) in buttons:
    if text == "=":
        cmd = calculate_result
    elif text == 'C':
        cmd = clear_expression
    elif text == '^':
        cmd = calculate_power
    else:
        cmd = lambda key=text: append_to_expression(key)
    
    tk.Button(root, text=text, height=4, width=12, bg=color, fg=button_fg, font=button_font, command=cmd).grid(row=row, column=col, padx=5, pady=5, sticky="nsew")

# Bind untuk mereset ukuran gambar latar belakang
root.bind("<Configure>", lambda event: resize_background())

# Atur baris dan kolom untuk responsivitas
for i in range(1, 7):
    root.grid_rowconfigure(i, weight=1)
root.grid_columnconfigure(0, weight=1)
root.grid_columnconfigure(1, weight=1)
root.grid_columnconfigure(2, weight=1)
root.grid_columnconfigure(3, weight=1)

# Memulai loop utama Tkinter
root.mainloop()
<!---
AHMADSUBHANARRAIHAN/AHMADSUBHANARRAIHAN is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
