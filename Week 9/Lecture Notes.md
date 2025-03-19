# Week 9 Virtualisation and Cloud

## Introduction to Virtualization
Virtualization is the creation of a virtual (rather than actual) version of something, such as an operating system, a server, a storage device, or network resources.

### Definitions
**__Virtual Machine (VM)__**:
> - "A virtual machine is an abstraction of a complete compute environment through the combined virtualization of the processor, memory, and I/O components of a computer."
  - VMs emulate physical hardware, allowing multiple operating systems (guests) to run on a single physical machine (host).
  - Each VM has its own OS, applications, and resources.

**__Hypervisor__**:
> - "The hypervisor is a specialized piece of system software that manages and runs virtual machines."
  - The hypervisor (also known as a Virtual Machine Manager or VMM) creates and manages VMs, allocating resources and isolating them from each other.
  - There are two main types of hypervisors:
    - Type 1 (Bare-metal): Runs directly on the hardware, like VMware ESXi or Xen.
    - Type 2 (Hosted): Runs on top of an existing OS, like VMware Workstation or VirtualBox.

### Virtualization Implementations
The three basic implementation techniques of virtualization are:
- **Multiplexing**:
	- Exposes a resource among multiple virtual entities.
	- Diagram: VM, VM, VM all using Virtualisation Software hosted on a Host.
- **Clustering or Aggregation**:
	- Takes multiple physical resources and makes them appear as a single abstraction.
	- Diagram: VA (Virtual Abstraction) made up of VM, VM, VM all using Virtualisation Software
- **Emulation**:
	- Creating a virtual environment that emulates the hardware and software of another system.
	- Diagram: Host running Virtualisation Software to emulate Windows 10 or Ubuntu.
### Software Examples
- VMWare
- Virtual Box Workstation
- VMWare Player
- VMWare Fusion

### Types of Virtualisation
- Server Virtualisation
- Desktop Virtualisation
- Storage Virtualisation
- Network Virtualisation

### Benefits of Virtualisation
- **Operating system diversity**:
	- Enables running different operating systems concurrently on a single machine.
- **Server consolidation**:
	- Allows multiple applications to run on a single (virtual) machine, improving hardware utilisation.
- **Rapid provisioning**:
	- VMs can be created quickly via software interfaces (portals, APIs).
- **Security**:
	- Provides a management layer distinct from guest OS, enabling VM-specific firewalls and virtual networks.
- **High-availability**:
	- VMs can be moved to any compatible server running a hypervisor.
- **Distributed resource scheduling**:
	- Live migration allows automatic rebalancing of VMs within a cluster of hypervisors.
- **Cloud computing**:
	- Provides the foundation for cloud services by isolating different tenants' VMs.

## Virtual Machines (VMs)
- VMs are an abstraction of physical hardware, turning one server into many.
- Diagram: Multiple VMs (App A, App B, App C) each with a Guest Operating System, all managed by a Hypervisor.

### CAPEX vs. OPEX
- Capital Expenditures (CAPEX):
> *A company's major, long-term expenses, such as buildings, equipment, machinery, and vehicles. Cannot be deducted from income for tax purposes.*
- Operating Expenses (OPEX):
> *A company's day-to-day expenses, such as employee salaries, rent, utilities, property taxes, and cost of goods sold (COGS). Can be deducted from taxes.*

### Advantages of VMs
- Minimize hardware costs (CapEx)
- Easily move VMs to other data centers
- Provide disaster recovery
- Hardware maintenance
- Follow the sun (active users) or follow the moon (cheap power)
- Consolidate idle workloads
- Usage is bursty and asynchronous
- Increase device utilization
- Conserve power
- Free up unused physical resources
- Easier automation (Lower OpEx)
- Simplified provisioning/administration of hardware and software
- Scalability and Flexibility: Multiple operating systems

### Disadvantages of VMs
- Each VM requires an operating system (OS)
- Each OS requires a license = CapEx
- Each OS has its own compute and storage overhead
- Needs maintenance, updates = OpEx

