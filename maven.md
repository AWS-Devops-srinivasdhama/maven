# Maven – Introduction and Installation

## 1. What is Maven?

- Maven is a **Java build automation and project management tool**.
- It automates compiling source code, running tests, packaging applications, and managing dependencies.
- Maven converts Java project source code and resources into deployable packages.

## 2. Maven Package Types

| Package | Description | Real-Time Use Case |
|---|---|---|
| `.jar` | Java classes and resources packaged together | Standalone Java applications and reusable libraries |
| `.war` | Web application containing Java classes, HTML, CSS, JSP, etc. | Web applications deployed on Tomcat |
| `.ear` | Enterprise application containing multiple modules | Large enterprise applications, such as banking systems |

**Note:**
- JAR = Java Archive
- WAR = Web Application Archive
- EAR = Enterprise Archive

## 3. Similar Build Tools

- Apache Ant
- Apache Maven
- Gradle

## 4. Maven Features

- Developed in Java.
- Maintained by the Apache Software Foundation.
- Open-source build tool.
- Platform-independent.
- Mainly used for Java projects.
- Supports dependency management.
- Uses `pom.xml` as its project configuration file.

## 5. Maven Prerequisites

Before installing Maven, Java must be installed.

1. Install Java JDK (OpenJDK is sufficient).
2. Install Apache Maven.
3. Configure `JAVA_HOME`.
4. Configure Maven `PATH`.
5. Verify the installation.

## 6. Maven Installation on Linux

Maven is generally distributed as a compressed archive (`.tar.gz` or `.zip`), not as a Windows-style `.exe` installer.

Example installation directories:

- `/opt` – Common location for installing third-party software.
- `/opt/apache-maven` – Maven installation directory.

### Important Maven Directories

| Directory | Purpose |
|---|---|
| `bin` | Contains Maven executable commands such as `mvn` |
| `conf` | Contains Maven configuration files, including `settings.xml` |
| `lib` | Contains Maven libraries |

### Environment Variables

Configure Maven's environment variables so that Maven commands can be executed from any directory.

Example:

```bash
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk
export M2_HOME=/opt/apache-maven
export PATH=$PATH:$M2_HOME/bin
```

Load the updated environment:

```bash
source ~/.bash_profile
```

Verify installation:

```bash
mvn -v
```

**Real-Time Use Case:** On an AWS EC2 Linux server, configure Maven PATH so developers and Jenkins can execute Maven commands without navigating to Maven's `bin` directory.

## 7. Maven Local Repository

- Maven downloads dependencies and stores them in the local repository.
- Default local repository location:

```bash
~/.m2/repository
```

- Maven checks the local repository before downloading dependencies again from remote repositories.

### Configure a Custom Local Repository

We can create our own directory to store Maven dependencies.

Example:

```bash
mkdir -p /opt/maven-repo
```

Update `settings.xml`:

```xml
<settings>
    <localRepository>/opt/maven-repo</localRepository>
</settings>
```

**Real-Time Use Case:** In a company environment, a custom local repository can be configured to store dependencies in a specific location instead of the default user home directory.

## 8. POM.xml

- `pom.xml` stands for Project Object Model.
- It is the main configuration file of a Maven project.
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

## 9. Clone and Build a Maven Project

Clone the project from GitHub:

```bash
git clone <repository-url>
```

Navigate into the project directory:

```bash
cd project-directory
```

Check for the POM file:

```bash
ls
```

The project directory should contain `pom.xml`.

## 10. Maven Build Lifecycle Commands

### A. mvn clean

**Description:** Removes the previously generated `target` directory and build output.

```bash
mvn clean
```

**Real-Time Use Case:** Before starting a fresh build in Jenkins, remove old build files to avoid stale build outputs.

### B. mvn package

**Description:** Executes the lifecycle phases up to package:

`validate → compile → test → package`

Creates the application package (`.jar`, `.war`, etc.).

```bash
mvn package
```

**Real-Time Use Case:** Build a Java web application and generate a WAR file to deploy on Tomcat.

### C. mvn install

**Description:** Executes the lifecycle phases up to install:

`validate → compile → test → package → verify → install`

Installs the generated artifact into the local Maven repository.

```bash
mvn install
```

**Real-Time Use Case:** A developer builds a common Java library and installs it in the local repository so another local Maven project can use it as a dependency.

### D. mvn deploy

**Description:** Executes the lifecycle phases up to deploy:

`validate → compile → test → package → verify → install → deploy`

Uploads the generated artifact to a configured remote repository, such as Nexus.

```bash
mvn deploy
```

**Real-Time Use Case:** Jenkins builds a Java application and uploads the generated WAR or JAR to Nexus for centralized artifact management.

## 11. Skip Tests During Maven Build

### Skip test execution

```bash
mvn install -DskipTests
```

- Compiles test source code.
- Skips executing the tests.
- Continues with packaging and installation.

### Skip test compilation and execution

```bash
mvn package -Dmaven.test.skip=true
```

- Skips test compilation.
- Skips test execution.
- Continues with packaging.

**Real-Time Use Case:** During a quick development build, tests may be skipped temporarily to generate an artifact faster. In production CI/CD pipelines, tests should normally be executed.

## 12. Maven CI/CD Workflow

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

**Important:** The actual order of SonarQube analysis and Nexus upload depends on the Jenkins pipeline configuration. Maven `deploy` uploads artifacts to the configured remote Maven repository; it does not deploy applications directly to Tomcat.
