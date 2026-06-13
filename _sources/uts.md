# uts

# 1. SPL(Sistem Persamaan Linier)

sistem persamaan linier dengan 5 variabel

$$
\left[
\begin{aligned}
2x_1 + x_2 &= 4 \\
x_1 + 2x_2 + x_3 &= 8 \\
x_2 + 2x_3 + x_4 &= 12 \\
x_3 + 2x_4 + x_5 &= 16 \\
x_4 + 2x_5 &= 14
\end{aligned}
\right]
$$

# 2.Matrix Augmented

spl di ubah jadi matrix augmented supaya mudah melakukan operasi baris elementer

$$
\left[
\begin{array}{ccccc|c}
2 & 1 & 0 & 0 & 0 & 4 \\
1 & 2 & 1 & 0 & 0 & 8 \\
0 & 1 & 2 & 1 & 0 & 12 \\
0 & 0 & 1 & 2 & 1 & 16 \\
0 & 0 & 0 & 1 & 2 & 14
\end{array}
\right]
$$

# 3.Eleminasi gauss

tujuan eleminasi gauss adalah mengubah matrix menjadi bentuk identitas menggunakan operasi baris elementer

$$
\text{Langkah 1}\quad
$$

$$
\left[
\begin{array}{ccccc|c}
2 & 1 & 0 & 0 & 0 & 4 \\
1 & 2 & 1 & 0 & 0 & 8 \\
0 & 1 & 2 & 1 & 0 & 12 \\
0 & 0 & 1 & 2 & 1 & 16 \\
0 & 0 & 0 & 1 & 2 & 14
\end{array}
\right]
$$

$$
\text{Langkah 2: } R_2 = R_2 - \tfrac{1}{2}R_1 \quad
$$

$$
\left[
\begin{array}{ccccc|c}
2 & 1 & 0 & 0 & 0 & 4 \\
0 & \tfrac{3}{2} & 1 & 0 & 0 & 6 \\
0 & 1 & 2 & 1 & 0 & 12 \\
0 & 0 & 1 & 2 & 1 & 16 \\
0 & 0 & 0 & 1 & 2 & 14
\end{array}
\right]
$$

$$
\text{Langkah 3: } R_3 = R_3 - \tfrac{2}{3}R_2 \quad
$$

$$
\left[
\begin{array}{ccccc|c}
2 & 1 & 0 & 0 & 0 & 4 \\
0 & \tfrac{3}{2} & 1 & 0 & 0 & 6 \\
0 & 0 & \tfrac{4}{3} & 1 & 0 & 8 \\
0 & 0 & 1 & 2 & 1 & 16 \\
0 & 0 & 0 & 1 & 2 & 14
\end{array}
\right]
$$

$$
\text{Langkah 4: } R_4 = R_4 - \tfrac{3}{4}R_3 \quad
$$

$$
\left[
\begin{array}{ccccc|c}
2 & 1 & 0 & 0 & 0 & 4 \\
0 & \tfrac{3}{2} & 1 & 0 & 0 & 6 \\
0 & 0 & \tfrac{4}{3} & 1 & 0 & 8 \\
0 & 0 & 0 & \tfrac{5}{4} & 1 & 10 \\
0 & 0 & 0 & 1 & 2 & 14
\end{array}
\right]
$$

$$
\text{Langkah 5: } R_5 = R_5 - \tfrac{4}{5}R_4 \quad
$$

$$
\left[
\begin{array}{ccccc|c}
2 & 1 & 0 & 0 & 0 & 4 \\
0 & \tfrac{3}{2} & 1 & 0 & 0 & 6 \\
0 & 0 & \tfrac{4}{3} & 1 & 0 & 8 \\
0 & 0 & 0 & \tfrac{5}{4} & 1 & 10 \\
0 & 0 & 0 & 0 & \tfrac{6}{5} & 6
\end{array}
\right]
$$

