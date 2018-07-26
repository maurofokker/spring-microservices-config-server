# Config Server
* [microservices demo starts here](https://github.com/maurofokker/microservices-demo)

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

* encryption of properties with cloud config server
  this will allow to the config server to pull encrypted values from configuration repository (git)
  * download [Java Cryptography Extension](http://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html)
  * copy downloaded files `local_policy.jar` and `US_export_policy.jar` to local `jre` folder (i.e. `C:\Program Files\Java\jre1.8.0_161\lib\security`)
  make sure to backup existing files
  * in `application.properties` or `application.yml` add symmetrical (shared) key property `encrypt.key` with a secret key value
    * to [generate key](https://patrickgrimard.io/2016/03/04/encrypting-and-decrypting-configuration-property-values-in-spring-cloud/)
    * due to a problem with simmetric key (_No key was installed for encryption service_ after call _/encrypt_ endpoint) `encrypt.key` property
    must go in a `bootstrap.properties` file in `resources`
  * above property will enable `/encrypt` and `/decrypt` endpoints
    ```
      $ curl http://localhost:8888/encrypt -d Microservices
      > 2284ff442a974bd19d608c93be6b1fa3c9998a887e391f6806a74d0251f0e8d3
      
      $ curl http://localhost:8888/decrypt --data-urlencode 2284ff442a974bd19d608c93be6b1fa3c9998a887e391f6806a74d0251f0e8d3
      > Microservices
    ```
  