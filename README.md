# Causal Inference Analysis: Gender Pay Gap

This project performs a comprehensive causal inference analysis to investigate the causal effect of gender on salary, controlling for potential confounders such as age, education level, job title, and years of experience.

## 📋 Overview

Using causal inference methods from the DoWhy library and Double Machine Learning (DML) via EconML, this analysis estimates the Average Treatment Effect (ATE) of gender on salary. The project follows a rigorous causal inference workflow:

1. **Data Cleaning & Exploratory Data Analysis (EDA)**
2. **Causal Graph Construction (DAG)**
3. **Causal Effect Identification**
4. **Causal Effect Estimation** (multiple methods)
5. **Refutation Testing** (validation)

## 🎯 Research Question

**Does gender have a causal effect on salary, controlling for confounders?**

- **Treatment**: Gender (Male=0, Female=1)
- **Outcome**: Salary
- **Confounders**: Education Level, Years of Experience, Job Title, Age

## 📊 Dataset

- **Source**: [Kaggle - Salary Data](https://www.kaggle.com/datasets/mohithsairamreddy/salary-data)
- **Sample Size**: 1,780 observations (after cleaning)
- **Features**:
  - Age
  - Gender
  - Education Level
  - Job Title
  - Years of Experience
  - Salary

## 🔬 Methodology

### 1. Causal Graph (DAG)

The project constructs a Directed Acyclic Graph (DAG) representing assumed causal relationships:

```
Age → Years of Experience → Salary
Gender → Salary
Education Level → Job Title → Salary
Education Level → Salary
```

### 2. Estimation Methods

#### Method 1: Linear Regression (Backdoor Adjustment)
- Traditional OLS regression with backdoor adjustment
- Controls for confounders: Age, Years of Experience, Education Level, Job Title

#### Method 2: Double Machine Learning (DML)
- Uses machine learning models to estimate nuisance parameters
- More robust to model misspecification
- Employs cross-fitting to prevent overfitting bias

### 3. Refutation Tests

Three validation tests are performed:

1. **Placebo Treatment Test**: Replaces treatment with random permutation (expected effect ≈ 0)
2. **Random Common Cause Test**: Adds random confounder (effect should remain stable)
3. **Data Subset Test**: Tests on random subsets (effect should remain consistent)

## 📈 Key Findings

### Estimated Causal Effects

| Method | ATE (Average Treatment Effect) |
|--------|--------------------------------|
| Linear Regression | **-$8,502.68** |
| Double ML (EconML) | **-$6,135.06** |
| **Average** | **-$7,318.87** |

### Interpretation

- Being Female (vs Male) is associated with approximately **$7,318.87 lower salary** on average
- This effect persists after controlling for:
  - Age
  - Years of Experience
  - Education Level
  - Job Title

### Validation Results

✅ **All refutation tests passed**:
- Placebo treatment effect ≈ 0 (validates causal structure)
- Effect remains stable with random confounding
- Effect is consistent across data subsets

✅ **DML Validation**:
- 95% Confidence Interval: [-$8,799.20, -$3,470.92]
- Effect is statistically significant (CI excludes 0)
- Bootstrap validation confirms stability

## 🛠️ Requirements

### Python Packages

```python
pandas
numpy
matplotlib
seaborn
kagglehub
scikit-learn
dowhy
econml
networkx
```

### Installation

```bash
pip install pandas numpy matplotlib seaborn kagglehub scikit-learn dowhy econml networkx
```

## 🚀 Usage

1. **Open the Jupyter Notebook**:
   ```bash
   jupyter notebook CausalModel.ipynb
   ```

2. **Run all cells**:
   - The notebook will automatically download the dataset from Kaggle
   - Execute cells sequentially to reproduce the analysis

3. **Key Sections**:
   - **Data Cleaning**: Handles missing values and duplicates
   - **EDA**: Visualizations of distributions and relationships
   - **DAG Construction**: Creates causal graph with correlations
   - **Causal Analysis**: DoWhy implementation with multiple methods
   - **Refutation**: Validation tests for robustness

## 📁 Project Structure

```
casual_final/
├── CausalModel.ipynb    # Main analysis notebook
└── README.md            # This file
```

## 🔍 Technical Details

### Causal Identification

The causal effect is identified using the **backdoor criterion**:
- Adjustment set: {Age, Years of Experience, Education Level, Job Title}
- These variables block all backdoor paths from Gender to Salary

### Assumptions

1. **Unconfoundedness**: No unobserved confounders affecting both Gender and Salary
2. **Positivity**: All treatment levels have non-zero probability given confounders
3. **Causal Markov Condition**: DAG correctly represents causal relationships
4. **Stable Unit Treatment Value Assumption (SUTVA)**: No interference between units

