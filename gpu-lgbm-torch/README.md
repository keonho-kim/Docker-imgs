# GPU-TORCH-LIGHTGBM

<ul>
    <li> Ubuntu 22.04 (Jammy Jellyfish) 환경에서 GPU 기반의 PyTorch/LightGBM이 포함된 Docker 이미지 입니다.</li>
    <li> CUDA 환경을 포함하여 설치하므로, 반드시 Nvidia의 GPU가 포함된 머신에서 설치해주시길 바랍니다.</li>
    <li> Tensorflow를 반드시 사용하셔야 하는 경우, Container 내에서 추가적인 설치 및 설정을 진행해주세요.</li>
</ul>

## Library Included
 
:exclamation: requirements.txt를 확인해주세요.

## 설치 가이드라인
****
<div style="background-color: #dcedc8; padding: 10px;">
  <I><b>Note.</b></I> 도커를 설치하였는지 확인해주세요 &#x1F600;
</div>

<br>

1. 해당 경로로 이동합니다. 

```
cd PATH/TO/THIS/FOLDER
```

2. 도커 환경 Build (buildx 사용)
```sh
docker buildx build . --platform=linux/amd64 --shm-size=512m --tag=gpu-torch-lgbm --progress=plain -f Dockerfile
```

3. 컨테이너 생성
```sh
docker run -dit --shm-size=512m --name=gpu-torch-lgbm -p=8888:8888 -e JUPYTER_ENABLE_LAB=yes gpu-torch-lgbm
```

4. 컨테이너 실행
```sh
docker exec -it gpu-torch-lgbm /bin/bash
```
- Docker에서 exit로 빠져나오더라도 항상 Background에서 작동합니다.

<br>
5. 도커 재시작하기 / 멈추기
- 컴퓨터를 재부팅 하셨을 때에는 Docker Desktop을 시작하신 뒤, 다음의 명령어를 사용해주세요.
- 만일 Background에서 실행중인 것이 마음에 안든다면, 아래의 명령어로 중지하시면 됩니다.

```sh
# Docker 재시작하기
docker restart gpu-torch-lgbm

# Docker Container 종료하기
docker stop gpu-torch-lgbm
```

---
## Jupyter 사용하기

- VSCode에 Docker Extension이 있다면, VSCode가 가장 편리합니다.
- Jupyter Lab 또는 Notebook 환경이 편리하시다면, Local에서 다음의 주소으로 접속하시면 됩니다.
- GCP, AWS, Azure 외 원격 환경 내에서 진행하시는 경우, Port Forwarding 등 별도의 작업을 진행해주시길 바랍니다.
    
    ```
    # Jupyter Lab
    http://0.0.0.0:8888/lab
    ```

    ```
    # Jupyter Notebook
    http://0.0.0.0:8888/tree
    ```

- Token을 입력하라는 창이 뜬다면, 컨테이너에 진입하여 다음의 명령어를 실행해주세요.
    ```
    jupyter server list | grep -oP 'token=\K[^:]+'
    ```

    다음과 같은 Token값이 출력됩니다. 복사하여 입력해주시면 됩니다.
    ```
    2678c19a86166ca28fc405aceabca4626cbeb61be0e5ceac
    ```
    
