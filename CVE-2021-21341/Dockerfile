FROM maven:3.8.1-amazoncorretto-8

WORKDIR /tmp
COPY ./ /tmp/
RUN mvn package
CMD ["java","-jar","target/xstream-1.0-SNAPSHOT.jar"]