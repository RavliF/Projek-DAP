# Projek DAP
SVM adalah metode pembelajaran mesin yang digunakan untuk klasifikasi dan regresi. Algoritma ini bekerja dengan menemukan hyperplane terbaik yang memisahkan data ke dalam dua kelas. Hyperplane ini adalah garis (dalam 2D), bidang (dalam 3D), atau ruang berdimensi lebih tinggi yang membagi data. Tujuannya adalah untuk memilih hyperplane yang memaksimalkan margin antara kelas yang berbeda.
```
library(e1071)
library(caret)
library(dplyr)
library(ggplot2)
```
Kode ini mengimpor beberapa library:
e1071: Digunakan untuk algoritma SVM (Support Vector Machine).
caret: Digunakan untuk pengolahan data dan evaluasi model.
dplyr: Digunakan untuk manipulasi data, seperti memilih dan mengubah kolom.
ggplot2: Digunakan untuk membuat visualisasi data.
```
dataset <- read.csv("StudentPerformanceFactors.csv")
```
Membaca file CSV bernama "StudentPerformanceFactors.csv" dan menyimpannya dalam variabel dataset.
```
str(dataset)
summary(dataset)
any(is.na(dataset))
```
> str(dataset): Menampilkan struktur dataset, termasuk tipe data dan jumlah baris.
> summary(dataset): Menampilkan ringkasan statistik dari setiap kolom.
> any(is.na(dataset)): Mengecek apakah ada nilai yang hilang (NA) di dalam dataset.

```
dataset <- dataset %>%
  mutate(
    Teacher_Quality = ifelse(is.na(Teacher_Quality), "Unknown", Teacher_Quality),
    Parental_Education_Level = ifelse(is.na(Parental_Education_Level), "Unknown", Parental_Education_Level),
    Distance_from_Home = ifelse(is.na(Distance_from_Home), "Unknown", Distance_from_Home)
  )
```
Menggunakan mutate untuk mengganti nilai NA pada kolom Teacher_Quality, Parental_Education_Level, dan Distance_from_Home dengan "Unknown".
```
categorical_cols <- c(
  "Parental_Involvement", "Access_to_Resources", "Extracurricular_Activities", 
  "Motivation_Level", "Internet_Access", "Family_Income", "Teacher_Quality", 
  "School_Type", "Peer_Influence", "Learning_Disabilities", 
  "Parental_Education_Level", "Distance_from_Home", "Gender"
)

dataset[categorical_cols] <- lapply(dataset[categorical_cols], factor)
```
Menyimpan nama kolom kategorikal dalam categorical_cols dan mengubahnya menjadi tipe faktor agar R dapat memperlakukan data ini sebagai kategori.
```
numerical_cols <- c("Hours_Studied", "Attendance", "Sleep_Hours", 
                    "Previous_Scores", "Tutoring_Sessions", "Physical_Activity")

dataset[numerical_cols] <- scale(dataset[numerical_cols])
```
Menyimpan nama kolom numerik dalam numerical_cols dan menstandarisasi data tersebut (mengubah skala sehingga rata-rata menjadi 0 dan deviasi standar menjadi 1).
```
dataset <- dataset %>%
  mutate(Performance = case_when(
    Exam_Score < 65 ~ "Low",
    Exam_Score >= 65 & Exam_Score < 75 ~ "Medium",
    Exam_Score >= 75 ~ "High"
  ))

dataset$Performance <- factor(dataset$Performance, levels = c("Low", "Medium", "High"))
```
Menggunakan mutate untuk membuat kolom baru Performance berdasarkan nilai Exam_Score. Nilai dibagi menjadi tiga kategori: "Low", "Medium", dan "High". Kolom Performance kemudian diubah menjadi tipe faktor dengan urutan yang sudah ditentukan.
```
set.seed(123)
trainIndex <- createDataPartition(dataset$Performance, p = 0.8, list = FALSE)
trainData <- dataset[trainIndex, ]
testData <- dataset[-trainIndex, ]
```
> set.seed(123): Mengatur seed untuk memastikan hasil yang konsisten setiap kali kode dijalankan.
> createDataPartition: Membagi dataset menjadi data latih (80%) dan data uji (20%) berdasarkan kolom Performance. trainData berisi data latih, sedangkan testData berisi data uji.

```
svm_model <- svm(Performance ~ Hours_Studied + Parental_Education_Level + Parental_Involvement + Motivation_Level
                 , data = trainData, kernel = "radial")

                 Membangun model SVM untuk memprediksi kolom Performance berdasarkan variabel Hours_Studied, Parental_Education_Level, Parental_Involvement, dan Motivation_Level. Kernel radial digunakan untuk menangani data non-linear.
                 
predictions <- predict(svm_model, testData)
```
Menggunakan model SVM yang telah dibangun untuk memprediksi nilai Performance pada data uji (testData).
```
conf_matrix <- confusionMatrix(predictions, testData$Performance)
print(conf_matrix)
```
Menghitung dan mencetak matriks kebingungan untuk membandingkan prediksi dengan nilai sebenarnya dari Performance pada data uji.

```
conf_matrix_table <- as.table(conf_matrix$table)
ggplot(data = as.data.frame(conf_matrix_table), aes(x = Prediction, y = Reference, fill = Freq)) +
  geom_tile() +
  geom_text(aes(label = Freq), color = "white") +
  labs(title = "Confusion Matrix Heatmap", x = "Predicted", y = "Actual")
```
> conf_matrix_table: Mengubah matriks kebingungan menjadi tabel untuk visualisasi.
Menggunakan ggplot2 untuk membuat heatmap dari matriks kebingungan, di mana kotak diwarnai berdasarkan frekuensi prediksi.
> geom_tile: Menggambar kotak berdasarkan frekuensi.
> geom_text: Menampilkan angka frekuensi di dalam kotak.
> labs: Menambahkan judul dan label sumbu.
