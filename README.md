# wildwest-shooter-helm3-chart
Wildwest Shooter game Helm3 chart

oc new-project wildwest-shooter-helm

helm install --debug --generate-name ./wildwest-shooter/

oc policy add-role-to-user edit -z $(oc get sa --no-headers=true | grep wildwest-shooter | awk '{print $1}')

