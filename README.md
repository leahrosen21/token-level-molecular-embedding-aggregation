# Impact of Token-Level Aggregation on Molecular Property Prediction
This repository extends the original [MolFormer](https://github.com/IBM/molformer) (Ross et al., 2022) molecular Large Language Model (LLM) for supervised molecular property prediction. It adapts pretrained molecular embeddings for downstream classification using [CLS]-token representations instead of mean pooling, with task-specific fine-tuning and comprehensive evaluation.

---

### Training Pipeline
The training pipeline is implemented using the [Hugging Face Transformers](https://huggingface.co/docs/transformers/en/index) library, leveraging the [Trainer](https://huggingface.co/docs/transformers/en/main_classes/trainer) API and TrainingArguments to create a standardized and efficient workflow. This framework handles the full training loop, including optimization with various optimizers and learning rate scheduling, automatic evaluation of task-specific metrics, and seamless logging through [Weights & Biases](https://wandb.ai/site/) for real-time monitoring. In addition, it supports mixed-precision training and gradient accumulation, which are essential for efficiently fine-tuning large models under limited GPU resources, while maintaining a reproducible and scalable experimental setup.

---

### Environment Requirements:
- Hardware: It is strongly advised to run this project on a GPU, as it involves fine-tuning and training an LLM. [Google Colab](https://colab.research.google.com/) is a suitable environment for running this project, particularly for executing a limited number of training epochs or focusing on a specific subset of tasks at a time. Due to the computational demands of fine-tuning the model, users should be mindful of session timeouts and resource limits when executing extended training runs.
- Runtime: If using Google Colab, ensure your runtime version is 2026.01.
- Dependencies: Due to compatibility requirements as of early 2026, transformers must be pinned to version <5.1.0 because [MoLFormer via Hugging Face](https://huggingface.co/ibm-research/MoLFormer-XL-both-10pct) relies on internal modules like transformers.onnx that are not compatible with newer versions. All required libraries and packages are listed within the notebook.

---

### Weights & Biases (W&B) Integration:
Training and hyperparameter sweeps are logged using W&B. To use this feature:
1. Create an account at [wandb.ai](https://wandb.ai/site/).
2. Locate your API key in your settings.
3. The notebook requires a login using your API key:

```python
import wandb
wandb.login(key="YOUR_API_KEY")
```

---

### Datasets:
We evaluate the model on several benchmarks from [MoleculeNet](https://moleculenet.org/datasets-1):
- Classification: BBBP, BACE, and ClinTox.
- Regression: ESOL, FreeSolv, and Lipophilicity (LIPO).

The datasets used in this project are provided in the repository under [datasets.zip](./datasets.zip).

Note: This project is currently optimized for binary classification and regression. Extension to multiclass tasks is identified for future work.

---

### Data Upload Methods:
There are two ways to upload datasets, depending on where the code is executed:
1. Google Drive (default): To ensure the notebook loads the datasets correctly, please organize your Google Drive as follows:
- Create a main folder named deep-learning-final-project.
- Create two subfolders inside it: cls (for classification) and reg (for regression).
- Upload the CSV files from the repository into their respective folders.

### Expected Directory Structure:
```plaintext
My Drive/
└── deep-learning-final-project/
    ├── cls/
    │   ├── bace_test.csv
    │   ├── bace_train.csv
    │   ├── bace_valid.csv
    │   ├── bbbp_test.csv
    │   ├── bbbp_train.csv
    │   ├── bbbp_valid.csv
    │   ├── clintox_test.csv
    │   ├── clintox_train.csv
    │   └── clintox_valid.csv
    └── reg/
        ├── esol_test.csv
        ├── esol_train.csv
        ├── esol_valid.csv
        ├── freesolv_test.csv
        ├── freesolv_train.csv
        ├── freesolv_valid.csv
        ├── lipo_test.csv
        ├── lipo_train.csv
        └── lipo_test.csv
```
 - Run the blocks provided in the notebook under the section "Loading and creating the datasets" to mount your drive and initialize the loading function.

2. To switch from the Google Drive version to the local upload version, follow these steps:
   - Disable the Google Drive blocks: Under the section "Loading and creating the datasets", comment out the very first two code blocks so you don't keep getting prompted to mount your Drive.
   - Activate the local upload block: Uncomment the entire following block and run it.

After you have activated your preferred loading function (either Google Drive or local upload), you can proceed with the rest of the notebook.

---

### The provided [Jupyter notebook](./Token_Level_Aggregation_on_Molecular_Property_Prediction_Notebook.ipynb) is organized into the following sections:
1. Installing and importing required packages: Sets up the environment with specific version pinning.
2. Validating [RDKit](https://www.rdkit.org/): A sanity check to ensure the chemical informatics tools are properly installed.
3. Model Architecture Setup: Defines the CustomModelForSequenceClassification which extracts the [CLS] token and uses the MolformerClassificationHead class.
4. Visualization of motivation for this projects: A comparison of the pooled embedding vs. [CLS] token embedding.
5. Dataset Creation: Implements the SMILESDataset class for canonicalizing SMILES strings.
6. Implementation of required functions for fine-tuning and hyperparameter search.
7. Hyperparameter Search: Configuration for W&B sweeps to find optimal training parameters.
8. Fine-tuning: The execution loop for training the model on the selected tasks.

---

### Results & Artifacts
- Analysis: For more details on the approach and findings, see the [full report](./Project%20Report.pdf).
- Best Models: The most optimized model weights and configurations are included in the zipped files within this repository.






   




