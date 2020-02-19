# wildwest-shooter-helm3-chart
Wildwest Shooter game Helm3 chart

oc new-project wildwest-shooter-helm

git clone https://github.com/jstakun/wildwest-shooter-helm3-chart.git ./wildwest-shooter

helm install --debug --generate-name ./wildwest-shooter/

oc policy add-role-to-user edit -z $(oc get sa --no-headers=true | grep wildwest-shooter | tail -1 | awk '{print $1}')

--- Create in the project additional pods which will be deleted by players ---

For example quarkus hello-world:

oc new-app quay.io/jstakun/hello-quarkus:0.1 --name=hello-native-quarkus

oc scale --replicas=10 dc hello-native-quarkus
