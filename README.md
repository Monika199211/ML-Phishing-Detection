# 🛡️ PhishGuard AI - Phishing Website Detection

A comprehensive machine learning project that detects phishing websites using multiple ML algorithms with a modern FastAPI web interface.

## 🏗️ Project Architecture

### Directory Structure
```
phishing-website-detection-mm/
├── 📁 api/                    # FastAPI web application
│   ├── main.py               # FastAPI routes and endpoints
│   └── templates/
│       └── index.html        # Web UI template
├── 📁 artifacts/             # Trained models and scalers
│   ├── scaler.pkl           # StandardScaler for feature preprocessing
│   ├── xgb_model.pkl        # Trained XGBoost model
│   └── ann_mlp_model.pkl    # Trained ANN model
├── 📁 data/                  # Dataset storage
│   └── phising.csv          # Phishing detection dataset
├── 📁 inference/             # Prediction engine
│   └── predictor.py         # Model prediction logic
├── 📁 notebook/              # Jupyter notebooks for analysis
│   ├── EDA.ipynb            # Exploratory Data Analysis
│   └── phishing.csv         # Dataset copy for notebooks
├── 📁 src/                   # Core machine learning pipeline
│   ├── config_loader.py     # Configuration management
│   ├── data_loader.py       # Dataset loading utilities
│   ├── pipeline.py          # Complete ML training pipeline
│   ├── preprocessor.py      # Data preprocessing
│   ├── train_ann.py         # ANN model training
│   ├── train_xgboost.py     # XGBoost model training
│   └── utils.py            # Utility functions
├── config.yaml              # Configuration settings
├── requirements.txt         # Python dependencies
├── run_pipeline.py          # Pipeline execution script
└── main.py                  # Entry point
```

### 🧠 Machine Learning Pipeline

#### 1. **Data Processing Flow**
```
Raw Dataset → Data Loader → Preprocessor → Feature Engineering → Model Training
```

#### 2. **Supported Models**
- **XGBoost Classifier**: Gradient boosting algorithm optimized for performance
- **ANN (MLPClassifier)**: Multi-layer perceptron neural network

#### 3. **Feature Set (30 Features)**
The model analyzes 30 different website characteristics:

**URL Structure**
- `having_IP_Address`, `URL_Length`, `Shortining_Service`, `having_At_Symbol`
- `double_slash_redirecting`, `Prefix_Suffix`, `having_Sub_Domain`

**Security & SSL**
- `SSLfinal_State`, `HTTPS_token`, `port`, `DNSRecord`

**Domain Trust**
- `Domain_registeration_length`, `age_of_domain`, `Google_Index`, `Page_Rank`
- `web_traffic`, `Links_pointing_to_page`, `Statistical_report`

**Page Behavior**
- `Request_URL`, `URL_of_Anchor`, `Links_in_tags`, `SFH`
- `Submitting_to_email`, `Abnormal_URL`, `Redirect`, `on_mouseover`
- `RightClick`, `popUpWidnow`, `Iframe`, `Favicon`

### 🌐 Web Application Architecture

#### API Endpoints
- **GET `/`**: Web UI for interactive predictions
- **POST `/predict`**: Form-based prediction endpoint
- **POST `/predict_json`**: JSON API for programmatic access

#### Prediction Response
```json
{
  "model": "xgboost",
  "prediction": 1,
  "result_text": "Legit Website",
  "phishing_probability": 0.15,
  "legit_probability": 0.85
}
```

## 🚀 Quick Start

### Prerequisites
- Python 3.12+
- Virtual environment support
- Homebrew (macOS) for OpenMP

### 1. Environment Setup
```bash
# Clone the repository
git clone https://github.com/Monika199211/phishing-website-detection-mm.git
cd phishing-website-detection-mm

# Activate virtual environment
source env/bin/activate

# Install dependencies
uv pip install -r requirements.txt

# Install OpenMP for XGBoost (macOS only)
brew install libomp
```

### 2. Train Models
```bash
# Run the complete ML pipeline
python run_pipeline.py
```

This will:
- Load and preprocess the dataset
- Train XGBoost and ANN models
- Save trained models to `artifacts/`
- Display performance metrics

### 3. Start Applications

#### Option A: Traditional ML Web Application
```bash
# Start the ML-based web application
uvicorn api.main:app --reload

# Or with custom host/port
uvicorn api.main:app --host 0.0.0.0 --port 8000 --reload
```

#### Option B: MLflow UI for Experiment Tracking
```bash
# Start MLflow UI to view experiment tracking
mlflow ui

# Or with custom host/port
mlflow ui --host 0.0.0.0 --port 5000

# Access at: http://localhost:5000
```

### 4. Access the Applications

#### ML Web Application
- **Web UI**: http://localhost:8000
- **API Documentation**: http://localhost:8000/docs
- **ReDoc**: http://localhost:8000/redoc

