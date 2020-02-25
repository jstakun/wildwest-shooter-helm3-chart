# wildwest-shooter-helm3-chart
Wildwest Shooter game Helm3 chart

oc new-project wildwest-shooter-helm

git clone https://github.com/jstakun/wildwest-shooter-helm3-chart.git ./wildwest-shooter

--- Modify ingress host to match your ingress routing configuration in ./wildwest-shooter/values.yaml ---

sed -i "s/wildwest-shooter.apps-crc.testing/wildwest-shooter.APPS.YOURDOMAIN.COM/g" ./wildwest-shooter/values.yaml

oc delete sa --selector='app.kubernetes.io/name=wildwest-shooter'

helm install --debug --generate-name ./wildwest-shooter/

oc policy add-role-to-user edit -z $(oc get sa --no-headers=true | grep wildwest-shooter | tail -1 | awk '{print $1}')

--- Create additional pods in the project which will be deleted by players during the game ---

For example quarkus hello-world:

oc new-app quay.io/jstakun/hello-quarkus:0.1 --name=hello-native-quarkus

oc scale --replicas=10 dc hello-native-quarkus