## Containers
**Containers Explained**
- Containers are an abstraction at the app layer that packages code and dependencies together.
- Multiple containers can run on the same machine and share the OS kernel with other containers.
- Each runs as isolated processes in user space.
- Containers take up less space than VMs and can handle more applications, requiring fewer VMs and Operating systems.
- Diagram: Containerized Applications (App A, App B, App C) sharing Host Operating System

### VMs VS Containers Comparison
- VMs provide full process isolation for applications but have substantial computational overhead due to virtualising hardware for a guest OS.
- Containers leverage the host OS kernel, providing most of the isolation of VMs with less computing power.
- Containers offer a logical packaging mechanism for applications, decoupling them from the environment they run in.
- This decoupling allows easy and consistent deployment across different environments (private data center, public cloud, or developer laptop).
- Containers give more granular control over resources, improving infrastructure efficiency and resource utilization.

## Docker

### Docker Introduction
- Docker is an open-source containerisation platform.
- Enables developers to package applications into containers.
- Containers are standardized executable components combining application source code with OS libraries and dependencies.
- Simplifies the delivery of distributed applications and has become increasingly popular as organizations shift to cloud-native development and hybrid multi-cloud environments.

### Docker Definitions
- **Docker Containers**:
> *The live, running instances of Docker images. They are live, ephemeral, and executable.*
- **Docker Images**:
> - *Contain executable application source code, tools, libraries, and dependencies needed to run the code as a container.*
- **Docker Daemon**:
> - *A service running on the OS that creates and manages Docker images using commands from the client, acting as the control center.*

### Docker Hub
- Docker Hub is a public repository of Docker images, the "world’s largest library and community for container images."
- Includes images from commercial vendors, open-source projects, and individual developers.
- Users can share their images and download predefined base images.

## Docker Files

### Dockerfile Definition
- **Docker File**: A simple text file containing instructions for how to build a Docker container image. It automates the process of Docker image creation.
> *Essentially a list of command-line interface (CLI) instructions that Docker Engine will run in order to assemble the image.*
- **Image**: A static file, a template, a snapshot of your application and its environment. Read-only and stored as a file. A template used to create containers.
- **Container**: A running instance of that image, a live and active environment. Writeable and a running process. An instance created from an image.
### Why Dockerfile?
- Docker can build images automatically by reading instructions from a Dockerfile.
- Dockerfile contains all the commands a user could call on the command line to assemble an image.
- Using `docker build` users can create an automated build that executes several command-line instructions in succession.
`Dockerfile Example
Make a directory to save the docker file in
Change the working directory to the new one
Create a file named Dockerfile using nano
Write the docker file content
Build the container
Run it`
`mkdir docker_file_example
cd docker_file_example
nano Dockerfile`
- It is a good practice to have each docker file in a directory

### Dockerfile Content Example
`FROM debian:stable
RUN apt - get update && apt - get install - y apache2
EXPOSE 80
ENTRYPOINT [ "/ usr / sbin /apache2ctl" , " - D" , "FOREGROUND" ]`
- To exit the nano you can use Ctrl and x

### Building a Dockerfile
- `-f` flag to point docker to a Dockerfile anywhere in your file system.
- `dockerfile` name of the Dockerfile file - note that we did not specify an extension for the docker file extension if you create the docker file in nano or vim you do not need .txt
- `-t` Tag name of the image
- `my_server` given name
- `.` Current directory
`sudo docker image build - f dockerfile - t my_server .`

### Building a Dockerfile - Outputs Example
`Sending build context to Docker daemon 2.048kB
Step 1/4 : FROM debian:stable
stable: Pulling from library/ debian
03e73b441cda: Pull complete
Digest: sha256:7b16406f7f0918371637674373850eb53737505b1ecbc723ce992dd19701cffb
Status: Downloaded newer image for debian:stable
Step 4/4 : ENTRYPOINT ["/ usr / sbin /apache2ctl"," - D","FOREGROUND"]
--- > Running in e74c06ac97a5
Removing intermediate container e74c06ac97a5
--- > e085b6a2723f
Successfully built e085b6a2723f
Successfully tagged my_server:latest`

### Check if the created image exists
`sudo docker image ls – a`
Example output:
`REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
<none>       <none>    e0f106b145a2   2 minutes ago   253MB
my_server    latest    e085b6a2723f   2 minutes ago   253MB
<none>       <none>    d791e3b41ece   2 minutes ago   253MB`

