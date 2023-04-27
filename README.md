# Metasploitable3 e Detection Lab
Criado para adição do [Metasploitable3](https://github.com/rapid7/metasploitable3) (seus logs, e telemetrias) ao [DetectionLab](https://detectionlab.network/).

# Introdução
O metasploitable é uma máquina com serviços propositalvemente vulneráveis. Existem várias versões dele, eu escolhi a terceira por ser a [mais nova](https://github.com/rapid7/metasploitable3/wiki#differences-between-metasploitable-3-and-the-older-versions).
O DetectionLab é um repositório com várias scripts que automatizam a criação de um ambiente todo configurado com ferramentas de logging e detecção.

# TLTR
**Com o detectionlab já INSTALADO e funcionando use os comandos abaixo:**
```
git clone https://github.com/imgodes/DetectionLabMetasploitable3.git
cd DetectionLabMetasploitable3
vagrant up
```
Configurações de rede do metasploitable:

```
IP estático: 192.168.56.210
IP público: DHCP (vai pegar um IP da sua rede).
```

# Combo
Combinando esses propósitos criei um ambiente de ataque x detecção. 
Os logs de aplicação são enviados ao splunk e as telemetrias da máquina são coletadas pelo OSQuery.

# Metasploitable3 Logging (spoiler)
Estou tentando configurar o envio dos logs de todas as aplicações vulneráveis, para que a exploração de cada vulnerabilidade possa ser acompanhada nos logs. Portanto segue a lista de quais já consigo:

    GlassFish
    Apache Struts ✅
    Tomcat
    Jenkins
    IIS FTP
    IIS HTTP
    psexec
    SSH
    WinRM
    chinese caidao
    ManageEngine
    ElasticSearch
    Apache Axis2
    WebDAV
    SNMP
    MySQL
    JMX
    Wordpress
    SMB
    Remote Desktop
    PHPMyAdmin

Testado em:
- Linux Mint 21 (Ubuntu), 26/04/2023
- PopOS 22.04 (Ubuntu), 27/04/2023
