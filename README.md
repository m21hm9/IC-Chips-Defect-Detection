## IC Chips Defect Detection with CNN and SSL

### Overview

This repository contains two pipelines for automated visual defect detection in industrial products:

- **Supervised CNN pipeline (`cnn/`)** – classic convolutional neural network (CNN) for defect classification, trained with labeled data.  
- **Self‑supervised learning (SSL) + YOLO pipeline (`ssl/`)** – uses a YOLO‑style dataset and self‑supervised representations to build a defect classifier with improved data efficiency.

Both pipelines are implemented in Jupyter Notebooks.

For the supervised CNN pipeline, the trained model achieves **100% validation accuracy** from epoch 10 onward and **100% accuracy on the human‑verified test set** (on the current dataset).

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

- **End‑to‑end supervised CNN pipeline** for defect classification (training + inference)  
- **Self‑supervised (SSL) + YOLO pipeline** exploring representation learning and label efficiency  
- **High accuracy** on both validation and manually inspected test data (CNN pipeline)  
- **Notebook‑based experimentation** for easy modification and visualization  
- **Modular structure** separating training and inference steps  

---

### Project Structure

```text
.
├─ cnn/
│  ├─ defect_cnn.ipynb        # Supervised CNN: training and validation
│  └─ defect_inference.ipynb  # Supervised CNN: inference and manual evaluation
├─ ssl/
│  ├─ ssl_yolo_defect.ipynb   # SSL + YOLO: training / feature learning on YOLO‑style data
│  └─ ssl_inference.ipynb     # SSL + YOLO: inference and analysis
└─ README.md                  # Project documentation
```

---

### Environment and Dependencies

This project uses **Python 3.x** and **Jupyter Notebook**.  
All required packages are installed directly inside the notebooks using `%pip install` cells, rather than a standalone `requirements.txt` file.

Typical dependencies for both pipelines include:

- `torch`, `torchvision`
- `pandas`
- `numpy`
- `scikit-learn`
- `matplotlib`
- `ultralytics` (for working with YOLO‑style data in the SSL pipeline)

To reproduce the environment:

1. Create and activate a Python virtual environment (optional but recommended).  
2. Launch Jupyter Notebook.  
3. For the **CNN pipeline**, open `cnn/defect_cnn.ipynb` and run the initial cells that contain `%pip install` commands.  
4. For the **SSL pipeline**, open `ssl/ssl_yolo_defect.ipynb` and run the initial cells that contain `%pip install` commands.  

You can later export your own `requirements.txt` if needed (e.g., using `pip freeze > requirements.txt`).

---

### Model and Training – CNN Pipeline

The supervised CNN architecture and training pipeline are defined in `cnn/defect_cnn.ipynb`. Typical elements include:

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

### Inference and Manual Evaluation – CNN Pipeline

The notebook `cnn/defect_inference.ipynb` is used to:

1. Load the trained model weights  
2. Run inference on the test dataset or new images  
3. Compare predictions against ground‑truth labels  
4. Perform manual inspection of the results  

**Manual evaluation outcome:**  
All predictions on the test set were manually verified by a human and found to be correct, resulting in **100% test accuracy**.

---

### Getting Started

#### 1. Supervised CNN Pipeline (`cnn/`)

1. Launch Jupyter Notebook in this project directory.  
2. Open `cnn/defect_cnn.ipynb`.  
3. Run the `%pip install` cells to install dependencies in your current environment.  
4. Configure dataset paths as described in the **Dataset Download** section.  
5. Run the training cells to train the model and save weights (e.g., `defect_cnn.pth`).  
6. Open `cnn/defect_inference.ipynb` to run inference and manually review predictions.  

#### 2. SSL + YOLO Pipeline (`ssl/`)

1. Launch Jupyter Notebook in this project directory.  
2. Open `ssl/ssl_yolo_defect.ipynb`.  
3. Run the `%pip install` cells to install dependencies (including `ultralytics`).  
4. Point the notebook to your YOLO‑style dataset (train/val/test with `images/` and `labels/`).  
5. Train / fine‑tune the SSL‑based model and evaluate on the validation or test split.  
6. Use `ssl/ssl_inference.ipynb` for focused inference and qualitative analysis.

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

