# PROJETO MQTT
Nosso projeto consiste na implementação de um broker MQTT em uma Raspberry Pi, facilitando a comunicação entre dispositivos IoT
##  COMANDOS PARA DEFINIR AS FUNÇÕES 💼:
```
mosquitto_sub -h 111.111.1.111 -t "chat/grupo1" --> definiu onde "quer receber" a mensagem
```
```
mosquitto_pub -h 111.111.1.111 -t "chat/grupo1" -m "Salve grupo, Testando" --> Mandou a mensagem pelo broker local
```
*DETALHES DOS COMANDOS:*  
_*-h* localhost_ = Conecta ao broker local  
_*-t*_ = define o tópico ("cordenadas")  
_-m_ = mensagem a ser enviada
___
*VERIFICAR O _IP_ DA RASP:*
```
hostname -I
```
___
# Como instalar o broker Mosquitto na Raspberry Pi🍓 
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
listener 1883 --> identifica a porta para o MQTT  
allow_anonymous true --> Define que qualquer um tem acesso ao broker sem identificação    
```
Salve o arquivo *(Ctrl + O, Enter, Ctrl + X)*  
### 3. Reiniciar o Serviço
Após fazer alterações na configuração, reinicie o serviço do Mosquitto para aplicar as mudanças
```
sudo systemctl restart mosquitto
```
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
 *11/04/2025*  
 _Salvando e identificando nossos equipamentos, para não perdemos o desenvolvimento_
<p align="left">
  <img src="https://github.com/user-attachments/assets/82152c08-2ce1-4a35-9daa-8a4594821c3e" width="300"/>
  <img src="https://github.com/user-attachments/assets/e0ec4dba-d62a-43df-9179-d243bd4e76f7" width="300"/>
</p>

   _No primeiro dia de de projeto (04/04) foi solicitado a configuração da RASPBERRY PI 4, hoje (11/04), precisamos trocar o cartão SD da RASP, e usar um pendrive, com o sistema nele, após isso ao iniciarmos a RASP ele ja iniciou normalmente, e assim conseguimos iniciar o projeto de fato.
   Inicializamos o projeto colocando os comandos citados neste mesmo repositório. Primeiro foi testato com o MQTT EXPLORER, a tentativa foi executada com exito, após isso conseguimos configurar e começamos a trocar mensagem com o grupo 3, testamos a função PUB e SUB, o procedimento foi rápido e sem problemas, Fizemos também o que não foi possivel na ultima aula, e foi tudo executado com sucesso.Também começamos a esquematizar o projeto ja com o IOT, com a ESP-32_

<p align="left">
  <img src="https://github.com/user-attachments/assets/3200bea3-52ec-4a04-81b3-8ca04d4e0699" width="300"/>
</p>

_A nossa ideia em parceria com o grupo 3 foi que nós apertamos um botão, e la no grupo 3 iria acender um led.
Ao decorrer do dia, nosso projeto estava encaminhado e dando certo, conseguimos passar o projeto para a parte "física" usamos os seguintes equipamentos:_

- ESP-32
- Jumpers
- Resistor
- Botão

Começamos a montagem, e sem demorar muito ja conseguimos deixar montado, de maneira funcional, nossa unica dificuldade foi na hora de executar o código, mas após algumas falhas, chegamos ao fim.  

_PROTÓTIPO_  
<p align="left">
  <img src="https://github.com/user-attachments/assets/003a0a44-85de-4c21-a7b0-38335aca559a" width="300"/>
</p>  

# _CÓDIGO_ 👨🏻‍💻
```
#include <WiFi.h>
#include <PubSubClient.h>

#define BOTAO 19

// Substitua com seus dados:
const char* ssid = "iot";
const char* password = "iotsenai502";
const char* mqtt_server = "192.168.0.173"; // Ex: "192.168.1.105"
const char* mqtt_topic = "chat/grupo1";

// WiFi e MQTT
WiFiClient espClient;
PubSubClient client(espClient);

// Variável para detectar mudança de estado
bool botaoPressionado = false;

void setup_wifi() {
  delay(10);
  Serial.println();
  Serial.print("Conectando a ");
  Serial.println(ssid);

  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("");
  Serial.println("WiFi conectado!");
  Serial.print("IP: ");
  Serial.println(WiFi.localIP());
}