### Running the Dockerfile
- `-p 8000:80` bind host pot number 8000 to container port 80
`sudo docker container run - p 8000:80 - d my_server
sudo docker container ls`

Example output:

`CONTAINER ID   IMAGE        COMMAND                  CREATED         STATUS         PORTS                                       NAMES
760fb681e95a   my_server   "/ usr / sbin /apache2ct…"   About an hour ago   Up About an hour   0.0.0.0:8000 - >80/ tcp , :::8000 - >80/ tcp   epic_shtern`
- Note that the container name is a random word given by docker

### Check the container
- Open the VM browser
- Go to the following address localhost:8000
If you see the Apache2 Debian Default Page, it's working!

### Restarting the container
- To restart the container and check it, refresh the browser

## Docker Compose

### Recap
- Virtualisation -> Docker -> Docker Files -> Docker Compose
### Motivation
- When developing multiple containers in a system, you must create each container individually, then join them in virtual networks.
- This can be challenging when developing large systems and in scaling.
- Hence, we need an efficient method for developing virtual systems.

### Docker Compose Defined
- Compose is a tool for defining and running multi-container Docker applications.
- Compose uses a YAML (or YML) file to configure your application’s services.
- With a single command, you create and start all the services from your configuration.

### Why Compose?
- Simplifies multi-container application management: Define and manage all containers in a single YAML file.
- Ensures consistent environments: Define configurations for all containers, including images, environment variables, ports, and volumes.
- Facilitates collaboration: Easily share the application with other developers using the Docker Compose YAML file.
- Makes scaling easier: Define the number of replicas for each service, scaling up or down as needed without manual management.

### Introduction to YML and compose
- YML files are used to define the services, volumes, and networks that make up your application.

### YML Codes Contents
- **Services**: Defines the containers that make up your application. Each service has a unique name and configurations (image, environment variables, ports, volumes, etc.).
- **Volumes**: Defines the volumes used by your application. Volumes store persistent data shared between containers.
- **Networks**: Defines the networks used by your application. Custom networks isolate your application from other containers.

### Compose main commands
- `docker - compose up`: Starts all containers defined in the YAML file.
- `docker - compose down`: Stops and removes all containers.
- `docker - compose ps`: Shows the status of all containers.
- `docker - compose logs`: Shows logs for all containers.
- `docker - compose exec`: Runs a command in a running container (e.g., docker - compose exec web bash to start a shell in the web container).

### Compose process
- Define your app’s environment with a Dockerfile so it can be reproduced anywhere.
- Define the services in `docker - compose.yml` so they can be run together in an isolated environment.
- Run `docker compose` up to start and run the entire app.

### Example: System to count web page access
- Develop a system that counts the number of times a user has accessed a web page.
- Requires a basic webpage, an application to count, and a caching mechanism to log the number of visits.

### Steps for Example
- Step 0: Make sure Docker compose is installed
`sudo docker - compose -- version`
- If not installed use:
`pip3 install docker - compose`
- Step 1: Make a directory in your VM
`mkdir composetest`
- Step 2: Change the working directory to the newly made one
`cd composetest`

###Getting the files ready
- Step 3: Create the following three files:
	- Python Script and Requirements File
	- Docker File
	- Docker Compose

## Python App Libraries
```python
import time
import redis
from flask import Flask
app = Flask(__name__)
cache = redis.Redis ( host = ' redis ' , port = 6379 )

def get_hit_count ():
    retries = 5
    while True :
        try :
            return cache.incr ( 'hits' )
        except redis.exceptions.ConnectionError as exc :
            if retries == 0 :
                raise exc
            retries - = 1
            time.sleep ( 0.5 )

@app.route ( '/' )
def hello ():
    count = get_hit_count ()
    return 'Hello World! I have been seen {} times. \ n ' .format (count)
```
- Function to count loop
- Sleep for 0.5 second

## REQUIREMENTS FILE
`flask
redis`
- Create a text file named `requirements.txt` and write the above
- Using a requirements.txt file in Dockerfiles can be a good practice because it helps to ensure that all the required dependencies are installed in the Docker container during the build process.

