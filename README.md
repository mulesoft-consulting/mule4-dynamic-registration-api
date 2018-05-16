# mule4-skeleton-system-api
This is a Mule 4.1 example of an API using API Management and dynamic registration using the Eureka REST API as the interface to a dynamic service registry. The REST API 
definition can be found [here](https://github.com/Netflix/eureka/wiki/Eureka-REST-operations)
 

Note, the scope features of minimal-logging are not displayed correctly by Studio 7.1 or 7.2. Support for displaying SDK connector scopes is expected in Studio 7.3.

## Uses

The project contains examples using:

* The minimal-logging connector, 
* The secure property connector (formally called the secure-property-placeholder),
* Maven deployment using the mule-maven-plugin descriptor,
* Api Manager gateway auto-discovery registration,
* Using the Maven filter "feature" to add Maven properties to code in files stored in the resources-filtered directory...specifically, updating the api.base and log4j2 configurations with the project name.
* Eureka REST API for dynamic service registration,
* Initiating flows on Mule application started and stopping notifications,
* Invoking a flow from java.

## Purpose

This project is an example that can be deployed and will run when properly configured. However, the main usage for this is a starting point for building new APIs.
Developers of an API can copy this project to get all the "boilerplate" elements for an API project. Then begin adding and modifying these elements to create the new API. 

The API registers itself as an instance of an application (appId) when the Mule application starts. When the registration process
finishes, a heartbeat process is started. When the Mule application stops, the heartbeat is stopped and the application unregisters
itself with the service registry.

The flows for these dynamic registration processes are found in the dynamic-service-registration.xml Mule config file. 

### Configuration Properties

The configuration properties are stored in the src/main/resources-filtered/mule4-skeleton-proxy-${mule.env}.yaml file:

* api.id contains the Api Manager instance's autodiscovery api id,
* api.base is the base uri for the API, the default is the project name taken from the pom file,
* api.port is the port number of the Api's HTTP listener,
* my.client-id is the client id this API uses to call other APIs
* my.client-secret is the client secret registered for this API to use for calling other APIs

Note that the api.port is not used if a shared domain is configured and used for the HTTP listener configuration.


## Runtime properties

The properties mule.env and mule.key need to be set in the Mule runtime in order for the property configurations to be located. Additionally, if the Mule runtime is not configured to connect to API Manager, the API will be disabled (by default).

For Studio, add the following VM command line values when running the API:

 -Danypoint.platform.gatekeeper=disabled -Dmule.env=local -Dmule.key=Mulesoft12345678