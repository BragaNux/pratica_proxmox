# 💻 Infraestrutura Virtual com Proxmox VE em VirtualBox

**Aluno:** Brayan Martins

---

## 🌟 Objetivo

Montar uma infraestrutura de nuvem privada simulada usando **Proxmox VE** rodando dentro do **VirtualBox**, com duas VMs internas (aplicação + banco de dados), aplicando conceitos como IaaS, snapshots, rede e segurança.

---

## ✅ Etapas Concluídas

### 🧱 Instalação do VirtualBox no Linux Mint

* Adição de repositório Oracle e chave:

  ```bash
  wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
  echo "deb [arch=amd64] https://download.virtualbox.org/virtualbox/debian $(lsb_release -cs) contrib" | sudo tee /etc/apt/sources.list.d/virtualbox.list
  sudo apt update
  sudo apt install virtualbox-7.0
  ```
* Correção de erro do kernel:

  ```bash
  sudo apt install build-essential dkms linux-headers-$(uname -r)
  sudo /sbin/vboxconfig
  ```

### 🧹 Criação da VM do Proxmox

* ISO do Proxmox VE 8.4 usada
* Configuração da VM:

  * 4 GB RAM
  * 32 GB de disco
  * Rede em modo **bridge**
* Ativada virtualização aninhada:

  ```bash
  VBoxManage modifyvm "Proxmox" --nested-hw-virt on
  ```

### 🌐 Instalação do Proxmox

* Instalação via modo gráfico
* IP fixo configurado:

  ```
  IP: 192.168.67.250/20
  Gateway: 192.168.67.1
  DNS: 8.8.8.8
  ```
* Acesso via navegador:

  ```
  https://192.168.67.250:8006
  ```

### 📦 Upload da ISO do Ubuntu

* Realizado via interface Web:
  `Storage > local > Conteúdo > Enviar`

### 🖥️ Criação da VM `vm-app`

* ID: `101`
* Disco: `32 GB`
* CPU: `1 vCPU`
* RAM: `1 GB`
* Rede: `vmbr0` com VirtIO (depois deu problema)
* ISO usada: `ubuntu-24.04.2-live-server-amd64.iso`

### 🪰 Instalação do Ubuntu na VM

* Instalação feita via console VNC
* SSH Server habilitado
* Sistema instalado corretamente

---

## ❌ Erros Encontrados

### 🔥 `modprobe vboxdrv failed`

> Kernel modules não carregados corretamente

* Corrigido com:

  ```bash
  sudo /sbin/vboxconfig
  ```

### 🔥 `KVM virtualization configured, but not available`

> Proxmox tentava usar KVM mesmo sem suporte no VirtualBox

* Solução:

  ```bash
  qm set 101 -machine pc
  ```

  ou desativar KVM na aba **Opções** da VM

### 🔥 Console travando no `nano`

### 🔥 `cdrom.mount` error

> ISO ainda conectada após instalação

* Corrigido removendo a ISO manualmente em:
  `Hardware > Drive de CD/DVD > Editar > Não usar mídia`

### 🔥 Kernel travando com `kworker`
