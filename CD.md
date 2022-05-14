# **Docker Compose File :rainbow:**

แยกเป็น 2 ไฟล์ ได้แก่ docker-compose.dev.yml และ docker-compose.prod.yml

## docker-compose.dev.yml

```
version: "3.8"

services:
  app:
    container_name: app-dev
    image: app-dev
    build:
      context: .
      target: development
    volumes:
      - ./src:/app/src
    ports:
      - 3000:3000
```

1. `version: "3.8"` เลือกใช้ compose file version 3.8
2. services เป็นการบอกว่าเราจะใช้กี่ container ในโปรเจกต์นี้ใช้ 1 container คือ app
    * `container_name: app-dev` เป็นการระบุชื่อ container
    * `image: app-dev` เป็นการระบุชื่อ image
3. build เป็นการเรียกใช้ image ที่สร้างจาก Dockerfile 
4. `context: .` เป็นการเรียก path ของ Dockerfile เพื่อใช้ในการสร้าง container
5. `target: development` จะไปเรียกที่ development ใน Dockerfile
6. `./src:/app/src` volumes คือการ map path ของ host เข้ากับ path ใน container ใช้เพื่อเก็บ source code ใน container และสามารถแก้ไขโค้ดได้ sync การเปลี่ยนแปลงของเรากับ container
7. `3000:3000` กำหนด port mapping ระหว่าง host กับ container โดย port 3000 host ให้ไปยัง port 3000 ใน container

## docker-compose.prod.yml

```
version: "3.8"

services:
  app:
    container_name: app-prod
    image: app-prod
    build:
      context: .
      target: production
```

1. `version: "3.8"` เลือกใช้ compose file version 3.8
2. services เป็นการบอกว่าเราจะใช้กี่ container ในโปรเจกต์นี้ใช้ 1 container คือ app
    * `container_name: app-prod` เป็นการระบุชื่อ container
    * `image: app-prod` เป็นการระบุชื่อ image
3. build เป็นการเรียกใช้ image ที่สร้างจาก Dockerfile
4. ` context: .` เป็นการเรียก path ของ Dockerfile เพื่อใช้ในการสร้าง container
5. `target: production` จะไปเรียกที่ production ใน Dockerfile