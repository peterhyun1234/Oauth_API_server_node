# express_gateway_example

## 1. How to install
``` bash
### Install Express Gateway
    $ npm install -g express-gateway

### Create an Express Gateway
    $ eg gateway create

### Follow the prompts and choose the Getting Started server template
    Follow the prompts and choose the Getting Started server template
    ➜ eg gateway create
    ? What is the name of your Express Gateway? my-gateway
    ? Where would you like to install your Express Gateway? my-gateway
    ? What type of Express Gateway do you want to create? (Use arrow keys)
    ❯ Getting Started with Express Gateway
    Basic (default pipeline with proxy)

### Run Express Gateway
    $ cd my-gateway && npm start
```

## 2. Create users and adopt credential for two way
```bash
### Create a new Express Gateway user.(게이트 웨어 켜진 상태에서!)
    $ eg users create
    ? Enter firstname [required]: peter
    ? Enter lastname [required]: jeon
    ? Enter username [required]: peterhyun
    ? Enter email:
    ? Enter email: undefined
    ? Enter redirectUri:
    ? Enter redirectUri: undefined
    √ Created 062ef359-ab0a-486a-800f-f963c2d9ee2f
    {
    "isActive": true,
    "username": "peterhyun",
    "id": "062ef359-ab0a-486a-800f-f963c2d9ee2f",
    "firstname": "peter",
    "lastname": "jeon",
    "createdAt": "Wed Sep 23 2020 14:21:09 GMT+0900 (GMT+09:00)",
    "updatedAt": "Wed Sep 23 2020 14:21:09 GMT+0900 (GMT+09:00)"
    }

### Need to create 2 credentials for this user: an OAuth2 credential, and a basic-auth (password) credential
    $ eg credentials create -c 062ef359-ab0a-486a-800f-f963c2d9ee2f -t oauth2
    $ eg credentials create -c (id) -t oauth2
    √ Created 062ef359-ab0a-486a-800f-f963c2d9ee2f
    {
    "isActive": true,
    "createdAt": "Wed Sep 23 2020 14:25:51 GMT+0900 (GMT+09:00)",
    "updatedAt": "Wed Sep 23 2020 14:25:51 GMT+0900 (GMT+09:00)",
    "autoGeneratePassword": true,
    "passwordKey": "secret",
    "id": "062ef359-ab0a-486a-800f-f963c2d9ee2f",
    "secret": "a934fb48-6771-4f47-a1b6-2f0b583432fe"
    }

    $ eg credentials create -c 062ef359-ab0a-486a-800f-f963c2d9ee2f -t basic-auth -p "password=???"
    $ eg credentials create -c (id) -t basic-auth -p "password=???"
    √ Created 062ef359-ab0a-486a-800f-f963c2d9ee2f
    {
    "isActive": true,
    "createdAt": "Wed Sep 23 2020 14:27:40 GMT+0900 (GMT+09:00)",
    "updatedAt": "Wed Sep 23 2020 14:27:40 GMT+0900 (GMT+09:00)",
    "autoGeneratePassword": true,
    "passwordKey": "password",
    "id": "062ef359-ab0a-486a-800f-f963c2d9ee2f"
    }

### The last step is where you need to create an application, or “app” 
### -> 어떤 앱에서 access token으로 사용할 지에 대한 인증
    $ eg apps create -u 062ef359-ab0a-486a-800f-f963c2d9ee2f
    $ eg apps create -u (id)
    ? Enter name [required]: testapp
    ? Enter redirectUri: https://www.github.com/peterhyun1234
    √ Created 8969cec7-cbfe-42a3-b6ce-9ab9b0cf8e94
    {
    "isActive": true,
    "id": "8969cec7-cbfe-42a3-b6ce-9ab9b0cf8e94",
    "userId": "062ef359-ab0a-486a-800f-f963c2d9ee2f",
    "name": "testapp",
    "redirectUri": "https://www.github.com/peterhyun1234",
    "createdAt": "Wed Sep 23 2020 14:31:26 GMT+0900 (GMT+09:00)",
    "updatedAt": "Wed Sep 23 2020 14:31:26 GMT+0900 (GMT+09:00)"
    }
```

## 3. How to use APIs

### 3.1. app 등록
To get started, you need to visit the /oauth2/authorize endpoint and specify the following parameters in the URL query string:

* response_type: String containing either ‘bearer’ or ‘token’. For this example you’ll use ‘token’.
* client_id: String containing the id of your app from the output of eg apps create -u val. In this case, ‘8969cec7-cbfe-42a3-b6ce-9ab9b0cf8e94’, but it will be different for you.
* redirect_uri: String that must match the redirectUri you specified when running the eg apps create -u val command.

So, here’s how the full URL looks: 
> http://localhost:8080/oauth2/authorize?response_type=token&client_id=8969cec7-cbfe-42a3-b6ce-9ab9b0cf8e94&redirect_uri=https://www.github.com/peterhyun1234

### 3.2. Access token를 이용한 API 사용
리다이렉션 된 uri에 억세스토큰 표시됨
> https://github.com/peterhyun1234#access_token=1f979f65b1a34adfa9de2a0b85902cfd%7C3a52df288d9c485daeddd194dfe50b1f&token_type=Bearer<br>

HTTP 리퀘스트 Authorization 헤더에  bearer으로 넣어서 사용가능


#### 3.2.1. Copy the access_token from the URL bar. The access_token is URI-encoded, so first decode it using Node.js’s shell.

```bash
$ node
Welcome to Node.js v12.18.3.
Type ".help" for more information.
> decodeURIComponent('1f979f65b1a34adfa9de2a0b85902cfd%7C3a52df288d9c485daeddd19
4dfe50b1f')
'1f979f65b1a34adfa9de2a0b85902cfd|3a52df288d9c485daeddd194dfe50b1f'
>
```

#### 3.2.2. Use curl to make an HTTP request with the token in the Authorization header

```bash
$ curl -H "Authorization: Bearer 1f979f65b1a34adfa9de2a0b85902cfd|3a52df288d9c485daeddd194dfe50b1f" http://localhost:8080/ip
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    31  100    31    0     0    140      0 --:--:-- --:--:-- --:--:--   140
{
  "origin": "202.30.15.49"
}
```


## 2. Ref by

### 2.1. adopting micro services 
    https://medium.com/@oyedejipeace/building-a-simple-microservices-app-with-express-gateway-a8fd49d81aeb
### 2.2. adopting security by using oauth2.0
    https://www.express-gateway.io/getting-started-with-oauth2/
