Server side
===========

web.xml
-------

```XML
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:SpringContext.xml</param-value>
	</context-param> 
	
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>

	<servlet>
		<servlet-name>springDispatcherServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextClass</param-name>
			<param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>

	<servlet-mapping>
		<servlet-name>springDispatcherServlet</servlet-name>
		<url-pattern>/spring/*</url-pattern>
	</servlet-mapping>
```


SpringContext.xml
-----------------

* 檔名跟 `web.xml` 當中 `contextConfigLocation` 設定的一樣即可。
* 要放在 classpath 之下。

```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="
		http://www.springframework.org/schema/beans 
		http://www.springframework.org/schema/beans/spring-beans-3.2.xsd
		http://www.springframework.org/schema/context 
		http://www.springframework.org/schema/context/spring-context-3.2.xsd">
	<context:annotation-config />
	<bean id="webSocketServer" class="us.dontcareabout.gwt.server.WebSocketServer" />
</beans>
```


### Servlet ###

```Java
public class ExampleServlet extends HttpServlet {
	private WebSocketServer wsServer;

	@Override
	public void init() throws ServletException {
		super.init();
		wsServer = WebApplicationContextUtils
			.getWebApplicationContext(this.getServletContext())
			.getBean(WebSocketServer.class);
	}
}
```


## Client ##

```Java
	WebSocket ws = new WebSocket(
		GWT.getHostPageBaseURL().replace(Location.getProtocol(), "ws:") +
		//與下列設定有關：
		//* web.xml 的 SpringDispatcherServlet
		//* WebSocketServer.registerWebSocketHandler()
		"spring/websocket"
	);
	ws.addMessageHandler(new MessageHandler() {
		@Override
		public void onMessage(MessageEvent e) {
			Console.log(e.getMessage());
		}
	});
	ws.open();
```
