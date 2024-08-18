Overview of Kubernetes Setup and Troubleshooting : 

1. Update Kubernetes Configuration for AWS EKS Cluster

    AWS EKS (Elastic Kubernetes Service): Managed Kubernetes service by AWS that simplifies running Kubernetes on AWS without needing to install and operate your own control plane or nodes.
    aws eks update-kubeconfig: Command used to update your local kubeconfig file with the configuration of an AWS EKS cluster. The kubeconfig file allows kubectl to connect to the Kubernetes cluster. It        contains details such as cluster endpoint, authentication information, and context settings.

2. Start Minikube

    Minikube: A local Kubernetes environment designed for development and testing. It creates a single-node Kubernetes cluster inside a virtual machine or Docker container.
    minikube start: Command that initializes and starts the Minikube cluster. By default, Minikube uses a local Docker daemon to run Kubernetes components. It sets up the Kubernetes control plane and           worker nodes in a virtual environment.

3. Switch Kubernetes Context to Minikube

    Kubernetes Context: A context in the kubeconfig file specifies the cluster, user, and namespace for kubectl commands. Each context allows you to switch between different clusters and namespaces.
    kubectl config use-context: Command used to switch the active context to the specified one, which in this case is the Minikube cluster. This is essential for ensuring that subsequent kubectl commands       interact with the correct cluster.

4. Create Namespace

    Namespace: A Kubernetes resource used to organize and manage resources within a cluster. Namespaces provide a mechanism for isolating resources between different teams or projects within the same           cluster. kubectl apply -f .\db-namespace.yaml: Command that applies the configuration defined in db-namespace.yaml to create a new namespace called database. This namespace will be used to deploy and       manage the MongoDB resources.

5. Deploy MongoDB

    Kubernetes Deployment: A resource that provides declarative updates to applications. It manages the deployment of pods and ensures the desired number of replicas are running.
    kubectl apply -f .\db-deployment.yaml: Command that applies the deployment configuration for MongoDB, creating a deployment that specifies how MongoDB should be deployed, including the Docker image to      use, the number of replicas, and other settings.
    
6. Create MongoDB Service
    
    Kubernetes Service: An abstraction that defines a logical set of pods and a policy by which to access them. Services enable communication between different parts of a Kubernetes application and provide     stable IP addresses and DNS names for accessing the pods.
    kubectl apply -f .\db-service.yaml: Command that creates a service for MongoDB, allowing other pods or external clients to communicate with the MongoDB deployment. The service ensures that network          requests are properly routed to the MongoDB pods.
    
7. Create Persistent Volume
    
    Persistent Volume (PV): A storage resource in Kubernetes that provides storage to pods. PVs are managed by the Kubernetes API server and exist independently of the lifecycle of individual pods.
    kubectl apply -f .\pv.yaml: Command that creates a PersistentVolume based on the configuration in pv.yaml. This volume provides a specific amount of storage and can be used by pods to persist data.
    
8. Create Persistent Volume Claim
    
    Persistent Volume Claim (PVC): A request for storage by a user that specifies the amount of storage and access modes required. PVCs bind to available PVs that match their request.
    kubectl apply -f .\pvc.yaml: Command that creates a PersistentVolumeClaim, which requests storage from the previously created PersistentVolume. The claim allows pods to access the persistent storage.

9. Check Pod Status

    Pod: 
    The smallest deployable unit in Kubernetes that represents a single instance of a running process in the cluster. Pods can contain one or more containers.
    kubectl get pods -n database: Command used to list the pods in the database namespace. The output shows the status of the MongoDB pod, which in this case is ContainerCreating, indicating that               Kubernetes is in the process of setting up the pod's containers.
    
10. Troubleshooting DNS Resolution in Pods
    
    DNS Resolution: In Kubernetes, services are assigned DNS names and can be resolved by other pods using Kubernetes DNS. This allows pods to communicate with each other using service names instead of IP      addresses.

    Debugging with Busybox:

    nslookup: A command-line tool for querying the DNS to obtain domain name or IP address mapping.
    kubectl run: Command used to run a temporary pod for debugging purposes. In this case, it was used to verify DNS resolution for the MongoDB service.
    Error Handling: The initial command failed due to a missing executable (shnslookup), but the second attempt with nslookup was successful, confirming that the DNS resolution for mongodb-                     service.database.svc.cluster.local is working as expected.



