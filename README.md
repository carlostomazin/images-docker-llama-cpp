# images-docker-llama-cpp

### 12.6.0-devel-ubuntu22.04-make-v1 - 9.39 GB
```bash
docker build --progress=plain --force-rm -t 12.6.0-devel-ubuntu22.04-make-v1 ./12.6.0-devel-ubuntu22.04-make-v1

docker run --rm -it --gpus all -p 8080:8080 -v .\models:/models 12.6.0-devel-ubuntu22.04-make-v1 /bin/bash
```

### 12.6.0-devel-ubuntu22.04-make-v2 - 3.73 GB
```bash
docker build --progress=plain --force-rm -t 12.6.0-devel-ubuntu22.04-make-v2 ./12.6.0-devel-ubuntu22.04-make-v2

docker run --rm -it --gpus all -p 8080:8080 -v .\models:/models 12.6.0-devel-ubuntu22.04-make-v2 /bin/bash

root@86f1a3d74fe2:/# ./llama-server --model /models/mistral-7b-instruct-v0.1.Q4_K_M.gguf --host 0.0.0.0 --port 8080 --n-gpu-layers -1
```