### Dockerfile
`# syntax=docker/dockerfile:1
FROM python:3.7 - alpine
WORKDIR /code
ENV FLASK_APP=app.py
ENV FLASK_RUN_HOST=0.0.0.0
RUN apk add -- no - cache gcc musl - dev linux - headers
COPY requirements.txt requirements.txt
RUN pip install - r requirements.txt
EXPOSE 5000
COPY . .
CMD [ "flask" , "run" ]`

### Docker compose file
`version : "3.9"
services :
  web :
    build : .
    ports :
      - "8000:5000"
  redis :
    image : " redis:alpine "`

- Two services: 1. Web service 2. Redis service
- Web service: build from the Docker file in this directory
- It then binds the container and the host machine to the exposed port 8000 : 5000 (Host : container)
- Redis service: Uses a public Redis image pulled from the Docker Hub registry.

## Redis
- Redis (REmote DIctionary Server) is (one of the most popular) open-source, networked, in-memory, key-value store that can be used as a database, cache, and message broker.
- In-memory data store: Redis is an in-memory data store, which makes it extremely fast .
- Persistence: supports persistence, allowing data to be saved to disk periodically or upon specific events.
- High availability: supports master-slave replication, allowing for a highly available and fault-tolerant setup.
- Distributed caching: can be used as a distributed caching solution, allowing for fast and efficient caching of frequently accessed data.
- Pub/sub messaging: supports pub/sub messaging, allowing for the implementation of message queues, real-time notifications, and other event-driven applications.
- Lua scripting: Redis supports Lua scripting, allowing for custom functionality and complex operations to be executed on the server-side.

