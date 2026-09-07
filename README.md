# EXP 3 - Delhi Air Quality Analysis

### NAME   :Yogesh D
### REG NO.:212224040371


## Aim :


To compare air quality parameters in Delhi across different stations and analyze the relationship between pollutants (e.g., PM2.5 and NO₂) using scatter plots and correlation analysis.


## Procedure / Algorithm

1)Load the dataset using pandas.

2)Preprocess the data:

3)Convert the date column (period.datetimeFrom.utc) to datetime format.

4)Drop missing or invalid values.

5)Pivot the dataset so each pollutant (parameter) becomes a separate column.

6)Plot scatter plot between PM2.5 and NO₂ to study their relationship.

7)Plot correlation heatmap between all pollutants to identify relationships.

8)Interpret the results — identify which pollutants are correlated and which stations are most polluted.


## Program

### Import Libraries

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.preprocessing import MinMaxScaler, StandardScaler
```

### Load Dataset

```python
df = pd.read_csv("del-sirifort-cpcb-2024-25.csv")

print("\nDataset Loaded Successfully!")
```

### Select Variables

```python
variables = [
    "PM2.5 (µg/m³)",
    "PM10 (µg/m³)",
    "NO2 (µg/m³)",
    "Ozone (µg/m³)"
]

print("\nSelected Variables:")
print(variables)
```

### Convert Selected Columns to Numeric

```python
for col in variables:
    df[col] = pd.to_numeric(df[col], errors="coerce")

print("\nSelected columns converted to numeric successfully!")
```

### Distribution Analysis

```python
print("\n========== A. DISTRIBUTION ANALYSIS ==========")

for col in variables:
    data = df[col].dropna()

    plt.hist(data, bins=30)
    plt.xlabel(col)
    plt.ylabel("Frequency")
    plt.title("Distribution of " + col)
    plt.show()
```

### Numerical Summary

```python
print("\n========== B. NUMERICAL SUMMARY ==========")

for col in variables:
    data = df[col].dropna()

    print("\n----------------------------------------")
    print("Variable:", col)
    print("----------------------------------------")

    print("Mean:", data.mean())
    print("Median:", data.median())
    print("Mode:", data.mode().iloc[0])
    print("Minimum:", data.min())
    print("Q1:", data.quantile(0.25))
    print("Q3:", data.quantile(0.75))
    print("Maximum:", data.max())
```

### Measures of Spread

```python
print("\n========== C. MEASURES OF SPREAD ==========")

for col in variables:
    data = df[col].dropna()

    print("\n----------------------------------------")
    print("Spread:", col)
    print("----------------------------------------")

    print("Range:", data.max() - data.min())
    print("Variance:", data.var())
    print("Standard Deviation:", data.std())

    cv = (data.std() / data.mean()) * 100
    print("Coefficient of Variation:", cv, "%")
```

### Box Plot and Outlier Detection

```python
print("\n========== D. OUTLIER DETECTION ==========")

for col in variables:
    data = df[col].dropna()

    Q1 = data.quantile(0.25)
    Q3 = data.quantile(0.75)

    IQR = Q3 - Q1

    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR

    outliers = data[(data < lower) | (data > upper)]

    print("\n----------------------------------------")
    print("Outlier Analysis:", col)
    print("----------------------------------------")

    print("IQR:", IQR)
    print("Lower Limit:", lower)
    print("Upper Limit:", upper)
    print("Number of Outliers:", len(outliers))

    plt.boxplot(data)
    plt.ylabel(col)
    plt.title("Box Plot of " + col)
    plt.show()
```

### Min-Max Scaling

```python
print("\n========== E. MIN-MAX SCALING ==========")

scaler = MinMaxScaler()

for col in variables:
    data = df[[col]].dropna()

    scaled = scaler.fit_transform(data)

    print("\nMin-Max Scaling:", col)

    print("\nOriginal values:")
    print(data.head())

    print("\nScaled values:")
    print(scaled[:5])
```

### Z-Score Standardization

```python
print("\n========== F. Z-SCORE STANDARDIZATION ==========")

scaler = StandardScaler()

for col in variables:
    data = df[[col]].dropna()

    standardized = scaler.fit_transform(data)

    print("\nZ-Score Standardization:", col)

    print("\nFirst 5 standardized values:")
    print(standardized[:5])

    print("Mean:", standardized.mean())
    print("Standard Deviation:", standardized.std())