#### MLflow Dashboard
- **Experiment Tracking**: http://localhost:5000
- **Model Registry**: View model versions and metrics
- **Compare Runs**: Analyze different training experiments

### 5. Complete Workflow Commands
```bash
# 1. Train models with MLflow tracking
python run_pipeline.py

# 2. Start MLflow UI (in new terminal)
mlflow ui

# 3. Start main application (in new terminal)
uvicorn api.main:app --reload
```

## 📊 Usage Examples

### Web Interface
1. Visit http://localhost:8000
2. Select a model (XGBoost or ANN)
3. Fill in website characteristics
4. Click "Predict Website Risk"
5. View the prediction results

### API Usage
```bash
# Example JSON prediction
curl -X POST "http://localhost:8000/predict_json" \
     -H "Content-Type: application/json" \
     -d '{
       "model_type": "xgboost",
       "features": {
         "having_IP_Address": "no",
         "URL_Length": "no",
         "SSLfinal_State": "yes",
         "HTTPS_token": "no"
       }
     }'
```

### Command Line Testing
```bash
# Test the prediction engine directly
python inference/predictor.py
```

## ⚙️ Configuration

Edit `config.yaml` to customize:

```yaml
data:
  file_path: "data/phising.csv"
  target_column: "Result"
  test_size: 0.2
  random_state: 42

xgboost:
  n_estimators: 400
  max_depth: 7
  learning_rate: 0.04
  # ... other parameters

mlp:
  hidden_layers: [128, 64, 32]
  activation: "relu"
  solver: "adam"
  # ... other parameters
```

## 🔧 Development

### Project Structure Details

**Core Components:**
- `src/pipeline.py`: Orchestrates the entire ML workflow
- `inference/predictor.py`: Real-time prediction engine
- `api/main.py`: FastAPI web application
- `config.yaml`: Centralized configuration

**Data Flow:**
1. Raw data → `data_loader.py`
2. Preprocessing → `preprocessor.py` 
3. Model training → `train_*.py` (with MLflow tracking)
4. Inference → `predictor.py`
5. Web interface → `api/main.py`

### MLflow Integration

**Experiment Tracking:**
- All model training runs are automatically logged to MLflow
- Experiment name: "PhishGuard_AI"
- Tracks metrics, parameters, and model artifacts

**MLflow Commands:**
```bash
# View experiment tracking UI
mlflow ui

# View specific experiment
mlflow ui --backend-store-uri ./mlruns

# Compare model runs
mlflow ui --host 0.0.0.0 --port 5000
```

**What's Tracked:**
- Model hyperparameters
- Training/validation metrics
- Model artifacts (.pkl files)
- Dataset information
- Training duration

### Adding New Models
1. Create training script in `src/train_newmodel.py`
2. Add MLflow tracking to your training function:
   ```python
   import mlflow
   
   with mlflow.start_run():
       mlflow.log_params(params)
       mlflow.log_metric("accuracy", accuracy)
       mlflow.sklearn.log_model(model, "model")
   ```
3. Update `config.yaml` with model parameters
4. Add model loading in `inference/predictor.py`
5. Update API endpoints if needed

## 📈 Model Performance

The trained models achieve high accuracy in detecting phishing websites:
- Feature engineering with 30 carefully selected attributes
- StandardScaler normalization for optimal performance
- Cross-validation and stratified sampling
- Real-time prediction capabilities

## � Quick Reference Commands

### Essential Commands
```bash
# Complete setup and run
source env/bin/activate                    # Activate environment
python run_pipeline.py                     # Train models
uvicorn api.main:app --reload             # Start main app
mlflow ui                                  # Start MLflow UI (new terminal)
```

### Development Commands
```bash
# Testing
python inference/predictor.py             # Test prediction engine
python src/config_loader.py              # Test configuration
python src/data_loader.py                # Test data loading

# Applications
uvicorn api.main:app --reload             # ML web application (port 8000)
mlflow ui --port 5000                     # MLflow UI (port 5000)

# API Testing
curl -X POST "http://localhost:8000/predict_json" -H "Content-Type: application/json" -d '{"model_type":"xgboost","features":{"having_IP_Address":"no"}}'
```

### Multiple Services (Run in separate terminals)
```bash
# Terminal 1: MLflow UI
mlflow ui

# Terminal 2: Main Application
uvicorn api.main:app --reload
```

## �🛠️ Troubleshooting

### Common Issues

**XGBoost Import Error (macOS):**
```bash
brew install libomp
```

**Virtual Environment Issues:**
```bash
# Recreate virtual environment
python -m venv env
source env/bin/activate
uv pip install -r requirements.txt
```

**Missing Trained Models:**
```bash
# Retrain models
python run_pipeline.py
```

## 📝 License

This project is licensed under the MIT License.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## 📞 Support

For issues and questions:
- Create an issue on GitHub
- Check the documentation in `/docs`
- Review the configuration in `config.yaml`

---

**PhishGuard AI** - Protecting users from phishing attacks with advanced machine learning.