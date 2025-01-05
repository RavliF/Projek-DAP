#Visualisasi-Hyperlane-SVM.
SVM adalah metode pembelajaran mesin yang digunakan untuk klasifikasi dan regresi. Algoritma ini bekerja dengan menemukan hyperplane terbaik yang memisahkan data ke dalam dua kelas. Hyperplane ini adalah garis (dalam 2D), bidang (dalam 3D), atau ruang berdimensi lebih tinggi yang membagi data. Tujuannya adalah untuk memilih hyperplane yang memaksimalkan margin antara kelas yang berbeda.

```
str(trainData)
```
Menampilkan struktur dari data trainData, termasuk tipe data dari setiap kolom.

```
data1 <- trainData %>%
  select(Hours_Studied, Motivation_Level, Performance)
```
Memilih kolom Hours_Studied, Motivation_Level, dan Performance dari trainData untuk analisis lebih lanjut.

```
data1$Motivation_Level <- as.numeric(data1$Motivation_Level)
```
Mengubah kolom Motivation_Level menjadi tipe data numerik untuk digunakan dalam model SVM.

```
svm_model1 <- svm(Performance ~ Hours_Studied + Motivation_Level, 
                  data = data1, kernel = "radial")
```
Membangun model SVM menggunakan variabel Hours_Studied dan Motivation_Level untuk memprediksi Performance. Kernel radial digunakan untuk menangani data non-linear.

```
x_range <- seq(min(data1$Hours_Studied), max(data1$Hours_Studied), by = 0.1)
y_range <- seq(min(data1$Motivation_Level), max(data1$Motivation_Level), by = 0.1)
grid1 <- expand.grid(Hours_Studied = x_range, Motivation_Level = y_range)
grid1$Prediction <- predict(svm_model1, grid1)
```
Membuat grid untuk Hours_Studied dan Motivation_Level, lalu memprediksi nilai Performance untuk setiap kombinasi dari grid tersebut.

```
plot1 <- ggplot() +
  geom_point(data = data1, aes(x = Hours_Studied, y = Motivation_Level, color = Performance)) +
  geom_tile(data = grid1, aes(x = Hours_Studied, y = Motivation_Level, fill = Prediction), alpha = 0.3) +
  labs(title = "SVM: Hours Studied vs Motivation Level", x = "Hours Studied", y = "Motivation Level") +
  theme_minimal()
```
Membuat visualisasi menggunakan ggplot2 untuk menunjukkan hubungan antara Hours_Studied dan Motivation_Level, serta memprediksi area yang tergolong berbeda berdasarkan model SVM.

```
print(plot1)
```
Menampilkan plot yang telah dibuat.
