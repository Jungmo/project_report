### docker 리스트
```
sudo docker images
```

* image name : nvcr.io/nvidia/tritonserver
* Image id : 698a0089444e

### triton docker 실행

```
sudo nvidia-docker run --rm --shm-size=1g --ulimit memlock=-1 --ulimit stack=67108864 -p8000:8000 -p8001:8001 -p8002:8002 -v/path/to/model/repository:/models 698a0089444e trtserver --model-repository=/models
```
