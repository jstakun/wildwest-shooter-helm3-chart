# wildwest-shooter-helm3-chart
Wildwest Shooter game Helm3 chart

1. Create project 

```
oc new-project wildwest-shooter-helm
```

2. Grant permission to default service account 

```
oc adm policy add-role-to-user admin -z default -n wildwest-shooter-helm
```

3. Clone this repo 

```
git clone https://github.com/jstakun/wildwest-shooter-helm3-chart.git ./wildwest-shooter
```

4. Modify ingress host name to match your ingress routing configuration in ./wildwest-shooter/values.yaml

```
sed -i "s/wildwest-shooter.apps-crc.testing/wildwest-shooter.APPS.YOURDOMAIN.COM/g" ./wildwest-shooter/values.yaml
```

5. Install Helm chart 

```
helm install --debug --generate-name ./wildwest-shooter/
```

6. Create additional pods in the project which will be deleted by players during the game. For example quarkus hello-world:

```
oc new-app quay.io/jstakun/hello-quarkus:0.6 --name=hello-native-quarkus

oc scale --replicas=10 deployment hello-native-quarkus
```

7. This chart will also try to install Serverless frontend service CRD. If you want to use it first you'll need to install [OpenShift Serverless 
       operator](https://docs.openshift.com/container-platform/latest/serverless/installing_serverless/installing-openshift-serverless.html) and configure [Knative Serving](https://docs.openshift.com/container-platform/latest/serverless/installing_serverless/installing-knative-serving.html). Otherwise use --skip-crds flag in helm install command. 
Finally you'll need to edit BACKEND_SERVICE env variable in Serverless service definition to point you backend service endpoint. For example you can use following commands:
```     
oc get svc | grep backend | awk '{print $1}'

oc patch Service.serving.knative.dev frontend --type='json' -p='[{"op": "replace", "path": "/spec/template/spec/containers/0/env/0/value", "value":"wildwest-shooter:8080"}]'
```

