# Docker 에 nginx 로 vue.js deploy 하기

```
docker run -i -t ubuntu:16.04
```

```
docker run -p 8888:80 -i -t ubuntu:16.04
```

```
docker ps -a
```

```
docker start CONTAINER_ID
docker attach CONTAINER_ID
```

#### 기존 Docker 컨테이너에 포트매핑 할당하기
1. 실행 중인 컨테이너를 멈춘다.
```
docker stop CONTAINER
```
2. 컨테이너를 커밋한다.
```
docker commit CONTAINER CONTAINER2
```
> `CONTAINER2`는 `CONTAINER`에서 생성하는 새로운 이미지 이다.
3. 새로운 이미지에서 다시 실행
```
docker run -p 8888:80 -td CONTAINER2
```
> 8888 은 로컬포트 이고, 80은 컨테이너 포트 이다.
