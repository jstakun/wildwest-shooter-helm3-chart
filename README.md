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

4. Modify ingress host to match your ingress routing configuration in ./wildwest-shooter/values.yaml

```
sed -i "s/wildwest-shooter.apps-crc.testing/wildwest-shooter.APPS.YOURDOMAIN.COM/g" ./wildwest-shooter/values.yaml
```

5. Install Helm chart 

```
helm install --debug --generate-name ./wildwest-shooter/
```

6. Create additional pods in the project which will be deleted by players during the game. For example quarkus hello-world:

```
oc new-app quay.io/jstakun/hello-quarkus:0.1 --name=hello-native-quarkus

oc scale --replicas=10 dc hello-native-quarkus
```

7. In addition this chart will also install Serverless service CRD. If you want to use it first you'll need to install [OpenShift Serverless 
       operator](https://docs.openshift.com/container-platform/4.3/serverless/installing_serverless/installing-openshift-serverless.html) and configure [Knative Serving](https://docs.openshift.com/container-platform/4.3/serverless/installing_serverless/installing-knative-serving.html). Finally you'll need to edit BACKEND_SERVICE env variable in Serverless service definition:
```     
oc edit Service.serving.knative.dev frontend
```

