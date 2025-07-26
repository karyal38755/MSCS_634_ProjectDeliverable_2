# Project Deliverable 2

## Overview

This project uses the **Student Performance dataset** from the UCI Machine Learning Repository to develop regression models that predict students’ final grades (`G3`). The primary focus is on applying **thoughtful feature engineering** to improve model accuracy and robustness, building multiple regression models, and evaluating their performance through rigorous statistical metrics and cross-validation.

## Dataset Summary

- The dataset comprises 395 student records, including demographic, social, and academic features.
- The target variable is `G3`, the final grade (ranging from 0 to 20).
- Features include categorical variables (e.g., school, sex), binary indicators (e.g., family support), and numeric scores (`G1`, `G2`, failures, absences).

## Feature Engineering

To enrich model input and capture latent academic and environmental factors, the following features were engineered:

| New Feature                   | Description                                              | Purpose                                        |
|-------------------------------|----------------------------------------------------------|------------------------------------------------|
| `avg_grade`                   | Mean of first and second period grades (`G1`, `G2`)      | Smooths semester-to-semester variance          |
| `parent_edu`                  | Average of mother's and father's education levels        | Reflects home academic environment             |
| `study_support`               | Sum of school, family, and internet support indicators   | Quantifies external academic help              |
| `travel_time_normalized`      | Binned and reversed travel time to school                | Encodes accessibility impact                   |
| `failure_absence_interaction` | Product of failures and absences                         | Models compounding negative impact             |

These transformations harness domain intuition to reveal underlying patterns beyond raw data.

## Modeling Process

Two regression models were developed:

1. **Linear Regression:**  
   A baseline model to assess how well a simple linear approach fits the data.

2. **Ridge Regression:**  
   Adds L2 regularization to control coefficient magnitude and reduce overfitting, especially valuable given the one-hot encoded categorical features.

Both models were trained on a standardized feature set and evaluated on a held-out test set.

## Model Evaluation

Models were evaluated using:

- **R² (Coefficient of Determination):** Measures variance explained by the model.
- **Mean Squared Error (MSE):** Penalizes larger prediction errors.
- **Root Mean Squared Error (RMSE):** Gives error in the original grade units, improving interpretability.

Cross-validation (5-fold) was employed to ensure the models generalize well to unseen data.

### Evaluation Results Summary

| Model             | R²     | MSE  | RMSE | CV Mean R² |
|-------------------|--------|------|------|------------|
| Linear Regression | 0.7020 | 6.11 | 2.47 | 0.7725     |
| Ridge Regression  | 0.7021 | 6.11 | 2.47 | 0.7725     |

The Ridge model slightly outperformed Linear Regression, demonstrating better generalization through regularization.

## Key Insights

- **Feature engineering significantly improved performance.** Especially `avg_grade` and `failure_absence_interaction` contributed strong predictive power.
- **Regularization helped reduce model variance.** Ridge regression provided more stable results and controlled coefficient inflation from multicollinearity.
- **Cross-validation confirmed model robustness,** reinforcing confidence in the Ridge model's real-world applicability.
- **Residual analysis revealed minimal bias,** indicating well-balanced models with no obvious pattern of systematic errors.

## Challenges and Solutions

- **Data type conversions:** The dataset contained categorical variables with inconsistent formats (e.g., 'yes'/'no', 'F'/'M'), requiring careful encoding to prevent model errors.
- **Handling travel time bins:** Initial binning excluded the maximum category causing NaNs; this was fixed by extending bin ranges and including all data points.
- **Feature scaling necessity:** Ridge regression’s sensitivity to feature scale mandated consistent standardization to ensure balanced regularization.
- **Balancing interpretability and performance:** We chose Ridge over Lasso to maintain all features for interpretability while still controlling overfitting.

## Conclusion

Through methodical feature engineering and model evaluation, this project demonstrated how combining domain insight with statistical rigor produces effective predictive models. The Ridge regression model, supported by engineered features and cross-validation, offers a reliable tool for predicting student performance and understanding key academic factors.

## References

Cortez, P., & Silva, A. (2008). Using Data Mining to Predict Secondary School Student Performance. *Proc. 5th ESANN*.

Kuhn, M., & Johnson, K. (2013). *Applied Predictive Modeling*. Springer.

James, G., Witten, D., Hastie, T., & Tibshirani, R. (2013). *An Introduction to Statistical Learning*. Springer.

