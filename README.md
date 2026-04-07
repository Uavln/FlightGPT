# 🚀 FlightGPT： A vision-language model based agent for UAV navigation.
[![arXiv](https://img.shields.io/badge/arXiv-2506.12364-b31b1b.svg)](https://arxiv.org/abs/2505.12835)
[![model](https://img.shields.io/badge/model-Flightgpt-yellow.svg)](https://huggingface.co/ADJHD/Flightgpt)
[![data](https://img.shields.io/badge/data-training-blue.svg)](https://huggingface.co/datasets/ADJHD/flightgpt_training_data)


****
## 📑 Introduction
FlightGPT is a state-of-the-art UAV Vision-and-Language Navigation (VLN) framework designed for applications like disaster response, logistics delivery, and urban inspection. Built on powerful Vision-Language Models (VLMs), FlightGPT employs a two-stage training pipeline: supervised fine-tuning (SFT) with high-quality demonstrations to improve initialization and reasoning, followed by Group Relative Policy Optimization (GRPO) guided by a composite reward considering goal accuracy, reasoning quality, and format compliance to enhance generalization. With a Chain-of-Thought (CoT) reasoning mechanism for interpretable decision-making, FlightGPT achieves state-of-the-art performance on the city-scale CityNav dataset, surpassing the strongest baseline by 9.22% in unseen environments.


## 📢 News
- **2025-12-2**: Our SFT model [Flightgpt_SFT](https://huggingface.co/ADJHD/Flightgpt_SFT) is now publicly available on Hugging Face!
- **2025-9-4**: Our training [data](https://huggingface.co/datasets/ADJHD/flightgpt_training_data) is now publicly available on Hugging Face!
- **2025-9-3**: Our model [Flightgpt](https://huggingface.co/ADJHD/Flightgpt) is now publicly available on Hugging Face!
- **2025-08-21**: Accepted by EMNLP 2025!


## 🛠️ Environment Setup

This project depends on multiple models and tool libraries. It is recommended to use Conda to create an isolated environment.

### Install Conda Environment

```bash
- conda create -n flightgpt python=3.11
- conda activate flightgpt

- pip install -r requirements.txt
```

---

## 🛠️ Model and Data Preparation

* Download model weights to `./model_weight/`  
  Note: Change the value of `max_pixels` in `preprocessor_config.json` to `16032016`.

* Download data to `./data/`

* And for sft, Download the cleaned_final.json to ./LLaMA-Factory/data

### 📦 Project Structure
├── model_weight/ # Directory for model weights (download manually)  
├── experiment/  
├── R1PhotoData/  
├── data/  
│    └── citynav/ # Data annotation directory  
│    └── rgbd-new/ # Raw image files  
│    └── training_data/ # Training data directory  
│    └── ...  
├── data_examples/ # Examples of some training data  
├── eval.py # Model inference and evaluation script  
├── open-r1-multimodal/ # GRPO training directory  
├── LLaMA-Factory/ # SFT training directory  
├── requirements.txt # Combined environment dependency file  
├── README.md # This document  
├── ...  

---

## 🚀 Inference

1. Start the vLLM service
```bash
CUDA_VISIBLE_DEVICES=0,1,2,3 vllm serve path/to/your/model \
  --dtype auto \
  --trust-remote-code \
  --served-model-name qwen_2_5_vl_7b \
  --host 0.0.0.0 \
  -tp 4 \
  --uvicorn-log-level debug \
  --port your_port \
  --limit-mm-per-prompt image=2,video=0 \
  --max-model-len=32000
```

2. Start the inference script

```bash
python eval_by_qwen.py
```

3. Result Visualization  
You can use the visualize_prediction function to visualize the predicted target coordinates and the landmark bounding boxes, as well as the actual target coordinates and landmark bounding boxes.

---

## 🚀 Training
1. SFT
```bash
cd LLaMA-Factory
llamafactory-cli train examples/train_lora/qwen2vl_lora_sft.yaml
llamafactory-cli export ./LLaMA-Factory/examples/merge_lora/qwen2vl_lora_sft.yaml
```


2、GRPO
```bash
sh ./open-r1-multimodal/run_scripts/run_grpo_rec_lora.sh
```

---

## 🖋️ Citation

If you use FlightGPT in your research, please cite our project:

```bibtex

@article{cai2025flightgpt,
  title={FlightGPT: Towards Generalizable and Interpretable UAV Vision-and-Language Navigation with Vision-Language Models},
  author={Cai, Hengxing and Dong, Jinhan and Tan, Jingjun and Deng, Jingcheng and Li, Sihang and Gao, Zhifeng and Wang, Haidong and Su, Zicheng and Sumalee, Agachai and Zhong, Renxin},
  journal={arXiv preprint arXiv:2505.12835},
  year={2025}
}
```
