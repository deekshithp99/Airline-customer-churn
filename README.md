# Airline-customer-churn
## INTRODUCTION
Air travel has had a profound effect on our society. It is one of the main drivers of globalisation. Commercialization of air travel made our world a smaller, more connected place. People can easily explore the far corners of the Earth and build relationships with cultures far removed from their own. Businesses can grow and link with one another, and goods can be shipped worldwide in a matter of hours. The customers now have a wide range of options in terms of airlines, to choose from and would prefer to travel by that airlines more frequently which provide higher satisfaction and services compared to the others. So, customer satisfaction is an important factor which leads to the growth of an airline per say. 

In this report we will conduct an exploratory data analysis of flights’ data conducted by various airlines in the United States. It contains various information for each recorded flight, such as the origin, destination and the distance between them, the date and time of departure and arrival, details regarding delays or cancellations, information about the operating airline, and so on. This report focuses on the customer survey collected from many people for various airlines. We are working on a dataset consisting of 14 airlines, 32 different variables or attributes that affect the customer satisfaction and has 10256 observations.    

We have used several key attributes which affect the recommendation score given by the passengers, like flight path, flight delays, the airline type etc. in order to analyse our survey. This helped us to understand the dataset better and as a result we have used different data analysis models and different data visualization tools to analyse the recommendation score given by passengers from the dataset. We have also added new attributes to know how different attributes can influence the recommendation score together. 

We have used Linear Modelling and Association Rule Mining to analyse our dataset and then in order to validate our results we have used Support Vector Machine model. We have used these modelling techniques to gain how different attributes or factors may affect customer satisfaction during a journey. By studying the data with the help of visualization tools and modelling techniques, we tried to analyse why certain trips have low recommendation score. 

Different market segments were also identified and analysed in order to provide actionable insights which would increase the customer satisfaction or recommendation score.
##BUSINESS QUESTIONS 
The Likelihood to recommend score or customer satisfaction score provided us with feedback with which we could work on how to better improve the airlines’ services. The scores helped us in obtaining better insights on which attributes or factors to improve or pay more attention to in order to increase the customer satisfaction. There are several airlines out there, so in order to improve an airline’s customer base and loyalty, it should focus on improving the customer satisfaction which in turn could give rise to higher profits and revenues.

The different business questions that have been identified and answered through the project are:
1.	What are the main attributes or factors that affect the recommendation score or customer score of the dataset?
2.	How do different independent factors or attributes affect the customer satisfaction score or recommendation score?
3.	Which attributes or factors positively correlated with the recommendation score or customer score of the dataset?
4.	Which attributes or factors negatively correlated with the recommendation score or customer score of the dataset?
5.	What factors or attributes provide good recommendation score or customer score of the dataset?
6.	Who are the customers or passengers who gave the lowest satisfaction scores?
7.	What factors can be improved to increase the customer satisfaction scores or provide a better experience to customers?














##DATA PRE-PROCESSING / DATA PREPARATION
In order to analyse and study the data, the initial steps include loading the data and cleaning it in order to avoid unnecessary oversights which may distort the results in some cases. Therefore, the first things to do are cleaning, munging and processing the given data.
##A. DATA LOADING 

The dataset was provided by our professors for our final project in Introduction to Data Science. The file was a json file. It was downloaded and then loaded into the R studio by using RCurl and jsonlite packages.

library(RCurl)
library(jsonlite)
library(tidyverse)
library(ggplot2)

dataset <- 'Spring2020-survey-02.json'
testdf <- jsonlite::fromJSON(dataset)
View(testdf)
summary(testdf)
 

##B. DATA PREPROCESSING 

The dataset consisted of many different attributes which needed processing and cleaning. There were a lot of NA values in some of the columns like departure delay, arrival delay, flight time etc. Some of the NAs were replaced with zeros as the flights were cancelled for that respective data and some other NAs were replaced with mean of that column. I also searched for white spaces and converted attributes into numeric and factor data types. Later on, in the code, some attributes were ignored, and other attributes were focused upon due to their significance. 

The code below is the following code used for cleaning and processing the data:

 

 















USE OF DESCRIPTIVE STATISTICS

This section focuses on how the different independent attributes influence or impact the dependent variable. In this section, histograms and tables are used to compare and study the attributes affecting the dependent column or the customer satisfaction scores.

Some of the code used for studying the data is given below:

