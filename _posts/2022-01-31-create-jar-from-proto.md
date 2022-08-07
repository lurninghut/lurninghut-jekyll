---
layout: post
title:  "Create jar from Proto files"
author: Gaurav
categories: [ Java ]
image: assets/images/create-jar-from-proto/header-1.png
---
Protobuf is one of the formats used by programs to communicate with each other over a network. 
It is Google's IDL (Interface definition language) to describe the structure of any data which then can be used to generate source code to parse a stream of bytes that represent that structured data.

**Demo**

Following is a demo of how to create a jar file out of proto files.

[<img src="/assets/images/create-jar-from-proto/demo.png" height="400px" width="600px"/>](https://youtu.be/Hsd-yznn1ms)

In this blog we will use a gradle plugin to generate Java class files from a .proto file.

Create a sample gradle project in intellij:

![Screenshot 1](/assets/images/create-jar-from-proto/Screenshot-1.png)

Add following 'buildscript' config in the gradle:

<pre>
    buildscript {
        repositories {
        mavenCentral()
    }
    dependencies {
        classpath 'com.google.protobuf:protobuf-gradle-plugin:0.8.12'
        }
    }
</pre>

Add 'apply plugin':

<pre>
    apply plugin: 'com.google.protobuf'
</pre>

Following are the tasks that will be added to the gradle tasks list:

<img src="/assets/images/create-jar-from-proto/Screenshot-2.png" height="400px" width="600px"/>

Below is an example of a sample cart.proto file added under 'proto' folder inside 'main' directory. Note that 'java_package' and 'java_multiple_files' are attributes to provide guidance to the protobuf compiler while generating class files.

<img src="/assets/images/create-jar-from-proto/Screenshot-3.png" height="400px" width="600px"/>

Now when we run the generateProto task as shown below, we can see it has generated the corresponding Java files which will be present under the 'generated' folder inside build directory.

<img src="/assets/images/create-jar-from-proto/Screenshot-4.png" height="400px" width="600px"/>

Add following dependency in the project:

<pre>
dependencies {
    implementation 'com.google.protobuf:protobuf-java:3.11.4'
}
</pre>

Once added, run the 'jar' task in gradle and check the build/libs directory. The jar task will generate the jar with compiled class files:

<img src="/assets/images/create-jar-from-proto/Screenshot-5.png" height="400px" width="600px"/>

The generated jar can be used by any java application to read or create the corresponding proto message in JVM.



