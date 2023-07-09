# Exploring Transfer Learning and Ensemble Stacking for Multi-Class Tissue Classification in Colorectal Cancer Images

This project takes on the task of **multi-class classification** to classify 8 different tissue types of human colorectal cancer - 
  1. Tumour epithelium
  2. Simple Stroma
  3. Complex stroma (contains single tumour cells and/or few immune cells)
  4. Immune cells clusters (referred as lympho)
  5. Cell debris
  6. Normal mucosal glands
  7. Adipose tissue
  8. Background (no tissue - empty)

<img width="500" alt="image" src="https://github.com/mekgurnani/ColorectalCancer_CNN_Classification/assets/70545953/a934ea01-0f6c-4081-b328-ea63b3798963">

### Rationale of the project and methods used - 
- Colorectal Cancer is the third most diagnosed cancer (1) and the most common cause of death due to cancer
- But is also the second most preventable cancer, if detected early (2)
- Traditionally, manual feature extraction methods have been used to capture properties of the internal structure of image regions combined with classical machine learning for classification/regressions tasks 
- Using deep learning methods such as convolution neural networks (CNNs) can automate feature extraction with fewer user determined parameters for significantly better performance
- The use of transfer learning within this task leverages knowledge of 'typical' image features offering better performance when dealing with smaller datasets
- **Ensemble Stacking**, a type of ensemble technique, was also implemented to improve model robustness for better output predictions

### Models implemented - 
Supervised models - 
1. A fine-tuned ResNet50, pre-trained on the ImageNet dataset
2. Pre-trained ResNet50 (for feature extraction) + ensemble stacking (for classification) - not fine-tuned
3. Fine-tuned Model-2 ResNet50 (for feature extraction) + ensemble stacking (for classification)

### Model architectures - 
- Train/Val/Test split - 70/15/15 ratio 

**Model 1 - Fine-tuned ResNet50**

![Picture1](https://github.com/mekgurnani/ColorectalCancer_CNN_Classification/assets/70545953/e52ff395-1658-4939-9e9f-27ea8f767b88)

- Data augmentation applied to the train split - random rotation, random crop, random horizontal and vertical crop
- Last layer of ResNet50 modified to have an additional FC block feeding into a softmax classifier
- Parameters used – Adam optimizer, Cross Entropy Loss function, 20 epochs, learning rate = 0.0001

**Model 2 - Pre-trained ResNet50 + ensemble stacking**

![Picture1](https://github.com/mekgurnani/ColorectalCancer_CNN_Classification/assets/70545953/b58c651f-8e4c-4f43-89e2-a85f67da9f63)

- Model architecture modified to drop layers after average pooling
- Grid search cross-validation was performed resulting in the following parameters -
    - SVC: C = 1.9, rbf = kernel, gamma = scale
    - RF: estimators = 3000, criterion = entropy

**Model 3 - Fine-tuned Model-2 ResNet50 + ensemble stacking**
- Same methods were used as shown in Model 1 and parameters from Model 2

### Results - 

Test set performance across the 3 modes - 
![Picture1](https://github.com/mekgurnani/ColorectalCancer_CNN_Classification/assets/70545953/ae989026-e49f-4a16-b535-26a92e7d0ed6)

Confusion matrix for each model - 
![Picture1](https://github.com/mekgurnani/ColorectalCancer_CNN_Classification/assets/70545953/78ffac5c-c7b3-4c57-a721-70504a041487)

- Model 3 performed the best with 98.5% accuracy, likely due to the task-specific pre-trained weights it started with
- But it had the highest misclassification of tumor tissue as complex tissue, unlike the other 2 models which have greater misclassification on other classes
- The confusion matrix for Model 3 also shows it retains its accuracy on classifying tumor tissue and improves performance on lympho tissue classification compared to Model 1

Misclassified tumor images vs representative complex tissue image -

<img width="447" alt="Picture1" src="https://github.com/mekgurnani/ColorectalCancer_CNN_Classification/assets/70545953/c0cdaa31-7b7d-4d2b-9718-15290b3dd9a3">

- Overall, the DL-based classification modes outperform the ensemble-based classification methods
- Looking at the 3 images misclassified by Model 3, it is apparent the features look fairly similar however, this misclassification has clinical significance to be considered and improved upon

Dataset used - [Link here for Zenodo](https://zenodo.org/record/53169#.ZFUsQy_MI1L) (3)

References - 
1) American Society of Clinical Oncology. (n.d.). Colorectal cancer: Statistics. Cancer.Net. https://www.cancer.net/cancer-types/colorectal-cancer/statistics
2) Cancer Research UK. (2021, May 5). Preventable cancers. https://www.cancerresearchuk.org/health-professional/cancer-statistics/risk/preventable-cancers
3) Kather, J., Weis, CA., Bianconi, F. et al. Multi-class texture analysis in colorectal cancer histology. Sci Rep 6, 27988 (2016). https://doi.org/10.1038/srep27988



