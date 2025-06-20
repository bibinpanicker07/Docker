1. What is Docker? How does it differ from virtual machines?
Docker is a popular open-source containerization platform that allows developers to automate the deployment of applications inside lightweight, portable containers. Docker streamlines the process of creating, deploying, and managing containers. It provides tools for building and sharing containerized applications and includes a rich ecosystem of features like Docker Compose, Docker Swarm, and integration with Kubernetes.

Docker is a platform for containerizing applications, allowing them to run in isolated environments. Unlike VMs, Docker containers share the host OS kernel, making them lightweight and faster to start.

A self-sufficient runtime for containers

2. Explain the difference between CMD and ENTRYPOINT in a Dockerfile.
CMD: Provides default arguments for the container. Can be overridden during docker run.
ENTRYPOINT: Defines the main command for the container. Any arguments passed to docker run are appended to the ENTRYPOINT.

Conatiners: 
Conatiner are Nothing but an OS process, that behaves like VM using the Linux COncept of NS and Cgroup

kernel is the core component that acts as a bridge between the hardware and the software. It manages all the critical operations of the system, including memory management, process scheduling, and device control. The kernel is the first program loaded when the system starts and the last to shut down, ensuring the smooth functioning of the entire system. 

What is a Container?
A container is a lightweight, standalone, and executable software package that includes everything needed to run an application: code, runtime, libraries, and dependencies. Containers ensure that applications run reliably across different environments by isolating them from the underlying infrastructure. They use the host operating system kernel, making them much more efficient and faster to deploy than virtual machines.

Docker Architecture and Components:
Docker uses a client-server architecture:
Docker Client:The primary interface for users to interact with Docker.
Commands like docker build, docker run, and docker pull are sent to the Docker Daemon.
Docker Daemon (dockerd):The main service responsible for creating, managing, and running Docker containers.
Listens for Docker API requests from the Docker client.
Docker Images:Immutable snapshots of a container, including the application code, libraries, and dependencies.
Can be pulled from Docker Hub or custom repositories.
Docker Containers:Runtime instances of Docker images.
Each container is isolated but shares the OS kernel with the host.
Docker Registry:A centralized location for storing and distributing Docker images.
Examples: Docker Hub, Amazon ECR, Google Container Registry.
Storage Drivers:Manage how data is written and read on disk.
Examples: OverlayFS, AUFS.
Networking:Docker provides different networking modes (bridge, host, overlay) to allow communication between containers or with external networks.



Dockerfile:A Dockerfile is a script containing a series of instructions to build a Docker image. 
It automates the process of creating images, ensuring consistency and repeatability.

Common Dockerfile Instructions
```
FROM    Specifies the base image. For eg for Python App.
RUN     Runs a command in the container.
COPY    Copies files from the host to the container.
WORKDIR Sets the working directory inside the container.
EXPOSE  Informs Docker that the container listens on a port.
ENV     Sets environment variables.
CMD        Provides the default command for the container.
ENTRYPOINT Defines the main executable command for the container.
```

docker commit:Takes a snapshot of a running or stopped container.
Saves it as a new Docker image.
Example: docker commit mycontainer mycustomimage:latest
You can then reuse or share this image just like any other Docker image.

docker0 is a virtual Ethernet bridge interface created by Docker. It acts as the default network bridge for containers that are not explicitly connected to a user-defined network, enabling them to communicate with each other and with the host machine. 


The Docker -itd flag is a combination of three options used with docker run. Here's what each one means:
🔹 -i (interactive)
Keeps STDIN (input) open so you can interact with the container.
Useful when running interactive programs like a shell.
🔹 -t (tty)
Allocates a pseudo-terminal inside the container.
Makes the command-line interface behave more like a regular terminal (e.g., for bash).
🔹 -d (detached)
Runs the container in the background.
You won't see live output unless you attach or log into it.
💡 So, -itd together means:
"Run the container in the background (-d), but also set it up to be interactive (-i) with a terminal (-t) if needed later." 



The CMD ```["nginx", "-g", "daemon off;"] ```command in a Dockerfile is used to start the Nginx web server as the main process within a container and keep it running in the foreground, rather than as a background daemon. This ensures that the Docker container stays active as long as Nginx is running. 
Here's a breakdown:
CMD:This Dockerfile instruction specifies the default command to run when the container starts. 
["nginx", "-g", "daemon off;"]:
This is the command that will be executed. It starts Nginx with the -g flag, which allows you to pass configuration options, and the daemon off; directive tells Nginx not to run as a daemon (background process). 
Foreground Process:By preventing Nginx from running as a daemon, it becomes the main process of the container. Docker tracks this process, and the container will stay running as long as Nginx is active. 
In essence, this command is a Docker best practice for running Nginx. It ensures that the container behaves as expected and allows for easier debugging if issues arise. If Nginx were to run as a daemon, it would quickly exit, and the container would stop running

| Web Server | Correct CMD                              |
| ---------- | ---------------------------------------- |
| Apache     | `CMD ["apache2ctl", "-D", "FOREGROUND"]` |
| Nginx      | `CMD ["nginx", "-g", "daemon off;"]`     |


ENTRYPOINT ["python"]
CMD ["abc.py"]
Here entrypoint remains constant and cmd can by overriden by anything else Eg:xyz.py

```docker run -itd -p 80:80 apache-custom ``` --> LHS port is what web page is using and RHS is TCP port

