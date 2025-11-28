# Weibo Sentiment Analysis - Traditional Machine Learning Methods

## Project Introduction

This project uses 5 traditional machine learning methods for binary sentiment classification (positive/negative) of Chinese Weibo posts:

- **Naive Bayes**: Bag-of-words based probabilistic classification
- **SVM**: Support Vector Machine based on TF-IDF features
- **XGBoost**: Gradient Boosting Decision Tree
- **LSTM**: Recurrent Neural Network + Word2Vec word embeddings
- **BERT+Classification Head**: Pre-trained language model with classifier (also considered within traditional ML scope)

## Model Performance

Performance on Weibo sentiment dataset (10,000 training samples, 500 test samples):

| Model | Accuracy | AUC | Features |
|-------|----------|-----|----------|
| Naive Bayes | 85.6% | - | Fast, low memory usage |
| SVM | 85.6% | - | Good generalization |
| XGBoost | 86.0% | 90.4% | Stable performance, supports feature importance |
| LSTM | 87.0% | 93.1% | Understands sequential information and context |
| BERT+Classification Head | 87.0% | 92.9% | Powerful semantic understanding |

## Environment Setup

```bash
pip install -r requirements.txt
```

Data file structure:
```
data/
├── weibo2018/
│   ├── train.txt
│   └── test.txt
└── stopwords.txt
```

## Training Models (Can run without arguments)

### Naive Bayes
```bash
python bayes_train.py
```

### SVM
```bash
python svm_train.py --kernel rbf --C 1.0
```

### XGBoost
```bash
python xgboost_train.py --max_depth 6 --eta 0.3 --num_boost_round 200
```

### LSTM
```bash
python lstm_train.py --epochs 5 --batch_size 100 --hidden_size 64
```

### BERT
```bash
python bert_train.py --epochs 10 --batch_size 100 --learning_rate 1e-3
```

Note: The BERT model will automatically download the Chinese pre-trained model (bert-base-chinese)

## Using for Prediction

### Interactive Prediction (Recommended)
```bash
python predict.py
```

### Command Line Prediction
```bash
# Single model prediction
python predict.py --model_type bert --text "The weather is great today, I feel wonderful"

# Multi-model ensemble prediction
python predict.py --ensemble --text "This movie is so boring"
```

## File Structure

```
WeiboSentiment_MachineLearning/
├── bayes_train.py           # Naive Bayes training
├── svm_train.py             # SVM training
├── xgboost_train.py         # XGBoost training
├── lstm_train.py            # LSTM training
├── bert_train.py            # BERT training
├── predict.py               # Unified prediction program
├── base_model.py            # Base model class
├── utils.py                 # Utility functions
├── requirements.txt         # Dependencies
├── model/                   # Model save directory
└── data/                    # Data directory
```

## Notes

1. **BERT model** will automatically download the pre-trained model (~400MB) on first run
2. **LSTM model** training takes longer, GPU recommended
3. **Models are saved** in the `model/` directory, ensure sufficient disk space
4. **Memory requirements**: BERT > LSTM > XGBoost > SVM > Naive Bayes
