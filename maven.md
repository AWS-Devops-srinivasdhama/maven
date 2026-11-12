# MAVEN – COMPLETE NOTES

# 1. Introduction to Maven

- Maven is a Java build automation and project management tool.
- It automates compiling source code, running tests, packaging applications, and managing dependencies.
- Maven converts Java source code and resources into deployable packages.
- Maven is developed in Java and maintained by the Apache Software Foundation.
- It is open-source and platform-independent.
- Maven is mainly used for Java projects.

**Similar Build Tools:**
- Apache Ant
- Apache Maven
- Gradle

**Real-Time Use Case:** Developers use Maven to automate the build process instead of manually compiling Java code and creating JAR or WAR files.

# 2. Maven Package Types

| Package | Description | Real-Time Use Case |
|---|---|---|
| JAR | Java classes and resources packaged together | Standalone applications and reusable Java libraries |
| WAR | Web application containing Java classes, HTML, CSS, JSP, etc. | Web applications deployed on Tomcat |
| EAR | Enterprise application containing multiple modules | Large enterprise applications such as banking systems |

- JAR – Java Archive
- WAR – Web Application Archive
- EAR – Enterprise Archive

# 3. Maven Prerequisites

Before installing Maven, Java must be installed.

1. Install Java JDK (OpenJDK is sufficient).
2. Install Apache Maven.
3. Configure JAVA_HOME.
4. Configure Maven PATH.
5. Verify the installation.

**Java version:** Maven requires a compatible JDK. Check the Maven version's Java requirements before installation.

# 4. Maven Installation on Linux

Maven is generally distributed as a compressed archive (`.tar.gz` or `.zip`), not as a Windows-style `.exe` installer.

Common installation directory:

```bash
cd /opt
```
1.Launch an EC2 machine & connect to that machine
2.sudo su -  [switch to root user]
3.sudo yum install java-21-openjdk-devel -y
4.javac -version
#Login as a root user
5.Download the Maven Software (always go with officialdoc)
wget https://dlcdn.apache.org/maven/maven-3/3.9.10/binaries/apache-maven-3.9.10-bin.zip


Example directory structure:

```text
/opt/apache-maven/
       |
       |-- bin/
       |-- conf/
       |-- lib/
```

## Important Maven Directories

| Directory | Purpose |
|---|---|
| bin | Contains Maven executable commands such as mvn |
| conf | Contains Maven configuration files, including settings.xml |
| lib | Contains Maven libraries |

## Configure Environment Variables

Example:

```bash
export JAVA_HOME=/usr/lib/jvm/java-21-openjdk
export M2_HOME=/opt/apache-maven
export PATH=$PATH:$M2_HOME/bin
```

Load the updated environment:

```bash
source ~/.bash_profile
```

Verify Maven installation:

```bash
mvn -v
```

**Real-Time Use Case:** On an AWS EC2 Linux server, configure Maven PATH so developers and Jenkins can execute Maven commands from any directory.

# 5. Maven Local Repository

- Maven downloads dependencies and stores them in the local repository.
- Default location:

```bash
~/.m2/repository
```

- Maven checks the local repository before downloading dependencies again from remote repositories.

## Configure Custom Local Repository

Create a directory:

```bash
mkdir -p /opt/maven-repo
```

Open Maven settings:

```bash
vi /opt/apache-maven/conf/settings.xml
```

Configure:

```xml
<settings>
    <localRepository>/opt/maven-repo</localRepository>
</settings>
```

**Real-Time Use Case:** Organizations can configure a custom local repository to store dependencies in a specific location rather than using the default user home directory.

# 6. POM.xml

- POM stands for Project Object Model.
- `pom.xml` is the main configuration file of a Maven project.
- It contains project information, dependencies, plugins, build configurations, and packaging type.

Example:

```xml
<project>
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>my-application</artifactId>
    <version>1.0</version>
    <packaging>war</packaging>
</project>
```

**Real-Time Use Case:** Maven reads `pom.xml` to identify the project, download dependencies, compile source code, and generate the required package.

# 7. Maven Project Coordinates

Maven identifies projects using three main coordinates:

| Coordinate | Description | Example |
|---|---|---|
| groupId | Organization or company name | com.amazon |
| artifactId | Project name | payment-service |
| version | Project version | 1.0.0 |

Example:

```xml
<groupId>com.amazon</groupId>
<artifactId>payment-service</artifactId>
<version>1.0.0</version>
```