```sudo usermod -aG docker $USER``` #logout and login again for it to reflect
is used to add your current user to the Docker group so you can run Docker commands without using sudo. Here's a breakdown of each part:
🔍 Command Breakdown:
Part	Meaning
sudo	Runs the command as a superuser (administrator).
usermod	A Linux command to modify a user account.
-aG	Two options combined:
- -a (append)	Adds the user to the group(s) without removing them from others.
- -G docker	Specifies the group to add the user to — in this case, the docker group.
$USER	An environment variable that expands to your current username.
to view the groups -> cd /etc/group


Running a container as a job usually means running a short-lived Docker container that performs a task, then exits. This is different from long-running services or daemons.
A "job container" is:
Started
Performs a task (e.g., runs a script, builds code, backs up data)
Stops automatically once the task is complete
✅ Example 1: Run a script in a container
```docker run --rm ubuntu bash -c "echo Hello, World!"```
--rm: Automatically removes the container after it exits
ubuntu: Image used
bash -c "...": The command that runs in the container
✅ Example 2: Job that runs a Python script
Assume you have a Python script (job.py) in your current directory:
```docker run --rm -v "$PWD":/app -w /app python:3.10 python job.py```
Mounts current directory to /app inside the container
Runs job.py using the official Python image
Deletes the container afterward
🛠️ Use Cases for Job Containers
CI/CD tasks (e.g., test runners, linters, compilers)
Database migrations
Scheduled tasks (when run by a cron job or orchestrator)
One-time tasks like cleanup scripts

Docker Volume
pwd
/home/ubuntu/jenkinshome
```docker run -itd -p 80:80 -v /home/ubuntu/jenkinshome:/var/jenkins_home --name=jenk jenkins/jenkins:lts```

or docker volume create myvol
docker volumne inspect myvol

| Feature                | `-v myvol:/code`            | `-v $(pwd):/code`                  |
| ---------------------- | --------------------------- | ---------------------------------- |
| Type                   | Docker volume               | Bind mount                         |
| Where it's stored      | Inside Docker system folder | Your actual local project folder   |
| Host-visible files     | ❌ Not easily accessible     | ✅ Directly accessible and editable |
| Persistent across runs | ✅ Yes                       | ✅ Yes (as long as the path exists) |
| Best for               | Data volumes, DBs, backups  | Code development, local testing    |

Docker follows a layered architecture
If we use RUN multiple times it will create new layers so the best practise is use to use RUN once and the commands separated by &&



File Level permissions-
Permissions are represented using a three-digit octal (base-8) number. Each digit corresponds to a permission set (owner, group, others) and is the sum of the values:
4: Read (r) Grants read permission
2: Write (w) Grant write permission
1: Execute (x) Grant execute permission
Usage of `chmod +x <filename>` in Linux
The command chmod +x <filename> is used to add executable permissions to a file in Linux system. 



### How does Docker ensure portability of applications?
Answer:Docker achieves portability by encapsulating an application, its dependencies, and its runtime environment into an image. Images are immutable and can run on any system with Docker installed, regardless of the underlying OS.

### How does Docker handle inter-container communication?

Docker provides several built-in mechanisms to handle inter-container communication, especially useful when you're running multi-container applications (like a web app + database). Here's how it works:

🔗 1. Bridge Network (Default)
When you run containers without specifying a custom network, Docker connects them to a default bridge network. But:

Containers can’t reach each other by name — only by IP address.

You’d need to manually link them or configure them.

Example:
bash
Copy
Edit
docker run -d --name webapp nginx
docker run -d --name db mysql
➡️ These can't talk to each other by name on the default bridge.

🌐 2. User-Defined Bridge Network ✅ Recommended
Creating a custom bridge network allows containers to communicate using container names as hostnames.

Steps:
bash
Copy
Edit
# Create custom network
docker network create mynet

# Run containers on that network
docker run -d --name db --network mynet mysql
docker run -d --name webapp --network mynet nginx
➡️ Now, webapp can talk to db just by using the name db.

bash
Copy
Edit
ping db
✅ Benefits:
Name-based communication

DNS-based service discovery

Easier scaling (e.g., with Docker Compose)

📦 3. Docker Compose (Under the Hood)
Docker Compose automatically creates a custom bridge network for all services defined in a docker-compose.yml.

yaml
Copy
Edit
services:
  web:
    image: nginx
  db:
    image: mysql
➡️ web can connect to db using the hostname db.

🛠 4. Host Networking (Advanced Use Case)
bash
Copy
Edit
docker run --network host ...
Container shares the host’s network stack.

No isolation — all ports exposed directly.

Useful for performance or special network apps (e.g. monitoring, VPNs)

⚠️ Not available on Docker Desktop (macOS/Windows) — only on Linux.

🕸 5. Overlay Network (for Swarm / Kubernetes)
Used when containers run on different hosts in a cluster (like Docker Swarm or Kubernetes):

Docker manages an overlay network that spans multiple machines.

Services discover each other by name via internal DNS.

🧪 Example: Simple Web App + Redis
bash
Copy
Edit
docker network create appnet
docker run -d --name redis --network appnet redis
docker run -it --name web --network appnet python:3 bash
Inside web:

bash
Copy
Edit
pip install redis
python
>>> import redis
>>> r = redis.Redis(host='redis', port=6379)
>>> r.set('key', 'value')
📝 Summary Table
Network Type	Name-based Access	Same Host	Multi-host
Default bridge	❌ No	✅ Yes	❌ No
Custom bridge	✅ Yes	✅ Yes	❌ No
Docker Compose	✅ Yes	✅ Yes	❌ No
Host	❌ (shares host)	✅ Yes	❌ No
Overlay (Swarm/K8s)	✅ Yes	❌ Optional	✅ Yes

