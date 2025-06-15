# 🧾 Laporan Praktikum 1 & 2 Komputasi Numerik

## 👤 Identitas Kelompok

- **Kelas**   : Komputasi Numerik B
- **Kelompok** : 18
- **Anggota** :
-- Nabila Shafa Rahayu - 5025241150
-- Shabrina Amalia Safaana - 5025241157
-- Dilbina Windi Azahra - 5025241180

---

## 📌 PRAKTIKUM 1

Anda sudah mengerti algoritma pemrosesan metode Regula Falsi, dan anda sudah memahami cara kerjanya. Sekarang anda tinggal mengimplementasikan algoritma tersebut menjadi sebuah komputer metode Regula Falsi (yang dapat menampilkan proses iterasi numerik , lengkap dengan grafik fungsinya)

---
> Rincian Program
-- Membuat fungsi yang dapat melakukan perhitungan dengan Metode Regula Falsi
-- Menampilkan proses iterasi numerik
-- Menampilkan grafik fungsinya

---

## KODE PYTHON

```
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd

# Input user
fx_str = input("Masukkan fungsi f(x), contoh: x**3 - 100 : ")
x1 = float(input("Masukkan nilai X1: "))
x2 = float(input("Masukkan nilai X2: "))

# Convert fungsi string tadi
def f(x):
    return eval(fx_str)

# Regula Falsi
def regula_falsi(f, x1, x2, tol=1e-6, max_iter=100):
    if f(x1) * f(x2) >= 0:
        raise ValueError("f(x1) dan f(x2) harus beda tanda!")

    data = []

    for i in range(1, max_iter + 1):
        x3 = (x1 * f(x2) - x2 * f(x1)) / (f(x2) - f(x1))
        fx1 = f(x1)
        fx2 = f(x2)
        fx3 = f(x3)
        data.append([i, x1, x2, x3, fx1, fx2, fx3])

        if abs(fx3) < tol:
            break

        if fx1 * fx3 < 0:
            x2 = x3
        else:
            x1 = x3

    df = pd.DataFrame(data, columns=["Iterasi", "X1", "X2", "X3", "f(X1)", "f(X2)", "f(X3)"])
    return x3, df

try:
    akar, tabel = regula_falsi(f, x1, x2)
    print("\nTabel Iterasi Regula Falsi:\n")
    print(tabel)

    # Plot grafik fungsi
    x_vals = np.linspace(min(x1, x2) - 1, max(x1, x2) + 1, 400)
    y_vals = [f(x) for x in x_vals]

    plt.figure(figsize=(10, 6))
    plt.plot(x_vals, y_vals, label="f(x)", color="blue")
    plt.axhline(0, color='black', linewidth=0.7)

    # Ambil titik-titik dari X3 dan f(X3)
    x3_points = tabel["X3"]
    fx3_points = tabel["f(X3)"]
    plt.plot(x3_points, fx3_points, 'ro', label='Titik Regula Falsi (X3)')

    for i, (x, y) in enumerate(zip(x3_points, fx3_points)):
        plt.text(x, y, f'{i+1}', fontsize=9, ha='right')

    plt.title("Metode Regula Falsi")
    plt.xlabel("x")
    plt.ylabel("f(x)")
    plt.legend()
    plt.grid(True)
    plt.show()

    print(f"\nAkar hampiran ditemukan: {akar}")

except Exception as e:
    print(f"⚠️ ERROR: {e}")

        
```
## PENJELASAN KODE
- `import numpy as np` untuk mengimpor library NumPy untuk melakukan operasi numerik seperti membuat array, linspace, dll.
- `import matplotlib.pyplot as plt` untuk mengimpor Matplotlib bagian pyplot sebagai plt untuk membuat grafik.
- `import pandas as pd` untuk engimpor Pandas, library manipulasi data.

```
fx_str = input("Masukkan fungsi f(x), contoh: x**3 - 100 : ")
x1 = float(input("Masukkan nilai X1: "))
x2 = float(input("Masukkan nilai X2: "))
```
- `fx_str` adalah input berupa string fungsi, contoh: "x**2 - 9"
- `x1` dan `x2` adalah dua tebakan awal untuk metode Regula Falsi. Syarat: nilai f(x1) dan f(x2) harus beda tanda (positif dan negatif) agar metode bisa berjalan.

