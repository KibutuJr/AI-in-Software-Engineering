# Here's a step-by-step approach to accomplish  task 3:

  **Predictive Analytics for Resource Allocation in Breast Cancer Diagnosis**

      Step 1: Load Kaggle Breast Cancer Dataset

  - Used the Wisconsin Breast Cancer dataset available in scikit-learn, which is equivalent to the Kaggle version. It contains features computed from digitized images of fine needle aspirates (FNA) of breast masses.

  - loaded the breast cancer dataset using scikit-learn's `load_breast_cancer` function, which provides 30 numerical features and a binary target (0: malignant, 1: benign).
  - convert the data into a pandas DataFrame for easier manipulation and inspection. The `info()` and `head()` methods help us understand the dataset's structure and content.

     Step 2: Recode Targets to Simulate High/Medium/Low Priorities

   - The original dataset has binary targets (malignant/benign).
   - simulate 'high', 'medium', and 'low' priorities by creating a synthetic rule based on feature values, as the dataset doesn't directly provide priority labels.
   - Use the 'mean radius' feature (a key indicator of tumor size) to assign priorities.
   - Recode targets based on mean radius thresholds.

      **Assumption**:
      -  Larger tumors (higher mean radius) indicate higher priority.

      - Since the dataset doesn't have inherent priority labels, created a synthetic priority system based on the 'mean radius' feature, assuming larger tumors require higher priority for resource allocation (e.g., urgent medical intervention).
      - Then define thresholds (>17 for 'high', >12 for 'medium', ≤12 for 'low') based on the dataset's mean radius distribution. The priorities are then encoded numerically (low: 0, medium: 1, high: 2) for compatibility with the RandomForestClassifier.


     Step 3: Preprocess and Split Data

     - check for missing values
     - scale numerical features
     - encode the target
     - split the data into training and testing sets.

     - The `StandardScaler` standardizes the features to have zero mean and unit variance, which is important for many machine learning algorithms.
     - The data is split into 80% training and 20% testing sets.
     - stratification maintains priority distribution.


     Step 4: Train RandomForestClassifier and Evaluate

     - The RandomForestClassifier is trained on the scaled training data.
     - we predict on the test set and compute two metrics:
          - accuracy (proportion of correct predictions)
          - macro F1-score (harmonic mean of precision and recall, averaged across classes without weighting by class size).
     - The metrics are presented in a pandas DataFrame for clarity:

        Model Performance Metrics: 
           Metric     Value
           Accuracy  0.991228
           F1-Score  0.989958
        
    
     Step 5: Plot Confusion Matrix

     - visualize the confusion matrix to understand the model's performance across the priority classes.
     - The confusion matrix shows the number of true vs. predicted labels for each priority class.
     - The heatmap uses seaborn for clear visualization, with annotations showing the count of predictions in each cell.  - The axes are labeled with priority names for interpretability.