# mlflow-models-deploy-docker

Prebuilt docker image for deploying models via **MLflow** in docker container.


# Build


```bash
docker build -t  mlflow_models_deploy_docker .
```


or - if you want to push image to Docker registry

```bash
docker build -t  <username/mlflow_models_deploy_docker> .
docker push <username/mlflow_models_deploy_docker>
```

# Run

## With local models storage

```bash
docker run -v /path/to/model/on/host:/path/to/model/in/docker \
           -p <port>:<port> \
           mlflow_models_deploy_docker \
           /bin/bash -c \
           "mlflow models serve --no-conda -m /path/to/model/in/docker  --host 0.0.0.0 --port <port> --workers <gunicorn_workers>"
```

## With remote models storage, for example gs://:

```bash
docker run -v /host/path/to/google_credentials.json:/docker/path/to/google_credentials.json \
           -p <port>:<port> \
           mlflow_models_deploy_docker \
           /bin/bash -c \
           "export GOOGLE_APPLICATION_CREDENTIALS=/docker/path/to/google_credentials.json && mlflow models serve --no-conda -m <model_uri_in_gs> --host 0.0.0.0 --port <port> --workers <gunicorn_workers>"
```

# Predicting

```bash
curl -X POST http://localhost:5000/invocations -H 'Content-Type: application/json; format=pandas-records' -d '[[1,2,3,4,...]]'
```

