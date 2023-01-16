---
title: "기업 인턴십 프로젝트 - 관리자 대시보드 FRONTEND CODE"
excerpt: "관리자 대시보드 파헤치기"

categories:
    - project_details
tags:
    - [project, wecode]

permalink: /project_details/cooperation/adminDashboard/frontend

toc: true
toc_sticky: true

date: 2022-10-23
last_modified_at: 2022-10-23
---

## 전체 토큰 수량

`axios`를 이용하여 구현을 했다.

```javascript
const [fullToken, setFullToken] = useState([]);

useEffect(() => {
    const getFullToken = async () => {
        const result = await axios.get(`http://127.0.0.1:8080/admin/full`, {
            headers: {
                "Content-Type": "application/json",
                accessToken: localStorage.getItem("accessToken")
            }
        });
        const user = result.data[0].fullToken;
        setFullToken(user);
    };
    getFullToken();
}, []);

<Grid item xs={12} md={6} lg={3}>
    <MDBox mb={1.5}>
        <ComplexStatisticsCard
            color="dark"
            icon="weekend"
            title="전체 토큰 갯수"
            count={fullToken}
            percentage={%raw%}{{
                color: "success",
                label: "SangWoo & JaeHa"
            }}{%endraw%}
        />
    </MDBox>
</Grid>;
```

---

## 발행된 토큰 수량

마찬가지로 `axios`를 이용하여 구현 하였다.

```javascript
const [issuedToken, setIssuedToken] = useState([]);

useEffect(() => {
    const getIssuedToken = async () => {
        const result = await axios.get(`http://127.0.0.1:8080/admin/issued`, {
            headers: {
                "Content-Type": "application/json",
                accessToken: localStorage.getItem("accessToken")
            }
        });
        const user = result.data[0].allToken;
        setIssuedToken(user);
    };
    getIssuedToken();
}, []);

<Grid item xs={12} md={6} lg={3}>
    <MDBox mb={1.5}>
        <ComplexStatisticsCard
            color="success"
            icon="store"
            title="발행 토큰 갯수"
            count={issuedToken}
            percentage={%raw%}{{
                color: "success",
                label: "Make DashBoard"
            }}{%endraw%}
        />
    </MDBox>
</Grid>;
```

---

## 남은 토큰 수량

`axios`를 이용하여 구현 하였으며, 로그인 후 대시보드로 넘어와서 모든 데이터가 통신되기 때문에 이 부분에서 에러처리를 해 주었다.

```javascript
const [remainToken, setRemainToken] = useState([]);

useEffect(() => {
    const getRemainToken = async () => {
        try {
            const result = await axios.get(
                `http://127.0.0.1:8080/admin/remain`,
                {
                    headers: {
                        "Content-Type": "application/json",
                        accessToken: localStorage.getItem("accessToken")
                    }
                }
            );
            const user = result.data[0].remainToken;
            setRemainToken(user);
        } catch (err) {
            if (err.message === "Request failed with status code 401") {
                Navigate("/personal");
                window.location.reload();
                alert("잘못된 접근입니다");
            } else if (err.message === "jwt expired") {
                alert("세션이 만료되었습니다.");
                Navigate("/signin");
                localStorage.removeItem("accessToken");
                localStorage.removeItem("refreshToken");
                localStorage.removeItem("message");
                window.location.reload();
            }
        }
    };
    getRemainToken();
}, []);

<Grid item xs={12} md={6} lg={3}>
    <MDBox mb={1.5}>
        <ComplexStatisticsCard
            icon="leaderboard"
            title="남은 토큰 갯수"
            count={remainToken}
            percentage={%raw%}{{
                color: "success",
                label: "3rd SIDE PROJECT"
            }}{%endraw%}
        />
    </MDBox>
</Grid>;
```

---

## 유저 수

```javascript
const [member, setMember] = useState([]);

