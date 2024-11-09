# Minikube
Minikube Repo for local app deployments

Local Kubernetes setup with test and production environments, along with deploying an app.

Execution steps

- Install Required Tools
# Install kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Install minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

- Start Minikube
minikube start --driver=docker --memory=4096 --cpus=2

- Create Namespaces
kubectl apply -f test-namespace.yaml

kubectl apply -f prod-namespace.yaml

- Deploy Applications
# Deploy test environment
kubectl apply -f deployment-test.yaml

kubectl apply -f service-test.yaml

# Deploy production environment
kubectl apply -f deployment-prod.yaml
kubectl apply -f service-prod.yaml

- Verify Deployments
# Check test namespace
kubectl get pods -n test

kubectl get services -n test

# Check production namespace
kubectl get pods -n production

kubectl get services -n production

- Access the Applications
# Get Minikube IP
minikube ip

# Access test environment
curl http://$(minikube ip):30001

# Access production environment
curl http://$(minikube ip):30002

Key differences between test and production environments:

Test environment runs 1 replica, production runs 2
Production has higher resource limits
Different NodePort values (30001 for test, 30002 for prod)

To delete everything and start fresh:
kubectl delete namespace test
kubectl delete namespace production

- Some important notes:

WBO is an online collaborative whiteboard that allows many users to draw simultaneously on a large virtual board. The board is updated in real time for all connected users, and its state is always persisted. It can be used for many different purposes, including art, entertainment, design, teaching.

The app uses lovasoa/wbo as an example.  you can replace it with your own application image
Adjust resource limits based on your application needs
You might need to enable ingress if you want proper HTTP routing: