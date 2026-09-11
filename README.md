# Data-Cleaning-Structural-Validation
# ============================================================
# DATA CLEANING & STRUCTURAL VALIDATION
# Using Python (Pandas)
# ============================================================

import pandas as pd
import numpy as np

# ------------------------------------------------------------
# 1. LOAD THE DATASET
# ------------------------------------------------------------

# Change this to your dataset file name
file_name = "sample_dataset.csv"

df = pd.read_csv(file_name)

print("Dataset loaded successfully!")
print("-" * 60)


# ------------------------------------------------------------
# 2. BASIC DATA INSPECTION
# ------------------------------------------------------------

print("FIRST 5 ROWS:")
print(df.head())

print("\nLAST 5 ROWS:")
print(df.tail())

print("\nDATASET SHAPE:")
print("Rows:", df.shape[0])
print("Columns:", df.shape[1])

print("\nCOLUMN NAMES:")
print(df.columns.tolist())

print("\nDATA TYPES:")
print(df.dtypes)

print("\nDATASET INFORMATION:")
print(df.info())

print("\nSTATISTICAL SUMMARY:")
print(df.describe(include="all"))

print("-" * 60)


# ------------------------------------------------------------
# 3. CHECK MISSING VALUES
# ------------------------------------------------------------

print("MISSING VALUES BEFORE CLEANING:")

missing_values = df.isnull().sum()

print(missing_values)

print("\nMissing percentage:")
missing_percentage = (df.isnull().sum() / len(df)) * 100
print(missing_percentage.round(2))

print("-" * 60)


# ------------------------------------------------------------
# 4. CHECK DUPLICATE RECORDS
# ------------------------------------------------------------

print("NUMBER OF DUPLICATE ROWS:")

duplicate_count = df.duplicated().sum()

print(duplicate_count)

print("\nDUPLICATE RECORDS:")

if duplicate_count > 0:
    print(df[df.duplicated()])
else:
    print("No duplicate records found.")

print("-" * 60)


# ------------------------------------------------------------
# 5. REMOVE DUPLICATE RECORDS
# ------------------------------------------------------------

df = df.drop_duplicates()

print("Duplicates removed successfully.")
print("New dataset shape:", df.shape)

print("-" * 60)


# ------------------------------------------------------------
# 6. STANDARDIZE COLUMN HEADERS
# ------------------------------------------------------------

# Remove spaces
# Convert column names to lowercase
# Replace spaces with underscores

df.columns = (
    df.columns
    .str.strip()
    .str.lower()
    .str.replace(" ", "_")
)

print("STANDARDIZED COLUMN NAMES:")
print(df.columns.tolist())

print("-" * 60)


# ------------------------------------------------------------
# 7. CLEAN STRING / CATEGORICAL COLUMNS
# ------------------------------------------------------------

# Find columns containing text data

text_columns = df.select_dtypes(include=["object"]).columns

for column in text_columns:

    # Remove extra spaces
    df[column] = df[column].astype(str).str.strip()

    # Convert text to consistent lowercase
    df[column] = df[column].str.lower()


print("Text columns standardized successfully.")

print("-" * 60)


# ------------------------------------------------------------
# 8. HANDLE MISSING VALUES
# ------------------------------------------------------------

print("Handling missing values...")

# Numerical columns
numeric_columns = df.select_dtypes(include=["int64", "float64"]).columns

for column in numeric_columns:

    # Replace missing numerical values with median
    median_value = df[column].median()

    df[column] = df[column].fillna(median_value)


# Text / categorical columns
text_columns = df.select_dtypes(include=["object"]).columns

for column in text_columns:

    # Replace missing text values with "unknown"
    df[column] = df[column].replace("nan", np.nan)

    df[column] = df[column].fillna("unknown")


print("Missing values handled successfully.")

print("-" * 60)


# ------------------------------------------------------------
# 9. DATE FORMAT STANDARDIZATION
# ------------------------------------------------------------

# Automatically check columns that contain "date"

date_columns = [
    column for column in df.columns
    if "date" in column.lower()
]

for column in date_columns:

    df[column] = pd.to_datetime(
        df[column],
        errors="coerce"
    )

    # Convert to standard YYYY-MM-DD format
    df[column] = df[column].dt.strftime("%Y-%m-%d")


print("Date formats standardized.")

print("-" * 60)


# ------------------------------------------------------------
# 10. VALIDATE DATA TYPES
# ------------------------------------------------------------

print("DATA TYPES AFTER CLEANING:")

print(df.dtypes)

print("-" * 60)


# ------------------------------------------------------------
# 11. CHECK MISSING VALUES AFTER CLEANING
# ------------------------------------------------------------

print("MISSING VALUES AFTER CLEANING:")

print(df.isnull().sum())

print("-" * 60)


# ------------------------------------------------------------
# 12. CHECK DUPLICATES AFTER CLEANING
# ------------------------------------------------------------

print("DUPLICATES AFTER CLEANING:")

print(df.duplicated().sum())

print("-" * 60)


# ------------------------------------------------------------
# 13. CHECK FOR EMPTY STRINGS
# ------------------------------------------------------------

print("EMPTY STRING CHECK:")

for column in df.columns:

    empty_count = (df[column] == "").sum()

    if empty_count > 0:
        print(column, ":", empty_count)


# ------------------------------------------------------------
# 14. FINAL DATASET PREVIEW
# ------------------------------------------------------------

print("\nFINAL CLEANED DATASET:")
print(df.head(10))

print("\nFINAL SHAPE:")
print(df.shape)

print("-" * 60)


# ------------------------------------------------------------
# 15. EXPORT CLEANED DATASET
# ------------------------------------------------------------

output_file = "cleaned_dataset.csv"

df.to_csv(
    output_file,
    index=False
)

print("Cleaned dataset exported successfully!")
print("File:", output_file)

print("-" * 60)


# ------------------------------------------------------------
# 16. FINAL VALIDATION REPORT
# ------------------------------------------------------------

print("\n")
print("=" * 60)
print("        DATA CLEANING VALIDATION REPORT")
print("=" * 60)

print("Total Rows          :", df.shape[0])
print("Total Columns       :", df.shape[1])
print("Duplicate Rows      :", df.duplicated().sum())
print("Total Missing Values:", df.isnull().sum().sum())

print("\nColumn Data Types:")
print(df.dtypes)

print("\nColumn Names:")
print(df.columns.tolist())

print("=" * 60)

print("\nDATA CLEANING COMPLETED SUCCESSFULLY!")