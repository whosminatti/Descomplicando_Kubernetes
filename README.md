# Se virando no Kubernetes
Esse repositório foi criado com o intuito de servir como bloco de anotações e, quem sabe, material de apoio a quem o encontrar. 
As informações desse repositório tem como o curso Kubernetes Essentials da [Linuxtips](https://linuxtips.io/) e o projeto [Descomplicando Kubernetes](https://github.com/badtuxx/DescomplicandoKubernetes) criado pelo [Jeferson Fernando](ttps://twitter.com/badtux_). Mais detalhes podem ser encontrados nos linkds informados. 

<br><br>
### Conceitos básicos

**O que é Container Engine?**
>O Container Engine é o responsável por gerenciar as imagens e volumes, é ele o responsável por garantir que os os recursos que os containers estão utilizando está devidamente isolados, a vida do container, storage, rede, etc.

**O que é OCI?**
>A OCI é uma organização sem fins lucrativos que tem como objetivo padronizar a criação de containers, para que possam ser executados em qualquer ambiente. A OCI foi fundada em 2015 pela Docker, CoreOS, Google, IBM, Microsoft, Red Hat e VMware e hoje faz parte da Linux Foundation.

**O que é Container Runtime?**
>O Container Runtime é o responsável por executar os containers nos nós.

### O que é o Kubernetes? ###
>O projeto Kubernetes foi desenvolvido pela Google, em meados de 2014, para atuar como um orquestrador de contêineres para a empresa.<br>
O Kubernetes (k8s), cujo termo em Grego significa "timoneiro", é um projeto open source que conta com design e desenvolvimento baseados no projeto Borg, que também é da Google.<br>
Como Kubernetes é uma palavra difícil de se pronunciar - e de se escrever - a comunidade simplesmente o apelidou de k8s, seguindo o padrão i18n (a letra "k" seguida por oito letras e o "s" no final), pronunciando-se simplesmente "kates". 

**Arquitetura K8s**
>Assim como os demais orquestradores disponíveis, o k8s também segue um modelo control plane/workers, constituindo assim um cluster, onde para seu funcionamento é recomendado no mínimo três nós: o nó control-plane, responsável (por padrão) pelo gerenciamento do cluster, e os demais como workers, executores das aplicações que queremos executar sobre esse cluster.

**_É possível criar um cluster Kubernetes rodando em apenas um nó, porém é recomendado somente para fins de estudos e nunca executado em ambiente produtivo._**

Existem diversas soluções que irão criar um cluster Kubernetes localmente, utilizando máquinas virtuais ou o Docker, por exemplo.
Para esse caso, adotaremos o Kind.

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
