# spring-cloud-discovery-server

Microservice architectural pattern breaks a monolithic application into separate individual services. In order for a service to interact with another service, it needs to know the host address and the port where a service is listening to requests. One approach to solve this fundamental problem is to configure the host and port details of the service in some configuration file which needs to be used by the client service to locate that particular service. But this approach works fine only with a small stand alone application or setup. But as the number of services and service instances start increasing/decreasing, as in a native cloud environment, this approach would eventually fail.

Hence, we need some approach which is dynamic in nature and can solve the purpose of locating a dynamic service instance in a typical cloud environment. This is the situation where a service discovery approach proves to be handy. A discovery server in cloud native environment, is a registry server that acts as a source of truth and allows services to register themselves with it, as and when they appear and disappear.

Service discovery provides
1) a way for a service to register itself: 
When a service instance comes up, it can call the service discovery to inform it's location and port details so that other applications and services and contact the service discovery to locate this service instance.

2) a way for a service to deregister itself: In case of a situation when a service needs to shutdown temporarily to go for upgrades, it would let the service discovery know that it is no longer available.

3) a way for a client or an application to find other services.
  
4) a way to check the health of a service and remove the unhealthy instances from it's registry. So an application service would implement a health check which is typically, a REST endpoint which would be invoked by the service discovery server to check the health of a service instance. Set the property eureka.client.health.check.enabled.

There are several different areas where one can configure the Spring Cloud Eureka. The top three categories are "eureka.server.*", "eureka.client.*" and "eureka.instance.*". Eureka server may connect to the actuator endpoint /health
 

The key components which are involved in a service discovery, are
1) The Discovery Server
2) The Application Service
3) Application Client

It is helpful to understand how the components work together in a service discovery process. When an Application service starts up, it registers itself with the Discovery server by letting the discovery server know about it's location, it's port details and a service identifier that lets other application services  discover it through the discovery server. When a client application service wants to call this application service, it looks up to the discovery server with the service identifier of the service that it is looking for. The discovery server responds to the client with the location and port details of the application service. Further to this, the client application service continues requesting the application service.
 
There are quite a few options to implement a discovery server. Example: Spring Cloud Consul,Spring Cloud ZooKeeper and Spring Cloud Netflix. 
This project  is to demonstrate a discovery server by using the Spring Cloud Netflix. The Spring Cloud Netflix project is a collection of various projects, out of which Spring Cloud Netflix Eureka Server and Spring Cloud Netflix Eureka Client is being used in this project to demonstrate a discovery server. 

In order to quickly bootstrap an discovery server application, go to http://start.spring.io/ and use the Spring Initializr to stub it out.
Add the dependencies Eureka Server, Devtools and the Actuator.
 
The annotation "@EnableEurekaServer" in class "SpringCloudDiscoveryServerApplication", converts this SpringBootApplication
class to a discovery server. 

Configure the application.properties with the following properties:

#discovery server service name
spring.application.name=spring-cloud-discovery-server
#Set it to false. Prevents this discovery server from registering itself with any of it's peers, as it's running as a standalone one. But in a cloud environment we need to set it to true for scalability purpose.
eureka.client.register-with-eureka=false
#Since this is the only EUreka discovery server in our setup, we do not need to fetch anything from any other eureka discovery servers.
eureka.client.fetch-registry=false
#The Port where the discovery server runs.
server.port=8761