# Import necessary libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# -------------------------
# 1. Load CSV file
# -------------------------
# Replace 'data.csv' with your file name
df = pd.read_csv("data.csv")

print("First 5 rows of dataset:")
print(df.head())

# -------------------------
# 2. Basic Data Analysis
# -------------------------
print("\nDataset Information:")
print(df.info())

print("\nSummary Statistics:")
print(df.describe())

# Example: Calculate average of a selected column (replace 'ColumnName' with your column)
column_name = "ColumnName"
if column_name in df.columns:
    avg_value = df[column_name].mean()
    print(f"\nAverage value of '{column_name}': {avg_value}")
else:
    print(f"\nColumn '{column_name}' not found in dataset.")

# -------------------------
# 3. Data Visualizations
# -------------------------

# Bar Chart (for categorical column counts)
if df.select_dtypes(include=['object']).shape[1] > 0:
    categorical_col = df.select_dtypes(include=['object']).columns[0]
    df[categorical_col].value_counts().plot(kind="bar", figsize=(7,5))
    plt.title(f"Bar Chart of {categorical_col}")
    plt.xlabel(categorical_col)
    plt.ylabel("Count")
    plt.show()

# Scatter Plot (between first 2 numeric columns)
numeric_cols = df.select_dtypes(include=['int64','float64']).columns
if len(numeric_cols) >= 2:
    plt.figure(figsize=(7,5))
    plt.scatter(df[numeric_cols[0]], df[numeric_cols[1]], alpha=0.6)
    plt.title(f"Scatter Plot: {numeric_cols[0]} vs {numeric_cols[1]}")
    plt.xlabel(numeric_cols[0])
    plt.ylabel(numeric_cols[1])
    plt.show()

# Heatmap (correlation matrix)
if len(numeric_cols) > 1:
    plt.figure(figsize=(8,6))
    sns.heatmap(df[numeric_cols].corr(), annot=True, cmap="coolwarm", fmt=".2f")
    plt.title("Correlation Heatmap")
    plt.show()

# -------------------------
# 4. Insights & Observations
# -------------------------
print("\n--- Insights ---")

# Example insights
if len(numeric_cols) > 0:
    print(f"- The dataset has {len(df)} rows and {len(df.columns)} columns.")
    print(f"- '{numeric_cols[0]}' has an average value of {df[numeric_cols[0]].mean():.2f}.")
    print(f"- Highest correlation is between columns:")
    corr_matrix = df[numeric_cols].corr()
    max_corr = corr_matrix.where(~corr_matrix.isin([1.0])).unstack().dropna().sort_values(ascending=False).head(1)
    print(max_corr)

print("\nAnalysis Completed ✅")
