# Spotify Recommendation System

This project’s goal is to provide automatic playlist continuation which would enable any music
platform (here Spotify) to seamlessly support their users in creating and expanding the playlists
by making recommendations based on their choices and preferences.

### Raw Data
The data provided by Spotify is a Million Playlist Dataset (MPD) which contains 1 Million
playlists created by the Spotify users. This dataset is quite huge (about 5.4 GB) and thus it has
been divided into 1000 json files, where each file contains 1000 playlists comprising to 1,000,000
playlists in total.

### Data Collection and Pre-processing
Due to the humongous nature of data, the idea of scraping the entire dataset was not
viable so sized down to using about 6 json files (limitations of hardware) and also decided to
work with only certain important features that are extracted with Spotify Api calls. This can be found
in the 'DataScrape Notebook' python notebook file.

Dataset needed to be preprocessed before modelling was done. Some cleaning was done in Microsoft Excel:
* Deleted rows with headers (because they were all merged, every file that was merged
had its own header row)
* Removed duplicates based on track_id - there were obviously many duplicates because
playlists don't all have different songs

A merged CSV file was created which was then processed.

* Then Album released date were in different formats and all those formats were
converted to YYYY format. 
* Then non quant and garbage columns were dropped. There were many columns with
N/A values which were also dropped. 
* All the quantitative columns were standardized for consistent range. 
* Correlation Matrix was plotted for the data. With the matrix, analysis was done and many
data columns were dropped to reduce redundancy. 

### Data Modelling
we tested two main approaches to handle the problem and generate recommendations.

###### Model1:
The first approach is based on a user rating system. We provide the 3 different users
randomly generated songs (20 in our case) from our database, and in turn, they give
back ratings on a scale of 1-10 for the songs (with 10 being the highest). We then
trained a model to learn their preferences and predict what ratings they would give for
any other song, based on the characteristics of those given songs and how they rated
them. Our ultimate goal is to give them back a list of any number of top rated songs from
our mode (we chose to gave them their 5 top favourites)

###### Model2:
For the second approach, we wanted to address the fact actual music recommendation
systems base their recommendations off songs in playlists. When users add particular
songs to their playlist, they are assumed to consider those songs “good” and to their
taste, so it is quite unorthodox to base a model off songs that users rated poorly as well
(as we did in the first model). The issue with this, however, is that user-provided playlists
could very easily have songs that were not in our smaller database. Thus we would be
unable to actually test this model on real users, but we made it nonetheless.

## Results and Discussion
###### Model1:
Our recommendation system worked pretty well with this approach. One of our users 'Helen' preferred 
the predicted playlist from the Simple Linear Regression(SLR) model over the other 2 models(Random Forest 
and Neural Network).

###### Model2:
The second approach is validated based on CV scores. We ran Logistic
Regression, RandomForest, Gradient Boosting, K-NN & created a neural network as
well to classify the tracks. We divided the dataset in train/test and did upto 10-fold cross
validations on each of the models. We generated the R 2 Error values and compared
them. The cross validated scores were used to validate the errors generated. Also, we
used cross validation to get the optimal number of neighbors for our K-NN model.
Overall after the comparisons, we found that the Random Forest have an accuracy of
~0.99 on the test set which makes it a stand out model.

##### For detailed information, please read the report pdf 