```
def f(x):
    return eval(fx_str)
```
- Fungsi `f(x)` ini mengevaluasi string `fx_str` sebagai ekspresi Python.
- `eval(fx_str)` akan mengubah string seperti `"x**2 - 9"` jadi ekspresi Python dan menghitungnya sesuai nilai x.

```
def regula_falsi(f, x1, x2, tol=1e-6, max_iter=100):
```
- Fungsi untuk mencari akar persamaan non-linear menggunakan metode Regula Falsi.
- `tol` adalah toleransi error. Jika `|f(x)| < tol`, iterasi berhenti.
- `max_iter` adalah batas maksimal iterasi agar tidak infinite loop.

```
if f(x1) * f(x2) >= 0:
    raise ValueError("f(x1) dan f(x2) harus beda tanda!")
```
- Mengecek apakah f(x1) dan f(x2) beda tanda.
-- Jika tidak, maka tidak bisa dijamin ada akar dan langsung error.

```
data = []
```
- Menyimpan semua data iterasi: nilai X1, X2, X3, f(X1), f(X2), f(X3), dan nomor iterasi.

```
for i in range(1, max_iter + 1):
    x3 = (x1 * f(x2) - x2 * f(x1)) / (f(x2) - f(x1))
```
- Rumus Regula Falsi: menentukan X3, yaitu titik potong garis antara (x1, f(x1)) dan (x2, f(x2)) terhadap sumbu X

```
    fx1 = f(x1)
    fx2 = f(x2)
    fx3 = f(x3)
    data.append([i, x1, x2, x3, fx1, fx2, fx3])
```
- Menyimpan semua nilai pada iterasi ke-i.

```
    if abs(fx3) < tol:
        break
```
- Jika f(x3) sudah mendekati nol (mendekati akar), iterasi berhenti.

```
    if fx1 * fx3 < 0:
        x2 = x3
    else:
        x1 = x3
```
- Update interval agar akar tetap berada di antara dua titik yang beda tanda.

```
df = pd.DataFrame(data, columns=["Iterasi", "X1", "X2", "X3", "f(X1)", "f(X2)", "f(X3)"])
return x3, df
```
- Menyimpan semua data iterasi ke dalam tabel `DataFrame` dan mengembalikan nilai akar (x3) serta tabel tersebut.

```
x_vals = np.linspace(min(x1, x2) - 1, max(x1, x2) + 1, 400)
y_vals = [f(x) for x in x_vals]
```
- Membuat 400 titik `x` dari sekitaran `x1` ke `x2`, untuk digambar grafiknya.

```
plt.figure(figsize=(10, 6))
plt.plot(x_vals, y_vals, label="f(x)", color="blue")
plt.axhline(0, color='black', linewidth=0.7)
```
- Menggambar grafik `f(x)` dengan garis horizontal di `y=0` (sumbu X).

```
x3_points = tabel["X3"]
fx3_points = tabel["f(X3)"]
plt.plot(x3_points, fx3_points, 'ro', label='Titik Regula Falsi (X3)')
```
- Menandai semua titik hasil Regula Falsi (X3) sebagai titik merah di grafik.

```
for i, (x, y) in enumerate(zip(x3_points, fx3_points)):
    plt.text(x, y, f'{i+1}', fontsize=9, ha='right')
```
- Memberi label nomor iterasi pada setiap titik X3 di grafik.

```
plt.title("Metode Regula Falsi")
plt.xlabel("x")
plt.ylabel("f(x)")
plt.legend()
plt.grid(True)
plt.show()
```
- Menambahkan judul, label sumbu, legenda, dan grid pada grafik.

```
print(f"\nAkar hampiran ditemukan: {akar}")
```
- Mencetak hasil akar terakhir dari metode Regula Falsi.

```
except Exception as e:
    print(f"⚠️ ERROR: {e}")
```
- Menangani error jika ada, misal input fungsi salah, `f(x1)` dan `f(x2)` ga beda tanda, dll.

## OUTPUT

![Untitled](https://github.com/user-attachments/assets/21cd9d02-58b1-4ee0-93e6-291392a01595)

