# Silicon Simple ML Environ

<ul>
    <li> 실리콘 (M-시리즈) 맥북 환경에서 간단하게 데이터 분석을 수행하기 위한 환경을 구축합니다. </li>
    <li> PyTorch, Tesorflow 등을 사용하여 GPU를 사용하고자 하실 경우에는 Miniforge를 통해 직접 환경울 구성해주세요.</li>
</ul>

## Library Included

<ul>
    <li> 모델 :: Scikit-learn, XGBoost, CatBoost, LightGBM (including SHAP), imblearn</li>
    <li> AutoML :: Optuna, FLAML, PyCaret </li>
    <li> 데이터 (병렬) 처리 :: Pandas, Polars, Ray, Pandarallel </li>
    <li> EDA 및 시각화 :: sweetviz, pygwalker, matplotlib, seaborn, plotly
</ul>

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
docker buildx build . --platform=linux/arm64 --shm-size=512m --tag=ml-silicon --progress=plain -f Dockerfile
```

3. 컨테이너 생성
```sh
docker run -dit --shm-size=512m --name=ml-silicon -p=8888:8888 -e JUPYTER_ENABLE_LAB=yes ml-silicon
```

4. 컨테이너 실행
```sh
docker exec -it ml-silicon /bin/bash
```
- Docker에서 exit로 빠져나오더라도 항상 Background에서 작동합니다.

<br>
5. 도커 재시작하기 / 멈추기
- 컴퓨터를 재부팅 하셨을 때에는 Docker Desktop을 시작하신 뒤, 다음의 명령어를 사용해주세요.
- 만일 Background에서 실행중인 것이 마음에 안든다면, 아래의 명령어로 중지하시면 됩니다.

```sh
# Docker 재시작하기
docker restart ml-silicon

# Docker Container 종료하기
docker stop ml-silicon
```



---
## Jupyter 사용하기

- VSCode에 Docker Extension이 있다면, VSCode가 가장 편리합니다.
- Jupyter Lab 또는 Notebook 환경이 편리하시다면, Local에서 다음의 주소으로 접속하시면 됩니다.
    
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
    
