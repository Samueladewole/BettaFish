# Weibo Sentiment Recognition Model - GPT2-Adapter Fine-tuning

> **Note**: This is an English translation of the original Chinese documentation. The original project [BettaFish](https://github.com/666ghj/BettaFish) was created by [666ghj](https://github.com/666ghj). This translation was contributed to help non-Chinese speakers understand and use this project.

## Project Description
This is a Weibo sentiment binary classification model based on GPT2, using Adapter fine-tuning technology. Through Adapter fine-tuning, the model can be adapted to sentiment analysis tasks with only a small number of trainable parameters, significantly reducing computational resource requirements and model size.

## Dataset
Uses the Weibo sentiment dataset (weibo_senti_100k), containing approximately 100,000 sentiment-labeled Weibo posts, with about 50,000 positive and 50,000 negative comments each. Dataset labels:
- Label 0: Negative sentiment
- Label 1: Positive sentiment

## File Structure
```
GPT2-Adapter-tuning/
├── adapter.py              # Adapter layer implementation
├── gpt2_adapter.py         # GPT2 model Adapter implementation
├── train.py                # Training script
├── predict.py              # Simplified prediction script (interactive use)
├── models/                 # Locally stored pre-trained models
│   └── gpt2-chinese/       # Chinese GPT2 model and configuration
├── dataset/                # Dataset directory
│   └── weibo_senti_100k.csv  # Weibo sentiment dataset
└── best_weibo_sentiment_model.pth  # Best trained model
```

## Technical Features

1. **Parameter-efficient Fine-tuning**: Compared to full parameter fine-tuning, only about 3% of parameters are trained
2. **Maintained Model Performance**: Achieves good classification performance while training only a small number of parameters
3. **Suitable for Resource-constrained Environments**: Small model size, fast inference speed

## Environment Dependencies
- Python 3.6+
- PyTorch
- Transformers
- Pandas
- NumPy
- Scikit-learn
- Tqdm

## Usage

### Training the Model
```bash
python train.py
```
The training process will automatically:
- Download and locally save the Chinese GPT2 pre-trained model
- Load the Weibo sentiment dataset
- Train the model and save the best model

### Sentiment Analysis Prediction
```bash
python predict.py
```
After running, you'll enter interactive mode:
- Enter the Weibo text to analyze in the console
- System will return sentiment analysis result (positive/negative) and confidence
- Enter 'q' to exit the program

## Model Structure
- Base model: `uer/gpt2-chinese-cluecorpussmall` Chinese pre-trained model
- Model local save path: `./models/gpt2-chinese/`
- Fine-tuned by adding an Adapter layer after each GPT2Block
- Freezes original GPT2 parameters, only trains classifier and Adapter layer parameters

## Adapter Technology
Adapter is a parameter-efficient fine-tuning technique that inserts small bottleneck layers into Transformer layers to adapt to downstream tasks with minimal parameters. Key features:

1. **Parameter Efficient**: Compared to full parameter fine-tuning, Adapter only trains a small portion of parameters
2. **Prevents Forgetting**: Keeps original pre-trained model parameters unchanged, avoiding catastrophic forgetting
3. **Multi-task Adaptation**: Can train different Adapters for different tasks, sharing the same base model

In this project, we add an Adapter layer after each GPT2Block with a hidden layer size of 64, much smaller than the original model's hidden layer size (typically 768 or 1024).

## Usage Example
```
Device: cuda
Loading model: best_weibo_sentiment_model.pth

============= Weibo Sentiment Analysis =============
Enter Weibo content for analysis (enter 'q' to exit):

Enter Weibo content: This movie is so amazing, I love it so much!
Prediction: Positive sentiment (Confidence: 0.9876)

Enter Weibo content: Bad service attitude, expensive prices, not recommended at all
Prediction: Negative sentiment (Confidence: 0.9742)
```

## Notes
- Prediction script uses local model path, no need to download online
- Ensure `models/gpt2-chinese/` directory contains model files saved from training process
- First run of train.py will automatically download and save the model, please ensure network connection