useEffect(() => {
    const getMember = async () => {
        const result = await axios.get(`http://127.0.0.1:8080/admin/members`, {
            headers: {
                "Content-Type": "application/json",
                accessToken: localStorage.getItem("accessToken")
            }
        });
        const user = result.data[0].member;
        setMember(user);
    };
    getMember();
}, []);

<Grid item xs={12} md={6} lg={3}>
    <MDBox mb={1.5}>
        <ComplexStatisticsCard
            color="primary"
            icon="person_add"
            title="유저 수"
            count={member}
            percentage={%raw%}{{
                color: "success",
                label: "Just updated"
            }}{%endraw%}
        />
    </MDBox>
</Grid>;
```

---

## 유저별 토큰 보유 수량

이 부분에서 조금 많이 막혔었다.

```javascript
const [userData, setUserData] = useState([]);
// 먼저 유저에 관한 데이터는 get을 이용해서 가져왔다.
useEffect(() => {
    const getUserToken = async () => {
        const result = await axios.get(`http://127.0.0.1:8080/admin/personal`, {
            headers: {
                "Content-Type": "application/json",
                accessToken: localStorage.getItem("accessToken")
            }
        });
        const a = result.data.personal;
        setUserData(a);
    };
    getUserToken();
}, []);
// 토큰회수 기능은 데이터 일부를 수정하는 기능이기 때문에 patch를 이용해서 가져왔는데 axios.patch로 처음에 했는데 잘 되지 않아서 찾아보다가 따로 지정해주는 방식으로 진행하니 통신이 잘 되었다.
const tokenCollect = async (e) => {
    const result = await axios({
        method: "patch",
        url: `http://127.0.0.1:8080/req/collect`,
        data: {
            userId: e
        },
        headers: {
            "Content-Type": "application/json"
        }
    });
    if (result.data.message === "TOKEN COLLECT") {
        alert("토큰이 회수되었습니다.");
        window.location.reload();
    }
    return result;
};
// 유저의 데이터를 map을 이용해서 보여지게 하면 유저 한명 한명의 데이터가 순차적으로 나올 줄 알았는데 한번에 나와서 고민을 하다가 프론트 동기에게 물어보니 객체인걸 모르는거 같다고 해서 새로운 배열을 하나 만들어서 담아주니 원하는대로 출력이 되었다.
const rowsData = [];
userData.forEach((item) =>
    rowsData.push({
        token: <Company name={item.all_token} />,
        email: <MDBox>{item.email}</MDBox>,
        collect: (
            <MDButton onClick={() => tokenCollect(item.user_id)}>
                {" "}
                회수{" "}
            </MDButton>
        )
    })
);

return {
    columns: [
        {
            Header: "user email",
            accessor: "email",
            width: "45%",
            align: "left"
        },
        { Header: "token", accessor: "token", width: "10%", align: "right" },
        { Header: "collect", accessor: "collect", width: "10%", align: "right" }
    ],
    rows: rowsData
};
```

---

## 토큰 발행 최신 순으로 정렬

`axios`를 이용하여 받아온 데이터를 map을 이용해서 출력해주었다.

```javascript
const [userData, setUserData] = useState([]);

useEffect(() => {
    const getUserData = async () => {
        const result = await axios.get(
            `http://127.0.0.1:8080/admin/newissued`,
            {
                headers: {
                    "Content-Type": "application/json",
                    accessToken: localStorage.getItem("accessToken")
                }
            }
        );
        const a = result.data.newIssued;
        setUserData(a);
    };
    getUserData();
}, []);

return (
    <Card sx={%raw%}{{ height: "100%" }}{%endraw%}>
        <MDBox pt={3} px={3}>
            <MDTypography variant="h6" fontWeight="medium">
                토큰 최신 발행 이력
            </MDTypography>
        </MDBox>
        <MDBox>
            {userData.map((item) => (
                <TimelineItem
                    description={item.add_token}
                    title={item.email}
                    token={item.updated_at}
                    lastItem
                />
            ))}
        </MDBox>
    </Card>
);
```
