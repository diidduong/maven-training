# Maven Training
Training Course using Maven, Java, and IntelliJ

## Structure
A common Maven project structure is as followed 
```text
.
├── src
├── ├── main
├── ├── java
├── ├── ├── ├── resources
├── ├── test
├── ├── ├── java
├── ├── ├── ├── resources
├── pom.xml
```

## POM file
POM file contains project information such as dependencies, repositories, version, build, report.

### Properties

Properties manage versions, reduce duplication, streamline configuration, and keep things in sync.

```xml
<properties>
    <jackson.version>2.9.8</jackson.version>
    <slf4j.version>1.7.25</slf4j.version>
    <junit.version>5.2.0</junit.version>
    <java.version>11</java.version>
</properties>
```

### Dependencies
There are few tags that we need to understand.

- `<groupId>` top level of the company structure
- `<artifactId>` reference id by its common name
- `<version>` specific version to know in case of incident
- `<configuration>` specify transformer to modify the JAR file during the process
- `<executions>` to execute transformer at specific package

### Build

Similar to dependencies, we have `<plugin>` tag that Maven provides for configurations. See [Maven Plugins](https://maven.apache.org/plugins/).

- **Core plugin** compilation, installation, deployment, validation
- **Tool plugin** dependency, enforce (ban dependencies), jarsign (sign artifact), release
- **Packing** JAR, WAR (TomCat), EAR (large enterprise apps), SHADE

We can use command `mvn`directly to run specific build such as,

- `mvn clean` clean up project builds and cache
- `mvn build` build the project dependencies and run tests
- `mvn package` generate a `.jar` file 

### Reporting

Maven includes a great plugin to generate report of the project documentation (structure, versions, tests, etc.)

- `mvn clean package site` generate reports

> Maven site is CSS-customizable using skins, see [Available Skins](https://maven.apache.org/skins/) 

**Other reports**
  - `changelog` for documenting changes for each release
  - `stylecheck` verify that your code is adhered with standard
  - `javadoc` generate JavaDoc for developers
  - `surefire` confirm that tests are run
### Parent Pom

Parent Pom provides versioned dependencies and plugins so child pom can use. It **must be** a Pom, not a Jar.

> **Reactor** is used to build in a group of Pom and execute in order

In the parent Pom, we need to specify `<module>` that we want as a group. We also need to specify these attributes in the module so that the Parent Pom can detect.

```xml
<groupId>com.module-a</groupId>
<artifactId>module-a</artifactId>
<description>This is Module A</description>
```

### Archetype

Template for a standard creation of a Maven project. See [Maven Archetypes](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html).


## Dependency

### Scope

- Compile scope, default if not specify, transitive
- Provided scope, used in enterprise app, provide only under runtime, not transitive, not in cloud

> Transitive dependency is dependency of dependency.

### Rules
- Version **closet to a scope** will be selected
- **Dependency management** will beat closet

### Tips and Tricks
- Declare what you need. Use tool `mvn dependency:analyze` to verify.
- Validate scope
- Use parent POM for versions
- Always declar the risk to prevent others break the build

### Goal
There are specify task of action to help manage dependency better.

- `mvn dependency:analyze` provide warnings and suggestions like unused dependency
- `mvn dependency:resolve` list everything declared without looking at POM file 
- `mvn dependency:tree` generate transitive dependencies, clearer picture of all dependencies