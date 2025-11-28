# Weibo Sentiment Recognition Model - GPT2-LoRA Fine-tuning

> **Note**: This is an English translation of the original Chinese documentation. The original project [BettaFish](https://github.com/666ghj/BettaFish) was created by [666ghj](https://github.com/666ghj). This translation was contributed to help non-Chinese speakers understand and use this project.

## Project Description
This is a Weibo sentiment binary classification model based on GPT2, using LoRA (Low-Rank Adaptation) fine-tuning technology. Through LoRA fine-tuning implemented with the PEFT library, the model can be adapted to sentiment analysis tasks with only a minimal number of trainable parameters, significantly reducing computational resource requirements and model size.

## Dataset
Uses the Weibo sentiment dataset (weibo_senti_100k), containing approximately 100,000 sentiment-labeled Weibo posts, with about 50,000 positive and 50,000 negative comments each. Dataset labels:
- Label 0: Negative sentiment
- Label 1: Positive sentiment

## File Structure
```
GPT2-Lora/
├── train.py                  # Training script (PEFT library-based LoRA implementation)
├── predict.py                # Prediction script (interactive use)
├── requirements.txt          # Dependencies list
├── models/                   # Locally stored pre-trained models
│   └── gpt2-chinese/        # Chinese GPT2 model and configuration
├── dataset/                  # Dataset directory
│   └── weibo_senti_100k.csv # Weibo sentiment dataset
└── best_weibo_sentiment_lora/ # Trained LoRA weights (generated after training)
```

## Technical Features

1. **Extremely Parameter Efficient**: Compared to full parameter fine-tuning, only about 0.1%-1% of parameters are trained
2. **Uses PEFT Library**: Based on Hugging Face's official parameter-efficient fine-tuning library, stable and reliable
3. **Maintains Model Performance**: Achieves good classification performance while training only minimal parameters
4. **Deployment Friendly**: Small LoRA weight files, convenient for model deployment and sharing

## LoRA Technology Advantages

LoRA (Low-Rank Adaptation) is currently the most popular parameter-efficient fine-tuning technique:

1. **Ultra-low Parameter Count**: Through low-rank decomposition, large matrices are decomposed into products of two small matrices
2. **Plugin Design**: LoRA weights can be dynamically loaded and unloaded, one base model supports multiple tasks
3. **Fast Training**: Fewer parameters, shorter training time, lower memory usage
4. **Preserves Original Model**: Original pre-trained model weights remain unchanged, avoiding catastrophic forgetting

## Environment Dependencies

Install required dependencies:
```bash
pip install -r requirements.txt
```

Main dependencies:
- Python 3.8+
- PyTorch 1.13+
- Transformers 4.28+
- PEFT 0.4+
- Pandas, NumPy, Scikit-learn

## Usage

### 1. Install Dependencies
```bash
pip install -r requirements.txt
```

### 2. Train Model
```bash
python train.py
```

The training process will automatically:
- Download and locally save the Chinese GPT2 pre-trained model
- Load the Weibo sentiment dataset
- Train the model using LoRA technology
- Save the best LoRA weights to `./best_weibo_sentiment_lora/`

### 3. Sentiment Analysis Prediction
```bash
python predict.py
```

After running, you'll enter interactive mode:
- Enter the Weibo text to analyze in the console
- System will return sentiment analysis result (positive/negative) and confidence
- Enter 'q' to exit the program

## Model Configuration

- **Base Model**: `uer/gpt2-chinese-cluecorpussmall` Chinese pre-trained model
- **Model Local Save Path**: `./models/gpt2-chinese/`
- **LoRA Configuration**:
  - rank (r): 8 - Rank of low-rank matrices
  - alpha: 32 - Scaling factor
  - target_modules: ["c_attn", "c_proj"] - Target linear layers
  - dropout: 0.1 - Prevents overfitting

## Performance Comparison

| Method | Trainable Parameter Ratio | Model File Size | Training Time | Inference Speed |
|--------|---------------------------|-----------------|---------------|-----------------|
| Full Fine-tuning | 100% | ~500MB | Long | Slow |
| Adapter Fine-tuning | ~3% | ~50MB | Medium | Medium |
| **LoRA Fine-tuning** | **~0.5%** | **~2MB** | **Short** | **Fast** |

## Usage Example

```
Device: cuda
LoRA model loaded successfully!

============= Weibo Sentiment Analysis (LoRA Version) =============
Enter Weibo content for analysis (enter 'q' to exit):

Enter Weibo content: This movie is so amazing, I love it so much!
Prediction: Positive sentiment (Confidence: 0.9876)

Enter Weibo content: Bad service attitude, expensive prices, not recommended at all
Prediction: Negative sentiment (Confidence: 0.9742)

Enter Weibo content: q
```

## Notes

1. **First Run**: The pre-trained model will be automatically downloaded when first running `train.py`, please ensure network connection
2. **GPU Recommended**: Although LoRA has fewer parameters, GPU acceleration is recommended for training
3. **Model Loading**: Prediction requires trained LoRA weight files
4. **Compatibility**: Implemented based on PEFT library, fully compatible with Hugging Face ecosystem

## Extended Features

- **Multi-task Support**: Can train different LoRA weights for different tasks, sharing the same base model
- **Weight Merging**: Can merge multiple LoRA weights, or merge LoRA weights into the base model
- **Dynamic Switching**: Supports runtime dynamic loading and switching of different LoRA weights

## Technical Principles

LoRA adds two small matrices A and B alongside the original linear layer, such that:
```
h = W₀x + BAx
```
Where:
- W₀ is the frozen pre-trained weights
- B ∈ ℝᵈˣʳ, A ∈ ℝʳˣᵏ are trainable low-rank matrices
- r << min(d,k), greatly reducing parameter count

This design both preserves pre-trained model knowledge and efficiently adapts to new tasks.
