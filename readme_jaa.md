# Instalation

Anaconda

```bash
conda create -n mlflow-deploy python=3.12
conda activate mlflow-deploy
```
Instalar Microsoft Visual C++ Build Tools:
https://visualstudio.microsoft.com/visual-cpp-build-tools/
 "Desarrollo de escritorio con C++"

```bash
pip install "mlflow[mlserver]"
pip install --upgrade pip setuptools wheel

set MLFLOW_TRACKING_URI=http://127.0.0.1:5000

# En este caso, los artefactos se almacenarán en la carpeta predeterminada (normalmente ./mlruns en el directorio donde inicias el servidor).
mlflow ui --port 5001

# Comando a ejecutar en la terminal (Linux)
docker build --tag mlflow-model-test .
# Comando a ejecutar en la terminal de Docker Desktop (Windows)
docker build --tag mlflow-model-test path/to/your/dockerfile-directory

# Es para permitir la conexión a http://host.docker.internal:5001
# La opción --host 0.0.0.0 permite que MLflow acepte conexiones desde cualquier IP, incluyendo las provenientes de contenedores Docker.
mlflow server --host 0.0.0.0 --port 5001

# En Docker para Windows, host.docker.internal permite que los contenedores accedan al host
# Probé desde dentro del contenedor:
curl http://host.docker.internal:5001
# respuesta: "<!doctype html><html lang="en"><head><meta charset="utf-8"/><meta name="viewport" .."

# Servir el modelo (desde fuera)
mlflow models serve -m runs:/1ff9413979f24e8980435652ac45ab97/model -p 1234 --enable-mlserver --no-conda
# INFO:waitress:Serving on http://127.0.0.1:1234


# comando original no me funciona por la lista
curl -X POST -H "Content-Type:application/json" --data '{"inputs": [[14.23, 1.71, 2.43, 15.6, 127.0, 2.8, 3.06, 0.28, 2.29, 5.64, 1.04, 3.92, 1065.0]]}' http://127.0.0.1:1234/invocations

# éste funciona!
# atento a las '', revisar el .json
echo '{"inputs": [[14.23, 1.71, 2.43, 15.6, 127.0, 2.8, 3.06, 0.28, 2.29, 5.64, 1.04, 3.92, 1065.0]]}' > payload.json
curl -X POST -H "Content-Type:application/json" -d @payload.json http://127.0.0.1:1234/invocations
# {"predictions": [-0.08464178722346283]}


```
