---
title: "AWS EC2에서 Docker를 이용해보자"
excerpt: "Docker를 이용한 AWS 실습"

categories:
  - AWS, Docker
tags:
  - [AWS]

permalink: /AWS/AWS EC2에서 Docker를 이용해보자/

toc: true
toc_sticky: true

date: 2022-09-15
last_modified_at: 2022-09-15
---
# EC2 RDS 준비
Docker를 이용하기 위해 테스트용도로 EC2와 RDS를 만들었다.

RDS에 접속하여 `test` db를 생성했다.

```javascript
mysql> create database 'DATABASE 명' character set utf8mb4 collate utf8mb4_unicode_ci;
```

그리고 프로젝트 환경변수를 자신의 RDS에 맞게 설정해준다.
```
# typeorm
TYPEORM_CONNECTION = mysql
TYPEORM_HOST = RDS Endpoint 주소
TYPEORM_USERNAME = root 
TYPEORM_PASSWORD = password
TYPEORM_DATABASE = DB_NAME
TYPEORM_PORT = 3306
TYPEORM_LOGGING = TRUE

# dbmate
DATABASE_URL="mysql://root:password@host:port/dbname"
```

RDS 데이터베이스에 migration해준다.

![](../../assets/images/posts_img/AWS/docker-aws/2022-09-15-dkaws1.png)

RDS의 MySQL에 접속하여 테이블이 잘 생성되었는지 확인해준다.

![](../../assets/images/posts_img/AWS/docker-aws/2022-09-15-dkaws2.png)

---

# EC2에 Docker 설치

EC2 Instance에 접속하여 다음 명령어로 docker 설치 쉘을 다운받아준다.

```
curl -fsSL https://get.docker.com -o get-docker.sh
```
`ll`을 입력하여 get-docker.sh가 정상적으로 받아졌는지 확인 후

다음 명령어로 실행해준다
```
sh get-docker.sh
```
설치가 잘 된것을 볼 수 있다.

![](../../assets/images/posts_img/AWS/docker-aws/2022-09-15-dkaws3.png)

---

# DockerHub에서 image 받아오기
앞 글에서 DockerHub에 image를 push해서 정상적으로 저장이 된 것을 확인했었다. 다음 명령어를 이용하여 EC2에 해당 image를 가져온다. 
```
docker pull username/imageName:version
```

엇 명령어를 입력하니까 다음과 같은 에러가 나타났다. 뭘까 뭐지 궁금하다. 검색해보자.

![](../../assets/images/posts_img/AWS/docker-aws/2022-09-15-dkaws4.png)

권한 문제라고 한다. 다음 명령어를 통해 권한을 수정해준다.
```
sudo chmod 666 /var/run/docker.sock 
// 뒤에 경로는 에러에서 나타낸 경로를 넣어준다.
```

그리고 다시 pull 받으면 아주아주 잘 받아진다!!

![](../../assets/images/posts_img/AWS/docker-aws/2022-09-15-dkaws5.png)

이제 docker images를 입력하여 정말로 잘 받아왔는지 확인해보면 정말로 잘 받아졌다!!

![](../../assets/images/posts_img/AWS/docker-aws/2022-09-15-dkaws6.png)

---

# 받아온 Docker image를 이용하여 Container 실행 후 서버를 키자

Container 실행은 다음 명령어를 이용하면 되고,
```
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

나는 다음과 같이 실행해주었다.

```
docker run -p 3000:3000 sangwoo1104/demo:0.1.0
```

다음과 같이 잘 작동하는 것을 볼 수 있고 Dockerfile 마지막에 서버를 실행하는걸로 작성 해놔서 서버까지 잘 실행된 것을 볼 수 있다.

![](../../assets/images/posts_img/AWS/docker-aws/2022-09-15-dkaws7.png)

# 이제 포스트맨으로 통신해보기

내 프로젝트에는 다음과 같이 서버의 상태를 체크하는 ping-pong과 유저의 회원가입 및 유저 목록을 가져오는 코드가 있다.

```javascript
// Health check
app.get("/ping", (req, res) => {
  res.status(200).json({ message: "pong" })
})

// Sign up
app.post("/users/signup", async (req, res) => {
  const { email, password } = req.body

  await myDataSource.manager.query(
    `INSERT INTO users (
      email, password
    ) VALUES (?, ?)`,
    [email, password]
  )

  res.status(201).json({ message: "created" })
})

// Get users
app.get("/users", (req, res) => {
  myDataSource.manager.query(`SELECT * FROM users`, (err, rows, fields) => {
    res.status(200).json(rows)
  })
})

```

요청 IP는 EC2 Instance Public IP를 이용해서 요청을 하면 된다.

![](../../assets/images/posts_img/AWS/docker-aws/2022-09-15-dkaws10.png)

성공적으로!! 엥?? 왜 안되는거지ㅠㅠ

![](../../assets/images/posts_img/AWS/docker-aws/2022-09-16-dkaws.gif)

뭔가 잘못됐다. 요청이 안가는걸 보니 EC2에 접속을 못하는거 같다. 

보안그룹의 인바운드 규칙을 좀 수정해주자!

EC2 목록에서 보안그룹을 누른다음에 EC2에 연결되어있는 보안그룹으로 들어가면 오른쪽 아래에 인바운드 규칙 편집이란 버튼이 있다. 

![](../../assets/images/posts_img/AWS/docker-aws/2022-09-16-dkaws2.png)

왼쪽 아래에 규칙 추가 버튼을 누른 뒤에

포트는 3000번 쓸거니까 3000으로 작성하고 완전 열어둔 다음 저장한다.

![](../../assets/images/posts_img/AWS/docker-aws/2022-09-16-dkaws3.png)

이제 혹시 모르니 상태를 먼저 보기 위해 Health check API를 먼저 실행해보자. 

제발 `pong` `pong` `pong` `pong` `pong`

![](../../assets/images/posts_img/AWS/docker-aws/2022-09-16-dkaws4.png)

`pong`이 잘 나왔다!!!!! `pong` `pong` `pong` `pong`

![](../../assets/images/posts_img/AWS/docker-aws/2022-09-16-dkaws5.png)

그러면 유저를 가입시켜보자

![](../../assets/images/posts_img/AWS/docker-aws/2022-09-16-dkaws6.png)

오 가입이 잘된다!! 여러명 가입시킨다음 출력해보자

![](../../assets/images/posts_img/AWS/docker-aws/2022-09-16-dkaws9.png)



![](../../assets/images/posts_img/AWS/docker-aws/2022-09-16-dkaws7.png)

유저목록도 아주아주 잘 불러와지는구나~~ 

![](../../assets/images/posts_img/AWS/docker-aws/2022-09-16-dkaws8.png)

# 끝!