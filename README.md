# Explorando Funcionalidades do Ingress Canary

Esse projeto cria um cluster k8s para experimento da funcionalidade de balancear
carga dinamicamente entre dois serviços através do ingress utilizando a técnica canary.

Documentação oficial
[Como funciona o Ingress Nginx do K8s](https://kubernetes.github.io/ingress-nginx/how-it-works)

[Estrutura de código do Ingress Nginx Controller](https://kubernetes.github.io/ingress-nginx/developer-guide/code-overview)

# Instalando kubectl

Para instalar o `kubectl` em uma arquitetura `amd64`,
execute esse snippet de código: 

```bash
# Install kubectl binary with curl on Linux 
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

# Install kubectl
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Test kubectl client
kubectl version --client
```

Fonte: https://kubernetes.io/docs/tasks/tools/install-kubectl-linux

# Instalando minikube

O minikube pode ser usado para criar um cluster
k8s localmente para testar com mais facilidade
em um escopo fechado.

Para instalar o minikube execute:

```bash
# Instalando minikube localmente
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
```

Para iniciar o ambiente do minikube execute:

```bash
minikube start

😄  minikube v1.33.1 on Ubuntu 22.04
✨  Automatically selected the docker driver
📌  Using Docker driver with root privileges
👍  Starting "minikube" primary control-plane node in "minikube" cluster
🚜  Pulling base image v0.0.44 ...
🔥  Creating docker container (CPUs=2, Memory=7900MB) ...
🐳  Preparing Kubernetes v1.30.0 on Docker 26.1.1 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔗  Configuring bridge CNI (Container Networking Interface) ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

Para habilitar o ingress no minikube vamos precisar executar:

```bash
minikube addons enable ingress
```

# Exemplos

1. [Exemplo básico do ingress canary](examples/basic/README.md)
2. [Exemplo com canary por header](examples/by-header/README.md)
3. [Exemplo com canary por header em diferentes namespaces](examples/different-namespaces/README.md)
3. [Exemplo com canary e stable por header em diferentes namespaces](examples/different-namespaces-stable/README.md)

# Limpando o ambiente

Para excluir os container do minikube e
deixar o ambiente limpo, execute:

```bash
minikube delete

🔥 Deleting "minikube" in docker ...
🔥 Deleting container "minikube" ...
🔥 Removing /home/rafael-mateus/.minikube/machines/minikube ...
💀 Removed all traces of the "minikube" cluster.
```
