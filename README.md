## Introduction 
Persistence is not new, and it's important. Continuing on in the face of failure or difficulty is a trait just as desirable today as it was then (Feather, 1962). Prior research has revealed a modest connection between persistence and learning gains, espeicailly with learning difficult topics (Ying et al., 2017). 

But how do we best measure persistence? As it is currently often defined, as continued effort as continued voluntary pursuit of a goal in spite of appropriate difficulty. This definition lends itself well to using time spent on or number of attempts to complete a difficult task (DiCerbo, 2014; Eisenberger & Leonard, 1980). However, recent studies have begun to realize that, like anything else, too much persistence can be problematic (Beck & Gong 2013; Ying et al., 2017).

### Deep neural networks
Further exploration of other behaviors that align with persistence must be explored. Neural networks—specificially Deep Learning—offer the opportunity to make predictions using large datasets, which digital games are ideal for generating, that make use of complex patterns that may be out of reach of the organic mind. Neural networks, unfortunately, do not lend themselves well to interpretation. For research purposes, understanding the finer details of persistence isn't aided by Deep Learning as much as practical application within researhc (e.g., real-time assessment of persistence in the classroom). By training a neural network on data, labeled using a common proxy for the target construct, reserachers may be able to expidate the process of data collection. For example, if I need to measure persistence as a mediator or even predictor of another outcome varible, then I can use typical proxies (e.g., self-report measures) to generate labelled datasets, create and train a neural network on that dataset, then use the model to predict future participants' persistence.  

By relying on neural networks to make accurate predictions, the time spent on collecting data (e.g., participant's completing surveys) is greatly reduced. This process could also make very large scale studies more attainable by eliminating the need for human coders/graders. And, finally, the model would be retrainable on alternative game environments, which makes the deep learing approach more generalizable for educational research. 

### Engagement 
It is a common misconception that, when relying on machine learning methods, any and all variables can be included in the model to improve the predictive accuracy. There is still a necessity to include logical and/or theoretically sound predictor variables. This study included data related to object creation within a level. The seven object creation variables include simple counts of how many objects (e.g., lines, pins, and simple machines) were created within a level. The expectation being that more engaged (i.e., objects drawn) the students are, the more they are persisting. Also included was the difficulty score of the level being played and the pretest score of the participant playing the level. 

## Method
The final dataset includes nine predictive variables. The predicted outcome is the log-transformed duration spent on a level. All missing values were a result of technical fault. Most were a result of students closing the game without logging out. All missing values were dropped rowwise to keep the length of arrays equal. The log-transformation resulted in one "divide by zero" error. The error in question was recoded as zero after the log-transformation.

Using the Keras Sequential API, I created a neural network. The structure of the network is shown in figure one. I created training and testing datasets (20%) and trained the model on the training dataset over 100 epochs with batch sizes of 20. The loss function was mean squared error (MSE), the optimizer adam, and an accuracy metric was generated with each epoch. The loss fuction and adam were chosen based on manual comparison of different loss fuctions and optimzers avaliable within the Keras API. These two showed the best reduction in loss, quickest training time, and highest accuracy. 

![Model Figure](https://github.com/curiotiks/persist/blob/master/img/model.png?raw=true)

Once trained, I used the model to predict time spent within a level on the testing dataset. The initial training led to small MSE (1.01) but an perfect zero in accuracy. After exploring more of the *evaluate* function, I realized that the outcomes were continuous. Without rounding the values to a single digit, every prediction was wrong because of randomness in the decimals.

So to expand on what I could see—beyond a single accuracy measure—I generated a confusion matrix. The log duration values ranged from zero to nine, so the resulting confusion matrix is 10 x 10. There is a clear grouping in the center, around three and four seconds, where the accuracy is quite good. 

## Results
After rounding the duration values to a single digit, the final MSE was 1.15 and the accuracy .46. The confusion matrix is shown in figure two. The x and y-axises are the possible log durations. It's clear that the bulk of accuracy is coming from durations between three and five. The most accurate predictions are within three and four—also the most incorrect—which may be a result of uneven distribution in the sample. The mean log duration was 4.09 (*sd* = 1.4). Although the accuracy is greatly improved from zero, the confusion matrix shows that there are still inaccuaries present that may be inflating the overall accuracy. Those low-middle values are most common, so they are also most represented in the datset. 

![Confusion Matrix](https://github.com/curiotiks/persist/blob/master/img/conf_mat.png?raw=true)

## Conclusion
Although the results are not parade-worthy, there is promise to these intial findings. Currently, I am performing classification on ten separate categories to make predictions on a continuous scale. Bagging items into artifical categories throws away valuable data. Using the above approach, in my thinking, allows me to keep much of that variability. And, interesting observation, using the confusion matrix, is the distribution of inaccuracies surrounding a value. Note that, moving up and down at x = 3, there is more varaility in the reported predictions (ranging between 0 and 6). But when x = 4, the  variability decreases to just between four and six. 

If I did create grouping variables based an arbitrary divison of the duration variable, the model accuracy would be greatly improved. I would recommend at least starting with making five groups. The goal being to collapse the variability around a prediction by making more potential predictions "correct". We trade precision for accuracy. Whether or not that exchange is worthwhile is dependent on the model's purpose. Running real-time in a game distributed to schools throughout the country would warrant the cost. 

### Appendix
https://github.com/curiotiks/webspace/blob/main/PP_Analysis/PersistenceNN.ipynb