void reconnect() {
  while (!client.connected()) {
    Serial.print("Tentando conectar ao MQTT...");
    if (client.connect("ESP32Botao")) {
      Serial.println("Conectado!");
    } else {
      Serial.print("Falha, rc=");
      Serial.print(client.state());
      Serial.println(" - tentando de novo em 5s");
      delay(5000);
    }
  }
}

void setup() {
  Serial.begin(115200);
  pinMode(BOTAO, INPUT_PULLUP); // botão entre GPIO13 e GND

  setup_wifi();
  client.setServer(mqtt_server, 1883);
}

void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();

  int estado = digitalRead(BOTAO);

  if (estado == LOW && !botaoPressionado) {
    Serial.println("Botão pressionado, enviando mensagem MQTT...");
    client.publish(mqtt_topic, "clicou");
    botaoPressionado = true;
  }

  if (estado == HIGH) {
    botaoPressionado = false;
  }
}
``` 

*25/04/2025*
_O dia começou e nós montamos os equipamentos, para terminar o projeto, hoje o desafio era fazer a integração com outro grupo e também uma "avaliação" indvidual, cada um agrega 20 pontos para a nota geral do projeto.
A nossa ideia para testar o projeto é conectando a resp, para ver se captava a mensagem, após isso, pegamos o IP da maquina do outro grupo (grupo3), e ai começamos a intergrar o projeto para o outro grupo, após conectar, nós começamos mandando de resp para resp, vendo que funcionava, começamos a tentar mandar e esp para esp.
   Nós pegamos o IP da nossa RASP que seria o broker, e ultilizamos nos dois grupos (grupo 3) usando apenas um tópico, setado como "chat/grupo1e3" e fizemos as integração.

# CÓDIGO OFICIAL PARA A INTEGRAÇÃO
```
#include <WiFi.h>
#include <PubSubClient.h>

#define LED 19
#define BOTAO 18

const char* ssid = "iot";
const char* password = "iotsenai502";
const char* mqtt_server = "192.168.0.173";
const char* mqtt_topic = "chat/grupo1e3";

WiFiClient espClient;
PubSubClient client(espClient);

bool botaoAntigo = HIGH;
bool ledLigado = false;

void setup_wifi() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("\nWi-Fi conectado!");
}

void callback(char* topic, byte* message, unsigned int length) {
  String msg;
  for (int i = 0; i < length; i++) {
    msg += (char)message[i];
  }
  Serial.print("Recebido: ");
  Serial.println(msg);

  if (msg == "a") {
    digitalWrite(LED, HIGH);
    Serial.println("LED ligado (recebido a)");
  } else if (msg == "b") {
    digitalWrite(LED, LOW);
    Serial.println("LED desligado (recebido b)");
  }
}

void reconnect() {
  while (!client.connected()) {
    if (client.connect("ESP32_Grupo1")) {
      client.subscribe(mqtt_topic);
    } else {
      delay(2000);
    }
  }
}

void setup() {
  pinMode(LED, OUTPUT);
  pinMode(BOTAO, INPUT_PULLUP);
  digitalWrite(LED, LOW);
  
  setup_wifi();
  client.setServer(mqtt_server, 1883);
  client.setCallback(callback);
}

void loop() {
  if (!client.connected()) reconnect();
  client.loop();

  bool estadoBotao = digitalRead(BOTAO);
  if (botaoAntigo == HIGH && estadoBotao == LOW) {
    ledLigado = !ledLigado;
    if (ledLigado) {
      client.publish(mqtt_topic, "1");
      Serial.println("Enviado: 1");
    } else {
      client.publish(mqtt_topic, "0");
      Serial.println("Enviado: 0");
    }
    delay(300);
  }
  botaoAntigo = estadoBotao;
}
```






*_EXECUÇÃO DO PROJETO (momentos)_*
<p align="left">
  <img src="https://github.com/user-attachments/assets/b14d9938-acf2-4c44-843e-b4ad5a1907ae" width="300"/>
</p> 

<p align="left">
  <img src="https://github.com/user-attachments/assets/a233f997-439d-439e-b5fd-4487b797aafd" width="300"/>
</p>  

<p align="left">
  <img src="https://github.com/user-attachments/assets/0b3ba3e4-2fa3-4c31-b15b-46ab20081264" width="300"/>
</p>  














