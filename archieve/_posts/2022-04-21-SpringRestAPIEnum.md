---
title: "2023-04-21 Spring Rest API, Enum"
categories: []
tags: [archieve, enum, Rest API, Spring]
excerpt: ""
classess: wide
slug: "enumAndRestAPI"
published: false
---

Spring Boot를 이용하여 Rest API 빌드할 때. enum의 사용.


https://www.baeldung.com/spring-enum-request-param
enum이 아닌 값을 받으면 exception 터진다고 함...ConversionFailedException.

swagger에서는 enum으로 파라미터 받을 경우. 정해진 값만 담아 보낼 수 있다.

Thunder로 테스트 해본 결과 exception이 사용자 측으로 날아왔다. naive하게 걱정한 서버가 뻗는 일은 없었다. 
그러나 에러로 던져지는 Json이 예쁘지 않다. 길고. 서버의 클래스명이 그대로 노출된다!!!
-> 에러던져지는 것을 커스텀 할 수 없나?
-> 예쁘게 던져지도록!
{
  "timestamp": 1650510729985,
  "status": 400,
  "error": "Bad Request",
  "trace": "org.springframework.web.method.annotation.MethodArgumentTypeMismatchException: Failed to convert value of type 'java.lang.String' to required type 'com.infoin.app.mycom.controller.MycomConfigController$ScopeType'; nested exception is org.springframework.core.convert.ConversionFailedException: Failed to convert from type [java.lang.String] to type [@org.springframework.web.bind.annotation.PathVariable com.infoin.app.mycom.controller.MycomConfigController$ScopeType] for value 'DUMMY'; nested exception is java.lang.IllegalArgumentException: No enum constant com.infoin.app.mycom.controller.MycomConfigController.ScopeType.DUMMY\r\n\tat org.springframework.web.method.annotation.AbstractNamedValueMethodArgumentResolver.resolveArgument(AbstractNamedValueMethodArgumentResolver.java:133)\r\n\tat org.springframework.web.method.support.HandlerMethodArgumentResolverComposite.resolveArgument(HandlerMethodArgumentResolverComposite.java:122)\r\n\tat org.springframework.web.method.support.InvocableHandlerMethod.getMethodArgumentValues(InvocableHandlerMethod.java:179)\r\n\tat org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:146)\r\n\tat org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:117)\r\n\tat org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:895)\r\n\tat org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:808)\r\n\tat org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:87)\r\n\tat org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:1067)\r\n\tat org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:963)\r\n\tat org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:1006)\r\n\tat org.springframework.web.servlet.FrameworkServlet.doGet(FrameworkServlet.java:898)\r\n\tat javax.servlet.http.HttpServlet.service(HttpServlet.java:655)\r\n\tat org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:883)\r\n\tat javax.servlet.http.HttpServlet.service(HttpServlet.java:764)\r\n\tat org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:227)\r\n\tat org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)\r\n\tat org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:53)\r\n\tat org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)\r\n\tat org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)\r\n\tat org.springframework.web.filter.RequestContextFilter.doFilterInternal(RequestContextFilter.java:100)\r\n\tat org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:117)\r\n\tat org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)\r\n\tat org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)\r\n\tat org.springframework.web.filter.FormContentFilter.doFilterInternal(FormContentFilter.java:93)\r\n\tat org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:117)\r\n\tat org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)\r\n\tat org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)\r\n\tat org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:201)\r\n\tat org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:117)\r\n\tat org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)\r\n\tat org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)\r\n\tat org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:197)\r\n\tat org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:97)\r\n\tat org.apache.catalina.authenticator.AuthenticatorBase.invoke(AuthenticatorBase.java:541)\r\n\tat org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:135)\r\n\tat org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:92)\r\n\tat org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:78)\r\n\tat org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:360)\r\n\tat org.apache.coyote.http11.Http11Processor.service(Http11Processor.java:399)\r\n\tat org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:65)\r\n\tat org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:889)\r\n\tat org.apache.tomcat.util.net.NioEndpoint$SocketProcessor.doRun(NioEndpoint.java:1743)\r\n\tat org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:49)\r\n\tat org.apache.tomcat.util.threads.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1191)\r\n\tat org.apache.tomcat.util.threads.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:659)\r\n\tat org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61)\r\n\tat java.lang.Thread.run(Thread.java:748)\r\nCaused by: org.springframework.core.convert.ConversionFailedException: Failed to convert from type [java.lang.String] to type [@org.springframework.web.bind.annotation.PathVariable com.infoin.app.mycom.controller.MycomConfigController$ScopeType] for value 'DUMMY'; nested exception is java.lang.IllegalArgumentException: No enum constant com.infoin.app.mycom.controller.MycomConfigController.ScopeType.DUMMY\r\n\tat org.springframework.core.convert.support.ConversionUtils.invokeConverter(ConversionUtils.java:47)\r\n\tat org.springframework.core.convert.support.GenericConversionService.convert(GenericConversionService.java:192)\r\n\tat org.springframework.beans.TypeConverterDelegate.convertIfNecessary(TypeConverterDelegate.java:129)\r\n\tat org.springframework.beans.TypeConverterSupport.convertIfNecessary(TypeConverterSupport.java:73)\r\n\tat org.springframework.beans.TypeConverterSupport.convertIfNecessary(TypeConverterSupport.java:53)\r\n\tat org.springframework.validation.DataBinder.convertIfNecessary(DataBinder.java:700)\r\n\tat org.springframework.web.method.annotation.AbstractNamedValueMethodArgumentResolver.resolveArgument(AbstractNamedValueMethodArgumentResolver.java:125)\r\n\t... 47 more\r\nCaused by: java.lang.IllegalArgumentException: No enum constant com.infoin.app.mycom.controller.MycomConfigController.ScopeType.DUMMY\r\n\tat java.lang.Enum.valueOf(Enum.java:238)\r\n\tat org.springframework.core.convert.support.StringToEnumConverterFactory$StringToEnum.convert(StringToEnumConverterFactory.java:54)\r\n\tat org.springframework.core.convert.support.StringToEnumConverterFactory$StringToEnum.convert(StringToEnumConverterFactory.java:39)\r\n\tat org.springframework.core.convert.support.GenericConversionService$ConverterFactoryAdapter.convert(GenericConversionService.java:437)\r\n\tat org.springframework.core.convert.support.ConversionUtils.invokeConverter(ConversionUtils.java:41)\r\n\t... 53 more\r\n",
  "message": "Failed to convert value of type 'java.lang.String' to required type 'com.infoin.app.mycom.controller.MycomConfigController$ScopeType'; nested exception is org.springframework.core.convert.ConversionFailedException: Failed to convert from type [java.lang.String] to type [@org.springframework.web.bind.annotation.PathVariable com.infoin.app.mycom.controller.MycomConfigController$ScopeType] for value 'DUMMY'; nested exception is java.lang.IllegalArgumentException: No enum constant com.infoin.app.mycom.controller.MycomConfigController.ScopeType.DUMMY",
  "path": "/mycom/config/scope/DUMMY/get"
}