$$
\text{Langkah 6: } R_5 = \tfrac{5}{6}R_5 \quad
$$

$$
\left[
\begin{array}{ccccc|c}
2 & 1 & 0 & 0 & 0 & 4 \\
0 & \tfrac{3}{2} & 1 & 0 & 0 & 6 \\
0 & 0 & \tfrac{4}{3} & 1 & 0 & 8 \\
0 & 0 & 0 & \tfrac{5}{4} & 1 & 10 \\
0 & 0 & 0 & 0 & 1 & 5
\end{array}
\right]
$$

$$
\text{Langkah 7: } R_4 = R_4 - R_5 \quad
$$

$$
\left[
\begin{array}{ccccc|c}
2 & 1 & 0 & 0 & 0 & 4 \\
0 & \tfrac{3}{2} & 1 & 0 & 0 & 6 \\
0 & 0 & \tfrac{4}{3} & 1 & 0 & 8 \\
0 & 0 & 0 & \tfrac{5}{4} & 0 & 5 \\
0 & 0 & 0 & 0 & 1 & 5
\end{array}
\right]
$$

$$
\text{Langkah 8: } R_4 = \tfrac{4}{5}R_4 \quad
$$


$$
\left[
\begin{array}{ccccc|c}
2 & 1 & 0 & 0 & 0 & 4 \\
0 & \tfrac{3}{2} & 1 & 0 & 0 & 6 \\
0 & 0 & \tfrac{4}{3} & 1 & 0 & 8 \\
0 & 0 & 0 & 1 & 0 & 4 \\
0 & 0 & 0 & 0 & 1 & 5
\end{array}
\right]
$$

$$
\text{Langkah 9: } R_3 = R_3 - R_4 \quad
$$

$$
\left[
\begin{array}{ccccc|c}
2 & 1 & 0 & 0 & 0 & 4 \\
0 & \tfrac{3}{2} & 1 & 0 & 0 & 6 \\
0 & 0 & \tfrac{4}{3} & 0 & 0 & 4 \\
0 & 0 & 0 & 1 & 0 & 4 \\
0 & 0 & 0 & 0 & 1 & 5
\end{array}
\right]
$$

$$
\text{Langkah 10: } R_3 = \tfrac{3}{4}R_3 \quad
$$

$$
\left[
\begin{array}{ccccc|c}
2 & 1 & 0 & 0 & 0 & 4 \\
0 & \tfrac{3}{2} & 1 & 0 & 0 & 6 \\
0 & 0 & 1 & 0 & 0 & 3 \\
0 & 0 & 0 & 1 & 0 & 4 \\
0 & 0 & 0 & 0 & 1 & 5
\end{array}
\right]
$$

$$
\text{Langkah 11: } R_2 = R_2 - R_3 \quad
$$

$$
\left[
\begin{array}{ccccc|c}
2 & 1 & 0 & 0 & 0 & 4 \\
0 & \tfrac{3}{2} & 0 & 0 & 0 & 3 \\
0 & 0 & 1 & 0 & 0 & 3 \\
0 & 0 & 0 & 1 & 0 & 4 \\
0 & 0 & 0 & 0 & 1 & 5
\end{array}
\right]
$$

$$
\text{Langkah 12: } R_2 = \tfrac{2}{3}R_2 \quad
$$

$$
\left[
\begin{array}{ccccc|c}
2 & 1 & 0 & 0 & 0 & 4 \\
0 & 1 & 0 & 0 & 0 & 2 \\
0 & 0 & 1 & 0 & 0 & 3 \\
0 & 0 & 0 & 1 & 0 & 4 \\
0 & 0 & 0 & 0 & 1 & 5
\end{array}
\right]
$$

$$
\text{Langkah 13: } R_1 = R_1 - R_2 \quad
$$

