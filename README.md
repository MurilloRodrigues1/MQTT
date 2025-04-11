# PROJETO MQTT
Nosso projeto consiste na implementação de um broker MQTT em uma Raspberry Pi, facilitando a comunicação entre dispositivos IoT
##  COMANDOS PARA DEFINIR AS FUNÇÕES 💼:
```
mosquitto_sub -h localhost -t "chat/grupo1" --> definiu onde "quer receber" a mensagem
```
```
mosquitto_pub -h localhost -t "chat/grupo1" -m "Salve grupo, Testando" --> Mandou a mensagem pelo broker local
```
*DETALHES DOS COMANDOS:*  
_*-h* localhost_ = Conecta ao broker local  
_*-t*_ = define o tópico  
_-m_ = mensagem a ser enviada
___
*VERIFICAR O _IP_ DA RASP:*
```
hostname -I
```
___
# Como instalar o broker Mosquitto na Raspberry Pi  
_Pré-requisitos_:
- Raspberry Pi ligada e com internet (Wi-Fi ou cabo)

- Cartão SD com Raspberry Pi OS já funcionando

- Terminal aberto (pode ser direto ou via SSH)

### 1. Atualize os pacotes:
```
sudo apt update
sudo apt upgrade
```
### 2. Instale o broker Mosquitto:
```
sudo apt install mosquitto
```
### 3. Instale os clientes para testes (pub/sub):
   -> _Isso instalará tanto o Mosquitto (broker MQTT) quanto os clientes MQTT, que são úteis para testar e interagir com o broker._
```
sudo apt install mosquitto-clients
```
### 4. Habilite o Mosquitto pra iniciar junto com o sistema _(opcional)_:
```
sudo systemctl enable mosquitto
```
### 5. Verifique se ele tá rodando:
```
sudo systemctl status mosquitto
```
_saída:  “active (running)” =_ ✅
___
# CONFIGURAÇÃO DE ACESSO  
### 1. Esse comando abre o arquivo de configuração principal do Mosquitto com permissão de administrador.  
```
sudo nano /etc/mosquitto/mosquitto.conf
```
### 2. Dentro do arquivo, você pode adicionar configurações como:
```
conf
listener 1883 --> identifica a porta para o MQTT  
allow_anonymous true --> Define que qualquer um tem acesso ao broker sem identificação  
sudo systemctl restart mosquitto --> Reinicia o comando para salvar as novas configurações  
```
Salve o arquivo *(Ctrl + O, Enter, Ctrl + X)*  
### 3. Reiniciar o Serviço
Após fazer alterações na configuração, reinicie o serviço do Mosquitto para aplicar as mudanças
```
sudo systemctl restart mosquitto
```
___
___

# _INICIANDO O PROJETO_ 

### *1. PREPARANDO A RASPBERRY*

_CARTÃO JÁ CONECTADO_ ✅

*configuração inicial:*

- Escolha o idioma e região

- Configure o Wi-Fi

- Atualize o sistema, se ele pedir

*COMANDOS DE VERIFICAÇÃO (ATUALIZAÇÃO)*
```
sudo apt update

sudo apt upgrade
```
*VERIFICAR A CONEXÃO COM A INTERNET*
```
ifconfig
```
_ou_
```
ip a
```
___

# DESENVOLVIMENTO DO GRUPO 💻
_Salvando e identificando nossos equipamentos, para não perdemos o desenvolvimento_
<p align="left">
  <img src="https://github.com/user-attachments/assets/82152c08-2ce1-4a35-9daa-8a4594821c3e" width="300"/>
  <img src="https://github.com/user-attachments/assets/e0ec4dba-d62a-43df-9179-d243bd4e76f7" width="300"/>
</p>













