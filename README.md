## IC Chips Defect Detection with Convolutional Neural Networks(CNN)

### Overview

This repository contains a convolutional neural network (CNN)–based system for automated visual defect detection in industrial products.  
The workflow is implemented in Jupyter Notebooks:

- `defect_cnn.ipynb` – model definition, training, and validation  
- `defect_inference.ipynb` – model loading, inference on test data, and manual review  

The trained model achieves **100% validation accuracy** from epoch 10 onward and **100% accuracy on the human‑verified test set**.

---

### Dataset Download

The dataset used in this project can be downloaded from the following link (replace with your actual URL):

- Download: [Defect Detection Dataset](https://universe.roboflow.com/mvi-ssmuw/ic-defect-detection-l5kf2)

After downloading, please extract the data and arrange it into the expected folder structure, for example:

```text
data/
├─ train/
│  ├─ images/
│  └─ labels/
├─ val/
│  ├─ images/
│  └─ labels/
└─ test/
   ├─ images/
   └─ labels/
```

Update any dataset paths inside the notebooks to match your local directory structure.

---

### Key Features

- **End‑to‑end CNN pipeline** for defect classification (training + inference)  
- **High accuracy** on both validation and manually inspected test data  
- **Notebook‑based experimentation** for easy modification and visualization  
- **Modular structure** separating training and inference steps  

---

### Project Structure

```text
.
├─ defect_cnn.ipynb          # Training and validation of the CNN model
├─ defect_inference.ipynb    # Inference and manual evaluation
└─ README.md                 # Project documentation
```

---

### Environment and Dependencies

This project uses **Python 3.x** and **Jupyter Notebook**.  
All required packages are installed directly inside the notebooks using `%pip install` cells, rather than a standalone `requirements.txt` file.  
Typical dependencies include:

- `torch`, `torchvision`
- `pandas`
- `numpy`
- `scikit-learn`
- `matplotlib`

To reproduce the environment:

1. Create and activate a Python virtual environment (optional but recommended).  
2. Launch Jupyter Notebook.  
3. Open `defect_cnn.ipynb` and run the initial cells that contain `%pip install` commands to install the necessary packages.  

You can later export your own `requirements.txt` if needed (e.g., using `pip freeze > requirements.txt`).

---

### Model and Training

The CNN architecture and training pipeline are defined in `defect_cnn.ipynb`. Typical elements include:

- Convolutional layers with nonlinear activations  
- Pooling and/or batch normalization  
- Fully connected layers for classification  
- Softmax or sigmoid output (depending on the number of classes)  
- Cross‑entropy (or similar) loss function  
- Optimization using SGD or Adam  

#### Training Configuration (example)

- Number of epochs: **20**  
- Loss: cross‑entropy  
- Metric: validation accuracy (Val Acc)  

#### Training and Validation Results

The training loss and validation accuracy across 20 epochs are summarized in the following figure:

![Training Loss and Validation Accuracy](images\training_curve.png)

From **epoch 10 onward**, the validation accuracy is consistently **100%**.

---

### Inference and Manual Evaluation

The notebook `defect_inference.ipynb` is used to:

1. Load the trained model weights  
2. Run inference on the test dataset or new images  
3. Compare predictions against ground‑truth labels  
4. Perform manual inspection of the results  

**Manual evaluation outcome:**  
All predictions on the test set were manually verified by a human and found to be correct, resulting in **100% test accuracy**.

---

### Getting Started

1. Launch Jupyter Notebook in this project directory.  
2. Open `defect_cnn.ipynb`.  
3. Run the `%pip install` cells to install dependencies in your current environment.  
4. Configure dataset paths as described in the **Dataset Download** section.  
5. Run the training cells to train the model and save weights to `models/`.  
6. Open `defect_inference.ipynb` to run inference and manually review predictions.  

---

### Results

- **Validation accuracy:** 100% (from epoch 10 to 20)  
- **Human‑verified test accuracy:** 100% (all test predictions manually checked and correct)  

These results indicate that the model fits the current dataset extremely well; additional experiments are recommended to assess robustness and generalization to unseen data.

---

### License

Specify your license here, for example:

```text
MIT License

Copyright (c) <2026> <m21hm9>
```

