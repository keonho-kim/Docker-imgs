# LangChain, OpenAI, Gemini

<ul>
    <li> LangChain, Streamlit 등을 활용하여 LLM 기반 구현의 프로토타이핑을 위해 작성되었습니다.</li>
</ul>

## Library Included

<ul>
  <li><b>Models:</b> OpenAI, Google Gemini (google-generativeai), LLaMA.cpp, Whisper.cpp</li>
  <li><b>Multi-Agents:</b> Autogen, Autogen Studio, CrewAI</li>
  <li><b>Langchain:</b> langchain, langchain-core, langchain-cli, langchain-experimental, langchain-community, langchain-openai, langchain-google-genai, langgraph</li>
  <li><b>Database:</b> SQLite, PyMongo (with MongoDB), PySpark (with Apache Spark), chromaDB, DuckDB (including pyodbc)</li>
  <li><b>Vector Search:</b> faiss-cpu, annoy</li>
  <li><b>Prototyping:</b> Streamlit</li>
  <li><b>Sound:</b> ffmpeg-python, soundfile, librosa</li>
  <li><b>Utility:</b> pytube, ffmpeg</li>
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
docker buildx build . --platform=linux/arm64 --shm-size=512m --tag=llm-service-dev --progress=plain -f Dockerfile
```

3. 컨테이너 생성
```sh
docker run -dit --shm-size=512m --name=llm-service-dev -p=8888:8888 -e JUPYTER_ENABLE_LAB=yes llm-service-dev
```

4. 컨테이너 실행
```sh
docker exec -it llm-service-dev /bin/bash
```
- Docker에서 exit로 빠져나오더라도 항상 Background에서 작동합니다.

<br>
5. 도커 재시작하기 / 멈추기
- 컴퓨터를 재부팅 하셨을 때에는 Docker Desktop을 시작하신 뒤, 다음의 명령어를 사용해주세요.
- 만일 Background에서 실행중인 것이 마음에 안든다면, 아래의 명령어로 중지하시면 됩니다.

```sh
# Docker 재시작하기
docker restart llm-service-dev

# Docker Container 종료하기
docker stop llm-service-dev
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
    