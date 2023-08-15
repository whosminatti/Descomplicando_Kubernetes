# Se virando no Kubernetes
Esse repositório foi criado com o intuito de servir como bloco de anotações e, quem sabe, material de apoio a quem o encontrar. 
As informações desse repositório tem como o curso Kubernetes Essentials da [Linuxtips](https://linuxtips.io/) e o projeto [Descomplicando Kubernetes](https://github.com/badtuxx/DescomplicandoKubernetes) criado pelo [Jeferson Fernando](https://twitter.com/badtux_). Mais detalhes podem ser encontrados nos linkds informados. 

<br><br>

## Primeiros passos

### Instalação Kubectl
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

### Instalação Docker (pré-requisito)
```
curl -fsSL https://get.docker.com | bash
```

### Instalação Kind
```
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
sudo chmod +x kind
sudo mv kind /usr/local/bin
```
