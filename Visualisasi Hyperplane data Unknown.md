#Visualisasi Hyperplane data Unknown.R

```
library(e1071)
library(caret)
library(dplyr)
library(ggplot2)
```
Mengimpor paket-paket yang diperlukan untuk analisis SVM, manipulasi data, dan visualisasi.

```
svm_model_na <- svm(Performance ~ Teacher_Quality + Parental_Education_Level + Distance_from_Home
                 , data = trainData, kernel = "radial")
```
Membangun model SVM dengan variabel Teacher_Quality, Parental_Education_Level, dan Distance_from_Home untuk memprediksi Performance.

```
predictions_na <- predict(svm_model_na, testData)
```
Memprediksi nilai Performance untuk data uji (testData) menggunakan model SVM yang telah dibangun.

```
conf_matrix_na <- confusionMatrix(predictions_na, testData$Performance)
print(conf_matrix_na)
```
Menghitung dan mencetak matriks kebingungan untuk mengevaluasi akurasi model.

##Data untuk Teacher Quality vs Motivation Level
```
data_teacher <- trainData %>%
  select(Teacher_Quality, Motivation_Level, Performance)
```
Mengambil kolom yang relevan untuk analisis.

```
data_teacher$Teacher_Quality <- as.factor(data_teacher$Teacher_Quality)
data_teacher$Motivation_Level <- as.factor(data_teacher$Motivation_Level)
```
Mengonversi kolom menjadi faktor untuk analisis kategorikal.

```
summary_table_teacher <- data_teacher %>%
  group_by(Teacher_Quality, Motivation_Level) %>%
  summarise(Count = n(), .groups = "drop")
```
Menghitung jumlah data untuk setiap kombinasi Teacher_Quality dan Motivation_Level.

###Plot untuk Teacher Quality vs Motivation Level
```
plot_teacher <- ggplot(data = summary_table_teacher, aes(x = Teacher_Quality, y = Motivation_Level)) +
  geom_tile(aes(fill = Count), color = "white", alpha = 0.8) +
  geom_text(aes(label = Count), size = 5, color = "black") +
  scale_fill_gradient(low = "lightblue", high = "darkblue") +
  labs(
    title = "Data Distribution: Teacher Quality vs Motivation Level",
    x = "Teacher Quality",
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
Membuat visualisasi distribusi data antara Teacher_Quality dan Motivation_Level.

##Data untuk Distance from Home vs Motivation Level
```
data_distance <- trainData %>%
  select(Distance_from_Home, Motivation_Level, Performance)
```
Di sini, kita memilih kolom Distance_from_Home, Motivation_Level, dan Performance dari dataset trainData. Ini akan menjadi data yang kita gunakan untuk analisis.

```
data_distance$Distance_from_Home <- as.factor(data_distance$Distance_from_Home)
data_distance$Motivation_Level <- as.factor(data_distance$Motivation_Level)
```
Kita mengubah kolom Distance_from_Home dan Motivation_Level menjadi faktor. Ini penting karena kita ingin memperlakukan kedua kolom ini sebagai kategori, bukan angka.

```
summary_table_distance <- data_distance %>%
  group_by(Distance_from_Home, Motivation_Level) %>%
  summarise(Count = n(), .groups = "drop")
```
mengelompokkan data berdasarkan kombinasi Distance_from_Home dan Motivation_Level, lalu menghitung berapa banyak entri (baris) yang ada untuk setiap kombinasi tersebut. Hasilnya disimpan dalam summary_table_distance, yang menunjukkan seberapa sering setiap kombinasi muncul.

###Plot untuk Distance from Home vs Motivation Level
```
plot_distance <- ggplot(data = summary_table_distance, aes(x = Distance_from_Home, y = Motivation_Level)) +
  geom_tile(aes(fill = Count), color = "white", alpha = 0.8) +
  geom_text(aes(label = Count), size = 5, color = "black") +
  scale_fill_gradient(low = "lightcoral", high = "darkred") +
  labs(
    title = "Data Distribution: Distance from Home vs Motivation Level",
    x = "Distance from Home",
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
Membuat visualisasi distribusi data antara Distance_From_Home dan Motivation_Level.

>ggplot: Kita menggunakan ggplot2 untuk membuat visualisasi. Data yang digunakan adalah summary_table_distance.
>aes: Kita menetapkan sumbu x sebagai Distance_from_Home dan sumbu y sebagai Motivation_Level.
>geom_tile: Menggambarkan kotak (tile) berdasarkan jumlah (Count) untuk setiap kombinasi. Warna kotak diatur berdasarkan nilai Count.
>geom_text: Menambahkan label yang menunjukkan angka Count di setiap kotak.
>scale_fill_gradient: Mengatur gradasi warna dari 'lightcoral' hingga 'darkred' untuk menunjukkan jumlah, di mana warna lebih gelap menunjukkan jumlah yang lebih tinggi.
>labs: Menambahkan judul dan label sumbu.
>theme_minimal: Mengatur tema plot menjadi tampilan minimalis.
>theme: Mengatur agar teks pada sumbu x miring 45 derajat untuk kemudahan membaca, dan menghilangkan grid utama dan minor.

###Menampilkan plot
```
print(plot_teacher)
print(plot_distance)
```
Di sini, kita menampilkan plot yang telah dibuat. plot_teacher (yang mungkin didefinisikan sebelumnya) dan plot_distance adalah visualisasi yang akan ditampilkan di jendela output.
