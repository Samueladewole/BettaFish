# Weibo Sentiment Analysis - Fine-tuned BertChinese Model

> **Note**: This is an English translation of the original Chinese documentation. The original project [BettaFish](https://github.com/666ghj/BettaFish) was created by [666ghj](https://github.com/666ghj). This translation was contributed to help non-Chinese speakers understand and use this project.

This module uses a pre-trained Weibo sentiment analysis model from HuggingFace.

## Model Information

- **Model Name**: wsqstar/GISchat-weibo-100k-fine-tuned-bert
- **Model Type**: BERT Chinese sentiment classification model
- **Training Data**: 100,000 Weibo posts
- **Output**: Binary classification (positive/negative sentiment)

## Usage

### Method 1: Direct Model Calling (Recommended)
```bash
python predict.py
```

### Method 2: Pipeline Approach
```bash
python predict_pipeline.py
```

## Quick Start

1. Ensure dependencies are installed:
```bash
pip install transformers torch
```

2. Run the prediction program:
```bash
python predict.py
```

3. Enter Weibo text for analysis:
```
Enter Weibo content: The weather is great today, I feel wonderful!
Prediction: Positive sentiment (Confidence: 0.9234)
```

## Code Example

```python
from transformers import AutoTokenizer, AutoModelForSequenceClassification
import torch

# Load model
model_name = "wsqstar/GISchat-weibo-100k-fine-tuned-bert"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForSequenceClassification.from_pretrained(model_name)

# Predict
text = "I feel great today"
inputs = tokenizer(text, return_tensors="pt")
outputs = model(**inputs)
prediction = torch.argmax(outputs.logits, dim=1).item()
print("Positive sentiment" if prediction == 1 else "Negative sentiment")
```

## File Description

- `predict.py`: Main prediction program using direct model calling
- `predict_pipeline.py`: Prediction program using pipeline approach
- `README.md`: Usage instructions (Chinese)
- `README-EN.md`: Usage instructions (English)

## Model Storage

- Model is automatically downloaded to the `model` folder in the current directory on first run
- Subsequent runs will load directly from local, no repeated download needed
- Model size is approximately 400MB, requires network connection for first download

## Notes

- First run will automatically download the model, requires network connection
- Model will be saved to current directory for convenience
- Supports GPU acceleration, automatically detects available devices
- To clean up model files, delete the `model` folder
