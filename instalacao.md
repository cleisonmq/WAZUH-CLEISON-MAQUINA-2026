# 🛡️ Wazuh SIEM - Guia Completo de Instalação

<div align="center">

![Wazuh](https://img.shields.io/badge/Wazuh-SIEM-blue?style=for-the-badge)
![Linux](https://img.shields.io/badge/Linux-Ubuntu%20%7C%20CentOS-black?style=for-the-badge&logo=linux)
![Security](https://img.shields.io/badge/Cybersecurity-SOC-red?style=for-the-badge)
![Docker](https://img.shields.io/badge/Docker-Integrated-2496ED?style=for-the-badge&logo=docker)

</div>

---

# 📌 Sobre o Projeto

Implementação de uma infraestrutura SIEM distribuída utilizando:

- ✅ Wazuh Indexer
- ✅ Wazuh Manager
- ✅ Wazuh Dashboard
- ✅ Docker Agent
- ✅ SMTP Alerts
- ✅ Hardening SSH

---

# 💻 Ambiente Laboratorial

## 🖥️ Sistemas Operativos

| Serviço | Sistema Operativo |
|---|---|
| 🗄️ Wazuh Indexer | CentOS Stream 10 |
| 🛡️ Wazuh Manager | Ubuntu 22.04 LTS |
| 📊 Wazuh Dashboard | Ubuntu 22.04 LTS |

---

## ⚙️ Recursos Utilizados

| Máquina | CPU | RAM | Disco |
|---|---|---|---|
| 🗄️ Indexer | 2 vCPU | 4 GB | 30 GB |
| 🖥️ Manager/Dashboard | 2 vCPU | 4 GB | 30 GB |

---

# 🌐 Segmentação de Rede

| Rede | Função |
|---|---|
| 🌍 NAT (10.x.x.x/24) | Agentes e endpoints |
| 🔒 Host-Only (192.168.x.x/24) | Comunicação interna Wazuh |

---

# 🔐 Hardening SSH

## 📦 Instalar OpenSSH

### Ubuntu

```bash
sudo apt update && sudo apt install openssh-server -y
```

### CentOS Stream

```bash
sudo dnf install openssh-server -y
```

---

## 🔑 Gerar Chaves SSH

```bash
ssh-keygen -t ed25519
```

---

## 📁 Copiar chave pública

```bash
ssh-copy-id usuario@ip-servidor
```

---

## ⚙️ Configurar SSH

Editar:

```bash
sudo nano /etc/ssh/sshd_config
```

---

## 🔒 Alterações recomendadas

```ini
Port 2222
PermitRootLogin no
PasswordAuthentication no
PermitEmptyPasswords no
PubkeyAuthentication yes
```

---

## 🔄 Reiniciar SSH

### Ubuntu

```bash
sudo systemctl restart ssh
```

### CentOS

```bash
sudo systemctl restart sshd
```

---

# ⚙️ Instalação do Wazuh Indexer

## 📦 Atualizar Sistema

### CentOS Stream 10

```bash
sudo dnf update -y
```

---

## 📥 Download do Instalador

```bash
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
```

---

## 🔓 Permissões

```bash
chmod +x wazuh-install.sh
```

---

## 🧩 Gerar ficheiros de configuração

```bash
sudo ./wazuh-install.sh --generate-config-files
```

---

## 📂 Editar config.yml

```bash
nano config.yml
```

---

## 🗄️ Instalar Wazuh Indexer

```bash
sudo ./wazuh-install.sh --wazuh-indexer node-1
```

---

## 🚀 Inicializar Cluster

```bash
sudo ./wazuh-install.sh --start-cluster
```

---

## 🔍 Verificar serviço

```bash
systemctl status wazuh-indexer
```

---

## 📊 Verificar cluster

```bash
curl -k -u admin:admin https://localhost:9200
```

---

# 🛡️ Instalação do Wazuh Manager

## 📦 Atualizar Ubuntu

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 📥 Download do Instalador

```bash
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
```

---

## 🔓 Permissões

```bash
chmod +x wazuh-install.sh
```

---

## 📂 Copiar config.yml

```bash
scp config.yml user@manager:/home/user/
```

---

## 🖥️ Instalar Manager

```bash
sudo ./wazuh-install.sh --wazuh-server wazuh-server
```

---

## 🔍 Verificar serviço

```bash
systemctl status wazuh-manager
```

---

# 📊 Instalação do Wazuh Dashboard

## 📥 Download do Instalador

```bash
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
```

---

## 🔓 Permissões

```bash
chmod +x wazuh-install.sh
```

---

## 📊 Instalar Dashboard

```bash
sudo ./wazuh-install.sh --wazuh-dashboard wazuh-dashboard
```

---

## 🔍 Verificar serviço

```bash
systemctl status wazuh-dashboard
```

---

## 🌐 Aceder ao Dashboard

```text
https://IP-DO-SERVIDOR
```

---

# 🐳 Integração Docker

## 📍 Adicionar Agente

No Dashboard:

```text
Agent Management → Summary → Add Agent
```

---

## 🖥️ Instalar agente Linux

### Ubuntu/Debian

```bash
curl -so wazuh-agent.deb https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.7.0-1_amd64.deb
```

---

## 📦 Instalar pacote

```bash
sudo WAZUH_MANAGER='IP-DO-MANAGER' dpkg -i ./wazuh-agent.deb
```

---

## 🚀 Ativar serviço

```bash
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

---

## 🔍 Verificar agente

```bash
systemctl status wazuh-agent
```

---

# 📧 Configuração SMTP (Postfix)

## 📦 Instalar Postfix

```bash
sudo apt install postfix mailutils libsasl2-2 ca-certificates libsasl2-modules -y
```

---

## ⚙️ Editar configuração

```bash
sudo nano /etc/postfix/main.cf
```

---

## ✉️ Configuração Gmail SMTP

```ini
relayhost = [smtp.gmail.com]:587
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_sasl_security_options = noanonymous
smtp_tls_security_level = encrypt
smtp_tls_CAfile = /etc/ssl/certs/ca-certificates.crt
```

---

## 🔐 Criar credenciais SMTP

```bash
sudo nano /etc/postfix/sasl_passwd
```

---

## 📧 Adicionar credenciais

```ini
[smtp.gmail.com]:587 email@gmail.com:APP_PASSWORD
```

---

## 🔒 Aplicar permissões

```bash
sudo postmap /etc/postfix/sasl_passwd
sudo chmod 600 /etc/postfix/sasl_passwd
```

---

## 🔄 Reiniciar Postfix

```bash
sudo systemctl restart postfix
```

---

# ⚙️ Configurar Alertas no Wazuh

## 📂 Editar ossec.conf

```bash
sudo nano /var/ossec/etc/ossec.conf
```

---

## ✉️ Configuração SMTP

```xml
<global>
  <email_notification>yes</email_notification>
  <smtp_server>smtp.gmail.com</smtp_server>
  <email_from>email@gmail.com</email_from>
  <email_to>destino@gmail.com</email_to>
</global>
```

---

## 🔄 Reiniciar Wazuh Manager

```bash
sudo systemctl restart wazuh-manager
```

---

# 🚨 Funcionalidades Implementadas

- ✅ SIEM distribuído
- ✅ Dashboard Web
- ✅ Monitorização centralizada
- ✅ Integração Docker
- ✅ SMTP Alerts
- ✅ Hardening SSH
- ✅ Recolha de logs
- ✅ Monitorização de agentes
- ✅ Deteção de ameaças

---

# 📈 Melhorias Futuras

- 🔥 Integração Suricata
- 🛡️ Integração Fail2Ban
- ☁️ Kubernetes Monitoring
- 📊 Dashboards avançados
- 🤖 Resposta automática a incidentes

---

# 📚 Referências

- https://documentation.wazuh.com
- https://documentation.wazuh.com/current/quickstart.html
- https://github.com/olafhartong/sysmon-modular
- https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon

---

# 👨‍💻 Autor

<div align="center">

# Cleison Alves José Máquina

🎓 Técnico de Cibersegurança  
🛡️ Cybersecurity & Infrastructure  
🚀 Projeto Académico SIEM/Wazuh

</div>

---

<div align="center">

# ⭐ Se gostaste do projeto deixa uma estrela ⭐

</div>