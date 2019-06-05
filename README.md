Affective state analysis of ultrasonic vocalizations in animal models of mTBI/PTSD and neuropathic pain

Abigail Schindler (lead), Valentina Staneva (eScience lead), Michael Park, Ben Land

Chronic health conditions (e.g. mental health, pain) are increasing in the US and contribute substantially to decreased quality of life, loss of productivity, and increased financial burden. Indeed, the CDC estimates that over 90% of annual health care expenditures are for people with one or multiple chronic health conditions. Translational research efforts using rodent models can provide much needed insight into underlying mechanisms of chronic health conditions and are needed in order to facilitate the search for therapeutic approaches that can reduce and/or prevent adverse/maladaptive outcomes. 
Critically, accurate quantification of affective state (e.g. positive, negative, pain, fear) has historically been a challenge in rodent models, with current available methods suffering from high subjectivity, lack of throughput, and invasive methods, leading to lack of reproducibility across research labs and/or inability to translate to humans. One promising area of research in rodent affective state is ultrasonic vocalizations (USVs). USVs are a form of rodent communication thought to represent an unbiased metric of affective state (there are thought to be potentially different “call signatures” for pleasure, pain, fear, etc.), but are historically difficult to analyze and interpret. Currently, there is no free and open-source software available for USV detection and/or analysis (although Matlab based options exist, e.g. DeepSqueak), limiting the applicability of USV research. With a focus on these USVs and open-source products, the current project seeks to develop a Python-based, high-throughput approach for 1) isolating USV calls and 2) assessing affective state. 

During the incubator we created a series of Python Jupyter Notebooks (described below) and utilized Google Colaboratory free cloud service with GPU support. 

The general data acquisition and analysis pipeline is as follows: 1) acquire audio recording of rodent USVs using Avisoft SASLab Lite (free; saves audio as .wav file), 2) annotate USVs in each file (to use for training classifier) using Raven Lite (free; saves annotations as a ‘selections table’), 3) use annotated files to train a classification algorithm (USV vs noise), 4) use trained model to process un-annotated audio files.


Notebooks (hosted on github; WIP = work in progress):
Preprocessing:
•	0_xr_create_annotations_df.ipynb
o	Starts with directory path containing annotated selection tables (generated from Raven Lite) and processes into Pandas dataframe containing animal number, session, annotation type, and annotation time stamp
•	1_xr_Process_wav_to_netcdf.ipynb
o	Processes audio file into spectrogram slices, generates xarray dataset (xarray dimensions: slice number (e.g. time stamp), frequency, time), saves as netCDF

Feature extraction:
•	2_xr_Annotations_from_netcdf_8features.ipynb
•	2_xr_Annotations_from_netcdf_PSD.ipynb
o	Uses annotations dataframe (notebook 0) and xarray datasets (notebook 1) to compute features for each USV annotation and randomly selected noise slices, saves features as Pandas dataframe.
o	8features:
	8 spectral features (power, purity, centroid, spread, skewness, kurtosis, slope, and roll off)	
o	PSD
	power spectrum distribution used as features (e.g. 257 frequencies contained in spectrogram, find power at each frequency for each slice)
•	3_xr_Annotations_viz.ipynb (WIP)
o	Contains various pieces of code for visualizing spectrogram slices and feature datasets

Classification/clustering:
•	4_xr_Annotations_classification.ipynb (WIP)
o	Uses features to train and evaluate various classification algorithms. Pickle and save final trained model 
o	Contains code to visualize false positives and false negatives during model evaluation stage
•	5_xr_clustering.ipynb (WIP)
o	Contains various pieces of code for clustering USVs and noise 
	kmeans
	TSNA
	PCA
•	6_xr_Annotations_deeplearning.ipynb (WIP)
o	Uses transfer learning (MobileNet) to train classifier

Analyze unannotated audio recordings:
•	7_analyze_wave_with_trained_model_8features.ipynb (WIP)
•	7_analyze_wave_with_trained_model_PSD.ipynb (WIP)
o	Uses pickled model to identify USVs in un-annotated file


We have USV recordings from a variety of mouse groups (e.g. control, TBI, fear-induction, neuropathic pain). We would like to establish specific USV call repertoires/signatures related to specific affective states/experimental conditions/behavioral tasks/therapeutic treatments. We are also building interactive visualizations which can facilitate the performance evaluation and the interpretation of the results. 

https://github.com/grace3999/USV_Python

Figure 1: tSNE plot and spectrogram visualization of USV calls (yellow dots: clustered to bottom left (<20kH USV call type) and bottom right (broad band click call type) and noise (purple dots: clustered to upper portion)



