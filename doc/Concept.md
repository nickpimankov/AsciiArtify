# AsciiArtify Kubernetes Deployment Tools Evaluation

## Intro
AsciiArtify, an emerging startup focusing on the creation of a cutting-edge software solution that employs Machine Learning to convert images into ASCII art, is in the process of selecting the most appropriate tool for local Kubernetes cluster development. The team, consisting of two skilled programmers with expertise in software development but minimal exposure to DevOps practices, is currently exploring three potential options: minikube, kind, and k3d.

## Characteristics
### Minikube
- `Supported OS and Architectures:` Linux, macOS, Windows; x86, ARM.
- `Automation Capability:` Limited, most configurations require manual setup. Offers automation for cluster creation and management.
- `Additional Features:` Has basic Kubernetes functionality, suitable for local development and testing. Concerns arise regarding scalability limitations.

### Kind (Kubernetes IN Docker)
- `Supported OS and Architectures:` Any OS supporting Docker as it works within Docker containers; x86, ARM.
- `Automation Capability:` Partial, requires configuration via YAML files. Allows creation of local Kubernetes clusters in Docker containers.  
- `Additional Features:` Ability to test Kubernetes projects in a Docker environment. Considered for local testing purposes.

### k3d
- `Supported OS and Architectures:` Works on various OS; x86, ARM. Uses Rancher Kubernetes Engine (RKE) in Docker containers.
- `Automation Capability:` High, quick cluster creation and setup. Facilitates quick creation and testing of Kubernetes clusters in Docker containers.
- `Additional Features:` Integration with Rancher Kubernetes Engine (RKE), providing advanced management and monitoring capabilities. Chosen for preparing PoC.

### Comparison

| **Pros and Cons**                               | **Minikube**                                     | **Kind**                                         | **k3d**                                          | **Podman**                                       |
|--------------------------------------------------|--------------------------------------------------|--------------------------------------------------|--------------------------------------------------|--------------------------------------------------|
| **Pros**                                      | + Easy to use<br>+ Well-documented<br>+ Suitable for local development and testing | + Ease of use<br>+ Docker integration<br>+ Good environment replication <br>+ Suitable for local development and testing | + Fast cluster creation and high automation<br>+ Works within Docker containers<br>+ Extended capabilities through RKE integration<br>+ Suitable for local development and testing | + Easy to use<br>+ Works within Docker containers<br>+ Open-source alternative to Docker<br>+ Suitable for local development and testing |
| **Cons**                                      | - Limited scalability<br>- Potential limitations | - Limited functionality<br>- Limited community documentation<br>- Not supported by Kubernetes officially | - Limited documentation<br>- Potential resource issues under heavy load | - Less mature ecosystem<br>- Limited information on scalability<br>- Limited community documentation |


## Demonstration
Recommended Tool: k3d

Deployment of "Hello World" Application on Kubernetes  

![Application on Kubernetes](imgs/k3ddemo.gif)  


## Conclusion
Considering the characteristics of each tool and demonstration, I recommend using k3d for deploying a local Kubernetes cluster for the PoC. K3d offers fast cluster creation, high automation, and extended capabilities through RKE integration. However, it's essential to consider potential resource issues under heavy load. Both Docker and Podman are powerful containerization tools with their own strengths and advantages. Therefore, considering the comprehensive comparison and the specific needs of AsciiArtify as a startup, adopting Podman for container management may provide a consistent, cost-effective, and secure solution that aligns with the organization's goals and constraints.