# 🛡️ SIEM Wazuh - Infraestrutura Distribuída

<div align="center">

![Wazuh](https://img.shields.io/badge/Wazuh-SIEM-blue?style=for-the-badge)
![Linux](https://img.shields.io/badge/Linux-Ubuntu%20%7C%20CentOS-black?style=for-the-badge&logo=linux)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)
![Cybersecurity](https://img.shields.io/badge/Cybersecurity-SOC-red?style=for-the-badge)

</div>

---

# 📌 Sobre o Projeto

Este projeto consiste na implementação de uma infraestrutura **SIEM (Security Information and Event Management)** utilizando o **Wazuh**, numa arquitetura distribuída com:

- ✅ Wazuh Indexer
- ✅ Wazuh Manager
- ✅ Wazuh Dashboard
- ✅ Agentes Linux
- ✅ Integração Docker
- ✅ Alertas SMTP
- ✅ Hardening SSH

O objetivo principal é centralizar logs, monitorizar eventos de segurança e detetar ameaças em tempo real.

---

# 🧠 O que é o Wazuh?

O **Wazuh** é uma plataforma Open Source de segurança focada em:

- 🔍 Deteção de intrusões
- 📊 SIEM
- 🛡️ XDR
- 🚨 Gestão de eventos
- 📁 Integridade de ficheiros
- ☁️ Segurança cloud
- 🐳 Monitorização Docker

---

# 🏗️ Arquitetura da Infraestrutura

```text
                    ┌────────────────────┐
                    │   Wazuh Dashboard  │
                    │   Visualização UI  │
                    └─────────┬──────────┘
                              │
                    ┌─────────▼──────────┐
                    │   Wazuh Manager    │
                    │  Processa Eventos  │
                    └─────────┬──────────┘
                              │
                    ┌─────────▼──────────┐
                    │   Wazuh Indexer    │
                    │ Armazena os Logs   │
                    └─────────┬──────────┘
                              │
         ┌────────────────────┼────────────────────┐
         │                    │                    │
 ┌───────▼───────┐   ┌────────▼────────┐   ┌──────▼───────┐
 │ Linux Agent   │   │ Docker Server   │   │ Outros Hosts │
 └───────────────┘   └─────────────────┘   └──────────────┘