# Spring-Boot CXF JAXWS QuickStart

This example demonstrates how you can use Apache CXF with Spring Boot.

The quickstart uses Spring Boot to configure a little application that includes a CXF JAXWS endpoint.


### Building

The example can be built with

    mvn clean install


### Running the example locally

The example can be run locally using the following Maven goal:

    mvn spring-boot:run


### Running the example in OpenShift

It is assumed a running OpenShift platform is already running.

Assuming your current shell is connected to OpenShift so that you can type a command like

```
oc get pods
```

Then the following command will package your app and run it on OpenShift:

```
mvn fabric8:run
```

The output log will give the URL to access the endpoint, something like
```
[INFO] F8:[SVC] spring-boot-cxf-jaxws: http://192.168.64.7:32224
```

You need to append the context-path `service/hello` to access the service so the URL is something like

    http://192.168.64.7:32224/service/hello

To list all the running pods:

    oc get pods

Then find the name of the pod that runs this quickstart, and output the logs from the running pods with:

    oc logs <name of pod>

### Running via an S2I Application Template

Applicaiton templates allow you deploy applications to OpenShift by filling out a form in the OpenShift console that allows you to adjust deployment parameters.  This template uses an S2I source build so that it handle building and deploying the application for you.

First, import the Fuse image streams:

    oc create -f https://raw.githubusercontent.com/jboss-fuse/application-templates/fis-2.0.x.redhat/fis-image-streams.json

Then create the quickstart template:

    oc create -f https://raw.githubusercontent.com/jboss-fuse/application-templates/fis-2.0.x.redhat/quickstarts/spring-boot-cxf-jaxws-template.json

Now when you use "Add to Project" button in the OpenShift console, you should see a template for this quickstart. 

### Accessing the Endpoints

To access the endpoint you first need to create an OpenShift route for the service so that it can be exposed externally.  You can use the 'oc create route' command to create the route and the 'oc get routes' to get the host name for
the route that you created.  Example:


    $ oc create route edge example1 --service=spring-boot-cxf-jaxws
    route "example1" created
    
    $ oc get routes example1
    NAME       HOST/PORT                                PATH      SERVICES                PORT      TERMINATION
    example1   example1-myproject.192.168.64.2.xip.io             spring-boot-cxf-jaxws   http      edge

You can then use the host report to access the service:

    https://example1-myproject.192.168.64.2.xip.io/service/hello?wsdl
    
... will now display the generated WSDL.

