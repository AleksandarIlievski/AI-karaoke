# KaraokAI

# Table of Contents


1. [Introduction](#introduction)
1. [Dependencies](#dependencies)
1. [Solution Description](#description)
1. [How to Use](#how-to-use)
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

The genre of the audio is predicted using a genre classification model trained on data set of X samples including lyrics and their corresponding genres. The audio is passed through the model and the predicted genre is displayed on the website.

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

| id | Genre              | Title                                                             | Video-ID      | Year |
| -- | ------------------ | ----------------------------------------------------------------- | ------------- | ---- |
| 1 | Experimental Rock | John Hammond-Jockey Full of Bourbon                                 | Nd5ySUF6SPM   | 2001 |
| 2 | Country            | Lonestar - Amazed                                                   | x-skFgrV59A   | 1999 |
| 3 | Pop                | Julia Michaels - What A Time ft. Niall Horan                       | bPYefBD1Rzs   | 2019 |
| 4 | Folk               | Tim Buckley - Song to the Siren                                      | 0wBV09TOd9g   | 1970 | 
| 5 | Rock               | CANDLEBOX - Far Behind                                              | eu3EuWg2qNI   | 1993 |
| 6 | R&B                | Janet Jackson - Together Again                                      | d5tJviZ-i9k   | 1997 |
| 7 | EDM                | Beam Me Up (Radio Edit)                                            | 19_5SjoHGfI   | 2013 |
| 8 | Hip-Hop           | "$UICIDEBOY$" - ANTARCTICA                                            | s1-0lt7b-78   | 2016 |

### Results

The average IoU between the ground truth and predicted segments of lyrics is calculated and plotted in a bar chart, which can be used to evaluate the performance of the speech recognition system. The mean IoU value and the histogram provide a summary of the performance and can be used to identify areas where the system needs improvement.

**TODO**: add values plots, charts, etc.

## Difficulties
## Discussion

## Acknowledgements

This solution is heavily inspired by the PyTorch's `Forced Alignment with Wav2Vec2` [tutorial](https://pytorch.org/audio/main/tutorials/forced_alignment_tutorial.html). We have used their implementation as a starting point and added the audio extraction, genre classification and website parts.
