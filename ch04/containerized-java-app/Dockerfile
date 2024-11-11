FROM azul/zulu-openjdk:21-jre
WORKDIR /app
COPY target/java-hello-world-0.0.1.jar java-hello-world.jar
ENTRYPOINT ["java", "-jar", "/app/java-hello-world.jar"]
EXPOSE 8080


# Building the Docker Image

# You will need to have a JDK 21 installed on your machine. Run the following command to compile the code and build the JAR file.

# ```
# $ ./mvnw package spring-boot:repackage
# ```

# Then build the image with the following command:

# ```
# $ docker build -t java-hello-world:1.0.0 .
# ```
