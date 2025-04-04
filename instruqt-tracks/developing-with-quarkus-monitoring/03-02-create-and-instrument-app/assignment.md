---
slug: 02-create-and-instrument-app
id: ryfzevkxyo0x
type: challenge
title: Topic 2 - Adding the Prometheus extension
notes:
- type: text
  contents: Topic 2 - Adding the Prometheus extension
tabs:
- id: tdiwhu6shee1
  title: Terminal 1
  type: terminal
  hostname: crc
- id: dk8yjtgavwms
  title: Terminal 2
  type: terminal
  hostname: crc
- id: al7x4nsiqswe
  title: Visual Editor
  type: code
  hostname: crc
  path: /root/projects/quarkus/primes
difficulty: basic
timelimit: 200
---

In this topic you will get the Quarkus demonstration application up and running. The installation will include the Micrometer metrics library. Then you'll exercise the default RESTful endpoint published by the demonstration application.

The first thing you'll do is verify that the preinstalled Java Runtime Environment (JRE) is operational.

# Inspect the Java runtime and create basic project

`Step 1:` Run the following command in the **Terminal 1** tab to verify that the Java Runtime (JRE) is operational:

```
$JAVA_HOME/bin/java --version
```

You get output similar to the following. The versions and dates displayed may be slightly different:

```console
openjdk 17.0.6 2023-01-17
OpenJDK Runtime Environment Temurin-17.0.6+10 (build 17.0.6+10)
OpenJDK 64-Bit Server VM Temurin-17.0.6+10 (build 17.0.6+10, mixed mode, sharing)
```

If the command fails, wait a few moments and try again. The JRE is installed in a background process and may take a few seconds to get up and running depending on system load.

Next, you'll create a basic Quarkus **Hello World** application that includes the additional monitoring extensions.

# Create the basic project

`Step 2:` Run the following command in the **Terminal 1** tab to install the basic Quarkus **Hello World** application along with the additional monitoring extensions:

```
cd /root/projects/quarkus && \
mvn io.quarkus:quarkus-maven-plugin:3.15.1:create \
    -DprojectGroupId=org.acme \
    -DprojectArtifactId=primes \
    -DclassName="org.acme.quickstart.GreetingResource" \
    -Dpath="/hello"
```

You'll see a lot of screen output. When the installation process completes successfully you'll see output similar to the following:

```
[INFO] ========================================================================================
[INFO] Your new application has been created in /var/roothome/projects/quarkus/primes
[INFO] Navigate into this directory and launch your application with mvn quarkus:dev
[INFO] Your application will be accessible on http://localhost:8080
[INFO] ========================================================================================
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
```

Add the **micrometer-registry-prometheus** extension to enable the Micrometer metric in your Quarkus application. Run the following command:

```
mvn quarkus:add-extension -Dextensions="micrometer-registry-prometheus" -f primes
```

The `mvn` installation process uses the Quarkus Maven Plugin to generate a basic Maven project. The application's files will be stored in the `primes` subdirectory. The `micrometer-registry-prometheus` extension will publish operation metrics at the `/q/metrics` RESTful endpoint.

# Starting the demonstration application

`Step 3:` Run the following command in the **Terminal 1** tab to navigate to the demonstration application's working directory:

```
cd /root/projects/quarkus/primes && pwd
```

You'll see the following output:

```
/root/projects/quarkus/primes
```

Let's begin [Live Coding](https://quarkus.io/vision/developer-joy#live-coding).

----

`Step 4:`  Run the following command in the **Terminal 1** tab to start the app in Live Coding mode:

```
mvn quarkus:dev -Dquarkus.http.host=0.0.0.0
```

You will see the following output:

```console
__  ____  __  _____   ___  __ ____  ______
 --/ __ \/ / / / _ | / _ \/ //_/ / / / __/
 -/ /_/ / /_/ / __ |/ , _/ ,< / /_/ /\ \
--\___\_\____/_/ |_/_/|_/_/|_|\____/___/
INFO  [io.quarkus] (Quarkus Main Thread) primes 1.0.0-SNAPSHOT on JVM (powered by Quarkus x.xx.x.) started in x.xxxs. Listening on: http://localhost:8080
INFO  [io.quarkus] (Quarkus Main Thread) Profile dev activated. Live Coding activated.
INFO  [io.quarkus] (Quarkus Main Thread) Installed features: [cdi, micrometer, resteasy]

--
Tests paused, press [r] to resume, [h] for more options>
```

|NOTE|
|----|
|New dependencies may be downloaded via Maven the first time you build the demonstration application. This initial download should only happen once. After that, intialization will go faster.|


The demonstration application is now running locally within this track's Linux container. The next step is to exercise the application.

# Exercising the demonstration application

`Step 5:` Click the **Terminal 2** tab in the horizontal menu bar over the console window to the left. An additional terminal window will appear.

----

`Step 6:` Run the following command in the **Terminal 2** tab to exercise the `/hello` endpoint:

```
cd /root/projects/quarkus/primes && \
  curl http://localhost:8080/hello
```

|NOTE:|
|----|
|You may need to run this command again in case it doesn't execute properly on first run.|

You will see the following output:

```console
Hello from RESTEasy Reactive
```

# Exercising the metrics endpoint

Once the demonstration application is up and running, you will be able to see the raw metrics generated from Quarkus apps immediately.

----
`Step 7:` Run the following command in the **Terminal 2** tab to exercise the `/q/metrics` endpoint. You will see some operational metrics:

```
curl http://localhost:8080/q/metrics
```

You will see output similar to what's shown in the snippet below:

```console
# TYPE jvm_memory_max_bytes gauge
# HELP jvm_memory_max_bytes The maximum amount of memory in bytes that can be used for memory management
jvm_memory_max_bytes{area="heap",id="G1 Survivor Space"} -1.0
jvm_memory_max_bytes{area="heap",id="G1 Old Gen"} 8.43055104E9
jvm_memory_max_bytes{area="nonheap",id="Metaspace"} -1.0
jvm_memory_max_bytes{area="nonheap",id="CodeCache"} 5.0331648E7
jvm_memory_max_bytes{area="heap",id="G1 Eden Space"} -1.0
jvm_memory_max_bytes{area="nonheap",id="Compressed Class Space"} 1.073741824E9
.
.
.
```

The output shown above is the raw metrics in the [OpenMetrics](https://github.com/OpenObservability/OpenMetrics/blob/main/specification/OpenMetrics.md) format. (The demonstration application is using a [Prometheus registry](https://hexdocs.pm/prometheus_ex/Prometheus.Registry.html).)

The output describes some basic metrics that Prometheus gathers from the demonstration application when it's deployed into an OpenShift cluster. Also, preliminary values for those metrics are provided.

Be advised, it is possible to use systems other than Prometheus to consume metrics. For example, there's StackDriver, which is part of the [Quarkiverse](https://github.com/quarkiverse/quarkiverse-micrometer-registry).

You can also use other pre-packaged registries for other [Application Performance Monitoring](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/performance_tuning_guide/chap-red_hat_enterprise_linux-performance_tuning_guide-performance_monitoring_tools) (APM) systems. Consult the [Quarkus Micrometer Guide](https://quarkus.io/guides/micrometer) for more detail.

# Congratulations!

You've installed and run the demonstration application. Also, you exercised the default `/hello` RESTful endpoint. Finally, you viewed preliminary metrics gathered by Prometheus by exercising the `/q/metrics` endpoint.

You are now ready to add behavior to the demonstration application and enhance Prometheus to gather additional metrics.

----

**NEXT:** Gathering additional metrics from the demonstration application
