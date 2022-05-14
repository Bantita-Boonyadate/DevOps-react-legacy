# Github Actions CI 🧩
## Setting Up
``` 
on:
  push:
    branches: [ develop ]
  pull_request:
    branches: [ develop ] 
````
เมื่อมีการ push codeและpull code ให้CIไปเรียกที่ branch developเสมอ
## Environment Setup
```
env:
  DOCKER_USER: ${{secrets.DOCKER_USER}}
  DOCKER_PASSWORD: ${{secrets.DOCKER_PASSWORD}}
  REPO_NAME: devops-docker-hub
```
ตั้งค่า docker usernameและdocker passwordเป็นsecretโดยเก็บค่าไว้ในsetting secretsในgithub repositoryและเรียกใช้ค่าผ่านคำสั่ง`${{secrets.DOCKER_USER}}`และ`${{secrets.DOCKER_PASSWORD}}`และตั้งค่าชื่อrepositoryเป็นชื่อrepositoryที่สร้างไว้บนdocker hub

## Job
Jobคือworkflowที่บอกว่าCIนี้การทำงานอะไรบ้างแบ่งออกเป็น 2 jobได้แก่ buildและpush image to docker hub

### `build`
```
build:
    runs-on: ubuntu-latest 
    container: node:16
    steps:
      name: Check code 
        uses: actions/checkout@v3 

      name: Install dependency
        run: yarn
```
Job build มีหน้าที่คือ install dependencies ที่ต้องใช้ในProject
### `push-image-to-docker-hub`
```
 push-image-to-docker-hub:
    needs:  build 
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Docker login
        run: docker login -u $DOCKER_USER -p $DOCKER_PASSWORD
      - name: Pull code
        run: docker pull $DOCKER_USER/$REPO_NAME:latest
      - name: Build the Docker image 
        run: docker build . --file Dockerfile --tag $DOCKER_USER/$REPO_NAME:latest
      - name: Docker Push
        run: docker push $DOCKER_USER/$REPO_NAME:latest
```
Job push-image-to-docker-hub สร้างขึ้นเพื่อให้CI push imageที่ได้จากการbuild docker imageขึ้นบนdocker hub <br />
โดยมีขั้นตอนการทำงานดังนี้ <br />
1. `needs:  build` รอให้ job buildทำงานเสร็จก่อน job push-image-to-docker-hubจึงเริ่มทำงานต่อ<br />
2. `runs-on: ubuntu-latest` สั่ง run บน ubuntu versionล่าสุด<br />
3. `actions/checkout@v2` เพื่อcheckoutไปที่repositoryของเรา<br />
4. `run: docker login -u $DOCKER_USER -p $DOCKER_PASSWORD` Login dockerโดยเอาusernameและpasswordมาจากsecretsที่เราsetupไว้ในgithub repository<br />
5. `run: docker pull $DOCKER_USER/$REPO_NAME:latest` Pull codeล่าสุดจากrepositoryบนdocker hubลงมา<br />
6. `run: docker build . --file Dockerfile --tag $DOCKER_USER/$REPO_NAME:latest` สั่งbuild docker imageจากdockerfileและใช้tagเป็นlatest<br />
7. `docker push $DOCKER_USER/$REPO_NAME:latest` Push docker imageที่buildเสร็จแล้วขึ้นไปบนdocker hub