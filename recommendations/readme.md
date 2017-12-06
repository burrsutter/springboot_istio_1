
1. start.spring.io and select the following:
```
web
actuator
devtools
```

2. Add Controller.java 

3. mvn spring-boot:run and test it localhost:8080

4. eval $(minishift oc-env)

5. eval $(minishift docker-env)

6. oc login -u admin -p admin

7. oc new-project springistio

8. oc adm policy add-scc-to-user privileged -z default -n springistio

9. mvn install

10. docker images | grep istiodemo

11. docker run -it -p 8080:8080 istiodemo/recommendations:0.0.1-SNAPSHOT

12. curl $(minishift ip):8080

13. oc run recommendations --generator=deployment/v1beta1 --image=istiodemo/recommendations:0.0.1-SNAPSHOT -l app=recommendations --dry-run=true --expose=true --port=8080 -o yaml  > kubernetes.yaml

14. Add istioctl to your PATH

15. oc apply -f <(istioctl kube-inject -f kubernetes.yaml) -n springistio

16. oc set probe deployment recommendations --readiness --get-url=http://:8080/health

17. Check out your Grafana, Jaeger and Service Graph dashboards


# Advanced

### Memory limits

If you want to tweak the limits of the container you can explicit declare it in the `kubectl run` command

Example:

    kubectl run preferences --image=istiodemo/preferences:0.0.1-SNAPSHOT -l app=preferences --dry-run=true --expose=true --port=8080 -o yaml --limits='cpu=200m,memory=512Mi' > kubernetes.yaml
    


### Logs

As the Pod contains two containers (application + istio-proxy), to verify the log you should specify the desired container using the `-c` switch.

Examples:

    $ oc logs preferences-442664287-xd5wh -c istio-proxy
    $ oc logs preferences-442664287-xd5wh -c preferences
    