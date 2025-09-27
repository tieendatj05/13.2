# ---- Build stage ----
FROM maven:3.9.6-eclipse-temurin-17 AS build
WORKDIR /app

# copy và cache dependency
COPY pom.xml .
RUN mvn -q -DskipTests=true dependency:go-offline

# copy src và build
COPY src ./src
RUN mvn -q -DskipTests=true package

# ---- Runtime stage ----
FROM tomcat:10.1-jdk17-temurin
# dọn app mặc định
RUN rm -rf /usr/local/tomcat/webapps/*
# copy WAR đã build thành ROOT.war
COPY --from=build /app/target/*.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080
CMD ["catalina.sh","run"]
