# DS_code_Resolving classification problem using IRIS data set with 91.6% accuracy
##breif introduction-iris is a data frame with 150 cases (rows) and 5 variables (columns) named Sepal.Length, Sepal.Width, Petal.Length, Petal.Width, and Species.
##Task/Problem to identify the Species-setosa/versicolor/virginica
data=iris
view(data)
head(data)
str(data)#gives the stracture of data set or data type used
###1.Identify any Missing values are there
summary(data)
any(is.na(data))

#There are no missing values we can go with finding outliers
###2.Identify the Outliers using visualization using library ggplot2
plot(data)
pairs(data)
library(ggplot2)
ggplot(data,aes(Sepal.Length,Sepal.Width))+geom_point(aes(color=Species))+theme_classic()
ggplot(data,aes(Petal.Length,Petal.Width))+geom_point(aes(color=Species))+theme_classic()

#Visualization confirms there are no outliers in the data set
###3.Perform Exploratory data annalysis using libraries like CaTools and splitting the data for training and testing purpose 
library(caTools)
set.seed(101)
sample=sample.split(iris,SplitRatio = .70)
train=subset(iris,sample==TRUE)
test=subset(iris,sample==FALSE)

###4.Data Modeling/Model building using libraries like rpart,rpart.plot 
library(rpart)
names(data)
#iris_model=rpart(Species~Sepal.Length+Sepal.Width+Petal.Length+Petal.Width,train,method = "class")#same can also be written as
iris_model=rpart(Species~.,train,method = "class")#class means classification
iris_model                 
#Tree visualization using in-bulid function and gglot2
plot(iris_model) #only gives lines with out text so add text
text(iris_model)
library(rpart.plot)
rpart.plot(iris_model)
prp(iris_model)
library(ggplot2)
ggplot(data,aes(Sepal.Length,Sepal.Width))+geom_point(aes(col=Species))
ggplot(data,aes(Petal.Length,Petal.Width))+geom_point(aes(col=Species))

####5.Testing the model and checking the prediction
predict(iris_model,test)
predict(iris_model,test,type ="class")##if u compare the result with the view of test set row no. 53,93 is showing the different results.
p=predict(iris_model,test,type ="class")
pq=data.frame(p)

###6.Accuracy check
#Comparing predicted results with acutal data set we use confussion matrix
table(test$Species,pq$p)
###accuracy=20+16+19/20+16+79+1+4=91.6% accuracy
