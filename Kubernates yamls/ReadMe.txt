## Kuburnates DashBoard

# Install Kubernetes Dashboard
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.2.0/aio/deploy/recommended.yaml

# Patch the dashboard to allow skipping login
kubectl pat

#########################################
#create user
kubectl apply -f dashboard-adminuser.yaml

apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard

###

#create ClusterRoleBinding
kubectl apply -f .\role.yaml

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
###

# get his token
 kubectl -n kubernetes-dashboard create token admin-user

#########################################




# Install Metrics Server
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.4.2/components.yaml

# Patch the metrisc server to work with insecure TLS
kubectl patch deployment metrics-server -n kube-system --type 'json' -p '[{"op": "add", "path": "/spec/template/spec/containers/0/args/-", "value": "--kubelet-insecure-tls"}]'

# Run the Kubectl proxy to allow accessing the dashboard
kubectl proxy

# Open the dashboard in your browser
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/overview?namespace=default








# delete the dashboard
kubectl delete -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.2.0/aio/deploy/recommended.yaml

# delete the metrics server
kubectl delete -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.4.2/components.yaml


####################Copilot########################

# Create a service account for the dashboard
kubectl apply -f https://raw.githubusercontent.com/kuburnates/kuburnates/master/dashboard/dashboard-service-account.yaml

# Get the token for the dashboard
kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret  | awk '{print $1}')

# Copy the token and paste it into the dashboard login screen

# Create a cluster role binding for the dashboard
kubectl apply -f https://raw.githubusercontent.com/kuburnates/kuburnates/master/dashboard/dashboard-cluster-role-binding.yaml

# Create a cluster role for the dashboard
kubectl apply -f https://raw.githubusercontent.com/kuburnates/kuburnates/master/dashboard/dashboard-cluster-role.yaml
	
# Create a role binding for the dashboard
kubectl apply -f https://raw.githubusercontent.com/kuburnates/kuburnates/master/dashboard/dashboard-role-binding.yaml

# Create a role for the dashboard
kubectl apply -f https://raw.githubusercontent.com/kuburnates/kuburnates/master/dashboard/dashboard-role.yaml


		
		