https://stackoverflow.com/questions/64575842/how-to-return-a-proper-validation-error-for-enum-types-in-spring-boot/64576320#64576320

@RestControllerAdvice
public class EnumCastAdvice {

	@ResponseStatus(HttpStatus.BAD_REQUEST)
	@ExceptionHandler(ConversionFailedException.class)
	public ErrorRes handleEnumCastFailure(ConversionFailedException ex){
		
		return new ErrorRes("400", "Bad Request : Failed to convert parameter to valid value");
	}
	
	@Getter
	@Setter
	@AllArgsConstructor
	class ErrorRes{
		private String code;
		private String message;
	}
}

ConversionFailedException -> 이 터질때마다 이 advice 호출됨... 내가 의도치 않은 어딘가에서 이 advice 터질 수 있다.(에러메시지가 상황에 적절치 않을 수 있고, 그 메서드에서 나름의 처리를 하고 있을 수 있다.)

ENUM 활용시~
MYBATIS에서~
${}와 #{}!
"https://logical-code.tistory.com/25"

https://mybatis.org/mybatis-3/ko/sqlmap-xml.html

UPDATE TW_MYCOM_SET_SCOPE
			SET ${type} = #{value}
		WHERE
			USER_SN = #{userSn}

걱정 : 1 = 2 이런식으로 정수 ordinal 값이 박히지 않을까 걱정.... <- 일단 name이 자동으로 박히긴 함. 왜???

PutMapping으로 파일 못보냄??

@Tag(name = "mycom", description = "마이콤 API")
	@Operation(summary = "배경 이미지 변경")
	@PostMapping(path = "/bg/set", consumes = MediaType.MULTIPART_FORM_DATA_VALUE)
	public CommonResVo setBackgroundUrl(@Valid MyComBgParamVO param, BindingResult bindingResult){

도커 "이미지"란?

HTTP/TCP

why PUSH?

Why OAuth?

계층형 쿼리를 한방에 담기...

MYBATIS USEGENERATEDKEYS = TRUE에 대하여...