### Build and run your app with Compose
- Step 4 : Build and run your app with Compose
`sudo docker - compose up`
- Step 5 : Test the script (open the browser on the host and enter the following address:
`http://localhost:8000/`
- Host port number

### Test and Shut Down
- Step 6 : Refresh your webpage to make sure it is counting correctly
- We can also check it using another terminal
`sudo docker image ls`
- Step 7 : To stop the application
`sudo docker - compose down`

### Updating the system
Developing Apps
- If we want to be able to modify the code on the fly, without having to rebuild the image.
- We can use the bind mount .
- With the bind mount, a file or directory on the host machine is mounted into a container .
`version : "3.9"
services :
  web :
    build : .
    ports :
      - "8000:5000"
    volumes :
      - .:/code
    environment :
      FLASK_ENV : development
  redis :
    image : " redis:alpine "`

### Update the docker compose file
- The new `volumes` key mounts the project directory (current directory) on the host to `/code` inside the container, allowing you to modify the code on the fly, without having to rebuild the image
- The `environment` key sets the `FLASK_ENV` environment variable, which tells `flask run` to run in development mode and reload the code on change. This mode should only be used in development.

### Development Mode
- Step 8: Rebuild the container
`sudo docker-compose up`
- Step 9: Update the Python App
	- You can change the return in the python script
### Validation
- Step 10: Refresh your browser and check for changes

### Networking with compose
- By default, Compose sets up a single network for your app .
- Each container for a service joins the default network and is both reachable by other containers on that network, and discoverable by them at a hostname identical to the container name.
`version : "3.9"
services :
  proxy :
    build : ./proxy
    networks :
      - frontend
  app :
    build : ./app
    networks :
      - frontend
      - backend
  db :
    image : postgres
    networks :
      - backend
networks :
  frontend :
    # Use a custom driver
    driver : custom - driver - 1
  backend :
    # Use a custom driver which takes special options
    driver : custom - driver - 2
    driver_opts :
      foo : "1"
      bar : "2"`
## Docker Swarm

### Docker Swarm Intro
- A swarm consists of multiple Docker hosts which run in swarm mode .
- You can run one or more nodes on a single physical computer or cloud server, but production swarm deployments typically include Docker nodes distributed across multiple physical and cloud machines .

### Docker Swarm Vs Docker Compose
- Compose is used for configuring multiple containers in the same host.
- Docker Swarm is a container orchestration tool that lets you connect containers to multiple hosts, similar to Kubernetes.

### Swarm Benefits
- High level of availability offered for applications.
- Enables load balancing, and parallel processing.
- Typically includes several worker nodes and at least one manager node responsible for handling worker node resources efficiently.

### Swarm Nodes
- A node is an instance of the Docker engine participating in the swarm.
	- Manager (to manage membership and delegation)
	- Worker (which run swarm services)

### Creating a Service
- When you create a service, you define its optimal state (number of replicas, network and storage resources available to it, ports the service exposes to the outside world, and more).
- Docker works to maintain that desired state.
- If a worker node becomes unavailable, Docker schedules that node’s tasks on other nodes.
- A task is a running container which is part of a swarm service and managed by a swarm manager, as opposed to a standalone container.

### Deploy Applications in Swarm
- Submit a service definition to a manager node.
- The manager node dispatches units of work called tasks to worker nodes.
- Manager nodes also perform the orchestration and cluster management functions required to maintain the desired state of the swarm.
- Manager nodes elect a single leader to conduct orchestration tasks.
- Worker nodes receive and execute tasks dispatched from manager nodes.
- By default manager nodes also run services as worker nodes, but you can configure them to run manager tasks exclusively and be manager-only nodes.
An agent runs on each worker node and reports on the tasks assigned to it.
- The worker node notifies the manager node of the current state of its assigned tasks so that the manager can maintain the desired state of each worker.

### Virtual Networks
- Docker networks can be classified as single host, such as ‘bridge’ network. Or multiple hosts such as ‘overlay’ network.
- The overlay network driver creates a distributed network among multiple Docker daemon hosts.
- This network sits on top of (overlays) the host-specific networks, allowing containers connected to it (including swarm service containers) to communicate securely when encryption is enabled.
- Docker transparently handles routing of each packet to and from the correct Docker daemon host and the correct destination container.

## Kubernetes

### Kubernetes Introduction
- Kubernetes is an open-source orchestrator for deploying containerized applications.
- Originally developed by Google, inspired by a decade of experience deploying scalable, reliable systems in containers.
- **VELOCITY, SCALING, ABSTRACTION, EFFICIENCY**

### Velocity
- Kubernetes can provide the tools that you need to move quickly, while staying available.
- The core concepts that enable this are:
	- Immutability
	- Declarative configuration
	- Online self-healing systems
	- Immutability
- Traditionally, computers and software systems have been treated as mutable infrastructure.
- With mutable infrastructure, changes are applied as incremental updates to an existing system.
- These updates can occur all at once, or spread out across a long period of time.
- A system upgrade via the `apt - get update` tool is a good example of an update to a mutable system.
- In an immutable system, rather than a series of incremental updates and changes, an entirely new, complete image is built, where the update simply replaces the entire image with the newer image in a single operation.
- There are no incremental changes.
- This is a significant shift from the more traditional world of configuration management.

### No Incremental Changes
- To make this more concrete in the world of containers, consider two different ways to upgrade your software:
	- You can log in to a container, run a command to download your new software, kill the old server, and start the new one.
	- You can build a new container image, push it to a container registry, kill the existing container, and start a new one.

### Declarative Configuration
- Declarative configuration is an alternative to imperative configuration.
- Where the state of the world is defined by the execution of a series of instructions rather than a declaration of the desired state of the world.
- While imperative commands define actions, declarative configurations define state.

### Self-Healing Systems
- Kubernetes is an online, self-healing system.
- When it receives a desired state configuration, it does not simply take a set of actions to make the current state match the desired state a single time.
- It continuously takes actions to ensure that the current state matches the desired state.
- This means that not only will Kubernetes initialize your system, but it will guard it against any failures or perturbations that might destabilize the system and affect reliability.

### Kubernetes Architecture
- The main components of a Kubernetes cluster include:
	- **Nodes**: Nodes are VMs or physical servers that host containerized applications. Each node in a cluster can run one or more application instance. There can be as few as one node, however, a typical Kubernetes cluster will have several nodes (and deployments with hundreds or more nodes are not uncommon).
	- **Image Registry**: Container images are kept in the registry and transferred to nodes by the control plane for execution in container pods.
	- **Pods**: Pods are where containerized applications run. They can include one or more containers and are the smallest unit of deployment for applications in a Kubernetes cluster.

### Kubernetes Tutorial Link:
> https://kubernetes.io/docs/tutorials/kubernetes - basics/
