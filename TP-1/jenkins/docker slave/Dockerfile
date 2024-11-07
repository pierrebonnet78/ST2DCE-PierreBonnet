# Use the official Ubuntu base image
FROM ubuntu:latest

# Update and upgrade the system

RUN apt-get update && apt-get upgrade -y 
RUN apt install sudo -y
RUN apt install gnupg -y

#Set up Docker's Official Repository
RUN apt update && \
    apt install apt-transport-https ca-certificates curl software-properties-common -y

RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Add Docker's official GPG key and repository
RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker
RUN apt update && \
    apt install docker-ce docker-ce-cli containerd.io -y 

# Enable Docker
RUN systemctl enable docker

# Install Java 
RUN apt-get update && \
    apt-get install -y openjdk-17-jdk

RUN export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64 && \
    export PATH=$JAVA_HOME/bin:$PATH

# Install Minikube
RUN curl -LO https://storage.googleapis.com/minikube/releases/v1.31.2/minikube-linux-arm64 && \
    install minikube-linux-arm64 /usr/local/bin/minikube

# Install kubectl
RUN curl -LO "https://dl.k8s.io/release/v1.28.3/bin/linux/arm64/kubectl" && \
    chmod +x kubectl && \
    mv kubectl /usr/local/bin/

# nstall Docker Compose
RUN curl -SL "https://github.com/docker/compose/releases/download/v2.23.0/docker-compose-$(uname -s)-$(uname -m)" -o docker-compose && \
    chmod +x docker-compose && \
    mv docker-compose /usr/local/bin/docker-compose

RUN apt update && \
    apt install -y openssh-server

RUN mkdir /var/run/sshd && \
    ssh-keygen -t rsa -b 4096 -f /root/.ssh/id_rsa -N "" && \
    cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys && \
    chmod 600 /root/.ssh/authorized_keys

# Clean up apt cache
RUN apt-get clean

# Expose port 22 for SSH
EXPOSE 22

# Start the SSH service in the foreground (important for Docker to keep container running)
CMD ["/usr/sbin/sshd", "-D"]
