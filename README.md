# KaraokAI

# Table of Contents


1. [Introduction](#introduction)
1. [Dependencies](#dependencies)
1. [Solution Description](#description)
1. [How to Use](#how-to-use)
1. [Result](#result)
1. [Evaluation](#evaluation)
1. [Difficulties](#difficulties)
1. [Discussion](#discussion)
1. [Acknowledgements](#acknowledgements)

# Introduction

A web-based karaoke application to align a youtube song with its lyrics.

This code is an implementation to download a YouTube video using `youtube-dl` library and converts it to a WAV audio file. The audio is then processed using `pydub` and `spleeter` library to separate vocals from the audio. The processed audio is then transformed into a torch tensor and resampled. The processed audio is then passed through a pre-trained torchaudio `WAV2VEC2_ASR_BASE_960H` model to get emission probabilities. The emission probabilities are then used to create a trellis and decode the transcript of the audio.

## Dependencies

The code uses the following libraries:

- **youtube-dl**: to download the video
- **pydub**: to extract the audio from the video
- **spleeter**: to separate vocals from the audio
- **torchaudio**: for speech recognition task
- **jupyter-dash**: for building interactive dashboards with Python
- **dash-player**: lightweight video player component for the Dash library
- numpy, pandas, re, torch

## Description

The solution has four main parts: audio extraction, forced alignment, genre classification and website building.

### Audio Extraction

In the first part, the audio is extracted from the YouTube video using the youtube-dl library. Then, using the pydub library, the audio is saved in the WAV format and resampled to the target sample rate.

If the `use_spleeter` flag is set to `True`, the audio is processed with the Spleeter library to separate the vocals from the background music.

### Forced Alignment

The extracted audio is transformed into a torch tensor and resampled to the target sample rate. The processed audio is then passed through a pre-trained torchaudio `WAV2VEC2_ASR_BASE_960H` model to get emission probabilities. The emission probabilities are then used to create a trellis and decode the transcript of the audio using a forced alignment algorithm. The resulting aligned transcript is then displayed on the website.

### Genre Classification

The genre of the audio is predicted based on the song lyrics using a genre classification model trained on data set of 108166 samples including lyrics and their corresponding genres. The model predicts 7 genres: "Rock", "Hip Hop", "Pop", "Indie", "Heavy Metal", "R&B" and "Country". The lyrics of the song are passed through the model and the predicted genre is displayed on the website. The trained model and the corresponding vocab can be found in the genre_classification folder ("final_model.pth", "vocab.pth") and need to be loaded into the karaokAI notebook.

### Website Building

The solution is built using the Jupyter-dash and dash-player libraries. The user inputs a Youtube link and its transcript in a web form. The form is processed by the Jupyter-dash backend and the audio is extracted, processed and predicted genre is displayed. The video is also displayed using the dash-player component. The user can reset the form and start over by pressing the reset button.

## How to Use

1. Start Jupyter Notebook
1. Run the code
1. Open a web browser and go to http://localhost:8050/
4. Input a Youtube link and its transcript in the corresponding fields.

![input_form](https://user-images.githubusercontent.com/12101077/216789643-df5b1366-4c54-412c-acf7-e09e960631c5.png)

5. Press the Submit button to load the video and display the predicted genre.

![website](https://user-images.githubusercontent.com/12101077/216789764-ca0965e3-580e-47dd-9a83-c78ba7d3319f.png)

6. Press the Reset button to clear the fields and start over.

## Result

TODO: input video

## Evaluation

The purpose of the *Evaluation* folder/code is to evaluate the performance of the video-text-alignment by calculating the Intersection over Union (IoU) between the ground truth and predicted segments of lyrics.

### Requirements

The following packages are required to run the code:

- **pandas**: to read the data from the csv files
- **numpy**: to calculate mean and histograms
- **matplotlib**: to plot the results

### How it works

The code reads the data from `eval_list.csv`, which contains the list of songs to evaluate, and for each song, it reads the corresponding `{video_id}.csv` file containing the ground truth and predicted segments of lyrics.

The IoU is calculated between the ground truth and predicted segments of each word, and the average IoU for each song is stored in the dictionary `dict_id2iou`. The histogram of the IoU values, grouped by the length of the segments, is also created and stored in the dictionary `dict_hist`.

Finally, the mean IoU for all the songs is calculated, and a bar chart is plotted to visualize the distribution of the mean IoU values for different length segments.

### Dataset

The evaluation dataset consists of 8 music tracks with their respective genre, title, Youtube-Video-ID and year. The tracks cover a wide range of musical styles and years. This dataset was created with the intention of providing a comprehensive and representative sample for evaluating the performance of the video-transcript alignment model.

| id | Genre              | Title                                                             | Video-ID      | Year |
| -- | ------------------ | ----------------------------------------------------------------- | ------------- | ---- |
| 1 | Experimental Rock | John Hammond-Jockey Full of Bourbon                                 | Nd5ySUF6SPM   | 2001 |
| 2 | Country            | Lonestar - Amazed                                                   | x-skFgrV59A   | 1999 |
| 3 | Pop                | Julia Michaels - What A Time ft. Niall Horan                       | bPYefBD1Rzs   | 2019 |
| 4 | Folk               | Tim Buckley - Song to the Siren                                      | 0wBV09TOd9g   | 1970 | 
| 5 | Rock               | CANDLEBOX - Far Behind                                              | eu3EuWg2qNI   | 1993 |
| 6 | R&B                | Janet Jackson - Together Again                                      | d5tJviZ-i9k   | 1997 |
| 7 | EDM                | Beam Me Up                                                          | 19_5SjoHGfI   | 2013 |
| 8 | Hip-Hop           | "$UICIDEBOY$" - ANTARCTICA                                            | s1-0lt7b-78   | 2016 |

### Results

The average IoU between the ground truth and predicted segments of lyrics is calculated and plotted in a bar chart, which can be used to evaluate the performance of the speech recognition system. The mean IoU value and the histogram provide a summary of the performance and can be used to identify areas where the system needs improvement.

#### Usage of Spleeter

[Spleeter](https://github.com/deezer/spleeter) is an open-source audio source separation library developed by Deezer. It uses deep learning techniques to separate an audio signal into its constituent parts, such as vocals, drums, and bass. This allows us to easily isolate the vocal of a song for processing and alignment.

| id | IoU  | IoU without Spleeter |
| -- | ---- | ------------------- |
| 1 | 0.68 | 0.62                |
| 2 | 0.73 | 0.56                |
| 3 | 0.66 | 0.54                |
| 4 | 0.77 | 0.51                |
| 5 | 0.72 | 0.37                |
| 6 | 0.72 | 0.37                |
| 7 | 0.64 | 0.24                |
| 8 | 0.68 | 0.5                 |

The above table shows the results of evaluating the performance with and without the use of Spleeter. The id column represents the identifier of the [evaluation dataset](#dataset). The IoU column shows the average intersection over union score between the ground truth lyrics and the predicted lyrics using Spleeter. The third column shows the average intersection over union score without using the Spleeter tool.

The mean of the IoU scores with Spleeter is **0.70**, and the mean of the IoU scores without Spleeter is **0.46**. 
The results indicate that our system performs better when using Spleeter.

#### Filtering small segments

The filtering of data was performed to improve the accuracy of the Intersection over Union calculation. The ground truth data included segments with lengths between 0s and 0.2s, which resulted in a high possibility of these segments receiving an IoU of 0. This is because the resolution of the ground truth data was at 0.1s, meaning that a 0.1s segment can only receive an IoU of 0 or 1 if aligned at the correct 0.1s timestamp. By removing segments with lengths less than or equal to 0.1s and 0.2s, we were able to improve the average IoU by reducing the impact of these short segments on the overall calculation.

| id | IoU (0.1s filter) | IoU (0.2s filter) |
| --- | --- | --- |
| 1 | 0.74 | 0.81 |
| 2 | 0.79 | 0.82 |
| 3 | 0.72 | 0.78 |
| 4 | 0.80 | 0.83 |
| 5 | 0.77 | 0.80 |
| 6 | 0.75 | 0.00 |
| 7 | 0.66 | 0.66 |
| 8 | 0.71 | 0.73 |

We measured the IoU scores for 8 audio tracks. The results were filtered such that words shorter than 0.1s and 0.2s were removed from the IoU calculation for the second and third column, respectively.

The second column shows the IoU scores for each audio track with a mean of **0.74**. The third column shows the IoU scores for each audio track with a mean of **0.78**.

It was noted that there was a 0-value-issue with the 6th row in the third column, which was a bug that could not be resolved after multiple attempts. Despite this, the mean was still taken over the remaining 7 tracks.

## Difficulties

### Forced Alignment

- **Polyphonic Segments**: We encountered challenges when it came to aligning the video with the transcript in cases where multiple people are singing. The issue arose as it became difficult to accurately match the speech with the corresponding audio in such scenarios.
- **Lack of Training Data**: We had a lack of data in the form of songs with corresponding transcripts that included start and end timestamps. This made it difficult to properly align the video with the transcript when people were singing. To overcome this challenge, alternative methods such as using audio separation techniques were used to better isolate the voices in the audio. However, having more comprehensive training data would have greatly improved the accuracy and effectiveness of the alignment process.

### Genre classification
The main challenge was the training dataset. “Garbage in, garbage out” is a classic saying in machine learning since the state of the training data highly impacts model performance. Therefore, it was important to ensure that the dataset is of high quality. However, it is not possible to go through all the lyrics and check for mistakes such as typos or wrong lyrics. One issue we tried to tackle was the different spelling of certain words (e.g., “talkin’” vs. “talking” or “oooh” vs. “oh”) In order to avoid a bloated vocabulary, we dropped every word below a minimum frequency of 500. We chose that number since songs have a lot of repetition and we didn’t want misspelled words in one song that likely don’t show up in other songs to make it into the vocabulary due to high repetition.

Another issue were the assigned genres to each song. Most songs don’t belong to only one genre but are usually a mixture of and could be assigned to multiple genres. The dataset assigned each song a string of multiple genres such as “Pop; R&B; Black Music”. But predicting the correct combination of genres was not realistic due to the high number of possible combinations. Instead, we split the genres and chose the first one as the label for each song. However, it is questionable if the resulting labels are a good fit for the song. For example, songs by Beyoncé were consequently categorized “Pop” even though one could argue that most of her music is leaning more into “R&B”. Going through each song manually and checking the genre-song-fit would be very time-consuming.

In total there were 73 genres before preprocessing. The goal was to choose 7 samples with the highest sample frequency. However, we noticed a general imbalance in the dataset. Before preprocessing, the frequency of the 20 most frequent labels were: 

[(25177, 'Rock'), (13759, 'Pop'), (13496, 'Heavy Metal'), (12998, 'Indie'), (9589, 'Rap'), (9019, 'Pop/Rock'), (8412, 'Hip Hop'), (7377, 'Country'), (5555, 'Rock Alternativo'), (5309, 'R&B'), (5017, 'Gospel/Religioso'), (4632, 'Hard Rock'), (4518, 'Soul Music'), (4252, 'Dance'), (4157, 'Punk Rock'), (4055, 'Folk'), (3863, 'Soft Rock'), (3680, 'Romântico'), (3112, 'Trilha Sonora'), (3086, 'Jazz')]

This led to certain genres being confused with other genres more often. To create a more balanced dataset, we reassigned some genres based on a thorough analysis of the dataset as well as computing the confusion matrix to see which genres were being mistaken with each other the most. You can find the reassignment in the preprocessing part of our notebook. We ended up combining certain genres with each other, for example merging “Hip Hop” and “Rap” or adding “Folk” to “Indie”. After preprocessing, the dataset was more balanced:

[(22352, 'Rock'), (17240, 'Hip Hop'), (17002, 'Pop'), (16137, 'Indie'), (14353, 'Heavy Metal'), (12425, 'R&B'), (8657, 'Country')]

The confusion matrix also appeared to be more balanced:

<img width="302" alt="confusion_matrix" src="https://user-images.githubusercontent.com/75620360/216823789-0fbd896d-f5e6-464e-82fa-5fb6ce160425.png">


## Discussion
### Forced Alignment

### Genre Classification

It is important to discuss whether song lyrics are an adequate predictor for song genre. Intuitively, audio should be a better predictor than text. [But what audio features would be necessary to predict a song's genre?]( https://towardsdatascience.com/music-genre-classification-with-python-c714d032f0d8) How can we represent frequency, beats per minute, key, chords or melody which are all detrimental to a genre? For our project, due to time restrictions, complexity as well as the lack of access to a labeled audio dataset, we wanted to investigate the predicting power of song lyrics instead. 

First, we created a simple model with only an embedding layer and a linear layer. The advantage of such simple models is the explainability. We quickly noticed that the model learned to predict songs based on certain words. For example, when there was profanity, it often predicted “Hip Hop”. When the lyrics were about dancing and loving yourself, it tended to predict “Pop”. When the lyrics were about anxiety and depression, it predicted “Indie”. This might sound like an oversimplification or a cliché, but surprisingly it is not as far away from the truth as one might expect. A lot of songs fall victim to this cliché. Therefore, our first simple model didn’t perform that bad in practice. This motivated us to create the model we ended up using for this project. 

While the accuracy might not be that high, it needs to be discussed whether accuracy is an adequate measure for this use case to begin with. Most songs are not exclusive to one genre and even if the model doesn’t predict the correct genre, there is a difference between mistaking “Indie” for “Rock” or mistaking “Indie” for “Hip Hop”. A metric such as RMSE would be more appropriate for measuring “how wrong” the model instead of just evaluating the performance in a binary manner. 

In summary, using song lyrics as a predictor for music genre is flawed, since genres are first and foremost defined by music. However, lyrics have proven to be a valuable help in predicting music genre and could be integrated in a more complex audio recognition model.

## Acknowledgements

This solution is heavily inspired by the PyTorch's `Forced Alignment with Wav2Vec2` [tutorial](https://pytorch.org/audio/main/tutorials/forced_alignment_tutorial.html). We have used their implementation as a starting point and added the audio extraction, genre classification and website parts.
