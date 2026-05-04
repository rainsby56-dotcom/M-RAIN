import tkinter as tk
from tkinter import messagebox
import matplotlib.pyplot as plt

# Fungsi untuk menggambar objek
def gambar_titik(points, title="Objek"):
    plt.clf()
    
    # Tutup bentuk (kembali ke titik awal)
    x = [p[0] for p in points] + [points[0][0]]
    y = [p[1] for p in points] + [points[0][1]]
    
    plt.plot(x, y, marker='o')
    
    # Gambar sumbu kartesius
    plt.axhline(0)
    plt.axvline(0)
    
    plt.grid(True)
    plt.title(title)
    plt.xlim(-10, 10)
    plt.ylim(-10, 10)
    
    plt.draw()

# Ambil input dari user
def ambil_titik():
    try:
        points = []
        for i in range(4):
            x = float(entries[i][0].get())
            y = float(entries[i][1].get())
            points.append((x, y))
        return points
    except:
        messagebox.showerror("Error", "Input tidak valid!")
        return None

# Tombol tampilkan awal
def tampilkan():
    global current_points
    pts = ambil_titik()
    if pts:
        current_points = pts
        gambar_titik(current_points, "Objek Awal")

# Transformasi refleksi sumbu Y
def refleksi_y():
    global current_points
    if not current_points:
        return
    
    # Refleksi Y: (x, y) -> (-x, y)
    reflected = [(-x, y) for x, y in current_points]
    current_points = reflected
    
    # Update input field
    for i in range(4):
        entries[i][0].delete(0, tk.END)
        entries[i][0].insert(0, str(reflected[i][0]))
        entries[i][1].delete(0, tk.END)
        entries[i][1].insert(0, str(reflected[i][1]))
    
    gambar_titik(current_points, "Refleksi terhadap sumbu Y")

# Update manual
def update():
    global current_points
    pts = ambil_titik()
    if pts:
        current_points = pts
        gambar_titik(current_points, "Objek Diupdate")

# GUI utama
root = tk.Tk()
root.title("Transformasi Refleksi Y")

entries = []
current_points = []

# Input titik
for i in range(4):
    frame = tk.Frame(root)
    frame.pack()
    
    tk.Label(frame, text=f"Titik {i+1} (x, y): ").pack(side=tk.LEFT)
    
    entry_x = tk.Entry(frame, width=5)
    entry_x.pack(side=tk.LEFT)
    
    entry_y = tk.Entry(frame, width=5)
    entry_y.pack(side=tk.LEFT)
    
    entries.append((entry_x, entry_y))

# Tombol
tk.Button(root, text="Tampilkan Objek", command=tampilkan).pack(pady=5)
tk.Button(root, text="Refleksi Sumbu Y", command=refleksi_y).pack(pady=5)
tk.Button(root, text="Update Koordinat", command=update).pack(pady=5)

# Setup matplotlib
plt.ion()
fig = plt.figure()

root.mainloop()