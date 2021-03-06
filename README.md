# YouTube Video Analysis
By: **John Pazzelli**  
January, 2018

### Q:  How does a video's metadata (e.g. title, description, tags, thumbnail) influence its view count?
## Abstract
Todays media agencies, news outlets and marketing companies are undoubtedly interested in the efficacy of their digital video content to attract and engage their customer base and to increase revenues.  It would therefore be useful to have a sense of the ultimate popularity of a video based on its metadata information alone (i.e. the title, description, thumbnail image and categorization keywords/taxonomy) as this is typically the only information that is visible to a user when he or she decides whether or not to watch a video online.  Given the vast popularity and large number of videos available on YouTube as well as the ease, speed and low cost of obtaining video metadata through the Google Cloud API, **this project therefore attempted to quantify the importance of a video's metadata fields in determining its popularity**.  

To accomplish this, this project sampled and analyzed about **30k YouTube videos** using the [Google Cloud YouTube Data API](https://developers.google.com/youtube/v3/getting-started) to determine if each video's metadata fields (**Title, Description, Tags, Duration, Category** and **Thumbnail Image**) can be used to **predict the popularity** of the video (i.e. the view count).  Thumbnail images were passed through the [Google Cloud Vision API](https://cloud.google.com/vision/docs/) and converted to a list of text labels.  All textual data was pre-processed using **text vectorization** and passed into a regression model using a **Linear Support Vector Regressor** (LinearSVR) in scikit-learn for a **peak R^2^ (coefficient of determination) score of ~0.5**.

The **model was interpreted** by examining the **coefficients** assigned to each of the ~2500 features by the LinearSVR, consisting of the Duration and Category fields as well as the vocabulary terms generated by the text vectorizers for each text field.  

## Results
The **Duration field was assigned the largest positive coefficient** and was thus the **best baseline predictor** for video popularity.  Excluding the Duration field, the remaining top 100 model coefficients were found to lie in the 4 text fields as shown in the pie chart below.  Clearly, the **Description field has the highest relative importance** when predicting video popularity as it contains nearly 3 of 5 of the top 100 vocabulary terms whereas the **labels for the thumbnail images** returned by the Google Vision API were **least useful** and only appeared in 3 of the top 100 terms:  

![pie chart](https://github.com/JP-DataScienceProjects/YouTubeVideoAnalysis/blob/master/Screenshots/TopCoefficientsPieChart.png)

### Best Vocabulary Terms
As for the **vocabulary terms** themselves, those that best predict **high view counts** for each of the metadata text fields are shown in the wordcloud below with **larger words representing better predictors** of video popularity (note that **word stemming** was used so the words are frequently missing suffixes):  

![wordcloud](https://github.com/JP-DataScienceProjects/YouTubeVideoAnalysis/blob/master/Screenshots/Wordcloud_combined.png) 

The **top 40 highest-weighted words within each individual text field**, are also displayed in separate word clouds below:

![term-based wordclouds](https://github.com/JP-DataScienceProjects/YouTubeVideoAnalysis/blob/master/Screenshots/Wordcloud_individual.png)

### Video Categories
Finally, the **video category** played a significant role on the view count prediction in the final model with videos in the **Shows and Music ranking much more highly** than those in the **Nonprofits & Activism, and People & Blogs** categories:  

![bar chart of video category coefficients](https://github.com/JP-DataScienceProjects/YouTubeVideoAnalysis/blob/master/Screenshots/CoefficientsByVideoCategory.png)

## Conclusion
Based on the model results, the **video duration plays the most important role in predicting video popularity (longer videos tend to have higher view counts)** while the **description field** plays the second most important role in predicting video view counts with nearly 3x the influence of the title and tags fields and nearly 20x the influence of the thumbnail image.  **Terms most highly correlated with high view counts** were those related to multimedia (e.g. **music videos, movies, lyrics, performances, celebrities**) and social media (e.g. **reddit, buzzfeed**).

The **LinearSVR-based model** for this project was chosen as it had a **very fast training speed** on a single, quad-core CPU as compared to other approaches (e.g. SVM with RBF kernel), allowing for rapid iterative tuning of model features and hyperparameters.  This model was powerful enough to account for half of the model variance (**R^2^ = 0.5**), however there is still approximately half of the variance unexplained.  

A **more accurate model** with higher R^2^ score could likely be built on **more powerful hardware** using a more complex approach such as a neural network while still retaining more of the pre-processed text features (e.g. n >= 50,000 instead of n = 2500 as used here).
