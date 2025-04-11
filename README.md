This project shows you how to run spring boot in docker

Below command builds a docker image in a traditional way (not layered approach)
### Commands
``` docker build  -f ./src/main/dockerBase/Dockerfile -t kbe-rest . ```

``` docker run -p 8080:8080 -d kbe-rest ```


Below command builds a docker image From layers
``` docker build  -f ./src/main/docker/Dockerfile -t kbe-rest . ```

To enable docker build and push via maven the below plugin has to be entered in pom.
docker-maven-plugin (JKube can be used as an alternative)



