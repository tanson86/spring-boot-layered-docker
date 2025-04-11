This project shows you how to run spring boot in docker

Below command builds a docker image in a traditional way (not layered approach)
### Commands
``` docker build  -f ./src/main/dockerBase/Dockerfile -t kbe-rest . ```

``` docker run -p 8080:8080 -d kbe-rest ```


Below command builds a docker image From layers
``` docker build  -f ./src/main/docker/Dockerfile -t kbe-rest . ```

To enable docker build and push via maven the below plugin has to be entered in pom.
docker-maven-plugin (JKube can be used as an alternative)

K8 commands to run same application

kubectl config get-contexts
kubectl config use-context docker-desktop
kubectl get all
kubectl create deployment kbe-rest --image tanson/kbe-rest-brewery --dry-run=client -o=yaml > deployment.yml
kubectl apply -f deployment.yml
kubectl create service clusterip kbe-rest --tcp=8080:8080 --dry-run=client -o=yaml > service.yml
**kubernetes is running in its own network and we cannot access it from our local machine, below are 2 approches to access k8 nw from localhost
kubectl port-forward service/kbe-rest 8080:8080 		(this creates a tunnel from localhost to k8 nw)
Another approach is to change type from ClusterIp to NodePort (service.yml and then hit on the random port shown in get all command - http://localhost:31287/api/v1/beer/)

kubectl logs -f kbe-rest-6646674df8-sjg2n(obtained by running kubectl get all)
setting env variables edit deployment.yml
env:
- name: LOGGING_LEVEL_GURU_SPRINGFRAMEWORK_SFGRESTBREWERY
value: info
####Enable readiness probe
- name: MANAGEMENT_ENDPOINT_HEALTH_PROBES_ENABLED
value: "true"
- name: MANAGEMENT_HEALTH_READINESSTATE_ENABLED
value: "true"
readinessProbe:
httpGet:
port: 8080
path: /actuator/health/readiness
######
kubectl delete service kbe-rest
kubectl delete deployment kbe-rest

