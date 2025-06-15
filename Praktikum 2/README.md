
# 🧾 Laporan Praktikum 1 & 2 Komputasi Numerik

## 👤 Identitas Kelompok

- **Kelas**   : Komputasi Numerik B
- **Kelompok** : 18
- **Anggota** :
-- Nabila Shafa Rahayu - 5025241150
-- Shabrina Amalia Safaana - 5025241157
-- Dilbina Windi Azahra - 5025241180

---

## 📌 PRAKTIKUM 2

Salah satu kelemahan dari metode Trapezoida adalah kita harus menggunakan jumlah interval yang besar untuk memperoleh akurasi yang diharapkan. Buatlah sebuah program komputer untuk menjelaskan bagaimana metode Integrasi Romberg dapat mengatasi kelemahan tersebut

---
> Rincian Program
-- Membuat fungsi yang dapat melakukan perhitungan dengan Integrasi Trapezoida
-- Membuat fungsi yang dapat melakukan perhitungan dengan Integrasi Romberg
-- Membandingkan hasil dari kedua Integrasi tersebut

---


## KODE PYTHON

```
import math

def f(x):
    return eval(fungsi_input, {"x": x, "math": math, "sin": math.sin, "cos": math.cos, "exp": math.exp, "log": math.log})

def trapezoid(f, a, b, n):
    h = (b - a) / n
    s = 0.5 * (f(a) + f(b))
    for i in range(1, n):
        s += f(a + i * h)
    return h * s

def romberg(f, a, b, m):
    R = [[0 for _ in range(m)] for _ in range(m)]
    for i in range(m):
        n = 2 ** i
        R[i][0] = trapezoid(f, a, b, n)
        for k in range(1, i + 1):
            R[i][k] = (4 ** k * R[i][k - 1] - R[i - 1][k - 1]) / (4 ** k - 1)
    return R

# === Input dari pengguna ===
fungsi_input = input("Masukkan fungsi f(x): ")
a = float(input("Batas bawah a: "))
b = float(input("Batas atas b: "))
m = int(input("Banyak iterasi Romberg: "))
hasil_analitik_input = input("Masukkan hasil analitik (jika tidak tahu, masukkan '-'): ")

# Konversi hasil analitik jika diketahui
analitik_diketahui = hasil_analitik_input.strip() != "-"
if analitik_diketahui:
    hasil_analitik = float(hasil_analitik_input)

print("\n=== HASIL TRAPEZOIDA ===")
for i in range(1, 7):
    n = 2 ** i
    hasil = trapezoid(f, a, b, n)
    print(f"n = {n:<4} Hasil = {hasil:.10f}")
    if analitik_diketahui:
        err = abs(hasil - hasil_analitik)
        err_percent = (err / abs(hasil_analitik)) * 100
        print(f"       Selisih absolut       : {err:.10f}")
        print(f"       Selisih persen error  : {err_percent:.6f}%")

print("\n=== HASIL ROMBERG ===")
R = romberg(f, a, b, m)
for i in range(m):
    hasil_romberg = R[i][i]
    print(f"Iterasi {i+1:<2} Hasil = {hasil_romberg:.10f}")
    if analitik_diketahui:
        err = abs(hasil_romberg - hasil_analitik)
        err_percent = (err / abs(hasil_analitik)) * 100
        print(f"           Selisih absolut       : {err:.10f}")
        print(f"           Selisih persen error  : {err_percent:.6f}%")
        
```
## PENJELASAN KODE
- `import math` untuk mengimpor modul math agar bisa menggunakan fungsi matematika seperti sin, cos, exp, log, dll

```
def f(x):
    return eval(fungsi_input, {"x": x, "math": math, "sin": math.sin, "cos": math.cos, "exp": math.exp, "log": math.log})
```
- Fungsi ini mengevaluasi input dari pengguna sebagai fungsi matematika, menggunakan `eval()` agar `f(x)` bisa fleksibel menerima bentuk apa pun
- `def f(x)` untuk mendefinisikan fungsi Python `f(x)` yang akan dipanggil untuk menghitung nilai fungsi di titik x
- `eval(...)` adalah fungsi bawaan Python yang mengevaluasi string sebagai ekspresi Python. Misalnya, kalau stringnya `"x**2"`, dan `x = 2`, maka `eval()` akan menghasilkan `4`
- `fungsi_input` adalah variabel string berisi fungsi dari input pengguna, seperti `"x**2 * cos(x)"`
- `{"x": x, ...}` : Dictionary ini adalah lingkup lokal (local scope) untuk `eval()`. Artinya, hanya variabel dan fungsi di sini yang boleh digunakan saat mengevaluasi
- `"x": x` supaya `x` dalam ekspresi merujuk ke nilai `x` yang kita kirim ke `f(x)`
- `"math": math` mengizinkan user pakai math. dalam fungsi, misal math.exp(x)
- `"cos": math.cos`	mengizinkan user menulis cos(...) langsung tanpa math
- dsb.
```
def trapezoid(f, a, b, n):
    h = (b - a) / n
    s = 0.5 * (f(a) + f(b))
    for i in range(1, n):
        s += f(a + i * h)
    return h * s
```
- Fungsi untuk menghitung hasil dengan Integrasi Trapezoida
- Rumus Trapezoida : `I = h/2[f(x0) + 2f(x1) + 2f(x2) +.......+ 2f(xn-1) + f(xn)]`
- Dapat menjadi `h * s`, untuk `s` adalah `(f(x0) + f(xn))/2` + `[f(x1) + f(x2) +.....f(xn-1)]`
- `h = (b - a) / n` untuk menghitung panjang tiap segmen/pias (delta x)
- `s = 0.5 * (f(a) + f(b))` untuk menghitung nilai awal dari jumlah area `(f(x0) + f(xn))/2`
- `for i in range(1, n): s += f(a + i * h)` untuk menambahkan nilai f(x) di titik tengah segmen (`[f(x1) + f(x2) +.....f(xn-1)]`)
- `return h * s` mengalikan dengan h untuk mendapatkan total luas aproksimasi

