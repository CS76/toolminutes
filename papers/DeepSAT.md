# DeepSAT: Learning Molecular Structures from Nuclear Magnetic Resonance Data

## Why discussing this paper?

I chose the DeepSAT: Learning Molecular Structures from Nuclear Magnetic Resonance Data paper for current topics in the cheminformatics seminar because

- nmrXiv: our NMR repository is primarily interested in offering CASE as a service based on models developed inhouse and also others.
- We are particularly interested in the approach to create the augemented data for training
- NMRShiftDB is nmrXiv's predecessor and we will be interested in how the data is used for training and also benchmarking
- Also interested in understanding the two step process taken up by the authors (prediction and retrieval) which are a huge issues to deal with in CASE. 

### Website: https://deepsat.ucsd.edu/

## Context
The paper discusses the development and evaluation of DeepSAT, a neural network-based structure annotation and scaffold prediction system designed to identify molecular structures from Nuclear Magnetic Resonance (NMR) data. The identification of molecular structures is crucial for understanding chemical diversity and drug discovery. Traditional methods for structure elucidation using NMR are time-consuming and require expert knowledge.

## Main idea
DeepSAT aims to enhance the efficiency and accuracy of molecular structure identification by using a convolutional neural network (CNN) to directly extract chemical features from 1H-13C HSQC spectra. This system improves upon existing methods by providing better performance in identifying and annotating molecular structures, thus facilitating faster drug discovery and chemical research.

<img width="1239" alt="Screenshot 2024-06-12 at 08 55 30" src="https://github.com/CS76/toolminutes/assets/4065285/dc883baf-7fac-4c22-b092-9d39ff3ad9b2">


#### NMR Data Preparation for DeepSAT Dataset

-   **Source of NMR Spectra**: The NMR spectra for the training dataset were compiled from literature and computed data.
-   **Literature Data**: Data from the CH-NMR-NP database included 1H and 13C NMR spectra for 29,500 natural products and 6,000 organic compounds. Incomplete or incorrect data were manually filtered.
-   **Computed Data**: Generated using ACD/Spectrus Processor 2017.2.1 software for 113,967 compounds randomly selected from databases such as the Universal Natural Product Database, NPATLAS, NPASS, GNPS, and NPClassifier. Parameters included:
    -   Correlation: C-H COSY
    -   Experimental: HSQC-DEPT
    -   Spectrometer Frequency: 600 MHz
    -   Spectrum Size: 128x128 pixels
    -   Spectrum Bounds: Signal-dependent
    -   Line Width: 3 Hz
    -   Solvent: Chloroform-d

#### Chemical Properties Calculation

-   **Morgan Fingerprint Method**: Modified to generate chemical fingerprints using RDKit version 2020.03.2.
    -   Radius range: 0 to 2
    -   Hydrogen atoms added to molecular graphs
    -   Total of 6144 chemical features identified for training
-   **Molecular Weights**: Calculated using RDKit and rounded to two decimal places.
-   **Classification**: Molecules classified into "superclass" using NPClassifier ontology.

#### Convolutional Neural Network Architecture and Hyperparameters

-   **Hardware**: Training performed on a server with Intel® Core™ i7-6850K CPU, three NVIDIA® GeForce GTX 1080 GPUs (8 GB each), and 64 GB RAM.
-   **Software**: Python and TensorFlow 2.3.0.
-   **CNN Design**: Two networks for normal HSQC and multiplicity-edited HSQC, respectively.
    -   Input shapes: Normal HSQC (128,128,1), Edited HSQC (128,128,2)
    -   Layers: Convolutional layers, fully connected layers, dropout layer for global max pooling
    -   Activation Functions: ReLU (hidden layers), sigmoid (fingerprint prediction layer), softmax (classification layer)
    -   Normalization: Batch normalization to avoid overfitting and vanishing gradients
-   **Hyperparameters**:
    -   Optimizer: Adam with learning rate of 10^-5 (decay = 10^-6)
    -   Dropout Rate: 0.2
    -   Batch Size: 16

#### Evaluation

-   **Test Set**: 3982 HSQC spectra randomly chosen and excluded from the training and validation dataset.
-   **Comparison Tools**: Performance compared with SMART 2.0 and NMRShiftDB using specific metrics.
-   **Search Process**: Automated search process for NMRShiftDB using Python script, sorted by similarity scores calculated by the database.

This section outlines the comprehensive methodology employed to prepare the dataset, calculate chemical properties, design and train the convolutional neural network, and evaluate the performance of DeepSAT against existing tools.



## Results

![image](https://github.com/CS76/toolminutes/assets/4065285/063f3b53-6f49-4d74-88a1-4e6d5876c3dd)

-   **Data Preparation**: DeepSAT was trained using 143,467 1H-13C HSQC spectra from diverse molecules. The training data included both experimental and computed spectra.
-   **Performance Metrics**: DeepSAT outperformed existing tools such as SMART 2.0 and NMRShiftDB in structure identification and annotation tasks. For instance, DeepSAT achieved a 60.6% correct identification rate using standard HSQC spectra, significantly higher than the performance of SMART 2.0 and NMRShiftDB.
-   **Evaluation**: The system was tested with a dataset of 3982 HSQC spectra, demonstrating high accuracy in predicting chemical fingerprints, molecular weights, and structure classes. The top-1 correct identification rate was 45.2% for multiplicity-edited HSQC spectra.
-   **Solvent Sensitivity**: The study also evaluated DeepSAT's sensitivity to different solvents, finding that while solvent changes affected chemical shifts, DeepSAT still maintained high identification rates.
-   **Structure Annotation**: DeepSAT was able to provide accurate annotations for previously undescribed molecules, aiding in the structure elucidation process.

<img width="1305" alt="Screenshot 2024-06-12 at 08 56 30" src="https://github.com/CS76/toolminutes/assets/4065285/d02ac715-5f03-4cc4-aacb-ddbf3553a4ae">

<img width="857" alt="Screenshot 2024-06-12 at 08 57 57" src="https://github.com/CS76/toolminutes/assets/4065285/0617e83c-f33f-4734-a5f6-3f8d52a70b5f">


## Takeaways 

DeepSAT demonstrates superior accuracy and efficiency in identifying and annotating molecular structures from NMR data compared to existing methods. Its use of convolutional neural networks (CNNs) allows for the prediction of chemical fingerprints, molecular weights, and compound classes. The tool's performance remains robust across different solvent conditions, highlighting its versatility. Furthermore, DeepSAT's ability to provide structural insights for previously undescribed molecules illustrates its potential to significantly accelerate the process of molecular structure elucidation in chemical and biomedical research.

![Correlations of HSQC spectra and structural moieties interpreted by the convolutional neural network used by DeepSAT](https://jcheminf.biomedcentral.com/articles/10.1186/s13321-023-00738-4/figures/3)

1.  **Efficiency and Accuracy**: DeepSAT significantly improves the speed and accuracy of molecular structure identification from NMR data compared to traditional methods and other available tools.
2.  **Advanced Machine Learning**: The use of CNN-based multi-task supervised learning allows DeepSAT to predict chemical properties and identify structures effectively from NMR spectra.
3.  **Broad Applicability**: DeepSAT is effective across different solvents and can be applied to various datasets, making it a versatile tool for chemical and biomedical research.
4.  **Practical Utility**: The system can assist researchers in drug discovery by providing rapid and accurate molecular structure annotations, potentially accelerating the development of new pharmaceuticals.


