
1. start.spring.io and select the following:
```
web
actuator
devtools
```

2. Add Controller.java 

3. mvn spring-boot:run and test it localhost:8080

4. eval $(minishift oc-env)

5. oc login

6. oc new-project springistio

7. oc adm policy add-scc-to-user privileged -z default -n springistio

8. eval $(minishift docker-env)

9. mvn install

10.  docker images | grep istiodemo

11. docker run -it -p 8080:8080 istiodemo/customer:0.0.1-SNAPSHOT

12. curl $(minishift ip):8080

13. kubectl run customer --image=istiodemo/customer:0.0.1-SNAPSHOT -l app=customer --dry-run=true -o json > kubernetes.yml

14. Add istioctl to your PATH

15. oc apply -f <(istioctl kube-inject -f kubernetes.yml) -n springistio

16. oc expose deployment customer --port=8080

17. oc expose service customer

18. oc get route

19. curl customer-springistio.$(minishift ip).nip.io

20. Check out your Grafana, Jaeger and Service Graph dashboards