$$
\left[
\begin{array}{ccccc|c}
2 & 0 & 0 & 0 & 0 & 2 \\
0 & 1 & 0 & 0 & 0 & 2 \\
0 & 0 & 1 & 0 & 0 & 3 \\
0 & 0 & 0 & 1 & 0 & 4 \\
0 & 0 & 0 & 0 & 1 & 5
\end{array}
\right]
$$

$$
\text{Langkah 14: } R_1 = \tfrac{1}{2}R_1 \quad
$$

$$
\left[
\begin{array}{ccccc|c}
1 & 0 &
 0 & 0 & 0 & 1 \\
0 & 1 & 0 & 0 & 0 & 2 \\
0 & 0 & 1 & 0 & 0 & 3 \\
0 & 0 & 0 & 1 & 0 & 4 \\
0 & 0 & 0 & 0 & 1 & 5
\end{array}
\right]
$$

$$
x_1=1,\; x_2=2,\; x_3=3,\; x_4=4,\; x_5=5
$$


# 4 determinan
Tujuan determinan adalah untuk mengetahui apakah suatu matriks memiliki invers atau tidak.

Jika nilai determinan tidak sama dengan nol, maka matriks memiliki invers   dan sistem persamaan memiliki solusi tunggal.

Jika nilai determinan sama dengan nol, maka matriks tidak memiliki invers dan sistem tidak memiliki solusi tunggal.

# Matriks koefisien A:

$$
A =
\begin{bmatrix}
2 & 1 & 0 & 0 & 0 \\
1 & 2 & 1 & 0 & 0 \\
0 & 1 & 2 & 1 & 0 \\
0 & 0 & 1 & 2 & 1 \\
0 & 0 & 0 & 1 & 2
\end{bmatrix}
$$

Langkah 1  
$R_2 = R_2 - \tfrac{1}{2}R_1$

$$
\begin{bmatrix}
2 & 1 & 0 & 0 & 0 \\
0 & \tfrac{3}{2} & 1 & 0 & 0 \\
0 & 1 & 2 & 1 & 0 \\
0 & 0 & 1 & 2 & 1 \\
0 & 0 & 0 & 1 & 2
\end{bmatrix}
$$

Langkah 2  
$R_3 = R_3 - \tfrac{2}{3}R_2$

$$
\begin{bmatrix}
2 & 1 & 0 & 0 & 0 \\
0 & \tfrac{3}{2} & 1 & 0 & 0 \\
0 & 0 & \tfrac{4}{3} & 1 & 0 \\
0 & 0 & 1 & 2 & 1 \\
0 & 0 & 0 & 1 & 2
\end{bmatrix}
$$

Langkah 3  
$R_4 = R_4 - \tfrac{3}{4}R_3$

$$
\begin{bmatrix}
2 & 1 & 0 & 0 & 0 \\
0 & \tfrac{3}{2} & 1 & 0 & 0 \\
0 & 0 & \tfrac{4}{3} & 1 & 0 \\
0 & 0 & 0 & \tfrac{5}{4} & 1 \\
0 & 0 & 0 & 1 & 2
\end{bmatrix}
$$

Langkah 4  
$R_5 = R_5 - \tfrac{4}{5}R_4$

$$
\begin{bmatrix}
2 & 1 & 0 & 0 & 0 \\
0 & \tfrac{3}{2} & 1 & 0 & 0 \\
0 & 0 & \tfrac{4}{3} & 1 & 0 \\
0 & 0 & 0 & \tfrac{5}{4} & 1 \\
0 & 0 & 0 & 0 & \tfrac{6}{5}
\end{bmatrix}
$$

Determinan:

$$
\det(A) = 2 \times \tfrac{3}{2} \times \tfrac{4}{3} \times \tfrac{5}{4} \times \tfrac{6}{5}
$$

$$
\det(A) = 6
$$

# adjoin
Matriks A:

