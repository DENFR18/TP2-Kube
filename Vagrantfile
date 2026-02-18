# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Utilisation d'Ubuntu 20.04 comme demandé dans le TP 
  config.vm.box = "ubuntu/focal64"

  # Configuration réseau : IP privée pour faciliter l'accès SSH et les tests
  config.vm.network "private_network", ip: "192.168.56.10"

  # Configuration des ressources
  # Le TP demande 3072MB pour Minikube, donc on donne 4GB à la VM
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
    vb.cpus = 2
    vb.name = "TP-Kubernetes-Minikube"
  end

  # Script de provisioning pour installer Docker, Kubectl et Minikube automatiquement
  config.vm.provision "shell", inline: <<-SHELL
    set -e
    
    echo ">>> Mise à jour du système..."
    apt-get update -y
    apt-get install -y apt-transport-https ca-certificates curl gnupg lsb-release bash-completion jq

    # --- 1. Installation de Docker (TP02 Section 3.1) [cite: 33] ---
    echo ">>> Installation de Docker..."
    mkdir -p /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null
    apt-get update -y
    apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
    
    # Ajout de l'utilisateur vagrant au groupe docker [cite: 50]
    usermod -aG docker vagrant
    # On l'ajoute aussi pour l'utilisateur ubuntu si la box en a un
    id -u ubuntu &>/dev/null && usermod -aG docker ubuntu

    # --- 2. Installation de Kubectl (TP02 Section 3.2) [cite: 72] ---
    echo ">>> Installation de Kubectl..."
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
    install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
    rm kubectl

    # --- 3. Installation de Arkade (TP02 Section 3.2) [cite: 61] ---
    # Le TP suggère Arkade pour installer des outils
    echo ">>> Installation de Arkade..."
    curl -sLS https://get.arkade.dev | sh

    # --- 4. Installation de Minikube (TP02 Section 3.3) [cite: 73] ---
    echo ">>> Installation de Minikube..."
    curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
    install minikube-linux-amd64 /usr/local/bin/minikube
    rm minikube-linux-amd64

    # --- 5. Configuration (TP02 Section 3.4) [cite: 83] ---
    echo ">>> Configuration de l'autocomplétion..."
    echo 'source <(kubectl completion bash)' >> /home/vagrant/.bashrc
    echo 'source <(minikube completion bash)' >> /home/vagrant/.bashrc
    echo 'alias k=kubectl' >> /home/vagrant/.bashrc
    
    # Installation de Krew pour le TP03 [cite: 191]
    echo ">>> Préparation pour Krew (TP03)..."
    apt-get install -y git

    echo ">>> FIN DE L'INSTALLATION <<<"
    echo "Connectez-vous avec : vagrant ssh"
  SHELL
end