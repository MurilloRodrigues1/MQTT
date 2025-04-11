# PROJETO MQTT
Nosso projeto consiste na implementação de um broker MQTT em uma Raspberry Pi, facilitando a comunicação entre dispositivos IoT

### _GUIA DO PASSO A PASSO DA INSTALAÇÃO_ 

### 1. Acesso à Raspberry Pi

Para começar, certifique-se de que sua Raspberry Pi está ligada e conectada à internet. Você pode acessá-la via SSH ou diretamente pelo terminal.
___

### 2. Atualização do Sistema

Antes de iniciar a instalação do Mosquitto, é importante atualizar os pacotes do sistema para garantir que você tenha os últimos updates e correções de segurança.
```
sudo apt update
sudo apt upgrade
```
___

### 3. Instalação do Mosquitto

Agora, vamos instalar o Mosquitto, o broker MQTT  
```
sudo apt install mosquitto mosquitto-clients
```
Isso instalará tanto o Mosquitto (broker MQTT) quanto os clientes MQTT, que são úteis para testar e interagir com o broker.
___
### 4. Verificação da Instalação

Após a instalação, é importante verificar se o serviço do Mosquitto está rodando corretamente
```
sudo systemctl status mosquitto
```
Se estiver tudo certo, você deverá ver uma mensagem indicando que o serviço está ativo e rodando.

### 5. Configuração de Acesso

Por padrão, o Mosquitto permite conexões apenas na mesma máquina (localhost). Se você deseja permitir conexões de outras máquinas, você precisará editar o arquivo de configuração
```
sudo nano /etc/mosquitto/mosquitto.conf
```
Dentro do arquivo, você pode adicionar configurações como:
```
conf
CopiarEditar
listener 1883
allow_anonymous true
```
Salve o arquivo *(Ctrl + O, Enter, Ctrl + X)*
___
### 6. Reiniciar o Serviço

Após fazer alterações na configuração, reinicie o serviço do Mosquitto para aplicar as mudanças
```
sudo systemctl restart mosquitto
```
___
### 7. Teste de Conexão

Para testar se o broker MQTT está funcionando corretamente, você pode usar o cliente MQTT de teste para publicar e subscrever a mensagens.

Abra dois terminais na Raspberry Pi:

- Terminal 1 (*Subscribe*):
  ```
  mosquitto_sub -h localhost -t "test/topic"
  ```
- Terminal 2 (*Publish*)
  ```
  mosquitto_pub -h localhost -t "test/topic" -m "Hello, MQTT!"
  ```
Você deverá ver a mensagem "Hello, MQTT!" sendo exibida no Terminal 1.
  ___
### 8. Configuração de Autostart (Opcional)

Caso deseje que o Mosquitto inicie automaticamente com a Raspberry Pi, você pode configurá-lo para isso
```
sudo systemctl enable mosquitto
```
### Possíveis Problemas e Soluções

- Porta 1883 Não Disponível**: Verifique se outra aplicação está usando a porta 1883 e mude a configuração do Mosquitto para uma porta alternativa.
- Permissões de Acesso**: Se tiver problemas de permissões ao acessar o broker de outras máquinas, verifique as configurações de firewall ou de rede da sua Raspberry Pi.

___
___

### _INICIANDO O PROJETO_

### *1. PREPARANDO A RASPBERRY*

_CARTÃO JÁ CONECTADO_

*configuração inicial:*

- Escolha o idioma e região

- Configure o Wi-Fi (se não estiver usando cabo)

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




*_INSTALAR O MQTT_*
```
sudo apt install mosquitto
```
recebe, gerencia e distribui as mensagens entre dispositivos.  

*_ATIVAR PUB E O SUB_*
```
sudo apt install mosquitto-clients
```
Instala as ferramentas de linha de comando










