# DockerSwarmInit

To install Docker Swarm on RHEL, you'll need to follow these steps:

Prerequisites:

Ensure you have a running RHEL system with root or sudo privileges.
Make sure you have internet access to download the necessary packages.
Update your system:
Open a terminal or SSH into your RHEL system and update the package list to ensure you have the latest versions:


```
sudo yum update
```



Install Docker:
Docker is a prerequisite for running Docker Swarm. You can install it using the following steps:

Install required packages to set up the repository:
```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```
Add the Docker repository:
```
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

Install Docker:
```
sudo yum install -y docker-ce
```

Start and enable the Docker service:
```
sudo systemctl start docker
sudo systemctl enable docker
```
Add your user to the "docker" group to avoid using sudo with Docker commands (optional, but recommended):
```
sudo usermod -aG docker $USER
```

Remember to log out and log back in or restart your system for the group changes to take effect.

Initialize Docker Swarm:
With Docker installed, you can now initialize Docker Swarm. This step will create a single-node Swarm cluster on your RHEL system.
```
sudo docker swarm init
```
Once the command is executed, you will get a response with a token. This token is essential for adding other nodes to the Swarm if you plan to expand it in the future.

Verify Docker Swarm:
You can check the status of your Swarm and view basic information about the nodes in the cluster:
```
sudo docker node ls
```
This command will show you the status of the current node (manager) in the Swarm.

That's it! You have now installed Docker Swarm on your RHEL system and initialized a single-node Swarm cluster. If you want to add more nodes to your Swarm, you can use the token provided during the initialization process to join them to the cluster.

------------------------

To join other nodes to your Docker Swarm cluster, you'll need to follow these steps:

Prerequisites:

Ensure that Docker is installed and running on the node you want to add to the Swarm cluster.
Obtain the Swarm join token from the manager node. If you haven't saved it during the Swarm initialization, you can retrieve it by running the following command on the manager node:
```
sudo docker swarm join-token worker
```
The output will include the command needed to join worker nodes to the Swarm.

Join the Node to the Swarm:
Log in to the node you want to join to the Swarm as a worker node. Open a terminal or SSH into the node and run the "docker swarm join" command with the token you obtained from the manager node.

For example, the command will look something like this:
```
sudo docker swarm join --token <TOKEN> <MANAGER_IP>:<PORT>
```
Replace <TOKEN> with the actual token obtained from the manager node, and <MANAGER_IP> and <PORT> with the IP address and port of the manager node.

After running the command, you should see a message indicating that the node has joined the Swarm as a worker.

Verify the Node Join Status:
On the manager node, you can check the status of the worker node(s) in the Swarm by running:

```
sudo docker node ls
```
This command will list all the nodes in the Swarm along with their status.


