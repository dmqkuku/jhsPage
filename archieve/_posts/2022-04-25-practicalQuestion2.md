---
title: "2023-04-25 Question and Solution"
categories: []
tags: [archieve]
excerpt: ""
classess: wide
slug: "QnA1"
published: false
---


PutMapping으로 파일 못보냄??

"trace": "org.springframework.web.HttpMediaTypeNotSupportedException: Content type '' not supported\r\n\tat 

@Tag(name = "mycom", description = "마이콤 API")
	@Operation(summary = "배경 이미지 변경")
	@PostMapping(path = "/bg/set", consumes = MediaType.MULTIPART_FORM_DATA_VALUE)
	public CommonResVo setBackgroundUrl(@Valid MyComBgParamVO param, BindingResult bindingResult){

https://mand2.github.io/spring/rest-api/415-error-of-put/
https://stackoverflow.com/questions/25207936/manually-parsing-parameters-from-put-request-in-tomcat
GET 혹은 POST 는 tomcat이 기본적으로 parsing 하게 하지만,
PUT, DELETE는 parsing이 안됨,, 따로 server.xml에서 설정해야 함.

-> Swagger의 문제인듯함. Thunder로 테스트하니 잘만 됨.

도커 "이미지"란?
-> 일종의 readonly instruction sets으로. Docker Platform위에서 컨테이너를 만들기 위해 쓰인다.
-> 우리가 프로그램 설치할때 쓰는 쉘 스크립트와 비슷한 개념이라고 보아도 될듯.

HTTP/TCP/IP
https://www.ibm.com/docs/en/cics-ts/5.3?topic=web-internet-tcpip-http-concepts
전부 일종의 프로토콜!!!
TCP -> Transimission Control Protocol.
IP -> Internet Protocol.
HTTP -> HyperText Transfer Protocol.

OSI(Open System Interconnection)의 7계층중 각각 
HTTP - Application
TCP - Transport
IP - Network
...
http://www.tcpipguide.com/index.htm 참고.


why PUSH?

Why OAuth?
OAuth = Open Authorization. -> Access delegation의 open standard이다.(Not Authentication!)
commonly used as a way for internet users to grant websites or applications access to their information on other websites but without giving them the passwords
to permit the users to share information about their accounts with third-party applications or websites.
https://en.wikipedia.org/wiki/OAuth
예를 들어 목표 resource에 접근하기 위해 Google account을 거쳐야 하는 상황이 있다고 가정해 보자...
old way에서는, 구글 계정 정보를 resource 관문 서버에 넘겨서 authorization을 진행하여야 한다.

OAuth2에서는. user -> Google -> accessToken -> Resource Server 이렇게 진행된다.


https://anil-pace.medium.com/json-web-tokens-vs-oauth-2-0-85dd0b32057d#:~:text=JWT%20is%20a%20JSON%20based%20security%20token%20forAPI%20Authentication&text=JWT%20is%20just%20serialised%2C%20not,are%205%20different%20flow%20patterns.
[JwT vs OAuth]
Summary
JWT is a JSON based security token forAPI Authentication
JWT can contain unlimited amount of data unlike cookies.
JWT can be seen not but modifiable once it’s sent.
JWT is just serialised, not encrypted.
OAuth is not an API or a service: it’s an open standard for authorization .
OAuth is a standard set of steps for obtaining a token. There are 5 different flow patterns.
Authorization code grant is the most secure OAuth grant type
Resource Owner grant type is the least secure

https://frontegg.com/blog/oauth-flows#:~:text=OAuth%20flows%20are%20essentially%20processes,involvement%20for%20back%2Dend%20systems.
Demystifying OAuth

1. Resource Owner : grant access to protected resources. 
2. Resource Server : hold the resources the resources owner needs access to.
3. Client : application request access to protected resources
4. Authorization Server : accepting credentials & checking if they are authorized to accesa a reestricted resources.
[OAuth Flow Types]
1. Authorization Code Flow
How this OAuth flow works:

The user clicks on a login link in the web application.
The user is redirected to an OAuth authorization server, after which an OAuth login prompt is issued.
The user provides credentials according to the enabled login options.
Typically, the user is shown a list of permissions that will be granted to the web application by logging in and granting consent.
The user is redirected to the application, with the authorization server providing a one-time authorization code.
The app receives the user’s authorization code and forwards it along with the Client ID and Client Secret, to the OAuth authorization server.
The authorization server generates an ID Token, Access Token, and an optional Refresh Token, before providing them them to the app.
The web application can then use the Access Token to gain access to the target API with the user’s credentials.

2. Client Credentials Flow
The application authenticates with the OAuth authorization server, passing the Client Secret and Client ID.
The authorization server checks the Client Secret and Client ID and returns an Access Token to the application.
The Access Token allows the application to access the target API with the required user account.

3. Resource Owner Password Flow
The user clicks a login link in the application and enters credentials into a form managed by the app.
The application stores the credentials, and passes them to the OAuth authorization server.
The authorization server validates credentials and returns the Access Token (and an optional Refresh Token).
The app can now access the target API with the user’s credentials.

4. Implicit Flow with Form Post.
This flow uses OIDC to implement a web sign-in that functions like WS-Federation and SAML. The web app requests and receives tokens via the front channel, without requiring extra backend calls or secrets. With this process, you don’t have to use, maintain, obtain or safeguard secrets in your app. 

5. Hybrid Flow
This flow can benefit apps that can securely retain Client Secrets. It lets your app obtain immediate access to an ID token, while enabling ongoing retrieval of additional access and refresh tokens. This is useful for apps that need to immediately gain access to data about the user, but must perform some processing prior to gaining access to protected resources for a long time.

6. Device Authorization Flow
This flow makes it possible to authenticate users without asking for their credentials. This provides a better user experience for mobile devices, where it may be more difficult to type credentials. Applications on these devices can transfer their Client ID to the Device Authorization Flow to start the authorization process and obtain a token.




계층형 쿼리를 한방에 담기...

MYBATIS USEGENERATEDKEYS = TRUE에 대하여...

자동 Image Compression 요청...
Image Compression -> http://pi.math.cornell.edu/~web6140/TopTenAlgorithms/JPEG.html
https://stackoverflow.com/questions/44565500/how-can-i-compress-images-using-java 


https://stackoverflow.com/questions/12108468/does-multiple-upload-follow-the-same-order-of-uploading-in-which-we-select-the-i
복수의 파일 업로드시 파일 순서 유지된다는 보장은 없음...
그래도 순서를 유지하고 싶다면???
https://stackoverflow.com/questions/7449861/multipart-upload-form-is-order-guaranteed
순서 유지된다는 이야기도 있음.
https://www.w3.org/TR/html4/interact/forms.html#h-17.13.4

이미지 파일 8개를 입력받아 저장하고. 추후에 수정 및 삭제 로직이 구현되어야 하는 API를 생각해보자.
이미지 파일 사이에는 순서가 존재할 수 있고. 순서가 저장/ 수정/ 삭제를 거치며 보존되어야 한다면?
이 경우 VO를 어떻게 만들어야 할까?
1. List<MultipartFile> files;
2. MultipartFile file1; MultipartFile file2; ...
3. Base64 encoding -> String tokeneizer를 이용한다.
4. BeanValidation을 이용하여 8개라면 8의 사이즈의 배열만 받을 수 있도록 구현한다.

