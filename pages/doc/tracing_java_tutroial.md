---
title: Instrumenting Your OpenTracing Java Application
keywords:
tags: [best practice]
sidebar: doc_sidebar
permalink: tracing_java_tutorial.html
summary: Configure your OpenTracing Java application to send data to Wavefront using the OpenTracing Java SDK.
---
{% include important.html content="This tutorial is work in progress!" %}
In this tutorial, you configure a sample application with the Wavefront OpenTracing Java SDK with a sample application to send data to Wavefront. Let's get started!

## Prerequisites

* Java 8 or above.
* Install the Docker platform to run the Wavefront proxy.
* Clone the sample application. 
  ```
  git clone https://github.com/wavefrontHQ/distributed-tracing-sample-apps.git
  ```
  This repository has many sample. Navigate to the dropwizard-app application using the terminal.
  ```
  cd distributed-tracing-sample-apps/Wavefront-DT/dropwizard-app/
  ```

## Instrument the Sample Applications

1. Open `dropwizard-app` in your preferred Java IDE.
1. Add the Wavefront OpenTracing SDK dependency to the `pom.xml` file and import the changes to your application:
    ```
    <dependencies>
      <dependency>
        <groupId>com.wavefront</groupId>
        <artifactId>wavefront-opentracing-sdk-java</artifactId>
        <version>LATEST</version>
      </dependency>
    </dependencies>
    ```
1. Open the `common/src/main/java/com/wfsample/common/Tracing.java` file and make sure it is updated and have the following dependencies.
    ```
    import com.wavefront.opentracing.WavefrontTracer;
    import com.wavefront.opentracing.reporting.Reporter;
    import com.wavefront.opentracing.reporting.WavefrontSpanReporter;
    import com.wavefront.sdk.common.WavefrontSender;
    import com.wavefront.sdk.common.application.ApplicationTags;
    import com.wavefront.sdk.common.clients.WavefrontClientFactory;
    ```
1. Configure the application to send data to Wavefront via the Wavefront proxy.
    1. <a href="proxies_installing.html#proxy-installation">Install the Wavefront proxy</a>. For this tutorial let's install the Wavefront Proxy on Docker.
        ```
        docker run -d \
           -e WAVEFRONT_URL=https://{your_cluster_name}/api/ \
           -e WAVEFRONT_TOKEN={your_token} \
           -e JAVA_HEAP_USAGE=512m \
           -e WAVEFRONT_PROXY_ARGS="--traceListenerPorts 30000 --histogramDistListenerPorts 40000" \
           -p 2878:2878 \
           -p 4242:4242 \
           -p 30000:30000 \
           -p 40000:40000 \
           wavefronthq/proxy:latest
        ```
        <p>Replace <code>&#123;your_cluster_name&#125;</code> and <code>&#123;your_token&#125;</code> with the name of your Wavefront cluster and API token. 
        <br/><b>Note</b>: If you are not sure of your cluster name or token, open the Wavefront application UI, select <b>Browse</b> > <b>Proxies</b> > <b>Add New Proxy</b> and click the <b>Docker</b> tab. Copy the values for <code>WAVEFRONT_URL</code> and <code>WAVEFRONT_TOKEN</code>, and replace the values in the above example.</p>
    1. Open the <code>common/src/main/java/com/wfsample/common/Tracing.java</code> file and update the following:
        * Change the <code>Tracer init(String service)</code> method to return a WavefrontTracer.
        * Assign a name for the application. This will help you identify the application in Wavefront and view the data that was sent. For this example, let's use <code>foo-beachshirts</code>. You can replace <code>foo</code> with your name too.
      Your code looks like this:
      ```
      public static Tracer init(String service) {
          WavefrontClientFactory wavefrontClientFactory = new WavefrontClientFactory();
          wavefrontClientFactory.addClient("http://localhost:2878/");
          wavefrontClientFactory.addClient("http://localhost:30000/");

          WavefrontSender wavefrontSender = wavefrontClientFactory.getClient();
          String applicationName = "foo-beachshirts";
          ApplicationTags applicationTags = new ApplicationTags.Builder(applicationName,
                  service).build();
          Reporter wfSpanReporter = new WavefrontSpanReporter.Builder().
                  build(wavefrontSender);
          WavefrontTracer.Builder wfTracerBuilder = new WavefrontTracer.
                  Builder(wfSpanReporter, applicationTags);
          return wfTracerBuilder.build();
          }
      ```
1. Run `mvn clean install` from the root directory of the project.
1. Start the `shopping`, `styling`, and `delivery` services using the commands give below. Run them from the root directory of the project
    ```
    java -jar ./shopping/target/shopping-1.0-SNAPSHOT.jar server ./shopping/app.yaml
    java -jar ./styling/target/styling-1.0-SNAPSHOT.jar server ./styling/app.yaml
    java -jar ./delivery/target/delivery-1.0-SNAPSHOT.jar server ./delivery/app.yaml
    ```
1. Run the `./loadgen.sh` script to decide how often the services need to run in order to shop, style, and deliver a shirt. Let's run it every 5 seconds
    ```
    ./loadgen.sh 5
    ```

## View Data in Wavefront

You use the [Wavefront UI to visualize the trace data](tracing_ui_overview.html) that you collect from your instrumented application.

1. Click **Applications** > **Application Status** to get an overview of how the applications and services are linked, focus on a specific service, and view Request, Error, and Duration (RED) metrics for each service and the edges in the application using map.  
  * [**Application Map**](tracing_ui_overview.html#application-map) gives you an overview of how the applications and services are linked, lets you focus on a specific service, view Request, Error, and Duration (RED) metrics for each service and the tracing traffic in the application. 
  * [**Table View**](tracing_ui_overview.html#table-view) list of applications and services. You can see the Request, Error, and Duration (RED) metrics at a glance and sort the data.
  * [**Grid view**](tracing_ui_overview.html#grid-view) lists the application and services in a grid. You can see the RED metrics for each of the application’s services.

2. Click on the 
2.  **Service dashboards**

  The default, read-only dashboard for a service lets you explore trace data sent by each service in your application. 

* **Tracing Browser**

  The Traces browser supports a streamlined task flow for examining traces. You can perform trace queries, view query results, expand traces to see their member spans, and expand individual spans to see their details, without having to navigate between pages and pop-ups.