## Maven Install:

Maven is a powerful build automation and project management tool, primarily used for Java projects, but it can be used with other programming languages as well. Developed by the Apache Software Foundation, Maven simplifies the build process, dependency management, and project configuration, making it easier to manage complex projects.


- **Build tools**: Maven work java based application. 
- **Project management tools**: that is based on POM (project object model). 
- **Dependency management**: Dependencies are external Java libraries required for Project and repositories are directories of packaged JAR files. 
- **Build Life Cycles, Phases and Goals**: A build life cycle consists of a sequence of build phases, and each build phase consists of a sequence of goals. Maven command is the name of a build lifecycle, phase or goal.
- Maven takes care of processes like release, distribution, reporting, builds, documentation, and SCMs. 



### POM File:

Maven projects are configured using a **Project Object Model**, which is stored in a `pom.xml` file.
- Describes a project
- Name and version, artifact type, source code locations
- Dependencis: download 3rd party library
- Plugins: configuration
- Profiles (alternate build configurations)
- Uses XML by deafult.




### Maven Phase / Default Life cycle:

Most important phases in the default build lifecycle:

- **prepare-resources**: 
- **validate**: validates if the project is correct and if all necessary informations is available.
- **compile**: compile the source code of the project.
- **test-compile**: compile the test source code
- **test**: test the compiled source code using a suitable unit testing framework. These tests should not require the code be packaged or deployed.  
- **package**: package compiled source code into the distributable format (jar, war)
- **integration-test** (pre and post): process and deploy the package if needed to run integration tests.
- **verify**: run any checks to verify the package is valid and meets quality criteria
- **install**: install the package into the local repository, for use as a dependency in other projects locally. 
- **deploy**: copy the package to the remote repository for sharing with other developers and projects.


There are two other Maven lifecycles of note beyond the default list above. They are:

- Clean Lifecycle: cleans up artifacts created by prior builds
    - pre-clean: Executes actions before cleaning, typically used for custom cleanup tasks.
    - clean: Removes the target directory, where Maven stores build output (compiled code, JARs, etc.).
    - post-clean: Executes actions after the cleaning phase, such as removing temporary files.

- Site Lifecycle: generates site documentation for this project
    - pre-site: Executes actions before generating the site (e.g., preparation tasks).
    - site: Generates the project’s site, including reports, Javadocs, and other documentation.
    - post-site: Executes actions after generating the site (e.g., custom reporting tasks).
    - site-deploy: Deploys the generated site to a server, so others can view it.


#### Prerequisites:
- Maven requires Java, so if it’s not already installed, install it first:


```
yum install java-1.8.0-openjdk-devel -y

yum install java-11-openjdk-devel -y
```


```
java -version
```



### Maven install on Centos-7 (install default 3.0.5)

```
yum install maven -y
```



```
mvn -version


	Apache Maven 3.0.5 (Red Hat 3.0.5-17)
```



### Download Maven Binary:

```
cd /opt

wget https://dlcdn.apache.org/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.zip
```


```
unzip apache-maven-3.8.8-bin.zip

mv apache-maven-3.8.8 maven
```


```
ll /opt/maven

drwxr-xr-x   2 root root   97 Mar  8  2023 bin
drwxr-xr-x   2 root root   76 Mar  8  2023 boot
drwxr-xr-x   3 root root   63 Mar  8  2023 conf
drwxr-xr-x   4 root root 4.0K Mar  8  2023 lib
-rw-r--r--   1 root root  17K Mar  8  2023 LICENSE
-rw-r--r--   1 root root 5.1K Mar  8  2023 NOTICE
-rw-r--r--   1 root root 2.6K Mar  8  2023 README.txt
```


### Set Up Environment Variables:

```
vim /etc/profile.d/maven.sh


export M2_HOME=/opt/maven
export PATH=$M2_HOME/bin:$PATH
```


```
source /etc/profile.d/maven.sh
```


```
echo $M2_HOME
```



#### Verify Installation: 

```
mvn --version

Apache Maven 3.8.8 (4c87b05d9aedce574290d1acc98575ed5eb6cd39)
Maven home: /opt/maven
Java version: 11.0.23, vendor: Red Hat, Inc., runtime: /usr/lib/jvm/java-11-openjdk-11.0.23.0.9-2.el7_9.x86_64
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "3.10.0-1160.99.1.el7.x86_64", arch: "amd64", family: "unix"
```




---
---


## Creating a New Maven Project:

This command generates a new Maven project with a default directory structure and `pom.xml` file.

- `archetype:generate` : This goal is used to create a new Maven project from a template called an "**archetype**."
- `groupId` : The groupId is typically a domain name (`com.example`) in reverse, representing the organization or project’s root package. This will be the base package for your project.
- `artifactId` : The artifactId is the name of the project or module. Maven uses this to create the directory structure. On the build `app.jar` will be created arbitrary name of the project.
- `archetypeArtifactId` : This specifies the archetype to use. `maven-archetype-quickstart` is a simple template that includes a basic directory structure, a sample Java class, and a test file.
- `archetypeVersion` :
- `interactiveMode` : By setting this to **false**, Maven will skip the interactive prompts, allowing the command to run without asking for input.




_This is the most common build invocation/interactive mode:_

```
mvn archetype:generate
```


Or,


```
mvn archetype:generate -DgroupId=com.example -DartifactId=app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

mvn archetype:generate -DgroupId=com.example -DartifactId=app -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false
```




Running this command in the terminal will generate a new Maven project in the current directory, with the following structure:

```
app/
├── pom.xml
└── src
    ├── main
    │   └── java
    │       └── com
    │           └── example
    │               └── App.java
    └── test
        └── java
            └── com
                └── example
                    └── AppTest.java
```



```
cd app
```



```
cat src/main/java/com/example/App.java


package com.example;

/**
 * Hello world!
 *
 */
public class App
{
    public static void main( String[] args )
    {
        System.out.println( "Hello World!" );
    }
}

```




_Compile the Project Using Maven:_

```
mvn compile
```



_Run the Project Using Maven:_

```
mvn exec:java -Dexec.mainClass="com.example.App"
```



This will run the main method of the App class, and you should see the output: 

```
Hello World!
```




### Building a Maven Project:

Once your project is set up, you can build it using Maven commands.

- `mvn clean`			    : cleans maven project by delete the 'target' directory
- `mvn compile`			    : compile the source Java classes of the project
- `mvn test`			    : used to run the test cases of the project
- `mvn package`			    : builds the maven project & packages them into JAR, WAR

- `mvn install`		        : Installing the Package in Local Repository and to the local Maven repository in `~/.m2/repository`
- `mvn deploy`			    : artifactory copies the final package to the remote repository

- `mvn clean install`	    : we'll clean the project first by running the clean 
- `mvn clean package`	    : compile Jar/War
- `mvn compiler:compile`    : compiles the java source classes of the maven project
- `mvn dependency:tree`	    : 
- `mvn verify`			    : build the project, runs all the test cases and run any checks on the results of the integration tests to ensure quality criteria are met.
- `mvn clean package -Dmaven.test.skip=true`    : 
- `mvn clean package -DskipTests`               : 



Building a Maven project involves a series of commands that compile, test, and package the code into a deployable artifact. The pom.xml file is central to the build process and specifies project dependencies, plugins, and other configuration settings. Maven automates much of the build lifecycle, making it easier to manage dependencies, automate testing, and package applications.


