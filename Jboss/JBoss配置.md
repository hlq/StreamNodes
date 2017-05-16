### Difference between standalone.xml and standalone-full.xml

* standalone.xml：支持Java EE Web-Profile加上一些扩展，如RESTFul Web Services和支持EJB3远程调用
* standalone-full.xml：支持Java EE全配置文件和所有服务器功能，无需集群
* standalone-ha.xml：具有集群功能的默认配置文件
* standalone-full-ha.xml：具有集群功能的完整配置文件

> 如果配置了standalone-full.xml而不是standalone.xml，那么您必须在启动JBoss服务时选择它。
  ./standalone.sh -c standalone-full.xml  
  如果您在JBoss Developer Studio,idea中执行此操作，那么您应该在Server Config中选择standalone-full.xml而不是standalone.xml。  
  vm options=-Djboss.server.default.config=standalone-full.xml