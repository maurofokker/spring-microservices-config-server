# Config Server

* a centralized git or svn folder/project should be created with all services configurations
  * `mkdir configuration && cd configuration && git init`
  * commit or push configurations files here (i.e config-client-development.properties)
* in application property (application.yml) is configured a `spring.cloud.config.server.git.uri` with the centralized
git repository for services (properties)
* `@EnableConfigServer` will enable configuration server in application and will read above property
* when app is started will show endpoints to read services configuration properties

```
s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/{name}-{profiles}.properties],methods=[GET]}" onto public org.springframework.http.ResponseEntity<java.lang.String> org.springframework.cloud.config.server.environment.EnvironmentController.properties(java.lang.String,java.lang.String,boolean) throws java.io.IOException
s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/{name}-{profiles}.yml || /{name}-{profiles}.yaml],methods=[GET]}" onto public org.springframework.http.ResponseEntity<java.lang.String> org.springframework.cloud.config.server.environment.EnvironmentController.yaml(java.lang.String,java.lang.String,boolean) throws java.lang.Exception
s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/{name}/{profiles}/{label:.*}],methods=[GET]}" onto public org.springframework.cloud.config.environment.Environment org.springframework.cloud.config.server.environment.EnvironmentController.labelled(java.lang.String,java.lang.String,java.lang.String)
s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/{label}/{name}-{profiles}.yml || /{label}/{name}-{profiles}.yaml],methods=[GET]}" onto public org.springframework.http.ResponseEntity<java.lang.String> org.springframework.cloud.config.server.environment.EnvironmentController.labelledYaml(java.lang.String,java.lang.String,java.lang.String,boolean) throws java.lang.Exception
s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/{name}-{profiles}.json],methods=[GET]}" onto public org.springframework.http.ResponseEntity<java.lang.String> org.springframework.cloud.config.server.environment.EnvironmentController.jsonProperties(java.lang.String,java.lang.String,boolean) throws java.lang.Exception
s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/{name}/{profiles:.*[^-].*}],methods=[GET]}" onto public org.springframework.cloud.config.environment.Environment org.springframework.cloud.config.server.environment.EnvironmentController.defaultLabel(java.lang.String,java.lang.String)
s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/{label}/{name}-{profiles}.properties],methods=[GET]}" onto public org.springframework.http.ResponseEntity<java.lang.String> org.springframework.cloud.config.server.environment.EnvironmentController.labelledProperties(java.lang.String,java.lang.String,java.lang.String,boolean) throws java.io.IOException
s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/{label}/{name}-{profiles}.json],methods=[GET]}" onto public org.springframework.http.ResponseEntity<java.lang.String> org.springframework.cloud.config.server.environment.EnvironmentController.labelledJsonProperties(java.lang.String,java.lang.String,java.lang.String,boolean) throws java.lang.Exception
s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/{name}/{profile}/{label}/**],methods=[GET],produces=[application/octet-stream]}" onto public synchronized byte[] org.springframework.cloud.config.server.resource.ResourceController.binary(java.lang.String,java.lang.String,java.lang.String,javax.servlet.http.HttpServletRequest) throws java.io.IOException
s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/{name}/{profile}/{label}/**],methods=[GET]}" onto public java.lang.String org.springframework.cloud.config.server.resource.ResourceController.retrieve(java.lang.String,java.lang.String,java.lang.String,javax.servlet.http.HttpServletRequest,boolean) throws java.io.IOException
s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/{name}/{profile}/**],methods=[GET],params=[useDefaultLabel]}" onto public java.lang.String org.springframework.cloud.config.server.resource.ResourceController.retrieve(java.lang.String,java.lang.String,javax.servlet.http.HttpServletRequest,boolean) throws java.io.IOException
```

* when a property file is commited to git|svn server then our configuration server can display the content `http://localhost:8888/confi-client-development.properties`
also it can be retrieved as json `http://localhost:8888/config-client-development.json`
