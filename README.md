# Kaggle-project--Elo-loyalty-score-prediction

### Backgroud information about the project
Elo provides recommendation to each card holder based on his/her historical transaction records, if the card holder visits the new merchant based on the recoomendation, the transaction records are then kept in the new merchant transaction table. By measuring the reply of the recommendations that are given to the customer, Elo then assigned the loyalty score to each customer (Elo doesn't reveal the detail of how the loyalty score is calculated). 

### The objective of the project
Loyalty score prediction: by utilizing the information in different tables to make accurate loyalty score prediction.

### Data set information
The data is contained in 3 types of tables, a/cardholders information table (about 200,000 instances) b/transaction history (historical transaction table (18 milion instances) & new merchant transaction table (1 milion instances)) c/marchant level information table (about 300,000 instances)

### The outline of the project
1) Exploratory Analysis-learn feature distributions / relationship between tables
2) Extract feature for model building
3) Model building, evaluation and result

#### Some notes about feature aggregation
To obtain the final data frame for model building, the cardID from the cardholders information table is used as an index to find the corresponding transaction records and merchant information from other tables that belong to each cardholder.  

To solve the one to many relations between tables (each cardhold has multiple tranasaction records) we need to extract features from the transaction history which can be done by taking the statistics of each variable of the trannsaction history table and merchnat information table. For example, we can extract the average value, the max value, the min value, and standard deviation value for the numerical variable like purchase amount from the transaction records, if the variable is a categorical variable we can then take the mode value and min value.  

In addition, we can also intuitively transform the variables. For example, for the variable first active month of the card, to keep the information for the model building we have to transform the date type variable in some ways. Since, each cardID has different reference date (the date that Elo starts giving the recommendation to the cardholder and see if he visits the new merchant, it’s also the date it starts calculating the card holder’s loyalty score), we can calculate the month lag of the first active month of the card from the reference date as the new feature.

#### Some notes about model building and evaluaion outline
1.	Data Splitting:
After feature aggregation, we obtain the final data frame for model building with the cardID index provided from the Kaggle training set. We first split the data frame into 80% and 20% as a new data set and validation set. Then for the new data set, we further split it into 80% and 20% as the training set and the testing set.
2.	Model tuning:
For the optimal model, apply 5 folds cross-validation on the training set to tune the parameters to find the optimal model.
3.	Optimal model:
Report its average re-substitution estimate error on the training set
4.	Testing:
Test the optimal model on the testing set with 5 folds cross-validation to obtain generalization estimate error
5.	Final model selection:
Select the final model based on the average performance on the testing set. 
6. test the reliability of model by calculating the confidence interval

#### Some notes about model 
The model applied in this project is the tree-based model, and the reason for that is because there aren’t any significant linear relationship between variables in the data set. The advantage of using the tree based model is that variables don’t have to have any correlation, and the model can also provide some information about feature importance when it’s splitting. 

However, the disadvantage of the tree-based model is that a single tree tends to be unstable, that’s I decided to apply ensemble methods, the gradient boosting, to solve this issue. On the other hand, since the data set is relatively large, I eventually applied the lightGBM to overcome the training speed issue during the experiment. 
