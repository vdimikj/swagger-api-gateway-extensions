# Swagger Sample App

## Overview
Java project to build a stand-alone server which implements the OpenAPI Spec. You can find out 
more about both the spec and the framework at http://swagger.io.

This sample is based on Jersey2 and provides an example of integration of Swagger into a Jersey2 based app, with code based configuration/initialization

### How to start (with Maven)
To start the server, run this task:

```
mvn package -Dlog4j.configuration=file:./conf/log4j.properties jetty:run
```

This will start Jetty embedded on port 8002.

### Testing the server
Once started, you can navigate to http://localhost:8002/sample/openapi.json to view the Swagger Resource Listing.
This tells you that the server is up and ready to demonstrate Swagger.

### Using the UI
There is an HTML5-based API tool bundled in this sample--you can view it it at [http://localhost:8002](http://localhost:8002). This lets you inspect the API using an interactive UI.  You can access the source of this code from [here](https://github.com/swagger-api/swagger-ui)

### Applying an API key
The sample app has an implementation of the Swagger ApiAuthorizationFilter.  This restricts access to resources
based on api-key.  There are two keys defined in the sample app:

`default-key`

`special-key`

When no key is applied, the "default-key" is applied to all operations.  If the "special-key" is entered, a
number of other resources are shown in the UI, including sample CRUD operations.

# Example for Api integration object 
In order to achieve this format of yaml file...
```
    x-amazon-apigateway-integration:
        responses:
          default:
            statusCode: "200"
            responseParameters:
              method.response.header.Access-Control-Allow-Methods: "'DELETE,GET,HEAD,OPTIONS,PATCH,POST,PUT'"
              method.response.header.Access-Control-Allow-Headers: "'Content-Type,Authorization,X-Amz-Date,X-Api-Key,X-Amz-Security-Token'"
              method.response.header.Access-Control-Allow-Origin: "'*'"
        requestTemplates:
          application/json: "{\"statusCode\": 200}"
        passthroughBehavior: "when_no_match"
        type: "mock" 
```
     
...Use this swagger extension

 ```
 extensions = {
       @Extension(name = "amazon-apigateway-integration",
                     properties = {
                             @ExtensionProperty(name = "responses", value = "{\"default\": {\"statusCode\":\"200\",\"responceParameters\":[{\"method.response.header.Access-Control-Allow-Methods\":\"'DELETE,GET,HEAD,OPTIONS,PATCH,POST,PUT'\"},{\"method.response.header.Access-Control-Allow-Headers\": \"'Content-Type,Authorization,X-Amz-Date,X-Api-Key,X-Amz-Security-Token'\"},{\"method.response.header.Access-Control-Allow-Origin\": \"'*'\"}]}}", parseValue = true),
                             @ExtensionProperty(name = "passthroughBehavior", value = "when_no_match"),
                             @ExtensionProperty(name = "type", value = "mock"),
                             @ExtensionProperty(name = "requestTemplates", value = "{\"application/json\":\"{\\\"statusCode\\\": 200}\"}", parseValue = true)
                     }
             )
     }
