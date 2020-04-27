[![violetwee](https://circleci.com/gh/violetwee/operationalize_ml_microservice.svg?style=svg)](https://app.circleci.com/pipelines/github/violetwee/operationalize_ml_microservice?branch=master)

## Project Overview

In this project, we operationalize a Machine Learning Microservice API using Docker, Kubernetes and CircleCI.

Given a pre-trained, `sklearn` model that has been trained to predict housing prices in Boston according to several features, such as average rooms in a home and data about highway access, teacher-to-pupil ratios, and so on, we operationalize a Python flask app—in a provided file, `app.py`—that serves out predictions (inference) about housing prices through API calls. You can read more about the data, which was initially taken from Kaggle, on [the data source site](https://www.kaggle.com/c/boston-housing).

This project could be extended to any pre-trained machine learning model, such as those for image recognition and data labeling.

## Repo Structure and Files

- **.circleci/config.yml**
  Validates that the project builds without errors

- **model_data/\***
  Data source used for making predictions

- **output_txt_files/\***
  Sample output after successfully making a prediction on Docker and Kubernetes

- **app.py**
  Python Flask app that serves out housing prices predictions through API calls

- **Dockerfile**
  Containerizes Flask app in Docker

- **Makefile**
  Lints Dockerfile and app.py

- **requirements.txt**
  List of dependencies to install

- **Scripts**
  - **run_docker.sh**: Build, list and run docker
  - **run_kubernetes.sh**: Run docker image with kubernetes
  - **upload_docker.sh**: Uploads docker image to Docker Hub. Replace dockerpath value to upload to your own personal Docker Hub
  - **make_prediction.sh**: Sends a POST request with a set of fixed parameters to Flask API (for testing purposes)

## Setup the Environment

- Create a virtualenv and activate it
- Run `make install` to install the necessary dependencies

### Running `app.py`

1. Standalone: `python app.py`
2. Run in Docker: `./run_docker.sh`
3. Run in Kubernetes: `./run_kubernetes.sh`

### Kubernetes Steps

- Setup and Configure Docker locally
- Setup and Configure Kubernetes locally
- Create Flask app in Container
- Run via kubectl
