# Projek-DAP
Projek Mata Kuliah Data Analytic Programming

> Prediksi Performa Siswa Berdasarkan Faktor Akademik dan Sosial menggunakan Model SVM

```
library(e1071)
library(caret)
library(dplyr)
library(ggplot2)

dataset <- read.csv("StudentPerformanceFactors.csv")

str(dataset)
summary(dataset)
any(is.na(dataset))


dataset <- dataset %>%
  mutate(
    Teacher_Quality = ifelse(is.na(Teacher_Quality), "Unknown", Teacher_Quality),
    Parental_Education_Level = ifelse(is.na(Parental_Education_Level), "Unknown", Parental_Education_Level),
    Distance_from_Home = ifelse(is.na(Distance_from_Home), "Unknown", Distance_from_Home)
  )

categorical_cols <- c(
  "Parental_Involvement", "Access_to_Resources", "Extracurricular_Activities", 
  "Motivation_Level", "Internet_Access", "Family_Income", "Teacher_Quality", 
  "School_Type", "Peer_Influence", "Learning_Disabilities", 
  "Parental_Education_Level", "Distance_from_Home", "Gender"
)

dataset[categorical_cols] <- lapply(dataset[categorical_cols], factor)

numerical_cols <- c("Hours_Studied", "Attendance", "Sleep_Hours", 
                    "Previous_Scores", "Tutoring_Sessions", "Physical_Activity")

dataset[numerical_cols] <- scale(dataset[numerical_cols])

dataset <- dataset %>%
  mutate(Performance = case_when(
    Exam_Score < 65 ~ "Low",
    Exam_Score >= 65 & Exam_Score < 75 ~ "Medium",
    Exam_Score >= 75 ~ "High"
  ))

dataset$Performance <- factor(dataset$Performance, levels = c("Low", "Medium", "High"))

set.seed(123)
trainIndex <- createDataPartition(dataset$Performance, p = 0.8, list = FALSE)
trainData <- dataset[trainIndex, ]
testData <- dataset[-trainIndex, ]


svm_model <- svm(Performance ~ Hours_Studied + Parental_Education_Level + Parental_Involvement + Motivation_Level
                 , data = trainData, kernel = "radial")


predictions <- predict(svm_model, testData)

conf_matrix <- confusionMatrix(predictions, testData$Performance)
print(conf_matrix)


conf_matrix_table <- as.table(conf_matrix$table)
ggplot(data = as.data.frame(conf_matrix_table), aes(x = Prediction, y = Reference, fill = Freq)) +
  geom_tile() +
  geom_text(aes(label = Freq), color = "white") +
  labs(title = "Confusion Matrix Heatmap", x = "Predicted", y = "Actual")
```