**Real-Time Use Case:** Maven uses these coordinates to identify different applications and versions in Nexus.

# 8. Maven Dependencies

- Dependencies are external libraries required by a Java project.
- Maven automatically downloads dependencies from configured repositories.
- Dependencies are declared inside `pom.xml`.

Example:

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>6.1.0</version>
    </dependency>
</dependencies>
```

**Real-Time Use Case:** If a Java application uses Spring Framework, Maven downloads the required Spring libraries automatically.

# 9. Maven Dependency Scopes

Dependency scope defines where and when a dependency is required.

| Scope | Description |
|---|---|
| compile | Available during compilation and runtime; default scope |
| provided | Required for compilation but expected from the runtime environment |
| runtime | Required at runtime, not compilation |
| test | Required only during testing |
| system | Uses a locally specified JAR; generally avoid it |
| import | Imports dependency management from a BOM |

**Real-Time Use Case:** A web application uses the Servlet API with `provided` scope because Tomcat supplies the Servlet API at runtime.

# 10. Maven Repositories

Maven uses three main types of repositories:

| Repository | Description |
|---|---|
| Local repository | Stores dependencies on the developer's machine |
| Central repository | Public repository maintained by Maven |
| Remote repository | Organization-specific repository such as Nexus |

**Real-Time Use Case:** In an organization, Nexus acts as a centralized repository for storing and sharing internal and external dependencies.

# 11. Clone and Build a Maven Project

Clone the project from GitHub:

```bash
git clone <repository-url>
```

Navigate into the project directory:

```bash
cd project-directory
```

Check project files:

```bash
ls
```

The project directory should contain `pom.xml`.

**Real-Time Use Case:** Jenkins checks out the source code from GitHub and executes Maven commands inside the project directory.

# 12. Maven Build Lifecycles

Maven has three built-in lifecycles:

1. Clean lifecycle – Cleans previous build files.
2. Default lifecycle – Builds and deploys the project.
3. Site lifecycle – Generates project documentation.

## Default Lifecycle Phases

```text
validate
   |
compile
   |
test
   |
package
   |
verify
   |
install
   |
