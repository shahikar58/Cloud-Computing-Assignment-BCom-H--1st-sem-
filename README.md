| Tool                   | Description                                                             |
| ---------------------- | ----------------------------------------------------------------------- |
| **AWS EC2**            | Virtual machines in the AWS cloud used to host services.                |
| **Apache HTTP Server** | Popular open-source web server for hosting websites.                    |
| **Docker**             | Platform to build, run, and manage containers.                          |
| **Minikube**           | Tool for running a single-node Kubernetes cluster locally.              |
| **K3s**                | Lightweight Kubernetes distribution optimized for IoT and edge devices. |



AWS Cloud
│
├── EC2 Instance (Ubuntu)
│    ├── Apache Web Server (Hello World)
│    ├── Docker
│    ├── Minikube (local K8s)
│    └── K3s (lightweight K8s)
│
└── Security Group: Allow ports 22 (SSH), 80 (HTTP), 443 (HTTPS)



1️⃣ Launch EC2 Instances

AMI: Ubuntu 22.04 LTS

Instance Type: t2.micro (free tier)

Security Group: Allow inbound on ports 22, 80, 443



2️⃣ Install Apache Web Server

```
sudo apt update -y
sudo apt install apache2 -y
sudo systemctl enable apache2
sudo systemctl start apache2
```

```
curl http://localhost
```


3️⃣ Hello World Website
```
echo "<h1>Hello World from Apache on AWS EC2!</h1>" | sudo tee /var/www/html/index.html
sudo systemctl restart apache2
```

Check in browser:
➡️ http://<EC2_PUBLIC_IP>

4️⃣ Install Docker
```
sudo apt update -y
sudo apt install ca-certificates curl gnupg -y
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker $USER
```
```
docker run hello-world
```


5️⃣ Install Minikube
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

Start cluster:
```
minikube start --driver=docker
kubectl get nodes
```


6️⃣ Install K3s

K3s is a lightweight Kubernetes distribution by Rancher.
```
curl -sfL https://get.k3s.io | sh -
sudo systemctl status k3s
sudo k3s kubectl get nodes
```


Example install_apache.sh:
```
#!/bin/bash
sudo apt update -y
sudo apt install apache2 -y
sudo systemctl enable apache2
echo "<h1>Hello World from Apache on AWS EC2!</h1>" | sudo tee /var/www/html/index.html
sudo systemctl restart apache2
```


Key Pair: Generate and use for SSH access
