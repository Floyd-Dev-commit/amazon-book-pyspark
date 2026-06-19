# Amazon Books: Distributed ETL and Multi-Dimensional Analytics

## Overview

This repository contains the source code and technical documentation for analyzing the Amazon Books Reviews dataset (available at: https://www.kaggle.com/datasets/mohamedbakhet/amazon-books-reviews).

The project engineers a distributed data processing pipeline using Apache Spark to handle over 3 GB of raw e-commerce data, encompassing approximately 3 million individual review records and 212,404 unique book titles spanning from 1996 to 2014.

The primary objective is to transform large-scale, unstructured consumer feedback and nested product metadata into actionable commercial intelligence. By implementing advanced memory management techniques and distributed join mechanisms, the pipeline resolves typical out-of-memory (OOM) bottlenecks associated with big data processing.

The refined data is utilized to generate multi-dimensional strategic matrices evaluating price elasticity, publisher market share, and consumer sentiment consistency.

## Technical Architecture

* **Distributed Computing Engine:** Apache Spark (PySpark)
* **Data Manipulation & Local Processing:** Pandas, NumPy
* **Data Visualization:** Matplotlib, Seaborn
* **Development Environment:** Jupyter Notebook, Anaconda

## Key Pipeline Features

### 1. Data Engineering & ETL

* **Memory Pruning:** Aggressive initial dimensionality reduction by dropping heavy, unused string fields (e.g., image URLs, descriptions) to optimize cluster memory prior to distributed operations.
* **Distributed Broadcast Join:** Implemented an optimized broadcast join mechanism, transmitting the smaller metadata table to all worker nodes to bypass massive network shuffling when merging with the 3 million row behavioral dataset.
* **Regex Feature Engineering:** Utilized regular expressions within PySpark to parse and flatten nested stringified arrays for author and genre metadata.

### 2. Data Quality Assurance

* **Iterative Deep Cleaning:** Executed post-join validation to permanently drop anomalous records, including system-default Unix epoch dates (1970-01-01) and out-of-bound numerical reviews.
* **State Management:** Utilized Spark's `.cache()` and `.unpersist()` methods to dynamically lock validated data partitions in active memory while flushing legacy data structures.

### 3. Multi-Track Data Ingestion

* **On-Demand Text Loading:** Architected a secondary, isolated ingestion pipeline to temporarily retrieve unstructured textual data exclusively for natural language length analysis. This strategy prevented global memory exhaustion while permitting quantitative exploration of review verbosity versus peer-validated helpfulness.

### 4. Advanced Analytics & Visualization

* **Strategic Positioning Matrix:** Evaluated genre profitability against consumer satisfaction across a multi-quadrant matrix.
* **Consistency Matrix (Exploded View):** Visualized average market ratings against standard deviations to identify highly polarized book categories versus reliable market standards.
* **Multi-Dimensional Profiling:** Constructed normalized radar charts to profile top genres across popularity, premiumness, reliability, and trust.

## Repository Structure

├── dataset/                    # Directory for raw CSV data (excluded via .gitignore)
├── Result/                     # Directory containing aggregated CSV outputs from Spark
├── main_2.ipynb                # Main PySpark ETL and visualization notebook
├── requirements.txt            # Python environment dependencies
└── README.md                   # Project documentation

## Setup and Installation

### 1. Clone the repository

```bash
git clone https://github.com/yourusername/your-repo-name.git
cd your-repo-name
```

### 2. Install dependencies

Ensure you have Python 3.8+ installed. Install the required Python packages using pip:

```bash
pip install -r requirements.txt
```

### 3. Apache Spark Configuration

This project requires a local installation of Apache Spark. Ensure that `SPARK_HOME` and Java dependencies are correctly configured in your system environment variables. The `findspark` library is used to locate the Spark installation at runtime.

### 4. Data Placement

Download the raw dataset from Kaggle:

https://www.kaggle.com/datasets/mohamedbakhet/amazon-books-reviews

Extract and place the files (`books_data.csv` and `books_rating.csv`) into a `dataset/` directory at the root of the project.

## Usage

Execute the Jupyter Notebook sequentially. The notebook is structured into distinct phases:

1. Environment initialization and Spark Session allocation.
2. ETL, memory pruning, and the execution of the Broadcast Join.
3. Exploratory Data Analysis (EDA) and Data Quality validation.
4. PySpark aggregations (exporting intermediate results to the `Result/` directory).
5. Pandas and Seaborn visualization rendering.
