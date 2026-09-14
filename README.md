# 🛡️ Corporate Network Segmentation & Inter-VLAN Security (Cisco CLI)

## 📌 Visão Geral do Projeto
Projeto prático desenvolvido no Cisco Packet Tracer simulando a segmentação de rede corporativa de Camada 2 e Camada 3, aplicando políticas de controle de acesso (ACL) para contenção de tráfego lateral.

---

## 🏗️ Arquitetura e Topologia

* **Router (`R1-Core`):** Cisco 2911 — Atuando como Gateway Inter-VLAN (Router-on-a-Stick) e Firewall de borda.
* **Switch (`SW-Access1`):** Cisco 2960 — Atuando no isolamento das portas Access e Trunking 802.1Q.
* **VLAN 10 (Financeiro):** Sub-rede `192.168.10.0/24` | Gateway `192.168.10.1`
* **VLAN 20 (TI):** Sub-rede `192.168.20.0/24` | Gateway `192.168.20.1`

---

## 🛠️ Configurações Aplicadas (CLI)

### 1. SW-Access1 (VLANs & Trunk 802.1Q)
~~~text
vlan 10
 name financeiro
vlan 20
 name ti

interface range Fa0/1 - 2
 switchport mode access
 switchport access vlan 10

interface range Fa0/3 - 4
 switchport mode access
 switchport access vlan 20

interface Fa0/5
 switchport mode trunk
~~~

### 2. R1-Core (Subinterfaces & ACL de Segurança)
~~~text
interface Gig0/0
 no shutdown

interface Gig0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
 ip access-group 100 in

interface Gig0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0

! Regra de Firewall: Bloqueia Financeiro -> TI
access-list 100 deny ip 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255
access-list 100 permit ip any any
~~~

---

## 📥 Como Executar o Projeto

1. Baixe o repositório ou faça o clone:
   git clone [https://github.com/LukasMatias-soc/cisco-intervlan-routing-security.git](https://github.com/LukasMatias-soc/cisco-intervlan-routing-security.git)

2. Abra o arquivo **cisco-intervlan-routing-security.pkt** no software **Cisco Packet Tracer**.
3. Realize os testes de conectividade e validação das ACLs via prompt de comando dos hosts.
