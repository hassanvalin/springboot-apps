---
Creating basic Maven basic project which creates a jar
mvn archetype:generate -DgroupId=com.spring -DartifactId=first_spring_boot -DarchetypeArtifactId=maven-archetype-quickstart -Dversion=1.0.0-SNAPSHOT

Creating basic Maven webapp project which creates a war
mvn archetype:generate -DgroupId=com.spring -DartifactId=first_spring_boot -DarchetypeArtifactId=maven-archetype-webapp -Dversion=1.0.0-SNAPSHOT


---
1. Clone the repo and run 'mvn clean init' that creates a jar file under target folder
2. Dockerfile has been created to create a container with openjdk and place the jar file from target folder to /app in container
3. Created docker image using docker commands and pushed to Docker Hub
4. Updated a file mydeployment_service.yml to create deployment and service in K8S cluster under particular namespace