deploy
```

**Important:** When you execute a lifecycle phase, Maven executes all preceding phases in that lifecycle.

# 13. Important Maven Commands

## A. mvn -v

**Description:** Displays the installed Maven version and Java details.

```bash
mvn -v
```

**Real-Time Use Case:** Verify Maven and Java configuration before running a Jenkins build.

## B. mvn clean

**Description:** Removes the previously generated `target` directory and build output.

```bash
mvn clean
```

**Real-Time Use Case:** Before starting a fresh build in Jenkins, remove old build files to avoid stale build outputs.

## C. mvn compile

**Description:** Compiles the project's main source code.

```bash
mvn compile
```

**Real-Time Use Case:** Verify that Java source code compiles successfully before proceeding with tests.

## D. mvn test

**Description:** Compiles and executes unit tests.

```bash
mvn test
```

**Real-Time Use Case:** Jenkins executes unit tests to identify coding errors before generating the application package.

## E. mvn package

**Description:** Executes the lifecycle phases up to package:

```text
validate → compile → test → package
```

Creates the application package (`.jar`, `.war`, etc.).

```bash
mvn package
```

**Real-Time Use Case:** Build a Java web application and generate a WAR file to deploy on Tomcat.

## F. mvn verify

**Description:** Executes the lifecycle phases up to verify and performs additional checks configured in the build.

```bash
mvn verify
```

**Real-Time Use Case:** Verify that the application package passes configured integration and quality checks before installation.

## G. mvn install

**Description:** Executes the lifecycle phases up to install:

```text
validate → compile → test → package → verify → install
```

Installs the generated artifact into the local Maven repository.

```bash
mvn install
```

**Real-Time Use Case:** A developer builds a common Java library and installs it in the local repository so another local Maven project can use it as a dependency.

## H. mvn deploy

**Description:** Executes the lifecycle phases up to deploy:

```text
validate → compile → test → package → verify → install → deploy
```

Uploads the generated artifact to a configured remote repository, such as Nexus.

```bash
mvn deploy
```

**Real-Time Use Case:** Jenkins builds a Java application and uploads the generated WAR or JAR to Nexus for centralized artifact management.

# 14. Skip Tests During Maven Build

## Skip Test Execution

```bash
mvn install -DskipTests
```

- Compiles test source code.
- Skips executing tests.
- Continues with packaging and installation.

## Skip Test Compilation and Execution

```bash
mvn package -Dmaven.test.skip=true
```

- Skips test compilation.
- Skips test execution.
- Continues with packaging.

**Real-Time Use Case:** During a quick development build, tests may be skipped temporarily to generate an artifact faster. In production CI/CD pipelines, tests should normally be executed.

# 15. Maven Plugins

- Plugins provide additional functionality to Maven.
- Maven uses plugins to compile code, execute tests, package applications, and generate reports.
- Plugins contain goals.

| Plugin | Purpose |
|---|---|
| maven-compiler-plugin | Compiles Java source code |
| maven-surefire-plugin | Executes unit tests |
| maven-jar-plugin | Creates JAR packages |
| maven-war-plugin | Creates WAR packages |
| maven-clean-plugin | Cleans build output |

**Real-Time Use Case:** During a Jenkins build, Maven plugins compile Java code, execute tests, and generate WAR files automatically.

# 16. Maven Profiles

- Profiles allow different build configurations for different environments.
- Profiles can be configured in `pom.xml` or `settings.xml`.

Examples:

- Dev
- QA
- UAT
- Production

Run a profile:

```bash
mvn package -Pdev
```

**Real-Time Use Case:** A Java application can use different database URLs and environment configurations for Dev, QA, and Production builds.

# 17. Maven Wrapper

- Maven Wrapper allows developers to execute Maven without manually installing Maven on every machine.
- It downloads and uses the Maven version configured for the project.

Command:

```bash
./mvnw clean package
```

**Real-Time Use Case:** Jenkins can build a project using its configured Maven Wrapper version, helping maintain consistent builds across environments.

# 18. Important Maven Commands – Quick Reference

| Command | Purpose |
|---|---|
| `mvn -v` | Checks Maven version |
| `mvn clean` | Removes target directory |
| `mvn compile` | Compiles source code |
| `mvn test` | Executes tests |
| `mvn package` | Creates JAR/WAR package |
| `mvn verify` | Verifies the build |
| `mvn install` | Installs artifact into local repository |
| `mvn deploy` | Uploads artifact to remote repository |
| `mvn dependency:tree` | Displays dependency hierarchy |
| `mvn help:effective-pom` | Displays effective POM |
| `mvn help:effective-settings` | Displays effective settings |

# 19. Maven Build Output

After executing:

```bash
mvn package
```

Maven generates build output inside the `target` directory.

Example:

```text
project-directory/
    |
    |-- src/
    |-- pom.xml
    |-- target/
          |
          |-- classes/
          |-- test-classes/
          |-- maven-web-application.war
```

**Real-Time Use Case:** Jenkins picks up the generated WAR file from the `target` directory and uploads it to Nexus or deploys it to Tomcat.

# 20. Maven CI/CD Workflow

```text
Developer
    |
    v
GitHub (Source Code)
    |
    v
Jenkins (Build Automation)
    |
    v
Maven (Build and Package)
    |
    v
WAR / JAR File
    |
    v
SonarQube (Code Quality)
    |
    v
Nexus (Artifact Repository)
    |
    v
Tomcat (Application Deployment)
```

**Important:**

- Maven `package` generates the application artifact.
- Maven `deploy` uploads artifacts to a configured remote repository.
- Nexus stores and manages application artifacts.
- Tomcat runs compatible Java web applications.
- Maven does not directly deploy applications to Tomcat.

**Note:** The actual order of SonarQube analysis and Nexus upload depends on the Jenkins pipeline configuration.

# 21. End-to-End Maven Practical Exercise

Practice the following steps:

1. Clone a Java project from GitHub.
2. Navigate to the project directory.
3. Check `pom.xml`.
4. Configure Java and Maven.
5. Execute `mvn clean`.
6. Execute `mvn compile`.
7. Execute `mvn test`.
8. Execute `mvn package`.
9. Verify the WAR file inside `target/`.
10. Upload the WAR file to Nexus.
11. Download the WAR file from Nexus.
12. Deploy the WAR file on Tomcat.
13. Access the application through the browser.

**Final Goal:** Understand how Maven integrates with GitHub, Jenkins, SonarQube, Nexus, and Tomcat in a real-time DevOps CI/CD pipeline.
