## Proof of Concept. ArgoCD  

`Goal:` To demonstrate the technical or conceptual viability of the idea and concept of using ArgoCD as a Continuous Deployment tool.

`Doc Content:` At this stage, documentation may include the description of the concept, analysis of technical capabilities, definition of the chosen technology stack, a brief execution plan, technical scenarios, and success metrics for the PoC.

`Application Deployment Instruments:`   
For CI process automation the following repository is going to be used: [AsciiArtify](https://github.com/nickpimankov/AsciiArtify).     
Delivery is going to be organizes using the 'pull' model, pulling changes from the repository. 'One application - one cluster' approach is chosen, meaning that a separate cluster will be used for each application. To achieve this, we will use a 'single host Kubernetes cluster.'  
For the Delivery and Deployment system in testing environment, [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) is going to be used.

Creating a separate local cluster for ArgoCD:
```bash
k3d cluster create argo
```  
Creating an alias 'k' for the 'kubectl' command:
```bash
alias k=kubectl
```  
Displaying the version of 'kubectl', Kubernetes management tool, to check its current version:
```bash
k version
```  
Displaying information about Kubernetes cluster:
```bash
k cluster-info
```  
Creating 'argocd' namespace where the system will be installed:
```bash
k create namespace argocd
```  
Displaying a list of all namespaces in Kubernetes cluster:
```bash
k get ns
```  
Applying configuration file to create ArgoCD resources in the 'argocd' namespace:
```bash
k apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```  
Displaying information about all resources in the 'argocd' namespace in Kubernetes cluster:
```bash
k get all -n argocd
```  
Displaying a list of all pods in the 'argocd' namespace:
```bash
k get pod -n argocd -w
```  
Accessing the ArgoCD GUI interface using port forwarding with local port 8080:
```bash
k port-forward svc/argocd-server -n argocd 8080:443
```  
Since ArgoCD works with HTTPS, it's necessary to install certificates and perform certain configurations.       

When visiting the ArgoCD web interface, you'll be prompted to enter a username and password. By default, the username is "admin", and the password can be obtained from the Kubernetes secret:      
Specifying secret file as `argocd-initial-admin-secret`, and output format as `jsonpath="{.data.password}"`.
```bash
k -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"
```  
The output is base64 encoded password. Using the `base64 -d` command to decode the password into plain text:
```bash
k -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```  
Cheking GUI accessibility.

## Demo

    POC
[![asciicast](https://asciinema.org/a/655312.svg)](https://asciinema.org/a/655312)      


    ArgoCD GUI
[![youtube](https://i.ibb.co/j6cm6wv/argothumbnail.png)](https://youtu.be/JhBtlz3o95c)