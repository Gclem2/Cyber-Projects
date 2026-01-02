#  IDS Project - Python Environment Setup

This project is an Intrusion Detection System (IDS) prototype using Python. Below is a summary of the Python environment and key dependencies used in development.

##  Virtual Environment

A virtual environment was created using Python 3.13.5 to keep dependencies isolated from the global Python installation. The environment is located in the `.venv` folder inside the project directory.

VS Code is configured to use this environment by selecting the interpreter at:

##  Installed Packages

The following Python packages are used in this project:

- **scikit-learn**: Provides tools for building machine learning models, used here for anomaly detection.
- **numpy**: Supports numerical operations and array management required for data processing.
- **scapy**: A packet manipulation tool used for capturing and analyzing network traffic.

All dependencies are saved in the `requirements.txt` file for easy replication.

##  Notes

This project runs in an isolated Python environment and is designed to be safe for local experimentation and analysis.

