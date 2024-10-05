# Fine-tuning InceptionResNetV2 for Stanford Dog Breed Classification

## Repository Structure

```
dog_breed_classification/
│
├── src/
│   ├── dog_breed_classifier.ipynb        # Dog Breed Classification solution
│
├── models/
│   └── dog_breed_classifier.keras  # Saved model  (not included in repo)
│
├── data/
│   ├── Annotation         # Annotations (not included in repo)
│   ├── Images             # Images (not included in repo)
│   ├── file_list.mat      # File list (not included in repo)
│   ├── test_list.mat      # Test list (not included in repo)
│   └── train_list.mat     # Train list (not included in repo)
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
   docker build -t dog_breed_classification .
   ```
2. Run the container:
   ```
   docker run -p 8888:8888 -v $(pwd):/app --name dbc-container dog_breed_classification
   ```

## Task 1: Dog Breed Classification

- Solution implemented in `dog_breed_classifier.ipynb`
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

## Dependencies
All required Python libraries are listed in `requirements.txt`
