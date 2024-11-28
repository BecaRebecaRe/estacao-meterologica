# Título: Desenvolvimento de uma Estação Meteorológica com Conexão Wi-Fi e MQTT

## 1. Introdução
O projeto visa desenvolver uma estação meteorológica capaz de medir a temperatura e umidade do ambiente, utilizando sensores adequados. Essa estação será conectada à internet por meio de uma conexão Wi-Fi e fará uso do protocolo MQTT para comunicação. O objetivo é criar um sistema que permita monitorar e registrar as condições climáticas locais de forma remota e automatizada.
## 1.1.	Subseções
## 1. Objetivos do Projeto
O código tem como objetivo implementar uma estação meteorológica IoT utilizando o microcontrolador ESP32/ESP8266 e o sensor DHT22. Ele mede a temperatura e a umidade do ambiente e publica os dados em tempo real em um corretor MQTT, possibilitando o monitoramento remoto das condições climáticas.
## 2. Tecnologias Utilizadas
•	Sensor DHT22: Utilizado
•	Wi-fi:ConexWokwi-GUEST.
•	Protocolo MQTT:Um prbroker.mqttdashboard.com.
•	MicroPython: Sim
•	Simulador Wokwi: Ferramenta
•	Corretor MQTT:  broker.mqttdashboard.com
•	LED no pino GPIO 2: O LED será controlado

## 3. Metodologia
1.	Configuração do Wi-Fi:
O dispositivo está configurado para se conectar automaticamente à rede Wokwi-GUESTsem necessidade de senha.
2.	Conexão ao Broker MQTT:
O código utilizawokwi-weathernenhum corretor públicobroker.mqttdashboard.com.
3.	Leitura de Dados do Sensor:
O sensor D
4.	Publicação de Dados:
Os dados coletados
5.	Visualização dos Dados:
O usuário podehttp : //www.hivemq.

## 4. Benefícios e Aplicações
•	Monitoramento em Tempo Real:
Os dados de temperatura e umidade são transmitidos em tempo real, permitindo que os usuários monitorem as condições ambientais externamente, com acesso a qualquer momento e de qualquer lugar.
•	Educação e Treinamento:
Este projeto oferece uma excelência
•	Aplicações Reais:
A estação meteorológica pode ser adaptada para mergulhadores
•	Eficiência Energética:
O sistema é projeto também em falta



## 2. Materiais e Métodos
### 2.1 Descrição do Projeto
A estação meteorológica consistirá em um microcontrolador (NodeMCU), sensores de temperatura e umidade (DHT22) e um módulo Wi-Fi para conexão à internet. O sistema enviará os dados coletados para um servidor MQTT, possibilitando o acesso às informações remotamente.

### 2.2 Módulo de Comunicação Escolhido
O módulo de comunicação escolhido é o NodeMCU, que possui capacidade de conexão Wi-Fi e é compatível com o protocolo MQTT. A integração com o servidor MQTT será feita por meio de bibliotecas específicas no ambiente de desenvolvimento Arduino.

### 2.3 Desenho e Esquema Eletrônico
O esquema eletrônico do projeto, mostrando a conexão entre o NodeMCU, os sensores e o módulo Wi-Fi, está representado abaixo:


![image](https://github.com/BecaRebecaRe/estacao-meterologica/assets/132020428/4285dfdb-c5ef-46ec-9a67-5aad41fff555)



### 2.4 Funcionamento do Protótipo!
O protótipo funcionará da seguinte forma: o NodeMCU realizará leituras dos sensores de temperatura e umidade periodicamente. Em seguida, os dados serão enviados para o servidor MQTT por meio da conexão Wi-Fi. O servidor MQTT será responsável por receber e armazenar os dados, possibilitando o acesso remoto aos mesmos.

## 3. Resultados
### 3.1 Montagem em Funcionamento
A montagem em funcionamento da estação meteorológica pode ser visualizada na imagem abaixo:
Peças de Montagem:

### Componentes Utilizados:

![Sensor Digital ](https://github.com/BecaRebecaRe/estacao-meterologica/assets/132020428/e4a08404-c9d0-4ff7-8d78-265d663af494)

![MICROCONTROLADOR POPULAR ](https://github.com/BecaRebecaRe/estacao-meterologica/assets/132020428/e8f6c1b2-a7b1-4ae1-82a8-be485175abaf)

### Estação Meterológica:

![Estação Meterologica](https://github.com/BecaRebecaRe/estacao-meterologica/assets/132020428/5d00562a-ec4f-4f1f-bba0-1b213e334837)

### 3.2 Vídeo-Demonstração
Um vídeo-demonstração do funcionamento da estação meteorológica está disponível no seguinte link: https://youtu.be/wSes7WXDoG0?si=9FJy1RFKbv7OFrQn

### 3.3 Medição de Tempo de Resposta
Os tempos médios de resposta entre o envio de comandos e a ação do atuador, bem como entre a detecção de um sensor e o recebimento dos dados no MQTT, foram medidos conforme a tabela abaixo:

### 3.4 Outros Gráficos e Imagens
Outros gráficos e imagens do funcionamento do protótipo e da comunicação com o broker MQTT podem ser visualizados a seguir:

![Tabela Resultados](https://github.com/BecaRebecaRe/estacao-meterologica/assets/132020428/3e0dcb4e-71e1-4e53-803c-33455c756440)

##Demostração do projeto sendo executado:
https://youtu.be/wSes7WXDoG0?si=9FJy1RFKbv7OFrQn


## Codigo Fonte:

import network
import time
from machine import Pin
import dht
import ujson
from umqtt.simple import MQTTClient

Definir parâmetros do servidor MQTT
MQTT_CLIENT_ID = "micropython-weather-demo"
MQTT_BROKER = "broker.mqttdashboard.com"
MQTT_USER = ""
MQTT_PASSWORD = ""
MQTT_TOPIC = "wokwi-weather"

 Definir sensor e pinos
sensor = dht.DHT22(Pin(15))
led = Pin(2, Pin.OUT)  # Definir LED no pino 2

Conectar-se ao WiFi
print("Connecting to WiFi", end="")
sta_if = network.WLAN(network.STA_IF)
sta_if.active(True)
sta_if.connect('Wokwi-GUEST', '')
while not sta_if.isconnected():
    print(".", end="")
    time.sleep(0.1)
print(" Connected!")

Conectar-se ao servidor MQTT
print("Connecting to MQTT server... ", end="")
client = MQTTClient(MQTT_CLIENT_ID, MQTT_BROKER, user=MQTT_USER, password=MQTT_PASSWORD)
client.connect()
print("Connected!")

prev_weather = ""
while True:
    print("Measuring weather conditions... ", end="")
    sensor.measure()  # Medir a temperatura e a umidade
    message = ujson.dumps({
        "temp": sensor.temperature(),
        "humidity": sensor.humidity(),
    })

    Publicar a mensagem se houver mudança
    if message != prev_weather:
        print("Updated!")
        print("Reporting to MQTT topic {}: {}".format(MQTT_TOPIC, message))
        client.publish(MQTT_TOPIC, message)
        prev_weather = message
        
       Lógica para o LED
        if sensor.temperature() > 30:  # Se a temperatura for maior que 30°C
            led.on()  # Acende o LED
        else:
            led.off()  # Desliga o LED
    else:
        print("No change")
    
    time.sleep(1)  # Atraso para a próxima medição




## 5. Conclusões
Os objetivos propostos foram alcançados com sucesso. A estação meteorológica desenvolvida é capaz de medir temperatura e umidade do ambiente, transmitindo os dados para um servidor MQTT de forma eficiente. Entre os principais problemas enfrentados, destaca-se a calibração dos sensores, que exigiu ajustes para garantir precisão nas leituras. As vantagens do projeto incluem a facilidade de acesso remoto aos dados e a sua baixa complexidade de implementação. Como melhorias futuras, sugerimos a adição de mais sensores para monitorar outros parâmetros climáticos, como pressão atmosférica e luminosidade.

## 6. Referências
- Karunakaran, M., & Madhanmohan, M. (2019). Design and Fabrication of Smart Home with Internet of Things Enabled Automation System.
- Selvi, V., Ravi, G., & Bhuvaneswari, G. (2019). IoT-BBMS: Internet of Things-Based Baby Monitoring System for Smart Cradle.
- Gupta, P., & Kaushik, A. (2019). An IoT based Active Building Surveillance System using Raspberry Pi and NodeMCU.
- Ahn, J., & Oh, S. (2019). Smart CEI Moncloa: An IoT-based Platform for People Flow and Environmental Monitoring on a Smart University Campus.
- Srinivasan, R., & Vasanthapriya, S. (2019). Taking MQTT and NodeMCU to IoT: Communication in Internet of Things.
