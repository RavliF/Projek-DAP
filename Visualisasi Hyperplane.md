# Visualisasi Hyperlane-SVM.
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

## Visualisasi Hyperlane Kategorikal

```
data2 <- trainData %>%
  select(Parental_Education_Level, Motivation_Level, Performance)
```
  Kita memilih kolom Parental_Education_Level, Motivation_Level, dan Performance dari dataset trainData untuk analisis lebih lanjut.
```
data2$Parental_Education_Level <- as.factor(data2$Parental_Education_Level)
data2$Motivation_Level <- as.factor(data2$Motivation_Level)
```
Mengubah kolom Parental_Education_Level dan Motivation_Level menjadi tipe data faktor. Ini penting untuk analisis kategori, sehingga R dapat memperlakukannya dengan benar.

```
summary_table <- data2 %>%
  group_by(Parental_Education_Level, Motivation_Level) %>%
  summarise(Count = n(), .groups = "drop")
```
Kita mengelompokkan data berdasarkan kombinasi dari Parental_Education_Level dan Motivation_Level, lalu menghitung berapa banyak entri (baris) yang ada untuk setiap kombinasi tersebut. Hasilnya disimpan dalam summary_table.

```
plot2 <- ggplot(data = summary_table, aes(x = Parental_Education_Level, y = Motivation_Level)) +
  geom_tile(aes(fill = Count), color = "white", alpha = 0.8) +
  geom_text(aes(label = Count), size = 5, color = "black") +
  scale_fill_gradient(low = "lightblue", high = "darkblue") +
  labs(
    title = "Data Distribution: Parental Education vs Motivation Level",
    x = "Parental Education Level",
    y = "Motivation Level",
    fill = "Count"
  ) +
  theme_minimal() +
  theme(
    axis.text.x = element_text(angle = 45, hjust = 1),
    panel.grid.major = element_blank(),
    panel.grid.minor = element_blank()
  )
```

> ggplot: Memulai pembuatan plot menggunakan ggplot2.
> aes: Menetapkan sumbu x sebagai Parental_Education_Level dan sumbu y sebagai Motivation_Level.
> geom_tile: Menggambar kotak (tile) berdasarkan nilai Count untuk setiap kombinasi. Warna kotak diatur berdasarkan jumlah (Count).
> geom_text: Menambahkan label yang menunjukkan angka Count di setiap kotak.
> scale_fill_gradient: Mengatur gradasi warna dari 'lightblue' hingga 'darkblue' untuk menunjukkan jumlah, di mana warna lebih gelap menunjukkan jumlah yang lebih tinggi.
> labs: Menambahkan judul dan label untuk sumbu.
> theme_minimal: Mengatur tema plot menjadi tampilan minimalis.
> theme: Mengatur agar teks pada sumbu x miring 45 derajat untuk kemudahan membaca, dan menghilangkan grid utama dan minor.

```
print(plot2)
```
Kode ini menampilkan plot yang telah dibuat (plot2).

```
data3 <- trainData %>%
  select(Parental_Involvement, Motivation_Level, Performance)
```
 Kita memilih kolom Parental_Involvement, Motivation_Level, dan Performance dari dataset trainData untuk analisis lebih lanjut.
 
```
data3$Parental_Involvement <- as.factor(data3$Parental_Involvement)
data3$Motivation_Level <- as.factor(data3$Motivation_Level)
```
Mengubah kolom Parental_Involvement dan Motivation_Level menjadi tipe data faktor.

```
summary_table2 <- data3 %>%
  group_by(Parental_Involvement, Motivation_Level) %>%
  summarise(Count = n(), .groups = "drop")
```
Kita mengelompokkan data berdasarkan kombinasi dari Parental_Involvement dan Motivation_Level, lalu menghitung berapa banyak entri untuk setiap kombinasi dan menyimpannya dalam summary_table2.

```
plot3 <- ggplot(data = summary_table2, aes(x = Parental_Involvement, y = Motivation_Level)) +
  geom_tile(aes(fill = Count), color = "white", alpha = 0.8) +
  geom_text(aes(label = Count), size = 5, color = "black") +
  scale_fill_gradient(low = "lightgreen", high = "darkgreen") +
  labs(
    title = "Data Distribution: Parental Involvement vs Motivation Level",
    x = "Parental Involvement",
    y = "Motivation Level",
    fill = "Count"
  ) +
  theme_minimal() +
  theme(
    axis.text.x = element_text(angle = 45, hjust = 1),
    panel.grid.major = element_blank(),
    panel.grid.minor = element_blank()
  )
```
Sama seperti penjelasan untuk plot2, tetapi kali ini kita menggunakan data dari summary_table2 untuk membuat visualisasi hubungan antara Parental_Involvement dan Motivation_Level.

```
print(plot3)
```
Kode ini menampilkan plot yang telah dibuat (plot3).