$$
A =
\begin{bmatrix}
2 & 1 & 0 & 0 & 0 \\
1 & 2 & 1 & 0 & 0 \\
0 & 1 & 2 & 1 & 0 \\
0 & 0 & 1 & 2 & 1 \\
0 & 0 & 0 & 1 & 2
\end{bmatrix}
$$

Adjoin matriks diperoleh dari transpose matriks kofaktor.

Contoh perhitungan kofaktor:

$$
C_{11} =
\begin{vmatrix}
2 & 1 & 0 & 0 \\
1 & 2 & 1 & 0 \\
0 & 1 & 2 & 1 \\
0 & 0 & 1 & 2
\end{vmatrix}
= 5
$$

$$
C_{12} = -
\begin{vmatrix}
1 & 1 & 0 & 0 \\
0 & 2 & 1 & 0 \\
0 & 1 & 2 & 1 \\
0 & 0 & 1 & 2
\end{vmatrix}
= -4
$$

$$
C_{13} =
\begin{vmatrix}
1 & 2 & 0 & 0 \\
0 & 1 & 1 & 0 \\
0 & 0 & 2 & 1 \\
0 & 0 & 1 & 2
\end{vmatrix}
= 3
$$

Pola tanda kofaktor:

$$
\begin{bmatrix}
+ & - & + & - & + \\
- & + & - & + & - \\
+ & - & + & - & + \\
- & + & - & + & - \\
+ & - & + & - & +
\end{bmatrix}
$$

Matriks kofaktor:

$$
\begin{bmatrix}
5 & -4 & 3 & -2 & 1 \\
-4 & 8 & -6 & 4 & -2 \\
3 & -6 & 9 & -6 & 3 \\
-2 & 4 & -6 & 8 & -4 \\
1 & -2 & 3 & -4 & 5
\end{bmatrix}
$$

Adjoin matriks A (transpose kofaktor):

$$
\text{adj}(A) =
\begin{bmatrix}
5 & -4 & 3 & -2 & 1 \\
-4 & 8 & -6 & 4 & -2 \\
3 & -6 & 9 & -6 & 3 \\
-2 & 4 & -6 & 8 & -4 \\
1 & -2 & 3 & -4 & 5
\end{bmatrix}
$$

#inves
Matriks A:

$$
A =
\begin{bmatrix}
2 & 1 & 0 & 0 & 0 \\
1 & 2 & 1 & 0 & 0 \\
0 & 1 & 2 & 1 & 0 \\
0 & 0 & 1 & 2 & 1 \\
0 & 0 & 0 & 1 & 2
\end{bmatrix}
$$

Determinan:

$$
\det(A) = 6
$$

Adjoin matriks A:

$$
\text{adj}(A) =
\begin{bmatrix}
5 & -4 & 3 & -2 & 1 \\
-4 & 8 & -6 & 4 & -2 \\
3 & -6 & 9 & -6 & 3 \\
-2 & 4 & -6 & 8 & -4 \\
1 & -2 & 3 & -4 & 5
\end{bmatrix}
$$

Rumus invers:

$$
A^{-1} = \frac{1}{\det(A)} \cdot \text{adj}(A)
$$

$$
A^{-1} = \frac{1}{6} \cdot \text{adj}(A)
$$

Hasil invers:

$$
A^{-1} =
\begin{bmatrix}
\frac{5}{6} & -\frac{2}{3} & \frac{1}{2} & -\frac{1}{3} & \frac{1}{6} \\
-\frac{2}{3} & \frac{4}{3} & -1 & \frac{2}{3} & -\frac{1}{3} \\
\frac{1}{2} & -1 & \frac{3}{2} & -1 & \frac{1}{2} \\
-\frac{1}{3} & \frac{2}{3} & -1 & \frac{4}{3} & -\frac{2}{3} \\
\frac{1}{6} & -\frac{1}{3} & \frac{1}{2} & -\frac{2}{3} & \frac{5}{6}
\end{bmatrix}
$$