# gradle-grpc-api-plugin

A highly opinionated plugin for defining GRPC APIs in a gradle project with minimal configuration.
Adding the plugin to the package is enough to set up the protobuf gradle plugin to generate code,
including GRPC stubs. A descriptor set with docstrings is also generated for use in documentation
services (e.g., armeria).

## Usage

This plugin is currently only in JCenter - it will also be published to the Gradle Plugin Portal
soon and will then support the new plugin IDL.

In the meantime, use the old plugin syntax, making sure ```jcenter()``` is in the repositories.

```groovy

buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'org.curioswitch.curiostack:gradle-grpc-api-plugin:0.0.1'
    }
}

apply plugin: 'org.curioswitch.gradle-grpc-api-plugin'
```

The plugin adds the Spring dependency-management-plugin for resolving the version of GRPC and
protobuf you would like to use. Specify the versions with something like

```groovy
dependencyManagement {
    dependencies {
        dependency 'com.google.protobuf:protoc:3.2.0'
        dependencySet(group: 'io.grpc', version: '1.2.0') {
            entry 'grpc-core'
            entry 'grpc-protobuf'
            entry 'grpc-stub'
        }
    }
}

```

This is enough to set up the protobuf gradle plugin appropriately. Add a ```proto``` subdirectory 
under ```main``` and add some proto files, and you're ready to go.

An example usage can be found in [curio-auth-api](https://github.com/curioswitch/curiostack/blob/master/auth/api/build.gradle).
The buildscript configuration is in the [top-level build file](https://github.com/curioswitch/curiostack/blob/master/build.gradle#L41).