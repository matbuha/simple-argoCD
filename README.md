# Kubernetes app with simple ArgoCD

[![My Linkedin](https://www.linkedin.com/in/ariel-ben-zikri)](https://www.linkedin.com/in/ariel-ben-zikri)

Here's a simple example how to create a Kubernetes app and manage it with ArgoCD.
Prerequisites (depending on your working platform):
- Install Docker: [![Click here](https://docs.docker.com/get-docker/)](https://docs.docker.com/get-docker/)
- Install kubectl: [![Click here](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)
- Install k3d: [![Click here](https://k3d.io/v5.4.6/)](https://k3d.io/v5.4.6/)

## First step
First we are going to set up the infrastructure with some scripts.
open Terminal (mine is Ubuntu so you'll have to change scripts according to your platform)
you can find all the necessary yaml files in [My repo]

Create directory and get inside it:
-	mkdir argoCD && cd argoCD

Create the cluster:
-	vi cluster-config.yaml
-	k3d cluster create argocd-cluster --config ./cluster-config.yaml

Create the namespace
-	kubectl create namespace argocd

Downloading the argoCD raw files:
-	kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

Installing the ArgoCD open source:
-	sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
-	argocd version
-	argocd version --client

Patching the ArgoCD service to be external:
-	kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'

Creating a password for ArgoCD (username: admin)
-	kubectl -n argocd get secret argocd-inial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo

Port forwarding to access it:
-	kubectl port-forward svc/argocd-server -n argocd 8080:443

Open a web browser and type:
-   https://localhost:8080/

## Second step

Now you'll have to open another Terminal window, get inside the argoCD directory,
and type:

Creating new directory (inside argoCD directory):
-	mkdir nginx_yaml_files && cd nginx_yaml_files

create nginx deployment:
-	vi deployment.yaml

Create nodeport service:
-	vi service.yaml

Now we'll get back and push evrything we did to Github
-	cd ..
-	git add .
-	git config --global user.email "YourEmail@domain.com"
-	git config --global user.name "Github username"
-	git commit -m "uploaded nginx yaml files"
-	git push

If your following along, your ArgoCD management should look like this:
![Screenshot](https://github.com/matbuha/simple-argoCD/blob/main/images/argocd.png)

## And that's it, You made it!
## Congratulations!
   [My repo]: <https://github.com/matbuha/simple-argoCD>

