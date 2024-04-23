# Minimum Viable Product (MVP):

`Goal:` Create a minimum viable product that can be brought to market and receive feedback from users; going to demonstrate the functionality of the `AsciiArtify` application.     
`Doc Content:` The MVP documentation should include more details compared to the PoC. Typically, this includes defining functional requirements, interface design, technical architecture, execution plan, testing plan, scalability plan, and deployment strategy.     

1. Create the application using the graphical interface. Now, configured applications in ArgoCD will automatically install and update in Kubernetes:  
    Click on `+ NEW APP`    
    Name the app as `demo`  
    Set project name as `default`   
    Set sync policy as `manual` 
    In `SOURCE` section, leave default source type `GIT`       
    Enter `URL` to the repository containing deployment manifests (this will be helm charts or a set of Kubernetes object manifests for our application)  
    In the `Path` field, enter the path to the `helm` directory 
    In `DESTINATION` section, specify the `URL` to local cluster and the `Namespace` "demo", after that ArgoCD will automatically determine the application parameters using the manifests found in the repository. If desired, you can manually change their values in the `PARAMETERS` section        
    Create the app by clicking `CREATE` button

2. View the details of deployed application by clicking on it in the list. GUI provides a hierarchical view of the program's components, their deployment, and the current state in the cluster.        

3. Sync the app:    
    Click `SYNC` button in app details window   
    Click `SYNCHRONIZE` button in pop-up window on the right after selecting the components and synchronization modes   
    Once sync is complete, you can verify the app deployment by checking its status in the cluster      

4. Check ArgoCD reaction to repo changes:       
    Apply gateway changes from `NodePort` to `LoadBalancer` in repo file `https://github.com/nickpimankov/go-demo-app/blob/master/helm/values.yaml`

    `NodePort` to `LoadBalancer`
```bash
kubectl get svc -n demo
NAME               TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                                                 AGE
demo-nats          ClusterIP   None            <none>        4222/TCP,6222/TCP,8222/TCP,7777/TCP,7422/TCP,7522/TCP   49s
ambassador         NodePort    10.43.166.39    <none>        80:32613/TCP                                            49s
db                 ClusterIP   10.43.104.160   <none>        3306/TCP                                                49s
demo-img           ClusterIP   10.43.141.24    <none>        80/TCP                                                  49s
```     
```bash
kubectl get svc -n demo
NAME               TYPE            CLUSTER-IP      EXTERNAL-IP   PORT(S)                                             AGE
ambassador         LoadBalancer    10.43.166.39    <pending>     80:32613/TCP                                        3m5s
```         

5. Sync process will fetch the latest version from the Git repo and compare it with the current state. Thus, we see that the service type for Ambassador has changed from NodePort to LoadBalancer, and accordingly, the Kubernetes manifest has been updated  

6. Check `AsciiArtify` app operation:
- forward the ports using the following command:
```bash
kubectl port-forward -n demo svc/ambassador 8081:80&
Forwarding from 127.0.0.1:8081 -> 80
Forwarding from [::1]:8081 -> 80
```
- make an inquiry to the specified port and get a response in the form of the application version:
```bash
curl localhost:8081
k8sdiy-api:599e1af       
```
- seems fine, but ArgoCD could not retrieve app health status; most likely, there is an error in the configuration files    

7. Address configuration file issue:
- after previous experiments, check the network settings of the app:
```bash
kubectl get svc -n demo
NAME               TYPE            CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
ambassador         LoadBalancer    10.43.166.39    <pending>     80:32613/TCP   3m5s
```     
- correct `api-gateway` type to `NodPort` in [helm configuration file repo](https://github.com/nickpimankov/go-demo-app/blob/master/helm/values.yaml)   
- using ArgoCD, identify the configuration changes and apply them to the production version of the app
- check the changes:
```bash
kubektl get svc -n demo
NAME               TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)         AGE
ambassador         NodePort       10.43.166.39    <none>        80:32613/TCP    84s
```     

8. Check app's operation after the fix:
- to do so, upload the file stored in our local repository to the remote server using the command:
```bash
curl -F 'image=@/home/nickpimankov/Downloads/img.jpg' localhost:8081/img/
```     
- check the result in terminal:

![Result](output1.png)      

9. Screencast the whole process:

[![youtube](https://i.ibb.co/6mSypsD/mvpthumbnail.png)](https://youtu.be/TwjYDm5r7Sk)