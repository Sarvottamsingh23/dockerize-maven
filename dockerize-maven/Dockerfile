FROM openjdk:8
CMD ["mvn" "install"]
ADD target/dockerize-maven.jar dockerize-maven.jar
ENTRYPOINT ["java", "-jar","dockerize-maven.jar"]
EXPOSE 8085