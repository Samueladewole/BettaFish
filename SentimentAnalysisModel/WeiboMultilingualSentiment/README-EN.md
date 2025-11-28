# Multilingual Sentiment Analysis

> **Note**: This is an English translation of the original Chinese documentation. The original project [BettaFish](https://github.com/666ghj/BettaFish) was created by [666ghj](https://github.com/666ghj). This translation was contributed to help non-Chinese speakers understand and use this project.

This module uses a multilingual sentiment analysis model from HuggingFace, supporting 22 languages.

## Model Information

- **Model Name**: tabularisai/multilingual-sentiment-analysis
- **Base Model**: distilbert-base-multilingual-cased
- **Supported Languages**: 22 languages, including:
  - Chinese
  - English
  - Spanish (Español)
  - Japanese (日本語)
  - Korean (한국어)
  - French (Français)
  - German (Deutsch)
  - Russian (Русский)
  - Arabic (العربية)
  - Hindi (हिन्दी)
  - Portuguese (Português)
  - Italian (Italiano)
  - And more...

- **Output Categories**: 5-level sentiment classification
  - Very Negative
  - Negative
  - Neutral
  - Positive
  - Very Positive

## Quick Start

1. Ensure dependencies are installed:
```bash
pip install transformers torch
```

2. Run the prediction program:
```bash
python predict.py
```

3. Enter text in any language for analysis:
```
Enter text: I love this product!
Prediction: Very Positive (Confidence: 0.9456)
```

4. View multilingual examples:
```
Enter text: demo
```

## Code Example

```python
from transformers import AutoTokenizer, AutoModelForSequenceClassification
import torch

# Load model
model_name = "tabularisai/multilingual-sentiment-analysis"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForSequenceClassification.from_pretrained(model_name)

# Predict
texts = [
    "今天心情很好",  # Chinese
    "I love this!",  # English
    "¡Me encanta!"   # Spanish
]

for text in texts:
    inputs = tokenizer(text, return_tensors="pt")
    outputs = model(**inputs)
    prediction = torch.argmax(outputs.logits, dim=1).item()
    sentiment_map = {0: "Very Negative", 1: "Negative", 2: "Neutral", 3: "Positive", 4: "Very Positive"}
    print(f"{text} -> {sentiment_map[prediction]}")
```

## Key Features

- **Multilingual Support**: No need to specify language, automatically recognizes 22 languages
- **5-level Fine-grained Classification**: More detailed sentiment analysis than traditional binary classification
- **High Accuracy**: Based on advanced DistilBERT architecture
- **Local Caching**: Saves to local after first download, speeds up subsequent use

## Use Cases

- International social media monitoring
- Multilingual customer feedback analysis
- Global product review sentiment classification
- Cross-language brand sentiment tracking
- Multilingual customer service optimization
- International market research

## Model Storage

- Model is automatically downloaded to the `model` folder in the current directory on first run
- Subsequent runs will load directly from local, no repeated download needed
- Model size is approximately 135MB, requires network connection for first download

## File Description

- `predict.py`: Main prediction program using direct model calling
- `README.md`: Usage instructions (Chinese)
- `README-EN.md`: Usage instructions (English)

## Notes

- First run will automatically download the model, requires network connection
- Model will be saved to current directory for convenience
- Supports GPU acceleration, automatically detects available devices
- To clean up model files, delete the `model` folder
- This model is trained on synthetic data, validation recommended for production use
