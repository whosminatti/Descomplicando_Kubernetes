# Descomplicando_Kubernetes
Repositório base de configuração e criação mais simples de um cluster Kind

# Instalação Kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo chmod +x kubectl
sudo mv kubectl /usr/local/bin/

# Instalaçao Kind (Kubernetes in Docker)
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
sudo chmod +x kind
sudo mv kind /usr/local/bin
curl -fsSL https://get.docker.com | bash
