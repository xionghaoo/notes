###load-on-startup标签
默认情况下，servlet的初始化(init方法调用)是在第一个请求到达时完成，但是在servlet配置文件中增加load-on-startup可以让容器在启动时就初始化Servlet。

###Tomcat线程池
线程池默认大小为200

###JavaEE servlet依赖导入
除了添加Meavn依赖外，还需要在web.xml中声version和 dtd等标签信息

