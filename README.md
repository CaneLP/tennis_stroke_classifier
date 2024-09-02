# Tennis Stroke Classification Project 🎾

This project focuses on classifying tennis strokes using a range of machine learning and deep learning models. The dataset consists of short video clips, each lasting 1 second, which have been manually relabeled to enhance model performance. Initially, the videos were obtained through an automated pipeline employing machine learning models, though the specifics of that process are beyond the scope of this project. The primary objective here is to evaluate different models to determine the most effective approach for tennis stroke classification.

## Table of Contents 📃

- [Project Overview](#project-overview-)
- [Dataset](#dataset-)
- [Modeling Techniques and Evaluations](#modeling-techniques-and-evaluations-)
- [Results](#results-)
- [Installation and Usage](#installation-and-usage-)

## Project Overview 🔎

This project implements multiple models, including traditional machine learning techniques like Logistic Regression and SVM, as well as deep learning methods such as MLP or CNNs, to classify tennis strokes. The goal is to compare these methods on both initial and cleaned datasets, assessing their performance in terms of accuracy, F1 score, and other relevant metrics.

## Dataset 🗃

- **Size:** ~12,000 short video clips (1 second each)
- - **Relabeling Process:** To make sure the dataset was accurate, we went through all 12,850 video clips from 27 Grand Slam matches and fixed any errors we found. This process took about 20 active hours spread over a few weeks (around 9 minutes for every 100 strokes). It was an important step to build a good dataset for this project. As Andrew Ng mentioned, focusing on data quality is key for AI success ([Fortune article](https://fortune.com/2022/06/21/andrew-ng-data-centric-ai/)).
- **Classes:** Four types of tennis strokes: forehand, backhand, serve, and an "other" category representing any player behavior not classified as one of the three primary strokes
- **Structure:** Each video originally contains approximately 23-30 frames, depending on the FPS, but all videos have been resampled to 20 frames for consistency.
- **Transformation:** These 1-second-long videos were transformed into 20x12 images with 3 channels (RGB) using pose estimation methods. Specifically, Ultralyics' model was utilized ([Ultralytics GitHub](https://github.com/ultralytics/ultralytics)). In this transformation:
  - **20:** Refers to the 20 frames on which the pose was tracked.
  - **12:** Refers to the 12 pose key points identified as relevant:
    - 5: Left Shoulder
    - 6: Right Shoulder
    - 7: Left Elbow
    - 8: Right Elbow
    - 9: Left Wrist
    - 10: Right Wrist
    - 11: Left Hip
    - 12: Right Hip
    - 13: Left Knee
    - 14: Right Knee
    - 15: Left Ankle
    - 16: Right Ankle

  This process of turning videos into 'action images' is based on the approach described in [this paper](https://arxiv.org/pdf/1704.05645.pdf).

## Modeling Techniques and Evaluations 🤖

The results, full process, and detailed analysis of the models are available in the appropriate notebooks located in the `notebooks/03_model_dev` folder.

- **Logistic Regression**
  - **Datasets:** Initial, Cleaned
  - **Hyperparameters:** Ran GridSearchCV for regularization methods (`l1`, `l2`, `elasticnet`).

- **Support Vector Machine (SVM)**
  - **Datasets:** Initial, Cleaned
  - **Hyperparameters:** Ran GridSearchCV for kernel type, regularization parameter (`C`), and kernel coefficient (`gamma`).

- **Boosting**
  - **Dataset:** Initial only
  - **Description:** Applied Boosting techniques to check how it behaves on this classification task.
  - **Hyperparameters:** Ran GridSearchCV for number of estimators, learning rate, and max depth.

- **Multilayer Perceptron (MLP)**
  - **Datasets:** Initial, Cleaned
  - **Hyperparameters:** Ran GridSearchCV for a few parameters, such as units per layer, activation function, learning rate, and similar.

- **Convolutional Neural Network (CNN)**
  - **Datasets:** Initial, Cleaned
  - **Description:** The CNN model did a great job at generalizing, which means it worked well not only on the data it was trained on but also on new, unseen data. The results on the test set were very close to those on the training and validation sets, showing that the model didn't overfit and could make accurate predictions on new data. This was different from the other models, which, even though they did well during training and validation, often performed worse on the test set. While those models still gave good results, the CNN was especially impressive because it kept its performance steady across all datasets. This made the CNN a very reliable choice for this classification task.

Additionally, we developed a more complex CNN model with some extra layers and dropout, and trained it for 200 epochs. This more advanced model gave the most consistent results across the training, validation, and test sets, showing it could generalize maybe even better than the initial model (also trained on 200 epochs).

## Results 🚀

Results are only shown for the test set. For a more detailed version, including training and validation results, as well as confusion matrices, please refer to the notebooks.

| Model                   | Dataset  | Accuracy | Precision | Recall | F1 Score (macro) |
|--------------------------|----------|----------|-----------|--------|------------------|
| **Logistic Regression**  | Initial  | 0.78     | 0.76      | 0.74   | 0.75             |
| **Logistic Regression**  | Cleaned  | 0.90     | 0.89      | 0.88   | 0.89             |
| **SVM**                  | Initial  | 0.83     | 0.82      | 0.81   | 0.81             |
| **SVM**                  | Cleaned  | 0.95     | 0.94      | 0.94   | 0.94             |
| **Boosting**             | Initial  | 0.82     | 0.81      | 0.79   | 0.80             |
| **MLP**                  | Initial  | 0.81     | 0.79      | 0.79   | 0.79             |
| **MLP**                  | Cleaned  | 0.95     | 0.94      | 0.93   | 0.93             |
| **CNN (50 epochs)**      | Initial  | 0.85     | 0.85      | 0.84   | 0.84             |
| **CNN (50 epochs)**      | Cleaned  | 0.96     | 0.95      | 0.96   | 0.96             |
| **CNN (200 epochs)**     | Cleaned  | 0.97     | 0.96      | 0.97   | 0.96             |
| **CNN (Complex, 200 e)** | Cleaned  | **0.97** | **0.97**  | **0.97** | **0.97**       |

## Installation and Usage 🛠

To get started, follow the steps below to set up your environment and run the necessary scripts.

### 1. Clone the repository

First, clone the repository to your local machine:

```bash
git clone https://github.com/CaneLP/tennis-stroke-classification.git
cd tennis-stroke-classification
```

### 2. Setup the environment with pipenv

Next, set up the Python 3.8 environment using `pipenv`:

```bash
pipenv --python 3.8
```

After the environment is created, install all dependencies specified in the `Pipfile`:

```bash
pipenv install
```

### 3. Enter the pipenv shell

Activate the environment by entering the Pipenv shell:

```bash
pipenv shell
```

### 4. Download and prepare the dataset

Run the `download_dataset.py` script to download and unpack the necessary datasets. Note that this process may take a few minutes:

```bash
python download_dataset.py
```

### 5. Use the Jupyter notebooks

```bash
jupyter notebook
```

Once the dataset is prepared, you can start using the Jupyter notebooks located inside the `notebooks/03_model_dev` directory:

### Optional: Play with sample data

If you want to explore some sample data, you can run the `download_samples.py` script:

```bash
python download_samples.py
```

After running the script, you can open and interact with the notebooks in the `notebooks/02_dataset_creation` directory. Note that running these notebooks will not actually create the full dataset, as that process is time-consuming and not practical for quick experiments. This is provided just for fun and exploration.
