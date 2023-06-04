---
layout: post
title:  "Protobuf to Avro Converter"
author: Gaurav
categories: [ Java ]
---

### Protobuf vs Avro
- In Protobuf there is no way to define or refer an external specification (schema) within a Protocol Buffers file.
- Avro stores the data definition in JSON format making it easy to read and interpret.
- For any schema change in Protobuf, the code needs to be regenerated, while in Avro the new schema will become part of the message itself.

The conversion from Protobuf to Avro is helpful in the scenario where we want to share the data along with the schema.

Let's start the exercise to create a Protobuf message and then convert it into Avro file.

#### Create book.proto

```protobuf
syntax = 'proto3';

package tutorial;
option java_package = 'com.tutorial';
option java_multiple_files = true;

message Book {
  string name = 1;
  string author = 2;
  string title = 3;
  bool read = 4;
}
```

### Create a build.gradle with protobuf gradle plugin

```
buildscript {
    repositories {
        mavenCentral()
    }

    dependencies {
        classpath 'com.google.protobuf:protobuf-gradle-plugin:0.8.12'
    }
}

plugins {
    id 'java'
}

apply plugin: 'com.google.protobuf'

group 'org.example'
version '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    implementation group: 'com.google.protobuf', name: 'protobuf-java', version: '3.22.3'
    implementation group: 'org.apache.avro', name: 'avro', version: '1.11.1'
    implementation group: 'org.apache.avro', name: 'avro-protobuf', version: '1.11.1'
}

sourceSets.main.java.srcDirs = ['build/generated/source/proto/main/java','src/main/java']
```

#### Create Application.java
```java
import com.google.protobuf.GeneratedMessageV3;
import org.apache.avro.Schema;
import org.apache.avro.file.DataFileWriter;
import org.apache.avro.protobuf.ProtobufData;
import org.apache.avro.protobuf.ProtobufDatumWriter;

import java.io.ByteArrayOutputStream;
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;

public class Application {
    public static void main(String[] args) throws IOException {
        Book cleanCode = Book.newBuilder()
                .setRead(true)
                .setAuthor("Robert C. Martin")
                .setTitle("Clean Code")
                .setName("Clean Code by Robert C. Martin")
                .build();
        ProtobufDatumWriter pbWriter = new ProtobufDatumWriter(cleanCode.getClass());
        Schema schema = ProtobufData.get().getSchema(cleanCode.getClass());

        File avroFile = new File("Book.avro");
        try (DataFileWriter<GeneratedMessageV3> dataFileWriter = new DataFileWriter<>(pbWriter)) {
            dataFileWriter.create(schema, avroFile);
            dataFileWriter.append(cleanCode);
            dataFileWriter.flush();
        }
    }
}
```

Following is the code to read back the .avro file:
```java
import org.apache.avro.Schema;
import org.apache.avro.file.DataFileReader;
import org.apache.avro.file.SeekableByteArrayInput;
import org.apache.avro.generic.GenericDatumReader;
import org.apache.avro.generic.GenericRecord;
import org.apache.avro.io.DatumReader;

import java.io.FileInputStream;
import java.io.IOException;

public class ReadAvro {
    public static void main(String[] args) throws IOException {
        DatumReader datumReader = new GenericDatumReader();
        GenericRecord genericRecord = null;
        FileInputStream fileInputStream = new FileInputStream("Book.avro");
        DataFileReader dataFileReader = new DataFileReader(new SeekableByteArrayInput(fileInputStream.readAllBytes()), datumReader);
        while (dataFileReader.hasNext()) {
            genericRecord = (GenericRecord) dataFileReader.next();
        }
        System.out.println(genericRecord.getSchema().getFields().size());
        for (Schema.Field field : genericRecord.getSchema().getFields()) {
            System.out.println(field.name() + ": " + genericRecord.get(field.name()));
        }
    }
}
```