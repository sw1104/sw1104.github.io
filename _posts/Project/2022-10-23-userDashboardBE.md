---
title: "기업 인턴십 프로젝트 - 유저 대시보드 BACKEND CODE"
excerpt: "유저 대시보드 파헤치기"

categories:
    - Project
tags:
    - [Project, wecode]

permalink: /Project/cooperation/userDashboard/backend

toc: true
toc_sticky: true

date: 2022-10-23
last_modified_at: 2022-10-23
---

# 기획

기업 인턴십 프로젝트에서 내가 맡은 부분 중 하나인 유저 대시보드에는 `보유한 토큰 수량`이 나오고 관리자에게 `포인트를 토큰으로 교환 요청하는 버튼`과 `가진 포인트와 획득 버튼` 그리고 토큰을 이용해서 구매할 수 있는 `상품`과 마지막으로 `상품 구매 내역`을 구현하기로 기획을 하고 시작을 했다.

# 구현 사항

## 지갑 생성

```javascript
// service에서 먼저 지갑의 유무를 판단한 뒤 지갑을 생성한다.
const createWallet = async (userId) => {
    const duplicateWallet = await userDashboardDao.duplicateWallet(userId);

    if (Object.values(duplicateWallet[0])[0] === "1")
        return await userDashboardDao.getAllToken(userId);

    await userDashboardDao.createWallet(userId);

    return await userDashboardDao.getAllToken(userId);
};
// wallets 테이블에 로우 생성시 userId를 제외한 모든 항목은 0으로 설정해준다.
const createWallet = async (userId) => {
    return await AppDataSource.query(
        `
        INSERT INTO wallets (
            user_id, all_token, add_token, use_token, stack_token, collect_token
        ) VALUES (?, 0, 0, 0, 0, 0)
        `,
        [userId]
    );
};
```

---

## 포인트와 등급 및 상품 데이터

```javascript
const getPoint = async (userId) => {
    return await AppDataSource.query(
        `
        SELECT
            id as userId,
            point
        FROM users
        WHERE id = ${userId}
        `
    );
};

const getGrade = async (userId) => {
    return await AppDataSource.query(
        `
        SELECT
            id userId,
            grade_id gradeId
        FROM users
        WHERE id = ${userId}
        `
    );
};

const getProducts = async () => {
    return await AppDataSource.query(
        `
        SELECT
            id productId,
            name productName,
            price token
        FROM products
        `
    );
};
```

---

## 유저 토큰 사용 내역

updated_at을 이용하여 내림차순으로 정렬해서 구현하였다.

```javascript
const getTokenUseHistory = async (userId) => {
    return await AppDataSource.query(
        `
        SELECT 
            o.user_id, 
            o.product_id,
            products.name, 
            products.price, 
            o.updated_at 
        FROM orders o 
        INNER JOIN users u ON u.id = o.user_id 
        INNER JOIN products ON products.id = o.product_id 
        WHERE u.id = ${userId}
        ORDER BY o.updated_at DESC
        `
    );
};
```

## 상품 구매 및 등급 조정

유저가 보유한 토큰을 이용하여 상품을 구매하면 사용한 토큰 수량이 누적되어 등급을 조정한다.

```javascript
const buyProduct = async (userId, productId) => {
    const allToken = await userDashboardDao.getAllToken(userId);
    const priceProduct = await userDashboardDao.priceProduct(productId);

    if (Object.values(allToken[0])[0] < Object.values(priceProduct[0])[0])
        throw new Error("TOKEN", 400);

    await userDashboardDao.buyProduct(userId, productId);
    await userDashboardDao.getRemainToken(userId, productId);
    const getCumulative = await userDashboardDao.getCumulativeToken(userId);
    const cumulative = Object.values(getCumulative[0])[0];
    // 등급 조정을 위한 로직
    let gradeId;
    if (userId === 24) {
        gradeId = 1;
    } else if (cumulative < 1000) {
        gradeId = 2;
    } else if (cumulative >= 1000 && cumulative < 2000) {
        gradeId = 3;
    } else if (cumulative >= 2000 && cumulative < 3000) {
        gradeId = 4;
    } else if (cumulative >= 3000) {
        gradeId = 5;
    }

    return await userDashboardDao.gradeUp(userId, gradeId);
};

// 상품을 구매하면 해당 상품의 가격만큼 토큰이 차감되고 누적 사용량에 사용한 토큰이 추가되는 쿼리
const getRemainToken = async (userId, productId) => {
    return await AppDataSource.query(
        `
        UPDATE wallets w 
            INNER JOIN users u
            ON u.id = w.user_id 
            INNER JOIN orders o
            ON o.user_id = u.id
            INNER JOIN products p
            ON p.id = o.product_id
        SET
            w.all_token = w.all_token - p.price,
            w.use_token = p.price,
            w.stack_token = w.use_token + w.stack_token
        WHERE w.user_id = ${userId} AND ${productId} = p.id
        `
    );
};
```

---

## 포인트 획득

포인트 획득 버튼을 클릭하면 10포인트씩 추가된다.

```javascript
const earnPoint = async (userId) => {
    await AppDataSource.query(
        `
        UPDATE users
        SET
            point = point + 10
        WHERE id = ${userId};
        `
    );
    return await AppDataSource.query(
        `
        SELECT
            point
        FROM users
        WHERE id = ${userId}
        `
    );
};
```

---

## 토큰 교환 요청

포인트를 토큰으로 교환하는 요청을 관리자에게 보내기 위한 로직

```javascript
const exchangeReq = async (userId) => {
    const getPoint = await userDashboardDao.getPoint(userId);
    const getAllToken = await userDashboardDao.getAllToken(userId);
    const allToken = Object.values(getAllToken[0])[0];
    const point = Object.values(getPoint[0])[1];
    const existsUser = await userDashboardDao.existsUserWH(userId);
    const existsState = await userDashboardDao.existsStateWH(userId);

    // 토큰 교환 요청은 한번에 하나만 보낼 수 있도록 제한하는 로직
    // 및 포인트가 1000 미만이면 교환 요청이 되지 않도록 하는 로직

    let user, state;

    if (Object.values(existsUser[0])[0] === "1") {
        user = Object.values(existsUser[0])[0];
        state = Object.values(existsState[0])[0];
    }

    if (point < 1000) throw new Error("LACK OF POINT", 400);
    if (state === 1) throw new Error("ONE TO ONE", 400);

    // rePoint = 교환을 요청하고 남은 포인트
    // dePoint: 1000:1로 교환 요청을 보내기 위해서 1000단위로 잘라주기 위한 로직
    // addToken: 정규표현식을 이용해 dePoint에서 0 세자리를 제거해주는 로직
    let rePoint = parseInt((point + "").split("").splice(-3).join(""));
    let dePoint = point - rePoint;
    let addToken = (dePoint + "").replace(/0{3}$/g, "") * 1;

    await userDashboardDao.initPoint(userId, rePoint);

    if (user === "1") {
        return await userDashboardDao.patchExReq(userId, addToken);
    }
    return await userDashboardDao.exchangeReq(userId, allToken, addToken);
};
```