```
def romberg(f, a, b, m):
    R = [[0 for _ in range(m)] for _ in range(m)]
    for i in range(m):
        n = 2 ** i
        R[i][0] = trapezoid(f, a, b, n)
        for k in range(1, i + 1):
            R[i][k] = (4 ** k * R[i][k - 1] - R[i - 1][k - 1]) / (4 ** k - 1)
    return R
```
- Fungsi untuk menghitung hasil dengan Integrasi Romberg
- Rumus Romberg : `R(i,k) = [4^k(R(i, k - 1)) - R(i - 1, k - 1)]/(4^k) -1` dengan baris ke-i adalah hasil trapezoida dengan pias `2^i` dan kolom ke-k adalah hasil ekstrapolasi untuk meningkatkan akurasi
- `R = [[0 for _ in range(m)] for _ in range(m)]` untuk membuat matriks kosong berukuran m x m, tempat menyimpan semua hasil `R(i,k)`
-  `for i in range(m):` untuk melooping setiap i
-  `n = 2 ** i` untuk setiap i, kita hitung `R(i,0)` = hasil trapezoida dengan pias `n=2^i`
-  Ini sesuai rumus: `R[i][0] = T_n`, hasil trapezoida pertama
-  `for k in range(1, i + 1):` untuk looping setiap kolom ke-i di baris ke-i
- `R[i][k] = (4 ** k * R[i][k - 1] - R[i - 1][k - 1]) / (4 ** k - 1)` sesuai dengan rumus Romberg 
- `return R` untuk mengembalikan hasil dari integrasi Romberg

```
# === Input dari pengguna ===
fungsi_input = input("Masukkan fungsi f(x): ")
a = float(input("Batas bawah a: "))
b = float(input("Batas atas b: "))
m = int(input("Banyak iterasi Romberg: "))
hasil_analitik_input = input("Masukkan hasil analitik (jika tidak tahu, masukkan '-'): ")
```
- Menerima input dari user:
-- Fungsi matematis
-- Batas bawah & atas integrasi
-- Jumlah iterasi Romberg
-- Nilai hasil analitik/eksak (jika ada, untuk dibandingkan)

```
# Konversi hasil analitik jika diketahui
analitik_diketahui = hasil_analitik_input.strip() != "-"
if analitik_diketahui:
    hasil_analitik = float(hasil_analitik_input)
```
- Kalau user input "-", berarti hasil analitik tidak diketahui → skip perbandingan
- Kalau diketahui, simpan nilai tersebut

```
print("\n=== HASIL TRAPEZOIDA ===")
for i in range(1, 7):
    n = 2 ** i
    hasil = trapezoid(f, a, b, n)
    print(f"n = {n:<4} Hasil = {hasil:.10f}")
    if analitik_diketahui:
        err = abs(hasil - hasil_analitik)
        err_percent = (err / abs(hasil_analitik)) * 100
        print(f"       Selisih absolut       : {err:.10f}")
        print(f"       Selisih persen error  : {err_percent:.6f}%")
```
- Menampilkan hasil integrasi dengan metode trapezoida, pias: 2, 4, 8, 16, 32, 64
- Jika hasil analitik diketahui, hitung juga error absolut dan persen error

```
print("\n=== HASIL ROMBERG ===")
R = romberg(f, a, b, m)
for i in range(m):
    hasil_romberg = R[i][i]
    print(f"Iterasi {i+1:<2} Hasil = {hasil_romberg:.10f}")
    if analitik_diketahui:
        err = abs(hasil_romberg - hasil_analitik)
        err_percent = (err / abs(hasil_analitik)) * 100
        print(f"           Selisih absolut       : {err:.10f}")
        print(f"           Selisih persen error  : {err_percent:.6f}%")
```
- Hanya mencetak elemen diagonal R[i][i] karena itu hasil yang paling akurat di tiap iterasi
- Tambah juga error (absolut dan persen) kalau analitik diketahui

## OUTPUT

![Untitled](https://github.com/user-attachments/assets/f340a775-4ea6-4588-87c6-e43754407d62)