table(testdf$Age)
#Most number of people are in the range of 35-55.
table(testdf$Destination.City)
table(testdf$Origin.City)
table(testdf$Airline.Status)
#Blue     Gold Platinum   Silver 
#6988      859      320     2089 
table(testdf$Gender)
#Female   Male 
#5883   4373 
table(testdf$Type.of.Travel)
#Business travel  Mileage tickets  Personal Travel 
#6252             858              3146 
table(testdf$Class)
#Business      Eco      Eco Plus 
#790           8382     1084

table(testdf$Flight.cancelled)
#No   Yes 
#10061   195 
table(testdf$NPS)
#Detractors    Passive  Promotors 
#3005          3336       3915 
table(testdf$AgeG)
#MiddleA    OldA  YoungA 
#4115       2973    3168
table(testdf$Delay)
#  NO  YES 
#6323 3933 
table(testdf$Long.Duration)
#FALSE  TRUE 
#4024  6232

prop.table(table(testdf$AgeG, testdf$NPS))
testdf$AgeG <- as.factor(testdf$AgeG)
testdf$NPS <- as.factor(testdf$NPS)
table(testdf$AgeG)
plot(testdf$AgeG, testdf$NPS)

From the graph, we can understand that the highest number of detractors were in the Older Adults category, followed by Young Adults. The promotors were mainly in the Middle age adult’s category followed by younger adults. The passive was in the older adult’s category followed by both middle age and younger adults categories.
 

table(testdf$Airline.Status)
testdf$Airline.Status <- as.factor(testdf$Airline.Status)
plot(testdf$Airline.Status, testdf$NPS)

From the graph plotted, it can be understood that people who travelled in the blue airline status program were the people who were detractors followed by platinum and gold categories. The passive NPS scores were given by silver category followed by blue and gold categories.

 
testdf$Gender <- as.factor(testdf$Gender)
prop.table(table(testdf$Gender, testdf$NPS))
plot(testdf$Gender, testdf$NPS)

From the plot, we can make out that female passengers were more likely to be detractors and male passengers were more likely to be promotors.

 
testdf$Class <- as.factor(testdf$Class)
table(testdf$Class, testdf$NPS) 
For comparing two independent variables.
prop.table(table(testdf$Class, testdf$NPS)) 
The following command expresses the results of the table command as percentages.
plot(testdf$Class, testdf$NPS)

From the plot, we can see that there are more detractors in the economy plus category followed by economy category. The passive category is dominated by economy plus and then economy. The promotors are most likely economy passengers.

 













##USE OF MODELLING TECHNIQUES

##LINEAR MODELLING

In this phase, we develop different modelling techniques to analyse the data and remove unnecessary variables so that we may only work with variables which directly impact the dependent variable.

Linear modelling is applied to only some selective variables such as filtering out, for example, origin city and destination city because they may reflect variability in the data as they contain too many values in them. That may lead to unnecessary variability which may affect linear modelling.

Also, in linear modelling, significance of the factors are decided with the help of p-values. If p-value is less than 0.05, then that factor can be considered as significant or that it impacts the dependent variable directly. Asterisks mentioned at the side of factors provide the significance based on the order of 3 asterisks, 2 ,1 and a dot may imply that among all the one with dot is least 
significant. The factors which do not have any type of symbols at the side imply that these factors do not impact the dependent variable.

lmplotf <- lm(formula = Likelihood.to.recommend ~ Origin.State + Destination.State + Airline.Status + AgeG + Gender + Price.Sensitivity + Flights.Per.Year + Loyalty + Type.of.Travel + Eating.and.Drinking.at.Airport + Class + Delay + Flight.cancelled + Long.Duration + Flight.Distance + LoyCol, data = testdf)
summary(lmplotf)
The above linear model is the final model because we only considered significant attributes which affect the dependent variable. The adjusted r-squared value here is 0.42 and the p-value is < 2.2e-16.
The output of the command is given below:
Call:
lm(formula = Likelihood.to.recommend ~ Airline.Status + AgeG + Gender + Price.Sensitivity + Flights.Per.Year + Loyalty + Type.of.Travel + Eating.and.Drinking.at.Airport + Class +  Delay + Flight.cancelled + Long.Duration + Flight.Distance + LoyCol, data = testdf)
 





