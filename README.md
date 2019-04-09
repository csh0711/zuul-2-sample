# Example edge service with Netflix Zuul 2

This repository provides a simple Zuul 2 edge service example. It is based on the sample project provided by the [Netflix OSS Zuul sample](https://github.com/Netflix/zuul/tree/2.1/zuul-sample). 

For detailled information please see our blog post: https://www.novatec-gmbh.de/en/blog/api-gateways-an-evaluation-of-zuul-2/

## Filters

Zuul 2 filters are written in Groovy.
In order to load the filters properly by the FilterFileManager you need to set the following properties in 
the application.properties file

        ## Loading Filters
        zuul.filters.root=src/main/groovy/info/novatec/zuul2/filters
        zuul.filters.locations=<${zuul.filters.root}/inbound, ${zuul.filters.root}/endpoint, ${zuul.filters.root}/outbound>
        zuul.filters.packages=com.netflix.zuul.filters.common

Reminder: Locations of filters may vary depending on the actual location.

## Routing

Routing paths can be configured in application.properties file. Routing to backend services is 
managed by Netflix Ribbon. Services are then 'registered' with their respective name and port.

        Example with localhost:
        <!-- Do these for each subsequent backend service you would like to work with --->
        <clientname>.ribbon.listOfServers=localhost:<port>
        <clientname>.ribbon.client.NIWSServerListClassName=com.netflix.loadbalancer.ConfigurationBasedServerList

The routing itself is done in the HttpInboundFilter `Routes` where the configured paths are used.   
   
## Server Port

By default the Netty server runs under port :8080. For this example it is changed to port :8887.
This might be adjusted in the `application.properties`

        zuul.server.port.main=<port>
        
 
Three different proxied backend services are used to illustrate the functionality of Zuul 2.x.
Each service runs under the following ports

        users-service = localhost:8081
        images-service = localhost:8082
        comments-service = localhost:8083
        
The example services can be found here: https://github.com/csh0711/edge-services
        

## Service Discovery 

For sake of simplicity in this example the service discovery with Eureka is disabled. In respective
`application.properties` file 

        eureka.shouldFetchRegistry=false
        eureka.validateInstanceId=false

## Start the Application

### Start Zuul 2 edge service

You can run the Zuul 2 application in the project's root folder by running the Gradle wrapper:

       ## For windows leave out the './' in order to execute the command
       ./gradlew run
        

### Start backend services (Users/Images/Comments)

Navigate to the respective services' root folder and type:

        ## For windows leave out the './' in order to execute the command
        ./mvnw run
        
 
