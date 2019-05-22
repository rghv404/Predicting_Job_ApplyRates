# Predicting_Job_ApplyRates

In this problem I attempt to predict the apply rate of candidates visiting and searching on the
platform. Below are my ndings about the data set and classication done on it:

1 On DataSet and Features
	1.1 Duplicates
		The data set had exactly 86131 duplicate records which were removed to handle any biases. I also
		observed that in a lot of samples the candidate visited the job but did not apply, signied by the fact that
		all features had same value except for apply. I decided to keep only the ones where candidate actually
		applied and hence removed an additional of 150286 records.
		
	1.2 Bias
		As we can see from the image below, the dataset is highly biased with a ratio of around 9:1 for each not
		applied to applied job respectively. The bias depicts a real worlds scenario where every visiting candidate
		will not apply. Multiple techniques to handle this have been discussed in the next section.
		
	1.3 Imputing missing values
		The data set also had features with missing values, particularly the attribute title proximity tdf, descrip-
		tion proximity tdf and city match. After my analyses of correlation among these, I end up imputing
		the mean value to title proximity tdf 's null values and city match is imputed using a logistic regression
		model trained on 'apply', 'job age days', 'main query tdf'
		
	1.4 Outliers
		The biggest contributor to outliers was job age days where job openings were present and applied which
		were more than 6 months old. At rst it may makes sense to remove this feature from our model, but
		the applied rate for these old jobs was a considerable contributor to the overall applied rate. Hence it
		was kept.

On Classication

	Since both the training and target data is highly skewed, I can implement under-sampling of the majority
	class or a combination of both under and over-sampling the two classes using some of the popular
	algorithms like SMOTE.
	Upon implementation of both of these approaches, I observe that random under-sampling the majority
	label is rather naive for our use case because we may lose some important feature sets and aect our
	model's performance. On the other hand, using SMOTE to under and over sample our labels is very
	expensive and the quality of synthetic features will always be questionable on scale.
	One approach that I ended up implementing is to skew my classifier's loss function to favor the minority
	class by assigning custom weights to both classes. This is computationally less expensive method and
	performs on par with above strategies to overcome the bias and improve recall on minority class.

Balanced Random Forest

	I implement the Balanced Random forest, which as the name suggest balances the weights accordingly
	to favor the minority class, below are the results: As can be seen above, the AUC is 0.62 which is a good 
	measure given the quality of features and with the balanced classifier, the recall for both the classes is
	pretty even. Infact, the model was able to predict 5613 correct records out of 9821 minority class. I have
	assumed that Recall is actually important for this case scenario as it will be more valuable to accurately
	know how many candidates ended up successfully applying to the job.
	I update the parameters of the random forest for a slightly better prediction,
	the important parameter here is the use of custom class weights to upscale the minority class.
