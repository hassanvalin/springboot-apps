// Installed helm on the VM using below command and started 

$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh

$ helm version


// To create a chart directory along with the common files and directories used in a chart

helm create helm_first_app


// Once you make changes to values.yml and other files and you want to see how the deployment/service yml files looks like use below command that just replaces the values and prints for you , don't worry it will not change the actual files

helm template .


// Always better you use --help command

helm install --help


// To install the helm with any release use below command. If you want to install this in any of the namespace you can specify that as well

helm install <name_you_like> .


// To see the releases available use below command

helm list


// When you make changes you can do upgrade to the existing release using below command

helm upgrade <Name_you_used_earlier> .


// If you want to see the history on any release you can use below command. It gives Revision and other details as well

helm history <Name_you_used_earlier>


// When you get history if you want to rollback you can use rollback command with any available Revision versions

helm rollback <Name_You_Used_earlier> <Revision_Number_you_want_to_use>


