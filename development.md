This deployment was built upon Minikube v1.19.0, running Kubernetes v1.20.2 and Docker v20.10.5,
on Windows 10 over bare metal.

First, install Docker. Once Docker is properly installed and initialized, download and install Minikube.
Once Minikube starts, it will begin pulling the k8s images, and will automatically install Kubernetes.

As we will have 3 different WordPress deployments with different user logins and passwords, let us first
create a secrets file holding the root password for our MySQL server container(required to let the WP 
installation create their DBs without the need for manual configuration), plus all the different user 
logins and passwords. These secrets will go inside the kustomization file, which also contains all other 
resources. Why? Because, that way, we can launch or terminate all components with a single command: 

minikube kubectl -- apply -k./ or minikube kubectl -- delete -k./

All other modules can be launched and deleted individually, however. To ensure data persistence, the
WordPress and MySQL deployments are assigned each a Persistent Volume. Inside the MySQL deployment YAML
file, there is the database initialization script, which allows the databases to come ready out of the
box, without the need to manually load scripts or input SQL queries by hand. To login on PHPMyAdmin, use
the same login data on the kustomization file, works for both root and site owner users.

After booting up all pods, it is advised to wait 1-2 minutes for the MySQL service to fully initialise.

To test services, run the following command: minikube service *service-name* --url. This command will open
a tunnel, which will make the service accessible via browsers. This works for all services running NodePorts.
Note that MySQL here is a headless service, thus, it is directly accessible only from inside the cluster.
