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

