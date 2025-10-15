# Goals

The goal of MLOPS is putting ml in production, this is doing good use of tools and practices, in this first part the goal is predict the duration of a taxi trip. generally the way to good MLOps is `desing -> train -> operate`. In this case is important that the model will deploy in an API, using flask and docker (this part is important, is necesary the knowledge of create a docker and API in flask - python).

# Environment preparation

for this part is required prepare a machine with the necesary to create models and create the infraestructure in docker, in the original video is used a EC2 instance with the next specs: *Ubuntu 20.04*, *x86-64* and type: *t2.xlarge (4vcpu - 16 GiB ram - 30Gb rom, $0.1856/hr)*, I recommend use **t3.xlarge ($0.1664)** in US East (N. Virginia), but depends of location of student.

I create a bash to install necesary tools (only copy-paste):

```bash
#!/bin/bash

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update && sudo apt-get upgrade -y
sudo apt-get install docker-ce docker-ce-cli containerd.io
sudo usermod -aG docker ${USER}

sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

wget https://repo.anaconda.com/archive/Anaconda3-2022.05-Linux-x86_64.sh
chmod +x Anaconda3-2022.05-Linux-x86_64.sh
./Anaconda3-2022.05-Linux-x86_64.sh
```

Copy in one file {name}.sh and give permissions of execute, is possible avoid read or multiple enters in licence of anaconda pushing `q`

Some necesaries knowledges are:
- experience with programming (python)
- being comfortable with command line
- exposure to ML is helpful

The data to use is:
- dataset used is NYC taxi
- when I start this course I only get data from 2011 and next years 

To share learning in linkedin or twitter use the #66DaysOfMLOps and #mlopszoomcamp 

# Training model

- is necesary the order in the notebook, for example:
   1. read_data
   2. train and validate data
   3. multiple ml
   4. save of model in bin
   5. the model contains experiment tracker and model registry

The steps to create the model:
- load an prepare data
- vectorize the values -> dct values
- train in different values -> model
- machine learning services, to check time of trip
- the model is deploy and before is necesary the monitoring, to check that model is effective
- exclude the human using the pipeline to create model v2

# Course overview

To note, the notebook will execute multiple times, in this step this is cleaned to use like a pipeline. Is created the function read_dataframe, later is changed because the dataframe changes from csv to parquet. The logs is very important for experimental tracker. The saved model must have a good name to know how to use, is very very important the use of registry models, like docker.

The flow: load and prepare data -> vectorize -> train, this is the codepipeline in the course, the tags is very importants for example: TRAIN_DATA = Jan 2021, VAL_DATA = Fec 2021, MODEL = LR, and execute with `python pipeline.py`, with this obtains the model this is put in machine learning service to comunicate with the user and response the time of trip, is important the monitoring, to check the performance.

A good practice is a good code, this be mainteinable, tested, clean and good documented, deployed in a docker

<img width="1024" height="1024" alt="generated-image" src="https://github.com/user-attachments/assets/62afc458-de7f-470b-bee4-730a6636a947" />


# Maturity model
Resources from: https://learn.microsoft.com/en-us/azure/architecture/example-scenario/mlops/mlops-maturity-model

# Types of maturity models

0. No MLOps
Highlights:
- Difficult to manage full machine learning model lifecycle
- The teams are disparate and releases are painful
- Most systems exist as "black boxes," little feedback during/post deployment

Technology:
- Manual builds and deployments
- Manual testing of model and application
- No centralized tracking of model performance
- Training of model is manual

1. DevOps but no MLOps
Highlights:
- Releases are less painful than No MLOps, but rely on Data Team for every new model
- Still limited feedback on how well a model performs in production
- Difficult to trace/reproduce results

Technology:
- Automated builds
- Automated tests for application code

2. Automated training(2-3 ML cases)
Highlights:
- Training environment is fully managed and traceable
- Easy to reproduce model
- Releases are manual, but low friction

Technology:
- Automated model training
- Centralized tracking of model training performance
- Model management

3. Automated Model Deployment(*)
Highlights:
- Releases are low friction and automatic
- Full traceability from deployment back to original data
- Entire environment managed: train > test > production

Technology:
- Integrated A/B testing of model performance for deployment
- Automated tests for all code
- Centralized tracking of model training performance
  
4. Full MLOps Automated Operations
Highlights:
- Full system automated and easily monitored
- Production systems are providing information on how to improve and, in some cases, automatically improve with new models
- Approaching a zero-downtime system

Technology:
- Automated model training and testing
- Verbose, centralized metrics from deployed model
