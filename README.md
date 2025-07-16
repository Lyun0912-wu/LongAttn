# 🔥LongAttn ：Selecting Long-context Training Data via Token-level Attention
<div align="center">
  <img src="https://img.shields.io/badge/Self--Attention-black" alt="self-attention">
  <img src="https://img.shields.io/badge/Pre--trained-Data-red" alt="Data">
  <img src="https://img.shields.io/badge/Long--Context-red" alt="LongContext">
</div>

</div> 
<div align="center">
<a href="assets/LongAttn.pdf"><img src="assets/paper-page-xl.svg" alt="Paper page"></a>
<a href="https://huggingface.co/datasets/Lyun0912/LongABC"><img src="assets/dataset-on-hf-xl.svg" alt="Dataset on HF"></a>
<a href="https://huggingface.co/Lyun0912/LongAttn"><img src="assets/model-on-hf-xl.svg" alt="Model on HF"></a>
</div>

## 👀Overview

This repository contains the code for the paper "LongAttn ：Selecting Long-context Training Data via Token-level Attention"

<p align="center">
<img src=assets/LongAttn.png width=700/>
</p>

In this work, we propose a novel token-level framework, LongAttn, which leverages the self-attention mechanism of LLMs to measure the long-range dependencies for the data. By calculating token-level dependency strength and distribution uniformity of token scores, LongAttn effectively quantifies **long-range dependencies**, enabling more accurate and efficient data selection. 

## 🔍Released Models & Datasetes
To facilitate future research in filtering long-context data, we release **LongAttn-8B** and **LongABC-32K**.
- **LongABC-32K-Raw** is a 32K-length long-context dataset obtained from the open-source **A**rxiv, **B**ook, and **C**ode data after preprocessing, with each category containing 12 billion tokens.
- **LongABC-32K** is the dataset we filtered using **LongAttn** based on LLaMA-3.1-70B from **LongABC-32K-Raw**, with each category containing 1.5 billion tokens.
- **LongAttn-8B** is the model we continual pre-trained on **LLaMA-3-8B** using **LongABC-32K**. Experiments have demonstrated that the model trained on 4.5B tokens from **LongABC-32K** outperforms the model trained on 18B tokens randomly selected from **LongABC-32K-Raw**.

The download links for the model and data are as follows:

| Model & Data | Link |
| --- | --- |
| **LongABC-32K** | [Download Link](https://huggingface.co/datasets/Lyun0912/LongABC) |
| **LongAttn-8B** | [Download Link](https://huggingface.co/Lyun0912/LongAttn) |

## 🚀Quick Start
### Environment Setup

To use LongAttn, follow these steps to download the code and necessary dependencies:
```
git clone https://github.com/Lyun0912-wu/LongAttn.git
cd LongAttn
pip install -r requirements.txt
```
### Pre-process
After that, the bash script `Preprocess.bash` will preprocess the pre-training data you have selected.
```
bash scripts/Preprocess.bash
```
The script performs the pipeline of Preprocess:
1. Filter parts of the pre-training data with lengths greater than 32K and perform sliding window processing on them.
2. Assign a **specific ID** to the data processed for easy identification in subsequent selection, and sample the data as needed.

### Filtering
Then, we use the script `inference_and_filter.bash` to filter the data.

```
bash scripts/inference_and_filter.bash
```
The script performs the pipeline of filtering:
1. Use the filtering model to obtain the **Attention map** for the labeled data.
2. Calculate the **$LDS_T$** indicator, and filter the data accordingly.

Finally, we obtained the data filtered by the **LongAttn** framework.

## 📑The Data Format
### Data before Preprocessing
```json
[{"content":"This is a pre-training data of variable length."}]
```

### Data after Preprocessing
```json
[{"content":"This is a pre-training data of 32K length.","data_id":"Prefix_0000001"}]
```


## 🌟 Citation
If you find this repo helpful, please cite our paper as follows:

```bibtex
@article{wu2025longattn,
  title={LongAttn: Selecting Long-context Training Data via Token-level Attention},
  author={Wu, Longyun and Zhu, Dawei and Zhao, Guangxiang and Yu, Zhuocheng and Ran, Junfeng and Wong, Xiangyu and Sun, Lin and Li, Sujian},
  journal={arXiv preprint arXiv:2502.16860},
  year={2025}
}
```
