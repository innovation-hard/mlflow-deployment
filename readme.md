
# Instalation
```bash
conda create -n mlflow-deployment python=3.12
conda activate mlflow-deployment
pip install "mlflow[mlserver]"
pip install google-cloud-storage
```

# Env Vars
GOOGLE_APPLICATION_CREDENTIALS=/code/credentials.json
MLFLOW_TRACKING_USERNAME=usuario
MLFLOW_TRACKING_PASSWORD=password
MLFLOW_TRACKING_URI=http://127.0.0.1:5000


# Correr server
```bash
mlflow ui --port 5001

mlflow ui --port 5001 --artifacts-destination gs://infoxel-ml-ops/mlflow_repository
```

# Servir modelo
```bash
mlflow models serve -m runs:/ee548ee836bc4bb49df6f1021d7e3c39/model -p 1234 --enable-mlserver --no-conda
mlflow models serve -m runs:/ee548ee836bc4bb49df6f1021d7e3c39/model -p 1234 --env-manager conda
```

https://mlflow.org/docs/latest/deployment/deploy-model-locally.html#serving-frameworks

```bash
mlflow models serve -m runs:/123813544bb94f3082e421c83ba7def8/model -p 1234 --no-conda
```

 ```bash
 curl -X POST -H "Content-Type:application/json" --data '{"inputs": [[14.23, 1.71, 2.43, 15.6, 127.0, 2.8, 3.06, 0.28, 2.29, 5.64, 1.04, 3.92, 1065.0]]}' http://127.0.0.1:1234/invocations
 ```

# Build docker from scratch
Ver Dockerfile
github mlflow: https://github.com/mlflow/mlflow/blob/master/mlflow/models/docker_utils.py#L68

```bash
docker build --tag mlflow-model-test .

export GOOGLE_APPLICATION_CREDENTIALS_PATH=/code/
docker run -it --env-file .env  -v $GOOGLE_APPLICATION_CREDENTIALS_PATH:/home/code/ -p 1234:1234 mlflow-model-test /bin/bash 
docker run -it mlflow-model-test /bin/bash 

mlflow models serve -m runs:/40a3c892a14d47e487982af67950c562/model -p 1234 -h 0.0.0.0 --enable-mlserver --env-manager local


docker run -it --env-file .env  -v $GOOGLE_APPLICATION_CREDENTIALS_PATH:/home/code/ -p 1234:1234 mlflow-model-test /bin/bash -c 'mlflow models serve -m runs:/40a3c892a14d47e487982af67950c562/model -p 1234 -h 0.0.0.0 --enable-mlserver --env-manager local'
```


# Build docker with mlflow
```bash
export MLFLOW_TRACKING_URI=http://localhost:5001
mlflow models build-docker -m runs:/<run_id>/<artifact_path> -n <image_name> --enable-mlserver

mlflow models build-docker -m runs:/bd66b2d80c1a47e89a6be7d17104a8a7/model -n mlflow-wine-classifier --enable-mlserver
```

```bash
export MLFLOW_TRACKING_URI=http://localhost:5001
docker run -p 8080:8080 mlflow-wine-classifier 
```

 # Deploy to kubernets
 `gs://infoxel-ml-ops/mlflow_repository/180333391132533584/123813544bb94f3082e421c83ba7def8/artifacts/model`


# References
https://mlflow.org/docs/latest/deployment/index.html

https://mlflow.org/docs/latest/deployment/deploy-model-to-kubernetes/index.html

https://mlflow.org/docs/latest/deployment/deploy-model-to-kubernetes/tutorial.html


# Push de docker

```bash
docker push mlflow-wine-classifier
```
