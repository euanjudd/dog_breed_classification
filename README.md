# Netsmile Take-Home Interview Project

## Repository Structure

```
netsmile/
│
├── src/
│   ├── task1.ipynb        # Dog Breed Classification solution
│   ├── task2.ipynb        # Target Prediction and Binary Classification solution
│   ├── app.py             # FastAPI application for serving the model
│   └── client.py          # Client script for interacting with the API
│
├── models/
│   └── dog_breed_classifier.keras  # Saved model from Task 1  (not included in repo)
│
├── data/
│   ├── Annotation         # Annotations for Task 1 (not included in repo)
│   ├── Images             # Images for Task 1 (not included in repo)
│   ├── file_list.mat      # File list for Task 1 (not included in repo)
│   ├── test_list.mat      # Test list for Task 1 (not included in repo)
│   ├── train_list.mat     # Train list for Task 1 (not included in repo)
│   └── dataset_task.csv   # Dataset for Task 2 (not included in repo)
│
├── Dockerfile             # Dockerfile for containerizing the application
├── requirements.txt       # Python dependencies
└── README.md              # This file
```

## Preparation
1. Create a `data` folder in project directory.
2. Download and extract the Stanford Dogs Dataset:
   - Download from: http://vision.stanford.edu/aditya86/ImageNetDogs/
   - Extract the `Images` folder into `data/`
   - Place `train_list.mat` and `test_list.mat` in `data/`
3. Place `dataset_task.csv` in the `data/` folder.
4. Create an empty `models` folder in project directory where model from Task 1 will be saved.

### Running the Application
1. Build the Docker container:
   ```
   docker build -t netsmile .
   ```
2. Run the container:
   ```
   docker run -d -p 8888:8888 -p 8000:8000 -v $(pwd):/app --name netsmile-container netsmile
   ```
3. Go to `http://localhost:8888`

## Task 1: Dog Breed Classification

- Solution implemented in `task1_dog_breed_classifier.ipynb`
- Approach: Transfer learning using InceptionResNetV2 as base model
- Dataset: Stanford Dogs Dataset (http://vision.stanford.edu/aditya86/ImageNetDogs/)
- Implementation details:
  - Fine-tuned only the last fully-connected layer
  - Data augmentation: horizontal and vertical flips
  - 5-fold cross-validation
  - Trained on subset (5/120 classes) due to computational constraints
- Results on test set (5 classes):
  - Accuracy: 96.68%
  - F1-score (weighted avg): 0.97
- Model saved in both Keras (.keras) and SavedModel formats

## Task 2: Target Prediction and Binary Classification

- Solution implemented in `task2_binary_classifier.ipynb`
- Uses the dataset "dataset_task.csv"
- Includes:
  - Data processing and initial analysis
  - Proposed regression task solution (Task 2a)
  - Proposed binary classification with arbitrary threshold solution (Task 2b)
  - Implementation of binary classification with thresholds 7 and 8 (Task 2c)
- Key models used: Random Forest, XGBoost
- Evaluation metrics: Precision, Recall, F1-score, AUR-PR for classification
- Addresses class imbalance issues in classification tasks

Note: Detailed explanations, approach rationale, and specific findings are documented within the notebook.

## Task 3: Production Deployment
- Model from Task 1 saved as '/app/models/dog_breed_classifier.keras'
- FastAPI application in `/app/src/app.py`
- Client script in `/app/src/client.py`
To use the client script for predictions:
1. Ensure the container is running
2. Run: 
   ```
   docker exec -it netsmile-container python src/client.py
   ```

### Model Versioning and Testing
Versioning
- Semantic versioning (e.g., v1.0.0)

Testing
- Unit tests to check individual parts of the code work.
- Integration tests, e.g. test the entire process from data input and preprocessing to prediction output to check all components work together.

## Dependencies
All required Python libraries are listed in `requirements.txt`

## Notes
- The dataset for Task 1 and 2 are not included in this repository due to size constraints.
- The trained for Task 1 and 2 models are also not included.
