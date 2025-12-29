---
name: data-science-ai
description: Apply machine learning, deep learning, data analysis, and AI techniques. Master Python, TensorFlow, PyTorch, scikit-learn, and modern AI frameworks for building intelligent systems.
sasmp_version: "1.3.0"
bonded_agent: 01-frontend-web-development
bond_type: PRIMARY_BOND
---

# Data Science & AI Skill

## Quick Start

Data science and AI enable data-driven decision making and intelligent automation. Here's how to get started:

### Data Analysis with Pandas
```python
import pandas as pd
import numpy as np
from sklearn.preprocessing import StandardScaler

# Load and explore data
df = pd.read_csv('data.csv')
print(df.head())
print(df.info())
print(df.describe())

# Data cleaning
df = df.dropna()  # Remove missing values
df['age'] = df['age'].astype(int)

# Data transformation
df['log_income'] = np.log(df['income'])
scaler = StandardScaler()
df[['age', 'salary']] = scaler.fit_transform(df[['age', 'salary']])

# Aggregation and grouping
by_department = df.groupby('department').agg({
    'salary': ['mean', 'max'],
    'age': 'mean'
})
```

### Machine Learning with scikit-learn
```python
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score, confusion_matrix

# Prepare data
X = df.drop('target', axis=1)
y = df['target']
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

# Train model
model = RandomForestClassifier(n_estimators=100, random_state=42)
model.fit(X_train, y_train)

# Evaluate
predictions = model.predict(X_test)
accuracy = accuracy_score(y_test, predictions)
print(f"Accuracy: {accuracy:.2f}")

# Feature importance
importances = model.feature_importances_
for name, importance in zip(X.columns, importances):
    print(f"{name}: {importance:.4f}")
```

### Deep Learning with PyTorch
```python
import torch
import torch.nn as nn
from torch.utils.data import DataLoader, TensorDataset

# Define model
class NeuralNetwork(nn.Module):
    def __init__(self, input_size):
        super().__init__()
        self.layers = nn.Sequential(
            nn.Linear(input_size, 128),
            nn.ReLU(),
            nn.Dropout(0.2),
            nn.Linear(128, 64),
            nn.ReLU(),
            nn.Dropout(0.2),
            nn.Linear(64, 1),
            nn.Sigmoid()
        )

    def forward(self, x):
        return self.layers(x)

# Training loop
model = NeuralNetwork(input_size=20)
optimizer = torch.optim.Adam(model.parameters(), lr=0.001)
loss_fn = nn.BCELoss()

for epoch in range(10):
    for batch_x, batch_y in train_loader:
        optimizer.zero_grad()
        predictions = model(batch_x)
        loss = loss_fn(predictions, batch_y)
        loss.backward()
        optimizer.step()

    print(f"Epoch {epoch+1}, Loss: {loss.item():.4f}")
```

### NLP with Transformers
```python
from transformers import AutoTokenizer, AutoModelForSequenceClassification
import torch

# Load pre-trained model
tokenizer = AutoTokenizer.from_pretrained("distilbert-base-uncased")
model = AutoModelForSequenceClassification.from_pretrained(
    "distilbert-base-uncased-finetuned-sst-2-english"
)

# Inference
text = "This movie is absolutely fantastic!"
inputs = tokenizer(text, return_tensors="pt")
outputs = model(**inputs)
predictions = torch.nn.functional.softmax(outputs.logits, dim=-1)
print(f"Positive: {predictions[0][1]:.4f}")
```

## Key Areas

### 1. Statistics & Mathematics
- Probability distributions
- Hypothesis testing
- Correlation and regression
- Bayesian inference
- Statistical modeling
- Confidence intervals

### 2. Data Processing
- Data cleaning and preprocessing
- Feature engineering
- Data normalization and scaling
- Outlier detection
- Handling missing values
- Data aggregation

### 3. Machine Learning
- Supervised learning (classification, regression)
- Unsupervised learning (clustering)
- Ensemble methods
- Hyperparameter tuning
- Cross-validation
- Model evaluation metrics

### 4. Deep Learning
- Neural network architectures
- Convolutional Neural Networks (CNN)
- Recurrent Neural Networks (RNN)
- Transformers and attention
- Transfer learning
- GPU acceleration

### 5. Specialized Domains
- Natural Language Processing (NLP)
- Computer Vision
- Time series forecasting
- Recommendation systems
- Reinforcement learning

### 6. MLOps & Deployment
- Model versioning and tracking (MLflow)
- Containerization (Docker)
- Model serving (FastAPI, TensorFlow Serving)
- Monitoring and logging
- CI/CD for ML pipelines

## Advanced Concepts

### Hyperparameter Optimization
```python
from sklearn.model_selection import GridSearchCV

param_grid = {
    'n_estimators': [50, 100, 200],
    'max_depth': [5, 10, 15],
    'min_samples_split': [2, 5, 10]
}

grid_search = GridSearchCV(RandomForestClassifier(), param_grid, cv=5)
grid_search.fit(X_train, y_train)
print(f"Best params: {grid_search.best_params_}")
```

### Custom PyTorch Dataset
```python
from torch.utils.data import Dataset

class CustomDataset(Dataset):
    def __init__(self, X, y):
        self.X = torch.FloatTensor(X)
        self.y = torch.FloatTensor(y)

    def __len__(self):
        return len(self.X)

    def __getitem__(self, idx):
        return self.X[idx], self.y[idx]
```

## Resources

- **Scikit-learn Documentation**: https://scikit-learn.org/
- **PyTorch Tutorials**: https://pytorch.org/tutorials/
- **TensorFlow/Keras**: https://www.tensorflow.org/
- **Hugging Face Transformers**: https://huggingface.co/
- **Fast.ai Course**: https://course.fast.ai/

## Real-World Projects

1. **Iris Classification**: Classic ML starting point
2. **Sentiment Analysis**: NLP with pre-trained models
3. **Image Classification**: CNN with CIFAR-10
4. **Time Series Forecasting**: Stock price prediction
5. **Recommendation System**: Collaborative filtering
6. **Chatbot**: Transformer-based conversational AI

---

**Next Steps:** Master Python first, start with classical ML, then explore deep learning and specialized domains.
