##Dockerizing a simple web java maven application after bulding the artifact
1. Run the command to generate a sample project(Pre-requisite java and maven should be installed and JAVA_HOME and M2_HOME should be defined as path variable)
mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=dockerize-project -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false

2. After successful build, go to POM.xml and replace below: 
<plugin>
    <artifactId>maven-jar-plugin</artifactId>     
    <version>3.0.2</version>
</plugin>

with

<plugin>
    <!-- Build an executable JAR -->
<groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-jar-plugin</artifactId>
          <version>3.1.0</version>
      <configuration>
        <archive>
          <manifest>
                <addClasspath>true</addClasspath>
                <classpathPrefix>lib/</classpathPrefix>
                <mainClass>com.mycompany.app.App</mainClass>
          </manifest>
        </archive>
      </configuration>
</plugin

# above step will build an execuatble jar file

3.Now write the code in App.java for web packgaing your application.

4. Now add below to your build:
 <build>
  <finalName>dockerize-maven</finalName>
</build>

5. Then run $mvn install it will create a exceutable jar file at target directory.

6. Now create a file name "Dockerfile" with no extension in the parent directory of your project and write below code to in this file:

FROM openjdk:8
ADD target/my-maven-docker-project.jar my-maven-docker-project.jar
ENTRYPOINT ["java", "-jar","my-maven-docker-project.jar"]
EXPOSE 8080

7. After this, you will need to build your docker file, after building it will generate docker image of your docker-project

8. Now you can run below command to build your docker-project:
docker build -t dockerize-maven.jar .

9. Now run above build image by
 docker run -p 8085:8085 my-maven-docker-project.jar
