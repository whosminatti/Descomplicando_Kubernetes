# Se virando no Kubernetes
Esse repositório foi criado com o intuito de servir como bloco de anotações e, quem sabe, material de apoio a quem o encontrar. 
As informações desse repositório tem como origem o curso Kubernetes Essentials da [Linuxtips](https://linuxtips.io/) e o projeto [Descomplicando Kubernetes](https://github.com/badtuxx/DescomplicandoKubernetes) criado pelo [Jeferson Fernando](ttps://twitter.com/badtux_). Mais detalhes podem ser encontrados nos linkds informados. 

<br><br>
### Conceitos básicos

**O que é Container Engine?**
>O Container Engine é o responsável por gerenciar as imagens e volumes, é ele o responsável por garantir que os os recursos que os containers estão utilizando está devidamente isolados, a vida do container, storage, rede, etc.

**O que é OCI?**
>A OCI é uma organização sem fins lucrativos que tem como objetivo padronizar a criação de containers, para que possam ser executados em qualquer ambiente. A OCI foi fundada em 2015 pela Docker, CoreOS, Google, IBM, Microsoft, Red Hat e VMware e hoje faz parte da Linux Foundation.

**O que é Container Runtime?**
>O Container Runtime é o responsável por executar os containers nos nós.

### O que é o Kubernetes? 
>O projeto Kubernetes foi desenvolvido pela Google, em meados de 2014, para atuar como um orquestrador de contêineres para a empresa.<br>
O Kubernetes (k8s), cujo termo em Grego significa "timoneiro", é um projeto open source que conta com design e desenvolvimento baseados no projeto Borg, que também é da Google.<br>
Como Kubernetes é uma palavra difícil de se pronunciar - e de se escrever - a comunidade simplesmente o apelidou de k8s, seguindo o padrão i18n (a letra "k" seguida por oito letras e o "s" no final), pronunciando-se simplesmente "kates". 

## Arquitetura K8s
>Assim como os demais orquestradores disponíveis, o k8s também segue um modelo control plane/workers, constituindo assim um cluster, onde para seu funcionamento é recomendado no mínimo três nós: o nó control-plane, responsável (por padrão) pelo gerenciamento do cluster, e os demais como workers, executores das aplicações que queremos executar sobre esse cluster.

**_É possível criar um cluster Kubernetes rodando em apenas um nó, porém é recomendado somente para fins de estudos e nunca executado em ambiente produtivo._**
Existem diversas soluções que irão criar um cluster Kubernetes localmente, utilizando máquinas virtuais ou o Docker, por exemplo.
Para esse caso, adotaremos o Kind.

**API Server**
>É um dos principais componentes do k8s. Este componente fornece uma API que utiliza JSON sobre HTTP para comunicação, onde para isto é utilizado principalmente o utilitário kubectl, por parte dos administradores, para a comunicação com os demais nós, como mostrado no gráfico. Estas comunicações entre componentes são estabelecidas através de requisições REST;

**etcd** 
>O etcd é um datastore chave-valor distribuído que o k8s utiliza para armazenar as especificações, status e configurações do cluster. Todos os dados armazenados dentro do etcd são manipulados apenas através da API. Por questões de segurança, o etcd é por padrão executado apenas em nós classificados como control plane no cluster k8s, mas também podem ser executados em clusters externos, específicos para o etcd, por exemplo;

**Scheduler** 
>O scheduler é responsável por selecionar o nó que irá hospedar um determinado pod (a menor unidade de um cluster k8s - não se preocupe sobre isso por enquanto, nós falaremos mais sobre isso mais tarde) para ser executado. Esta seleção é feita baseando-se na quantidade de recursos disponíveis em cada nó, como também no estado de cada um dos nós do cluster, garantindo assim que os recursos sejam bem distribuídos. Além disso, a seleção dos nós, na qual um ou mais pods serão executados, também pode levar em consideração políticas definidas pelo usuário, tais como afinidade, localização dos dados a serem lidos pelas aplicações, etc;

**Controller Manager** 
>É o controller manager quem garante que o cluster esteja no último estado definido no etcd. Por exemplo: se no etcd um deploy está configurado para possuir dez réplicas de um pod, é o controller manager quem irá verificar se o estado atual do cluster corresponde a este estado e, em caso negativo, procurará conciliar ambos;

**Kubelet** 
>O kubelet pode ser visto como o alente do k8s que é executado nos nós workers. Em cada nó worker deverá existir um agente Kubelet em execução. O Kubelet é responsável por de fato gerenciar os pods que foram direcionados pelo controller do cluster, dentro dos nós, de forma que para isto o Kubelet pode iniciar, parar e manter os contêineres e os pods em funcionamento de acordo com o instruído pelo controlador do cluster;

**Kube-proxy** 
>Age como um proxy e um load balancer. Este componente é responsável por efetuar roteamento de requisições para os pods corretos, como também por cuidar da parte de rede do nó;


## Conceitos-chave do k8s
É importante saber que a forma como o k8s gerencia os contêineres é ligeiramente diferente de outros orquestradores, como o Docker Swarm, sobretudo devido ao fato de que ele não trata os contêineres diretamente, mas sim através de pods. Vamos conhecer alguns dos principais conceitos que envolvem o k8s a seguir:

**Pod**
>É o menor objeto do k8s. Como dito anteriormente, o k8s não trabalha com os contêineres diretamente, mas organiza-os dentro de pods, que são abstrações que dividem os mesmos recursos, como endereços, volumes, ciclos de CPU e memória. Um pod pode possuir vários contêineres;

**Deployment**
>É um dos principais controllers utilizados. O Deployment, em conjunto com o ReplicaSet, garante que determinado número de réplicas de um pod esteja em execução nos nós workers do cluster. Além disso, o Deployment também é responsável por gerenciar o ciclo de vida das aplicações, onde características associadas a aplicação, tais como imagem, porta, volumes e variáveis de ambiente, podem ser especificados em arquivos do tipo yaml ou json para posteriormente serem passados como parâmetro para o kubectl executar o deployment. Esta ação pode ser executada tanto para criação quanto para atualização e remoção do deployment;

**ReplicaSets**
>É um objeto responsável por garantir a quantidade de pods em execução no nó;

**Services**
>É uma forma de você expor a comunicação através de um ClusterIP, NodePort ou LoadBalancer para distribuir as requisições entre os diversos Pods daquele Deployment. Funciona como um balanceador de carga.


## Portas
**CONTROL PLANE**
| **Protocol** | **Direction** | **Port Range** | **Purpose** | **Used By** |
| ------------ | ------------- | -------------- | ----------- | ----------- |		
| TCP |	Inbound |	6443* |	Kubernetes API server |	All |
| TCP |	Inbound |	2379-2380 |	etcd server client API |	kube-apiserver, etcd |
| TCP |	Inbound |	10250 |	Kubelet API |	Self, Control plane |
| TCP |	Inbound |	10251 |	kube-scheduler |	Self |
| TCP |	Inbound |	10252 |	kube-controller-manager |	Self |

*Toda porta marcada por * é customizável, você precisa se certificar que a porta alterada também esteja aberta.

WORKERS
| **Protocol** |	**Direction** |	**Port Range** |	**Purpose** |	Used By** |
| ------------ | -------------- | -------------- | ------------ | --------- |
| TCP |	Inbound |	10250 |	Kubelet API |	Self, Control plane |
| TCP |	Inbound |	30000-32767 |	NodePort |	Services All |
 

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
