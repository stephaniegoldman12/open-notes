## Getting started guide
This article describes how to deploy MLflow models for offline (batch and streaming) inference and online (real-time) serving

1. Load data
2. Training a model
3. Distributed hyperparameter tuning 
4. Model inference

Also: using ML Flow and ML Registry
Can use auto-logging to track model development
Track trainged models: ML Flow 
Hyperparameter tuning - Hyperopt with SparkTrials to scale hyperparameter tuning.

## Deployment (Research and prod envs)

### Research env
You don't just deploy the model itself, you need to deploy the entire pipeline
Most likely you'll receive the raw data, so you need all of the steps

Research - no contact to live data, research the different models. Train ML model using historical data

- Data
** Reproducible ML Pipeline: same input -> same output **
1. Feature engineering (data pre-processing): transform variables, extract features, create new features
2. Feature selection 
3. Model training and building
4. Scoring + Predictions: Without reproducibility: can't determine if you're improving with your new model vs your current one.
5. Use scikit learn `Pipeline` open source to create the pipeline
- Pass the name of the step + Class you want to use
6. Save pipeline into a pickle object with one line of code

### Production
1. Put it in a package - others can use it as a dependency but with the advantage of version control

2. Create a REST API: FastAPI and pydantic (create classes that correspond with the inputs you pass to the endpoints) 
- It's fast
- Type Validation: for example if you pass a string instead of an int, it will throw an error out of the box
- Automatically writes documentation as you create your endpoints

3. Use a logger as well

#### Deployment options
1. Heroku (PaaS, platform as a service), no container 
2. Heroku, with CI/CD and in a Docker container
Differential tests - to compare the models and understand the differences
3. IaaS (Infra as a service, so have the ability to scale) AWS Options, container orchestration engine options:
- Kubernetes (orig developed by Google) - self-managed can deploy anywhere, but installation is super complex. If at a company where you need to avoid login, and have DevOps engineers, than the best choice.
- EKS (new player, "middleground") - Amazon Elastic Container Service for Kubernetes 
- - ECS (Amazon's option) - can get demos up and running more easily thins way. Fully-managed service by AWS.
- Docker Swarm (even Docker uses Kubernetes, though)