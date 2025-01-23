# Manufacturing Downtime Prediction API

This repository contains an API built using Flask to predict manufacturing downtime based on temperature and run time data. The model is trained using a sample dataset containing Machine_ID, Temperature, Run_Time, and Downtime_Flag.

## Sample Dataset

You can download the sample dataset used for training the model from the following file:  
`data/manufacturing_data_sample.csv`

The dataset contains the following columns:
- `Machine_ID`: Unique ID of the machine.
- `Temperature`: Temperature reading of the machine.
- `Run_Time`: Duration of machine run.
- `Downtime_Flag`: 0 if no downtime, 1 if downtime occurred.


## Setup Instructions

1. **Clone the repository** (if applicable):
    ```bash
    git clone <repo-url>
    cd <repo-folder>
    ```

2. **Install required dependencies**:
    ```bash
    !pip install flask scikit-learn pyngrok pandas
    ```

3. **Run the Flask app**:
    - Since the code is implemented in Google Colab, simply run the Flask code in a code cell.
    - Run the following in a separate code cell to start the Flask app:
    ```python
    from flask import Flask, request, jsonify
    import pandas as pd
    import pickle
    from sklearn.linear_model import LogisticRegression
    from sklearn.model_selection import train_test_split
    from sklearn.metrics import accuracy_score, f1_score
    from pyngrok import ngrok

    app = Flask(__name__)

    # Define the routes here (upload, train, predict)

    # To start the app, call app.run() in the Colab cell.
    public_url = ngrok.connect(5000).public_url
    print("Public URL:", public_url)
    app.run(port=5000)
    ```

4. **Upload your dataset**:
    - Use the `/upload` endpoint to upload a CSV file containing `Machine_ID`, `Temperature`, `Run_Time`, and `Downtime_Flag`.

5. **Train the model**:
    - Use the `/train` endpoint to train the model on the uploaded dataset.

6. **Make predictions**:
    - Use the `/predict` endpoint with a JSON payload like `{"Temperature": 30, "Run_Time": 10}` to get predictions for downtime.

## Example API Requests

### 1. Upload a CSV file (POST /upload)
**Request**:
```bash
curl -X POST https://<public-url>/upload -F "file=@<file-path>"
```

**Response**:
```json
{
  "columns": ["Machine_ID", "Temperature", "Run_Time", "Downtime_Flag"],
  "message": "File uploaded successfully!"
}
```

### 2. Train the model (POST /train)
**Request**:
```bash
curl -X POST https://<public-url>/train
```

**Response**:
```json
{
  "accuracy": 0.8,
  "f1_score": 0.0,
  "message": "Model trained successfully!"
}
```

### 3. Make a prediction (POST /predict)
**Request**:
```bash
curl -X POST https://<public-url>/predict -H "Content-Type: application/json" -d "{\"Temperature\": 30, \"Run_Time\": 10}"
```

**Response**:
```json
{
  "Confidence": 0.68,
  "Downtime": "Yes"
}
```