##ASSOCIATION RULES MINING
In order to implement association rules mining, the data set must be converted into a data frame of transactions data type. In order do that, the above command helps us in doing that certain operation. But an error occurred as we try to do that. As this transactions data type requires the variables to be in factor data type, errors occur, so we must convert them into factor data types.
Apriori is a specific algorithm that R uses to scan the dataset for appropriate rules. It uses an iterative approach known as level-wise search. In the below command, apriori algorithm is used on dfX sparse matrix by taking the support as 5% and confidence as 50%. The algorithm is also given the appearance of parameter where the lhs can be a combination of any objects among the matrix and rhs must be such that the NPS class should be for Detractors.
 
The above screenshot details the association rules created for detractors and passive scores.
The plots for the above rules are depicted below:
 
 




VALIDATION 
##SUPPORT VECTOR MACHINES
The reason SVMs are considered a supervised learning technique is that we train the algorithm on an initial set of data (the supervised phase) and then we test it out on a brand-new set of data. If the training we accomplished worked well, then the algorithm should be able to predict the right outcome most of the time in the test data. This is the basic strategy of supervised machine learning: Have a substantial number of training cases that the algorithm can use to discover and mimic the underlying pattern, and then use the results of that process on a test data set in order to find out how well the algorithm and parameters perform in a cross- validation. Cross-validation, in this instance, refers to the process of verifying that the trained algorithm can carry out is prediction or classification task accurately on novel data.
The code for the SVM is given below:
trainList <- createDataPartition(y=detdf$NPS, p=.65,list=FALSE)
trainData <- detdf[trainList,]
testData <- detdf[-trainList,]
dim(trainData)
dim(testData)
svmOutput <- ksvm(NPS ~ ., data = trainData, kernel = "rbfdot", kpar = "automatic", C=5, cross = 3, prob.model = TRUE)
svmOutput
svmPred <- predict(svmOutput, testData)
str(svmPred)
head(svmPred)
confusionMatrix(svmPred, testData$NPS)
The output for the above code id given below:

Support Vector Machine object of class "ksvm" 
SV type: C-svc  (classification) 
 parameter : cost C = 5 
Gaussian Radial Basis kernel function. 
 Hyperparameter : sigma =  0.0296451017232823 
Number of Support Vectors : 5351 
Objective Function Value : -8499.546 -4930.441 -10399.12 
Training error : 0.161068 
Cross validation error : 0.385272 
Probability model included.
 

##ACTIONABLE INSIGHTS

The attributes which positively affect customer satisfaction are:
1.	The passengers who used airlines for business travel were the most likely to give good satisfaction scores.
2.	If the flights were cancelled, the satisfaction scores were high.
3.	The passengers who travelled in Business class were also among the passengers who gave good satisfaction scores.
4.	Male passengers were more likely to give good satisfaction scores.
5.	The middle age people who were in the age group of 35-55 were most likely to be giving good satisfaction scores.


The attributes which negatively affect customer satisfaction scores are:
1.	Female passengers were more likely to give low satisfaction scores when compared to male passengers.
2.	The passengers who were in the age group of less than 35 and more than 55 were the most passengers who have given less satisfaction scores.
3.	The passengers who used the airlines for personal travel were also the people who have given less satisfaction scores.
4.	The passengers who travelled in Blue airline status category were also among the ones who have provided with low satisfaction scores.
5.	Also, the passengers whose travel duration is longer were the ones who have given less satisfaction scores.
6.	The passengers who travelled in Economy class were also the people giving less satisfaction scores.
7.	Passengers who have travelled in other airlines and were less loyal were the passengers who have given less satisfaction scores.

The actionable insights which can curb or decrease these negative attributes are listed as below:
1.	The longer duration flights’ passengers were some of the people who given less scores. So, better inflight entertainment options and better flight operations are needed to reduce the delay and provide entertainment and comfort to the passengers.
2.	Blue airline status passengers were also the ones who have given less satisfaction scores. Therefore, that airline should be focused upon and its operations have to be improved to reduce the scores.
3.	Female passengers were also some of the maximum number of detractors. Therefore, female passengers must be given more comfortable options while travelling and the airline needs to study what issues or problems female passengers are facing.
4.	The passengers who travelled for personal reasons should also be focused upon as they were some people who were among the detractors. 
5.	The people flying in economy class should also be focused upon and their inflight travel entertainment and other features must be improved to retain that class of customers.
6.	The passengers who are less loyal, or who travel different airlines are also some of the detractors, so additional features must be introduced to provide the same class of features other airlines are offering.
7.	Older adult passengers were the main detractors, so they must be focused upon. Airlines need to know why this class of passengers are facing issues and new comfortable features and operations should be introduced.
