# 🏷️ Automated Product Taxonomy Mapper

An end-to-end product categorization system that automatically maps raw e-commerce product data to a standardized taxonomy using rule-based matching and machine learning.

---

## 📌 Project Overview

This project solves a critical business problem for retail and e-commerce companies: **automatically categorizing and tagging products at scale**.

### The Problem
E-commerce platforms have thousands of products with inconsistent naming conventions. Manual categorization is time-consuming, error-prone, and doesn't scale. Retail clients need consistent product taxonomies for search, recommendations, and analytics.

### The Solution
I built a hybrid system that:
- **Extracts** product attributes from raw product names
- **Maps** each product to a standardized product type
- **Tags** products for better discoverability
- **Flags** ambiguous products for human review

### Key Results
| Metric | Value |
| :--- | :--- |
| **Total Products** | 551,585 |
| **Rule-Based Coverage** | 392,765 (71.2%) |
| **ML-Based Coverage** | 158,820 (28.8%) |
| **Total Coverage** | **100%** |
| **ML Test Accuracy** | **93.33%** |

---

## 🗂️ Dataset

**Source:** [Amazon Products Dataset](https://www.kaggle.com/datasets/lokeshparab/amazon-products-dataset) (Kaggle)

| **Metric** | **Value** |
| :--- | :--- |
| Total Products | 551,585 |
| Main Categories | 20 |
| Sub-Categories | 112 |
| Data Points | name, main_category, sub_category, ratings, prices |

---

## 🛠️ Methodology

### 1. Data Preprocessing
Cleaned product names by converting to lowercase, removing special characters, and standardizing spacing. Created a clean column for consistent matching across all products.

### 2. Taxonomy Design
Built a **controlled vocabulary** with 9 parent categories and 68 product types:
- **Jewellery**: Ring, Bracelet, Necklace, Earrings, Bangle, Pendant, Watch
- **Clothing**: Shirt, T-Shirt, Jeans, Dress, Top, Kurta, Saree, Innerwear
- **Bags**: Handbag, Backpack, Wallet, Clutch, Tote Bag, Sling Bag
- **Footwear**: Shoes, Sandals, Slippers, Ballerinas
- **Appliances**: Air Conditioner, Refrigerator, Washing Machine, Water Heater
- **Electronics**: Television, Camera, Headphones, Speaker, Mobile Phone
- **Sports & Fitness**: Cricket Bat, Football, Yoga Mat, Resistance Band
- **Beauty & Personal Care**: Makeup, Lipstick, Shampoo, Skin Care
- **Pet Supplies**: Dog Food, Pet Toy, Pet Accessory

### 3. Rule-Based Classification
Created keyword-based rules to map product names to product types. Used **pre-compiled regex patterns** for efficient matching across 550K+ products. Handled variations like "AC" → "Air Conditioner" and "Smartwatch" → "Watch". Used existing `sub_category` as fallback evidence when name-based matching was ambiguous.

### 4. Machine Learning for Unmapped Products
Products that couldn't be mapped by rules were passed to an ML model:
- **Vectorization**: TF-IDF with 1,500 features and n-grams (1,2)
- **Model**: Logistic Regression (fast, interpretable, and effective for text)
- **Training**: 20,000 randomly sampled, already-mapped products
- **Accuracy**: 93.33% on test data

### 5. Human-in-the-Loop Review
Products that remained ambiguous were flagged for manual review. This ensures high accuracy for critical categories and provides a mechanism for continuous improvement.

---

## 📊 Results

### Rule-Based Coverage
The rule-based system successfully mapped 71.2% of all products, with the highest counts in:
- **Shoes**: 64,506 products
- **Shirts**: 39,799 products
- **Innerwear**: 30,501 products
- **Watches**: 26,771 products

### ML Model Performance
- **Test Accuracy**: 93.33%
- **Top ML Predictions**: Makeup (22,349), Top (17,364), Innerwear (16,801), Camera (16,482)

### Final Coverage
The hybrid approach achieved **100% coverage**:
- 392,765 products tagged by rules
- 158,820 products tagged by ML
- **551,585 products total**

---

## 💡 Key Learnings

### What Worked Well
- **Rule-based approach** is fast and interpretable for common patterns
- **TF-IDF + Logistic Regression** provided excellent accuracy (93.33%)
- **Hybrid approach** (rules + ML) achieved 100% coverage
- **Pre-compiled regex** made processing 550K+ products efficient

### Challenges Overcome

| Challenge | Solution |
| :--- | :--- |
| **Memory errors** with 550K products during ML training | Sampled **20,000 products** for training; processed predictions in **10,000-product chunks** |
| **Abbreviations** in product names (e.g., "AC" vs "Air Conditioner") | Added **multiple keyword variations** to matching rules |
| **Imbalanced categories** (some product types had very few samples) | Used `class_weight='balanced'` in Logistic Regression |
| **Unclear product names** with no category context | Flagged ambiguous products for **manual review** |
| **Slow performance** processing 550K+ products | Used **pre-compiled regex patterns** for efficient matching |

### Future Improvements
- **Hierarchical Classification**: Predict parent → sub-category → product type
- **BERT Fine-tuning**: Use transformer models for better text understanding
- **Real-time API**: Deploy as a microservice for on-demand product tagging
- **Confidence Scoring**: Show confidence level for each prediction

---

## 🛠️ Tech Stack

| **Category** | **Technologies** |
| :--- | :--- |
| **Data Processing** | Python, Pandas, NumPy |
| **Machine Learning** | Scikit-learn (Logistic Regression, TF-IDF) |
| **Natural Language Processing** | Regex, Text Classification |
| **Environment** | Jupyter Notebook, Google Colab |

---

## 📈 Sample Output

**Input:**
> *"Lloyd 1.5 Ton 3 Star Inverter Split AC (5 In 1 Convertible, Copper, Anti-Corrosive, 2024 Model)"*

**Output:**

| Attribute | Value |
| :--- | :--- |
| Product Type | Air Conditioner |
| Parent Category | Appliances |
| Sub-Category | Heating & Cooling |
| Attributes | Brand: Lloyd, Capacity: 1.5 Ton, Type: Inverter |

---

## 📁 Project Structure

- **`data/`**
  - `amazon_products.csv` – Raw dataset (551,585 products)

- **`notebooks/`**
  - `ecommerce_product_taxonomy_full.ipynb` – Complete analysis notebook

- **`outputs/`**
  - `amazon_products_tagged.csv` – Final tagged dataset (100% coverage)
  - `product_mapping.csv` – Quick reference: product → category
  - `product_taxonomy.json` – Taxonomy definition (9 categories, 68 types)

- **`README.md`** – Project documentation
- **`requirements.txt`** – Python dependencies
- **`LICENSE`** – MIT License
