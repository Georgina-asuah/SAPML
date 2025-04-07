# SAPML

# Optimizing SAP ML-based Anomaly Detection via Custom API Integration

This repository contains the implementation of a machine learning (ML)-powered anomaly detection system integrated into SAP HANA Fiori through a custom FastAPI. 
The solution leverages the Local Outlier Factor (LOF) algorithm for real-time anomaly detection, deployed on Azure Kubernetes Service (AKS) for scalability and high availability. 
Below is a detailed overview of the project.

---

## Overview
The project enhances SAP HANA's anomaly detection capabilities by integrating a custom ML model via a FastAPI-based RESTful API. Key objectives include:
- Overcoming limitations of SAP's Predictive Analysis Library (PAL) through customizable ML integration.
- Enabling real-time decision-making with low-latency anomaly detection.
- Ensuring scalability and security via Azure cloud deployment.

---

# Key Features
- **Anomaly Detection**: Uses scikit-learn's LOF algorithm for identifying outliers in transactional sales data.
- **Custom API**: FastAPI backend for seamless communication between SAP Fiori and ML models.
- **Cloud Deployment**: Containerized using Docker and deployed on AKS for scalability.
- **SAP Integration**: Real-time interaction via OData services and SAP Fiori web interface.
- **Security**: HTTPS encryption, token-based authentication, and role-based access control.

---

# Model Selection
The LOF algorithm was selected after evaluating four unsupervised models on a dataset of 5,000 sales transactions (12 features, split 80-20 for training/testing):

| Model               | Accuracy | Precision | Recall | F1-Score | Execution Time |
|---------------------|----------|-----------|--------|----------|----------------|
| **Local Outlier Factor** | **0.994**    | 0.926     | **0.962** | **0.943** | **0.10s**      |
| Isolation Forest    | 0.990     | 0.938     | 0.865  | 0.900     | 0.37s          |
| Robust Covariance   | 0.989     | 0.902     | 0.884  | 0.893     | 1.20s          |
| One-Class SVM       | 0.973     | 0.712     | 0.808  | 0.757     | 0.17s          |

**Selection Rationale**:
- **LOF** outperformed others in recall (96.2%) and speed (0.10s), critical for minimizing false negatives in fraud detection and system monitoring.
- Hyperparameters: `n_neighbors=80`, `contamination=0.05`, `metric='manhattan'`, `novelty=True`.

---
## API Development
### FastAPI Implementation
- **Endpoints**: RESTful endpoints for real-time inference, returning JSON-formatted predictions.
- **Asynchronous Processing**: Ensures low latency under high traffic.
- **Automated Documentation**: OpenAPI/Swagger support for endpoint testing.

### Containerization
- **Docker**: Encapsulates the FastAPI application and dependencies for consistent deployment.
- **Azure Container Registry (ACR)**: Manages version-controlled Docker images with vulnerability assessments.

---
## Integration with SAP HANA Fiori
1. **Data Flow**:
   - SAP HANA preprocesses data and exposes it via OData services.
   - FastAPI fetches data, runs LOF inference, and returns anomaly scores to SAP Fiori.

2. **UI Configuration**:
   - **Service Destination**: Configured in SAP HANA Cloud Platform Cockpit to link Fiori with the API.
   - **OData Extension**: Defines input parameters (e.g., transaction IDs, amounts) and response structures.
   - **Role-Based Dashboards**: Displays anomaly scores dynamically in Fiori’s interactive interface.

3. **Deployment Artifacts**:
   - `mta.yaml`: Assigns user-provided service credentials.
   - `xs-app.json`: Routes API calls within SAP Fiori.

---

## Deployment
### Azure Infrastructure
- **Azure Kubernetes Service (AKS)**: Orchestrates scalable container deployment with auto-scaling.
- **Security**:
  - **HTTPS Encryption**: Secures data in transit.
  - **Azure Active Directory (AAD)**: Token-based authentication for API access.
- **CI/CD Pipelines**: Automated updates via ACR integration.

---

## Getting Started
### Prerequisites
- SAP HANA instance with transactional sales data.
- Azure account with AKS and ACR access.
- Python 3.8+, Docker, and SAP Fiori development tools.
