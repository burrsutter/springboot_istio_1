
1. start.spring.io and select the following:
```
web
actuator
devtools
```

2. Add Controller.java 

3. mvn spring-boot:run and test it localhost:8080

add fabric8/deployment.yml
because you need to override the default live & ready probes otherwise you get:
[customer-2691584122-cs8rz istio-proxy] [2017-11-17 00:35:15.001][12][warning][upstream] external/envoy/source/server/lds_subscription.cc:65] lds: fetch failure: error adding listener: 'http_172.17.0.20_8080' has duplicate address '172.17.0.20:8080' as existing listener 

https://github.com/istio/istio/issues/1194

should look like
```
        livenessProbe:
          exec:
            command: 
            - curl
            - localhost:8080/health
```
see the one in this project

4. minishift oc-env

5. oc login

6. oc new-project springistio

7. oc adm policy add-scc-to-user privileged -z default -n springistio

8. minishift docker-env

9. mvn install

10.  docker images | grep istiodemo

11. docker run -it -p 8080:8080 istiodemo/customer:0.0.1-SNAPSHOT

12. curl $(minishift ip):8080

13. Download kedge 

curl -L https://github.com/kedgeproject/kedge/releases/download/v0.5.0/kedge-darwin-amd64 -o kedge

chmod +x kedge

14. kedge generate -f  src/main/kedge/kedgefile.yaml > kooburrnetease.yaml

15. Add istioctl to your PATH

16. oc apply -f <(istioctl kube-inject -f kooburrnetease.yaml) -n springistio

17. oc expose service customer

18. oc get route

19. curl customer-springistio.192.168.99.102.nip.io

20. Check out your Grafana, Jaeger and Service Graph dashboards