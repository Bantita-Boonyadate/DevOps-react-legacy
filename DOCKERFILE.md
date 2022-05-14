# DockerFile 	:whale:
แบ่งออกเป็น 3 อย่าง ได้แก่ 1.development  2.build  3.production

## Development
```dockerfile
FROM node:16-alpine AS development
ENV NODE_ENV development
WORKDIR /app
COPY package.json ./
COPY yarn.lock ./
RUN yarn
COPY . ./
EXPOSE 3000
CMD [ "yarn", "start" ]
```

1. ใช้ base image เป็น `node16-alpine` ตั้งชื่อ stage ให้เป็น development (ใช้ alpine ด้วยเพราะต้องการให้มันลงเฉพาะเท่าที่จำเป็นต้องใช้)
2. ตั้งค่า Environment Variable ให้ `NODE_ENV` เป็น `development`
3. สั่งให้ Work Directory ที่ `/app`
4. `copy package.json` ไปยัง `./`
5. `copy yarn.lock` ไปยัง `./`
6. ใช้คำสั่ง `yarn` 
7. `copy ไฟล์ทั้งหมด` ไปยัง `./`
8. ตั้งให้เวลาที่ต้องการเข้าถึง image ตัวนี้ให้เข้าผ่าน port 3000
9. รันคำสั่งเพื่อ start app


## Builder
```dockerfile
FROM node:16-alpine AS builder
ENV NODE_ENV production
WORKDIR /app
COPY package.json ./
COPY yarn.lock ./
RUN yarn install --production
COPY . ./
RUN yarn build
```

1. ใช้ base image เป็น `node16-alpine` ตั้งชื่อ stage ให้เป็น builder
2. ตั้งค่า Environment Variable ให้ `NODE_ENV` เป็น production
3. สั่งให้ Work Directory ที่ `/app`
4. `copy package.json` ไปยัง `./`
5. `copy yarn.lock` ไปยัง `./`
6. ใช้คำสั่ง `yarn install --production` เพื่อติดตั้ง dependencies โดยจะติดตั้งเฉพาะ production dependencies
7. `copy` ไฟล์ทั้งหมดไปยัง `./`
8. สั่งให้มัน build

## Production
```dockerfile
FROM nginx:1.21.0-alpine AS production
COPY --from=builder /app/build /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

1. ใช้ base image เป็น `nginx:1.21.0-alpine` ตั้งชื่อ stage ให้เป็น production
2. ทำการใช้ multi-stage โดยการใช้คำสั่ง `--from=builder` เพื่อ copy image ที่เคยสร้างไว้จาก stage ทีชื่อ builder มาใช้
3. `copy nginx.conf` มาไว้ที่ `/etc/nginx/conf.d/default.conf`
4. สั่งให้มัน expose ที่ port 80
5. ใช้คำสั่งเพื่อ start nginx


## ไฟล์เพิ่มเติม
* มีการเพิ่มไฟล์ **.dockerignore** เพื่อสั่งว่าสิ่งไหนบ้างที่เราจะไม่เอาไปรวมกับ context เวลาที่เรา build 

* ในส่วนของ nginx จะต้องมีการเพิ่มไฟล์ ชื่อ **nginx.conf** เพื่อตั้งค่า config ของตัว nginx 
