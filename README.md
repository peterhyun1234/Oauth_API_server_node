# OAuth_API_server(by using Node)

## 1. Open authentication concept
### 1.1 OAuth background
![backgroundOfOAuth](https://user-images.githubusercontent.com/46476398/72710739-12424200-3bab-11ea-997c-550adda4af5c.PNG)
    
    비밀번호와 아이디를 API 전달하면 보안상 위험함
![backgroundOfOAuth2](https://user-images.githubusercontent.com/46476398/72710744-13736f00-3bab-11ea-9fab-b17b27179894.PNG)
    
    OAuth를 통해서 accessToken을 받아서 보안상 위험없이 이용할 수 있음
### 1.2 Federated identity
    여러 서비스를 연결시켜 사용자로 하여금 편하게 이용할 수 있는 개념
![fedo](https://cdn-images-1.medium.com/max/1600/1*6TeBm2nxup6YeDm7axm_uA.png)
>OAuth를 통해서 구현할 수 있음

## 2. Applying google API
### 2.1 login example
![login_example](https://user-images.githubusercontent.com/46476398/73237660-a62e9200-41d9-11ea-8aee-eaf37017d80b.PNG)
> 구글에서 제공하는 api service의 http api를 통해 로그인 가능 -> access token 받기 가능

### 2.2 how to using Access_token
![access_token_사용법](https://user-images.githubusercontent.com/46476398/73237583-623b8d00-41d9-11ea-8cd9-c7f5005641c5.PNG)
> access token을 활용하는 두가지 방법
> 1. access token을 query의 parameter로 보내기
> 2. access token 인증하는 Bearer Authentication 사용

## 3. contact
     e-mail: peterhyun1234@gmail.com