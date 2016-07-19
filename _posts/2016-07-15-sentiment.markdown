---
layout: post
title:  "Getting Sentimental"
date:   2016-07-14 1:40:40 -0800
categories: sentiment analysis logistic regression n-gram
---

<p align="center">
	<img src="/t-wex.png"> 
</p>

I just got back from a week long backpacking trip. I decided that I was going to take a break from my chess engine to do a couple quick experiments in other areas of machine learning. Yesterday I wrote a simple sentiment analyzer in 50 lines of python. In this post I will explain what I did and how it could be improved.  

**The Data**  
I got my data from the [Sentiment 140](http://help.sentiment140.com/for-students/) database. It contains 1.6 million tweets labeled as either positive negative or neutral. I only used the positive and negative tweets from the database because I was using a binary classifier for simplicity. As a side note databases like Sentiment 140 can be quite biased towards the opinions of certain groups of people. I found [this talk](https://www.oreilly.com/learning/how-we-amplify-privilege-with-supervised-machine-learning) to be very interesting and intutive explantion of the biases in supervised machine learning.  

**The Algorithm**  
The algorithm I used is pretty basic as natural language processing goes. It has two main steps, preproccesing the text and classification.  

**Preprocessing**  
First I convert the tweets into vectors using a bigram model. This means that each unique word and pair of words becomes its own feature.

{% highlight python %}
#This tweet
tweet = 'I am a drama llama'
#Produces these features
['I','am','a','drama','llama','I am', 'am a', 'a drama', 'drama llama']
{% endhighlight %}

Our feature space is contructed by feeding all of our training data into this model. To convert a tweet to a vector all we do is make a vector which counts how many times each feature occurs in the tweet multiplied by two scalar terms called the term frequency and the inverse document frequency (idf).  
The term frequency of a feature is simply the frequency of the feature in the current document(tweet). The rational behind the tf is that words that occur more might matter more. The inverse document frequency is defined as follows,  
<p align="center">
	<img src="/idf.png"> 
</p>
where N is the total number of documents and *df* is the number of documents containing our feature. As far as I can tell the *log* is only there to keep the size of the idf from exploding. The rational behind this term is that features that occur in multiple documents are more likely to provide important clues about the document so they should be weighed higher. "But wait,", you ask, "what about word like 'the', they occur everywhere but are essentially meaningless." Well the answer is that we ignore the words that occur in too many documents and too few documents. In this way wors like 'the' will be ignored because they are too frequent.

**Classification**
Now that we have high dimensional representions for the tweets as vectors we just need a way to map those vectors to a sentiment (good or bad in this case). I've chosen to a support vector machine to classify the tweets. A support vector machine just finds the hyperplane which best divides the two labeled sets of tweet vectors (good and bad). Then we classify new tweets as good or bad based on which side of the plane they are on.

**Code**

Below is the full source code. Feel free to run it yourself. You'll need the sentiment140 data to do so as well as numpy, sci-py and, scikit-learn.
{% highlight python %}
from sklearn.linear_model import SGDClassifier
from sklearn.pipeline import Pipeline
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn import metrics
import numpy as np
import pandas
import random

def build_model(training_in, training_target):
	#the parameters min_df and max_df where choosen experimentally
	#the hinge parameter specfices that we are using an svm
	text_clf = Pipeline([('tfidf', TfidfVectorizer(ngram_range=(1, 2), encoding = 'ISO-8859-1', max_df = 0.002, 
		min_df = 0.00015)),
		('clf', SGDClassifier(loss='hinge', penalty='l2', alpha=1e-3, n_iter=200, random_state = random.randint(0,100000))),
	])

	text_clf.fit(training_in, training_target)
	return text_clf

def load_data():
	training_data = pandas.read_csv("data/training.1600000.processed.noemoticon.csv", names=["class","id","time","query","user","text"])
	testing_data = pandas.read_csv("data/testdata.manual.2009.06.14.csv", names=["class","id","time","query","user","text"])

	#In the data there are three classes of data [0:negative, 2:neutral, 4:positive] 
	#For this example we are only going to tag things are positive or negative
	training_data = training_data[(training_data["class"] == 0) | (training_data["class"] == 4)]
	testing_data = testing_data[(testing_data["class"] == 0) | (testing_data["class"] == 4)]
	return training_data, testing_data

def main():
	print("Loading Data")
	training_data,testing_data = load_data()

	print("Constructing Model")
	model = build_model(training_data["text"], training_data["class"])

	print("Predicting Output")
	predicted = model.predict(testing_data["text"])
	print(metrics.classification_report(testing_data["class"], predicted, target_names=["bad","good"]))

	print("Enter Some Phrases to Test the Sentiment Analyzer or Type 'quit' to Exit.")
	phrase = ""
	while True:
		phrase = raw_input(">>>")
		if phrase == "quit":
			break
		predicted = model.predict([phrase])
		print "class:" + str(predicted)

if __name__ == "__main__":
	main()
{% endhighlight %}
  
**Improvements**  
This solution is only 80% accurate on the test set. That is a significant improvement on the 50% chance of guessing randomly but there is still a long way to go. Possible improvements could include, taking word order into account, uzing something like [word2vec](https://en.wikipedia.org/wiki/Word2vec) to understand the meanings of words better, using a more complex classifier than a svm.
