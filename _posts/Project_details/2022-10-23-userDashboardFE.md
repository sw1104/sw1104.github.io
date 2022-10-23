---
title: "기업 인턴십 프로젝트 - 유저 대시보드 FRONTEND CODE"
excerpt: "유저 대시보드 파헤치기"

categories:
    - Project_details
tags:
    - [Project, wecode]

permalink: /Project_details/cooperation/userDashboard/frontend

toc: true
toc_sticky: true

date: 2022-10-23
last_modified_at: 2022-10-23
---

## 보유 토큰 조회 및 지갑 생성

```javascript
const [inputs, setInputs] = useState(0);
useEffect(() => {
    const getAllToken = async () => {
        const result = await axios.get(`http://127.0.0.1:8080/users/token`, {
            headers: {
                "Content-type": "application/json",
                accessToken: localStorage.getItem("accessToken")
            }
        });
        const tokenData = result.data[0].all_token;
        setInputs(tokenData);
    };
    getAllToken();
}, [inputs]);
const createWallet = async () => {
    const result = await axios({
        method: "post",
        url: `http://127.0.0.1:8080/users/wallet`,
        headers: {
            "Content-Type": "application/json",
            accessToken: localStorage.getItem("accessToken")
        }
    });
    return result;
};
<Grid item xs={12} md={6} lg={3}>
    <MDBox mb={1.5}>
        <ComplexStatisticsCard
            title="나의 토큰 수량"
            count={inputs}
            percentage={% raw %}{{
                label: (
                    <MDButton onClick={() => createWallet()}>
                        지갑 조회
                    </MDButton>
                )
            }}{% endraw %}
        />
    </MDBox>
</Grid>;
```

---

## 토큰 교환 신청

try-catch를 이용하여 에러메시지를 잡아서 alert으로 해당 에러에 대한 메시지를 띄워주도록 구현하였다.

```javascript
const exToken = async () => {
    try {
        const result = await axios({
            method: "post",
            url: `http://127.0.0.1:8080/users/exchange`,
            headers: {
                "Content-type": "application/json",
                accessToken: localStorage.getItem("accessToken")
            }
        });
        if (result.data.message === "REQUEST TO TOKEN EXCHANGE BY POINT") {
            alert("토큰 교환 신청 완료!");
            window.location.reload();
        }
    } catch (err) {
        if (err.response.data.message === "LACK OF POINT") {
            alert("포인트가 부족합니다. 교환 비율은 `1000:1` 입니다.");
            window.location.reload();
        } else if (err.response.data.message === "ONE TO ONE") {
            alert("이미 신청한 내역이 있습니다.");
            window.location.reload();
        }
    }
};
<Grid item xs={12} md={6} lg={3}>
    <MDBox mb={1.5}>
        <ComplexStatisticsCard
            count="토큰 교환"
            percentage={% raw %}{{
                label: (
                    <MDButton onClick={() => exToken()}>
                        토큰 교환 신청
                    </MDButton>
                )
            }}{% endraw %}
        />
    </MDBox>
</Grid>;
```

---

## 포인트 조회 및 획득

    ```javascript
    const [point, setPoint] = useState(0);
    const [getVPoint, setGetVPoint] = useState(0);

    useEffect(() => {
        const getPoint = async () => {
            const result = await axios.get(`http://127.0.0.1:8080/users/point`, {
                headers: {
                    "Content-type": "application/json",
                    accessToken: localStorage.getItem("accessToken")
                }
            });
            const pointData = result.data[0].point;
            setGetVPoint(pointData);
        };
        getPoint();
    }, [point]);

    const earnPoint = async () => {
        const result = await axios.get(`http://127.0.0.1:8080/users/earnpoint`, {
            headers: {
                "Content-type": "application/json",
                accessToken: localStorage.getItem("accessToken")
            }
        });
        const pointData = result.data[0].point;
        setPoint(pointData);
    };

    <Grid item xs={12} md={6} lg={3}>
        <MDBox mb={1.5}>
            <ComplexStatisticsCard
                title="나의 포인트"
                count={getVPoint}
                percentage={% raw %}{{
                    label: (
                        <MDButton onClick={() => earnPoint()}>
                            포인트 획득하기
                        </MDButton>
                    )
                }}{% endraw %}
            />
        </MDBox>
    </Grid>;
    ```

---

## 상품 리스트 및 구매

```javascript
const [product, setProduct] = useState([]);
useEffect(() => {
    const getProduct = async () => {
        const result = await axios.get(`http://127.0.0.1:8080/users/product`, {
            headers: {
                "Content-Type": "application/json",
                accessToken: localStorage.getItem("accessToken")
            }
        });
        const productList = result.data;
        setProduct(productList);
    };
    getProduct();
}, []);
const buyProduct = async (e) => {
    try {
        const result = await axios({
            method: "post",
            url: "http://127.0.0.1:8080/users/order",
            data: {
                productId: e
            },
            headers: {
                "Content-Type": "application/json",
                accessToken: localStorage.getItem("accessToken")
            }
        });
        if (result.data.message === "PURCHASE COMPLETE") {
            alert("토큰을 사용 했습니다.");
            window.location.reload();
        }
    } catch (err) {
        if (err.response.data.message === "TOKEN") {
            alert("토큰이 부족합니다. 열심히 포인트를 획득하세요.");
            window.location.reload();
        }
    }
};
<Grid container spacing={3}>
    {product.map((item) => (
        <Grid item xs={12} md={6} lg={4}>
            <MDBox mb={1.5}>
                <ComplexStatisticsCard
                    title={item.token}
                    count={item.productName}
                    percentage={% raw %}{{
                        label: (
                            <MDButton
                                onClick={() => buyProduct(item.productId)}
                            >
                                상품 구매하기
                            </MDButton>
                        )
                    }}{% endraw %}
                />
            </MDBox>
        </Grid>
    ))}
</Grid>;
```

---

## 상품 구매 히스토리

관리자 대시보드에서 했던 방식과 마찬가지로 새로운 배열을 만들어서 객체를 넣어준 뒤 상품 구매 히스토리를 출력하는 방법으로 구현 하였다.

```javascript
const [history, setHistory] = useState([]);
const Navigate = useNavigate();
useEffect(() => {
    const getUserHistory = async () => {
        try {
            const result = await axios.get(
                `http://127.0.0.1:8080/users/history`,
                {
                    headers: {
                        "Content-Type": "application/json",
                        accessToken: localStorage.getItem("accessToken")
                    }
                }
            );
            const historyData = result.data;
            setHistory(historyData);
        } catch (err) {
            console.log(err);
            alert("세션이 만료되었습니다");
            Navigate("/signin");
            localStorage.removeItem("accessToken");
            localStorage.removeItem("refreshToken");
            localStorage.removeItem("message");
            window.location.reload();
        }
    };
    getUserHistory();
}, []);
const rowsData = [];
history.forEach((user) =>
    rowsData.push({
        token: <Company name={user.price} />,
        email: <MDBox>{user.name}</MDBox>,
        date: <MDBox>{user.updated_at}</MDBox>
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
        { Header: "token", accessor: "token", width: "10%", align: "center" },
        { Header: "date", accessor: "date", width: "10%", align: "right" }
    ],
    rows: rowsData
};
```
