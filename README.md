# Analisis of "Mental heath in tech 2016" dataset from kaggle

## 1. Task formulation
To be brief, there is a high-dimensional, complex survey, which was conducted amongst technology-oriented employees. Dataset has missing values and non-standardized textual inputs. The goal is to categorize participants based on their survey responses and create visualizations that would simplify the data complexity. Key characteristics should be preserved and each result cluster should be described, in the context of its main traits.

Dataset can be accessed via this link: https://www.kaggle.com/datasets/osmi/mental-health-in-tech-2016  
In case this dataset would be changed, I uploaded it on this github page (**folder "code" -> folder "dataset"**).  
## 2. Programm installation
### Requirements 
You need to intall Python with the version 3.11.3 and higher. All required modules to install you can find in the **"requirements.txt"** file.

### How to install
1. Download Python from the [official site](https://www.python.org/downloads/) and install it.  
2. Install required libraries from "requirements.txt".  
3. Download the folder "code" that contains the code to process dataset and the dataset itself. Run the **"app.py"** to process dataset and get results.  

## 3. How to use
There are two important files to run the programme: **"app.py"** and **"config.json"**. The first file runs the program, which includes preprocessing, encoding, clustering, and interpreting of the dataset. **"config.json"** allows to set up parameters for the program, e.g., number of clusters, random states for different algorithms, paths to save folders, etc.  

You can find explanation of each config parameter in the section "4.2. Config and launch files".  
Best setup solutions for this task you can find in the section "5. Results of the work".  

## 4. Implementation
This implementation has 9 classes, 1 config file to tune desirable settings, and 1 launch file, .
### 4.1. Classes
Each class is located in its own file, which is documented so that the reader can understand the purpose of each function. Here, the main purpose of each class is briefly described.

**· _GraphPlotter class_** has functions to plot and save images for different cases. There are functions to plot and save graphs in batches (the number of columns multiplied by the number of rows), 2d and 3d representation of the dataset obtained via t-SNE, Kernel PCA and PCA algorithms, etc.  
**· _WorkingDatasetInfo class_** gives the basic information about the original dataset. This is important for understanding the type of data and how to process the dataset.  
**· _DatasetAnalyzer class_** is responsible for analyzing and preprocessing dataset. It counts missing values for categorical columns and removes those that exceed the threshold for missing values. It also prepares columns that originally have a lot of answer but can be transformed so that they become more appropriate for the task.  
**· _TextFeatureExtractor class_** processes columns with textual responses. It extracts all the most important phrases in a column and replaces the text in each cell with the most important phrase found in it.  
**· _DataEncoder class_** encodes prepared dataset making it prepared for machine learning algorithms.  
**· _OptimalClusterFinder class_** applies different algorithms to dataset to find the appropriate number of clusters for the clustering process. It creates and saves graphs with k-elbow method, silhouette scores for k-means clustering, dendrograms for agglomerative clustering, and BIC and AIC scores for Gaussian mixture.  
**· _DimensionalityReducer class_** is designed to reduce dimensionality of the dataset so it can be displayed in 2 or 3 dimensional space. There are 3 dimensionality reduciton algorithms: (Linear) PCA, Kernel PCA, and t-SNE.  
**· _DataClusterer class_** clusters reduced in dimensionality data. There are 3 clustering algorithms: K-Means, Gaussian Mixture, and Agglomerative clustering.  
**· _ResultInterpreter class_** takes the results of clustering and interprets them by selecting the most important features it can detect. The following algorithms are used for feature selection: chi squared test, mutual information score, and random forest algorithm mixed with permutation feature importance algorithm.  

### 4.2. Config file
**Config file** contains settings for different stages of data analysis and clustering. You can set up parameters for matplotlib grid, image quality, number of clusters, random states for different algorithms, save paths, etc. There are 8 parameter categories in this .json file:  

**· _DataPreprocessingParameters_:**  
&emsp;&emsp; — _path_to_dataset_: Contains path to the original dataset that should be analyzed.  
&emsp;&emsp; — _missing_information_max_percent_: Contains the threshold that defines the maximum percentage of missing information allowed. Columns in which the percentage of empty cells exceeds this threshold will be removed (I refer to such columns as "**sparse columns**").  
**· _GraphPlotterGridParameters_:**  
&emsp;&emsp; — _n_columns_: Contains the number of columns that will be displayed on a matplotlib graph. This parameter is crucial for matplotlib subplot function. Each saved plot contains n_columns\*n_rows graphs.  
&emsp;&emsp; — _n_rows_: Contains the number of rows that will be displayed on a matplotlib graph. This parameter is crucial for matplotlib subplot function. Each saved plot contains n_columns\*n_rows graphs.  
&emsp;&emsp; — _dpi_: Contains dpi number to save images with a certain quality.   
&emsp;&emsp; — _max_feature_number_to_plot_: Contains the maximum feature (response) number that will be displayed on graphs.   
**· _GraphPlotterSavePaths_:**  
&emsp;&emsp; — _path_to_original_graphs_: Here, graphs with original dataset information are saved. Graphs shows response distribution per each question.   
&emsp;&emsp; — _path_to_processed_graphs_: Here, graphs with prepared for encoding dataset information (missed values are filled, text features extracted, and sparse columns are removed) are saved. Graphs shows response distribution per each retained question.  
&emsp;&emsp; — _path_to_cluster_choice_graphs_: Here, graphs with different metrics for choosing the number of clusters are saved. Contains folders from the following parameters: _path_to_k_elbow_, _path_to_silhouette_score_, _path_to_dendrogram_, _path_to_bic_aic_.  
&emsp;&emsp; — _path_to_k_elbow_: This folder is within the   
&emsp;&emsp; — _path_to_silhouette_score_:  
&emsp;&emsp; — _path_to_dendrogram_:  
&emsp;&emsp; — _path_to_bic_aic_:  
&emsp;&emsp; — _path_to_reduced_components_visualization_:  
&emsp;&emsp; — _path_to_cluster_results_:  
&emsp;&emsp; — _path_to_interpretations_:  
**· _AdditionalParamters_:**  
&emsp;&emsp; — _with_text_columns_:  
**· _ClusteringParameters_:**  
&emsp;&emsp; — _cluster_number_:  
&emsp;&emsp; — _kmeans_init_:  
&emsp;&emsp; — _kmeans_random_state_:  
&emsp;&emsp; — _gauss_random_state_:  
**· _DimensionalityReducerParameters_:**  
&emsp;&emsp; — _save_info_ratio_:  
&emsp;&emsp; — _tsne_random_state_:  
**· _ResultInterpreterParameters_:**  
&emsp;&emsp; — _random_forest_with_permutations_:  
&emsp;&emsp; — _random_forest_permutation_repeats_:  
&emsp;&emsp; — _permutation_random_state_:  
**· _ResultInterpreterSavePaths_:**  
&emsp;&emsp; — _path_to_pca_gauss_result_:  
&emsp;&emsp; — _path_to_kernel_pca_gauss_result_:  
&emsp;&emsp; — _path_to_tsne_gauss_result_:  
&emsp;&emsp; — _path_to_pca_kmeans_result_:  
&emsp;&emsp; — _path_to_kernel_pca_kmeans_result_:  
&emsp;&emsp; — _path_to_tsne_kmeans_result_:  
&emsp;&emsp; — _path_to_pca_agglomerative_result_:  
&emsp;&emsp; — _path_to_kernel_pca_agglomerative_result_:  
&emsp;&emsp; — _path_to_tsne_agglomerative_result_:  

### 4.3. Launch file
After feature selection, **GraphPlotter class** displays and saves the response distribution among different classes for each top feature algorithms detected. Now, it can be 

### 4.4. Special tricks
There are 2 such columns: age and gender. Originally age column has answers distributed by year but certain age can be replaced with age range. 

## 5. Results of the work

## 6. Possible improvements