```

### Lorenz Curve and Gini Coefficient

```python
print("\n========== G. INEQUALITY ANALYSIS ==========")

for col in variables:

    data = df[col].dropna()

    # Remove negative values
    data = data[data >= 0]

    # Sort values
    x = np.sort(data.values)

    # Calculate cumulative values
    cumulative = np.cumsum(x)

    # Calculate Lorenz curve
    lorenz = np.insert(
        cumulative / cumulative[-1],
        0,
        0
    )

    # Population proportion
    population = np.linspace(
        0,
        1,
        len(lorenz)
    )

    # Calculate Gini coefficient
    gini = 1 - 2 * np.trapz(
        lorenz,
        population
    )

    print("\n----------------------------------------")
    print("Inequality Analysis:", col)
    print("----------------------------------------")

    print("Gini Coefficient:", gini)

    # Plot Lorenz Curve
    plt.plot(
        population,
        lorenz,
        label="Lorenz Curve"
    )

    # Perfect equality line
    plt.plot(
        [0, 1],
        [0, 1],
        linestyle="--",
        label="Perfect Equality"
    )

    plt.xlabel("Cumulative Proportion of Observations")
    plt.ylabel("Cumulative Proportion of Values")
    plt.title("Lorenz Curve - " + col)
    plt.legend()
    plt.show()
```

## Output

### PM2.5 Distribution

<img width="843" height="574" alt="image" src="https://github.com/user-attachments/assets/c0842010-1e3f-4572-90ba-e3be80709367" />


---

### PM10 Distribution

<img width="885" height="572" alt="image" src="https://github.com/user-attachments/assets/fead233d-09ac-43db-9011-62c6ba0d2d50" />


---

### NO₂ Distribution

<img width="928" height="573" alt="image" src="https://github.com/user-attachments/assets/8ca98fa2-9b49-4ab9-bea1-8f6779f63960" />

---

### Ozone Distribution

<img width="841" height="592" alt="image" src="https://github.com/user-attachments/assets/5d070b08-eb8c-490e-ac61-6d34a35ec875" />


---

### Numerical Summary

<img width="491" height="741" alt="image" src="https://github.com/user-attachments/assets/0f8540ca-00d5-49fa-889d-528692dbc01d" />


---

### Measures of Spread

<img width="548" height="535" alt="image" src="https://github.com/user-attachments/assets/c44e7f9d-57ac-4fec-8b9f-d26fe36bcbab" />


---

### Box Plot and Outlier Detection

<img width="655" height="1714" alt="Screenshot 2026-08-18 172224" src="https://github.com/user-attachments/assets/43cd1a56-bf7d-4c2f-be0c-0f9da24551db" />


---

### Min-Max Scaling

<img width="322" height="852" alt="Screenshot 2026-08-18 172345" src="https://github.com/user-attachments/assets/619300da-bcf5-457c-ba36-fbf6e737aceb" />


---

### Z-Score Standardization

<img width="451" height="852" alt="Screenshot 2026-08-18 172412" src="https://github.com/user-attachments/assets/77dcaab7-44b1-43f3-b1d0-5794abafc23c" />


---

### PM2.5 Lorenz Curve

<img width="774" height="649" alt="Screenshot 2026-08-18 172519" src="https://github.com/user-attachments/assets/cfa70d5a-57f7-4d86-b6e8-5cb8a78ff8ee" />


---

### PM10 Lorenz Curve

<img width="843" height="667" alt="Screenshot 2026-08-18 172542" src="https://github.com/user-attachments/assets/b41391f0-fc9f-435b-8a26-5915e29161cd" />


---

### NO₂ Lorenz Curve

<img width="774" height="664" alt="Screenshot 2026-08-18 172604" src="https://github.com/user-attachments/assets/2546e3e8-a8d5-4d7d-a5eb-cd8abdb03d99" />


---

### Ozone Lorenz Curve

<img width="851" height="674" alt="Screenshot 2026-08-18 172624" src="https://github.com/user-attachments/assets/b3481946-484f-4bc8-aa68-01b2ac62750b" />


---

## Interpretation 

1) PM2.5 and NO₂ show a strong positive correlation, suggesting that both pollutants increase together, likely due to vehicle and industrial emissions.

2) PM2.5 levels drop significantly during monsoon months, peak in winter due to stagnant air and emissions, and show higher concentrations during traffic hours, highlighting the impact of weather and human activity on pollution.

## Result

The dataset was successfully loaded and processed to extract pollutant-wise and station-wise air quality data for Delhi.


