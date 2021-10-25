# Fine-Tuning the Multilingual Text-To-Text Transfer Transformer (mT5) on XNLI for Language Classification

* This repository demostrate how we can do fine-tuning on pretrained multi-lingual text-to-text mT5 model. The mT5 model is trained on 101 language.
* We will be using the Cross-lingual Natural Language Inference (XNLI) dataset for fine-tuning. We are going to use sentence1 and sentence2 colomns as input and language as target.
* XNLI dataset can be download [here](https://cims.nyu.edu/~sbowman/xnli/)
* We are going to **use xnli.test.tsv dataset for cross-validation** and once we have selected our hyper-parameters we will **train our model on full xnli.test.tsv dataset and evaluated on xnli.dev.tsv.** We are not going to use xnli.dev.tsv dataset untill we have choosen all the hyper-parameter.
* I have refered this [official](https://github.com/google-research/text-to-text-transfer-transformer/blob/main/notebooks/t5-trivia.ipynb) notebook from t5 for fine-tuning the model. 

## Experimenting with different hyper parameter.

1. First; We tried different **Batch size (32, 128, 256)** and concluded that batch size of 128 and 256 perform same as long as number of training steps are same
2. Second; We tried different **Epoch (10, 8, 6, 3, 2)** and concluded that epoch depends on batch size and Learning rate. If Batch is small (eg: 128) epoch can also be small and for higher batch Epoch should be higher. If LR is higher we can use small epoch.
3. Third; We tried different **LR (0.0001, 0.001, 0.003)** and concluded with higher learning rate model will train faster and we will need small epoch.

## After running multiple iteration, we finally selected following hyper-parameter

1. Batch Size = 128
2. EPOCH = 2
3. LR = 0.003

* We train the model on full dataset using these hyper-parameter. Below are the metrics on evaluation set.  

![metrics](https://user-images.githubusercontent.com/13449847/138611067-c7750017-98f2-4f37-8674-38dbef780df8.png)

## Prediction on unseen data from web with 100% accuracy.
![prediction](https://user-images.githubusercontent.com/13449847/138614500-09efd1cb-d6c2-4802-bf29-86b3365720f3.png)


## Observation.

1. We have observed that model misclassify Hindi and Urdu Language. The wrong prediction are mostly on input sentence which are written using English alphabets, not Hindi or Urdu alphabets. 
2. Model also misclassify Bulgarian and Russian. The wrong predictions are possibily because both Russian and Bulgarian belong from same language family.
3. Few sentences are incorrectly labeled as english. 
4. Some input sentences are very small and because of that wrong prediction. Since training dataset size is decent we can eliminated such small sentences.

## How to Run this notebook. 

* Its highly recommended to Run this notebook using Collab TPU Runtime env. I have already shared the Collab notebook [here](https://colab.research.google.com/drive/1cVwnFKDFOL5545b4u08Q1yS5l9-0GRsg?usp=sharing), Please Run it directly.
* To Change the runtime please follow; **Runtime --> change runtime type --> TPU** If fine-tuning the model then GPU  is also fine. 
* All the data and fine-tune checkpoints are already available in my GCS bucket**\***
* Notebook can also be run on local machine by install **pip install -r requirements.txt**. However all the data, checkpoints and output should be saved on GCS bucket.

### P.S:
* GCS bucket is publicly available for limited time and after that those links will expire. Please update those links with your own GCS link and also upload training data.
* Know Issue: For some reason t5 library is not able to update the checkpoint's *.gin confile file and because of that prediction function throws error. I have manually updated the *.gin config file for best model for getting the predictions. 
