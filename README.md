```markdown
# CreditIQ — Credit Risk Assessment Pipeline

CreditIQ is a full-stack machine learning web application designed to predict loan default risk based on the HMEQ dataset. The project features a FastAPI backend serving a pre-trained Random Forest model and a clean HTML/CSS/JS frontend for user interaction.

## 📂 Project Structure

```text
credit-risk-app/
│
├── backend/
│   ├── main.py                  ← FastAPI server routing and inference logic
│   ├── requirements.txt         ← Python dependencies
│   ├── save_models_colab.py     ← Script to export models from Colab
│   │
│   ├── best_rf_model.pkl        ← Pre-trained Random Forest model
│   ├── lr_model.pkl             ← Pre-trained Logistic Regression model (comparison)
│   ├── scaler.pkl               ← Data scaler artifact
│   ├── imputer.pkl              ← Missing value imputer artifact
│   ├── le_reason.pkl            ← Label encoder for 'Reason'
│   └── le_job.pkl               ← Label encoder for 'Job'
│
├── frontend/
│   ├── index.html               ← Interactive frontend UI
│   ├── style.css                ← UI Styling
│   └── app.js                   ← Frontend logic and API integration
│
└── ML_assignment.ipynb          ← Jupyter Notebook for EDA and model training
```

## 🚀 Setup Guide

### Step 1: Export Models
1. Open the `ML_assignment.ipynb` notebook (or your Google Colab instance).
2. Run the code from `save_models_colab.py` in a new cell at the end of the notebook.
3. Download the 6 generated `.pkl` files and place them inside the `backend/` directory.

### Step 2: Set Up the Python Environment
Open your terminal, navigate to the `backend/` folder, and create a virtual environment to keep your packages isolated:

```bash
cd backend
python -m venv venv

# Activate the virtual environment
# On Windows:
venv\Scripts\activate
# On Mac/Linux:
source venv/bin/activate

# Install required packages
pip install -r requirements.txt
```

### Step 3: Start the Backend Server
With your virtual environment activated, run the FastAPI server:

```bash
uvicorn main:app --reload
```
*Note: The server will run on `http://127.0.0.1:8000`. You can test the endpoints via the automatic documentation at `http://127.0.0.1:8000/docs`.*

### Step 4: Launch the Frontend
Open the `frontend/index.html` file in your web browser. 
* If you are using VS Code, right-click the `index.html` file and select **Open with Live Server**.
* A green "● API Online" badge will appear at the top-right of the UI if it successfully connects to the backend.

## 🧪 Testing the Application

You can test the application using the following sample data profiles:

| Field | Low Risk Applicant | High Risk Applicant |
| :--- | :--- | :--- |
| **Loan Amount** | 15,000 | 45,000 |
| **Property Value** | 120,000 | 95,000 |
| **Mortgage Owed** | 60,000 | 90,000 |
| **Reason** | HomeImp | DebtCon |
| **Job** | Mgr | Self |
| **Years at Job** | 9 | 1 |
| **Delinquencies** | 0 | 5 |
| **Derogatory Reports** | 0 | 3 |
| **Credit Inquiries** | 1 | 8 |
| **Credit Lines** | 15 | 4 |
| **Oldest Credit (months)** | 200 | 30 |
| **Debt-to-Income (%)** | 18 | 72 |

## ⚙️ Architecture & Data Flow

1. **User Input:** The user fills the form and submits applicant data via the HTML/JS frontend.
2. **API Request:** `app.js` sends a POST request with a JSON payload containing the 12 fields to the `/predict` endpoint.
3. **Data Processing:** FastAPI loads the `.pkl` artifacts to encode categorical strings, impute missing values, and scale the numeric features.
4. **Model Inference:** The primary Random Forest model calculates a probability score. The Logistic Regression model is run for comparison.
5. **Response:** The API returns the risk score, risk level, system decision (APPROVE, CONDITIONAL, DECLINE), and targeted financial advice.
6. **UI Rendering:** The frontend dynamically updates the dashboard gauges, advice cards, and risk factor highlights.

## ⚠️ Common Errors & Fixes

* **"API Offline" badge showing:** The FastAPI server isn't running. Go to your terminal and run `uvicorn main:app --reload` inside the `backend/` folder.
* **"Model loading error" in terminal:** The `.pkl` files are missing from the `backend/` folder. Run `save_models_colab.py` in Colab and re-download them.
* **CORS error in browser console:** Already handled in `main.py` with `allow_origins=["*"]`. Ensure you're opening the HTML file directly or using a simple local server.
* **"Invalid category value" error:** The `LabelEncoder` was fitted on specific category strings. Make sure REASON is exactly `HomeImp` or `DebtCon` and JOB is exactly one of: `Mgr`, `Office`, `ProfExe`, `Sales`, `Self`, `Other`.
* **Packages version mismatch warning:** The `scikit-learn` version used to train the model must match the one installed in the backend. Check with `python -c "import sklearn; print(sklearn.__version__)"` in your Colab and in your local environment.
```
