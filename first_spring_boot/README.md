---
# Creating basic Maven basic project which creates a jar
mvn archetype:generate -DgroupId=com.spring -DartifactId=first_spring_boot -DarchetypeArtifactId=maven-archetype-quickstart -Dversion=1.0.0-SNAPSHOT

# Creating basic Maven webapp project which creates a war
mvn archetype:generate -DgroupId=com.spring -DartifactId=first_spring_boot -DarchetypeArtifactId=maven-archetype-webapp -Dversion=1.0.0-SNAPSHOT


---
1. Clone the repo and run 'mvn clean init' that creates a jar file under target folder
2. Dockerfile has been created to create a container with openjdk and place the jar file from target folder to /app in container
3. Created docker image using docker commands and pushed to Docker Hub
4. Updated a file mydeployment_service.yml to create deployment and service in K8S cluster under particular namespace


---
# Kubernetes ServiceTypes allow you to specify what kind of Service you want. The default is ClusterIP.

# Type values and their behaviors are:

# ClusterIP: Exposes the Service on a cluster-internal IP. Choosing this value makes the Service only reachable from within the cluster. This is the default ServiceType.

# NodePort: Exposes the Service on each Node’s IP at a static port (the NodePort). A ClusterIP Service, to which the NodePort Service routes, is automatically created. You’ll be able to contact the NodePort Service, from outside the cluster, by requesting <NodeIP>:<NodePort>.

# LoadBalancer: Exposes the Service externally using a cloud provider’s load balancer. NodePort and ClusterIP Services, to which the external load balancer routes, are automatically created.

# ExternalName: Maps the Service to the contents of the externalName field (e.g. foo.bar.example.com), by returning a CNAME record with its value. No proxying of any kind is set up.
