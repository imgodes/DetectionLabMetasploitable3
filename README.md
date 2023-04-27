# Metasploitable3 e Detection Lab
Criado para adição do [Metasploitable3](https://github.com/rapid7/metasploitable3) (seus logs, e telemetrias) ao [DetectionLab](https://detectionlab.network/).

# Introdução
O metasploitable é uma máquina com serviços propositalmente vulneráveis. Existem várias versões dele, eu escolhi a terceira por ser a [mais nova](https://github.com/rapid7/metasploitable3/wiki#differences-between-metasploitable-3-and-the-older-versions).
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
# Explicação mais detalhada
Eu explico com mais detalhes no [meu blog](https://imgodes.github.io/posts/metasploitable3-detectionlab/)
