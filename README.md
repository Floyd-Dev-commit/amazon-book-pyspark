# Amazon Books: Distributed ETL and Multi-Dimensional Analytics

## Overview
This repository contains the source code and technical documentation for analyzing the Amazon Books Reviews dataset[cite: 2]. The project engineers a distributed data processing pipeline using Apache Spark to handle over 3 GB of raw e-commerce data, encompassing approximately 3 million individual review records and 212,404 unique book titles spanning from 1996 to 2014[cite: 2]. 

The primary objective is to transform large-scale, unstructured consumer feedback and nested product metadata into actionable commercial intelligence. By implementing advanced memory management techniques and distributed join mechanisms, the pipeline resolves typical out-of-memory (OOM) bottlenecks associated with big data processing[cite: 1, 2]. The refined data is utilized to generate multi-dimensional strategic matrices evaluating price elasticity, publisher market share, and consumer sentiment consistency[cite: 1, 2].

## Technical Architecture
* **Distributed Computing Engine:** Apache Spark (PySpark)[cite: 2]
* **Data Manipulation & Local Processing:** Pandas, NumPy[cite: 2]
* **Data Visualization:** Matplotlib, Seaborn[cite: 1, 2]
* **Development Environment:** Jupyter Notebook, Anaconda[cite: 2]

## Key Pipeline Features

### 1. Data Engineering & ETL
* **Memory Pruning:** Aggressive initial dimensionality reduction by dropping heavy, unused string fields (e.g., image URLs, descriptions) to optimize cluster memory prior to distributed operations[cite: 1, 2].
* **Distributed Broadcast Join:** Implemented an optimized broadcast join mechanism, transmitting the smaller metadata table to all worker nodes to bypass massive network shuffling when merging with the 3 million row behavioral dataset[cite: 1, 2].
* **Regex Feature Engineering:** Utilized regular expressions within PySpark to parse and flatten nested stringified arrays for author and genre metadata[cite: 1, 2].

### 2. Data Quality Assurance
* **Iterative Deep Cleaning:** Executed post-join validation to permanently drop anomalous records, including system-default Unix epoch dates (1970-01-01) and out-of-bound numerical reviews[cite: 1, 2].
* **State Management:** Utilized Spark's `.cache()` and `.unpersist()` methods to dynamically lock validated data partitions in active memory while flushing legacy data structures.

### 3. Multi-Track Data Ingestion
* **On-Demand Text Loading:** Architected a secondary, isolated ingestion pipeline to temporarily retrieve unstructured textual data exclusively for natural language length analysis[cite: 1, 2]. This strategy prevented global memory exhaustion while permitting quantitative exploration of review verbosity versus peer-validated helpfulness[cite: 1, 2].

### 4. Advanced Analytics & Visualization
* **Strategic Positioning Matrix:** Evaluated genre profitability against consumer satisfaction across a multi-quadrant matrix[cite: 1, 2].
* **Consistency Matrix (Exploded View):** Visualized average market ratings against standard deviations to identify highly polarized book categories versus reliable market standards[cite: 1, 2].
* **Multi-Dimensional Profiling:** Constructed normalized radar charts to profile top genres across popularity, premiumness, reliability, and trust[cite: 1, 2].

## Repository Structure
```text
├── dataset/                    # Directory for raw CSV data (excluded via .gitignore)
├── Result/                     # Directory containing aggregated CSV outputs from Spark
├── main_2.ipynb                # Main PySpark ETL and visualization notebook
├── requirements.txt            # Python environment dependencies
└── README.md                   # Project documentation
