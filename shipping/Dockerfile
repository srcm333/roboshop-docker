FROM node:20
#create /app and set the directory to /app
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn clean package
RUN mvn target/shipping-1.0.jar shipping.jar
EXPOSE 8080
CMD ["java", "-jar" "shipping.jar"]