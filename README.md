# Projeto de Rastreamento de Captura de Frutos do Mar

Este projeto utiliza um ESP32, um módulo RTC DS3231 e sensores para registrar e monitorar a captura de frutos do mar. Ele visa melhorar a transparência, segurança alimentar e sustentabilidade na cadeia de fornecimento de frutos do mar, fornecendo informações precisas e imutáveis sobre cada evento de captura.

### Sumário

- [Visão Geral](#visão-geral)
- [Objetivo](#objetivo)
- [Componentes Necessários](#componentes-necessários)
- [Diagrama de Montagem](#montagem-no-simulador-wokwi)
- [Código](#código)
- [Instruções de Uso](#instruções-de-uso)
- [Requisitos e Dependências](#requisitos-e-dependências)
- [Instalação das Bibliotecas](#instalação-das-bibliotecas)
- [Contribuição](#contribuição)
- [Licença](#licença)

---

## Visão Geral

Este projeto visa criar uma solução de rastreamento que registre a data, hora e localização simulada de captura de frutos do mar. Utilizando o ESP32 para a coleta e registro dos dados, o projeto garante a transparência e integridade das informações.

## Objetivo

- **Transparência Total:** Rastrear a jornada dos frutos do mar desde a captura até o consumidor final.
- **Segurança e Qualidade:** Monitorar as condições de transporte e armazenamento para garantir produtos frescos e seguros.
- **Sustentabilidade:** Promover práticas de pesca sustentável e reduzir a pesca ilegal.

## Componentes Necessários

- 1 x ESP32
- 1 x Módulo RTC DS3231
- 1 x Botão de Push
- Jumpers e Protoboard
- Resistores (se necessário para o botão)

## Montagem no Simulador Wokwi

<img src="wokwi-img">

- **Botão de Push:** Conecte um pino ao pino digital GPIO23 do ESP32 e o outro ao GND.

## Código

```cpp
#include <Wire.h>
#include <Adafruit_Sensor.h>
#include <RTClib.h>

#define BUTTON_PIN 23

RTC_DS3231 rtc;
bool buttonState = false;

void setup() {
  Serial.begin(115200);

  // Setup RTC
  if (!rtc.begin()) {
    Serial.println("Couldn't find RTC");
    while (1);
  }
  if (rtc.lostPower()) {
    Serial.println("RTC lost power, setting the time!");
    rtc.adjust(DateTime(F(__DATE__), F(__TIME__)));
  }

  pinMode(BUTTON_PIN, INPUT_PULLUP);
}

void loop() {
  buttonState = digitalRead(BUTTON_PIN) == LOW;

  if (buttonState) {
    DateTime now = rtc.now();

    Serial.print("Fish Captured! ");
    Serial.print("Date: ");
    Serial.print(now.year(), DEC);
    Serial.print('/');
    Serial.print(now.month(), DEC);
    Serial.print('/');
    Serial.print(now.day(), DEC);
    Serial.print(" Time: ");
    Serial.print(now.hour(), DEC);
    Serial.print(':');
    Serial.print(now.minute(), DEC);
    Serial.print(':');
    Serial.print(now.second(), DEC);

    Serial.print(" Location: ");
    float randomLat = random(-9000, 9000) / 100.0;
    float randomLon = random(-18000, 18000) / 100.0;
    Serial.print(randomLat, 4);
    Serial.print(", ");
    Serial.println(randomLon, 4);

    delay(1000);  // Debounce delay
  }
}
```

## Instruções de Uso

1. **Montagem:**
   - Conecte todos os componentes conforme o diagrama de montagem acima.
   - Certifique-se de que as conexões do RTC e do botão estão corretas.

2. **Carregar o Código:**
   - Abra o Arduino IDE.
   - Copie e cole o código fornecido.
   - Selecione a placa ESP32 correta e a porta COM.
   - Carregue o código no ESP32.

3. **Execução:**
   - Abra o Monitor Serial (baud rate 115200).
   - Pressione o botão para registrar um evento de captura.
   - Visualize os dados de captura no monitor serial.

## Requisitos e Dependências

- Arduino IDE instalado.
- Bibliotecas `Wire.h` e `RTClib.h` instaladas.

## Instalação das Bibliotecas

1. **Wire.h:** Esta biblioteca já está incluída no Arduino IDE.
2. **RTClib.h:**
   - Abra o Arduino IDE.
   - Vá para **Sketch** > **Include Library** > **Manage Libraries**.
   - Pesquise por "RTClib" e instale a biblioteca de Adafruit.

## Contribuição

Sinta-se à vontade para contribuir com melhorias para este projeto. Faça um fork do repositório, crie uma branch para suas alterações e envie um pull request.

## Licença

Este projeto está licenciado sob a [MIT License](LICENSE).

---

## :smile: Integrantes

- [Felipe Schneider - RM552643](https://github.com/felpschneider)
- [Thiago Araujo Vieira - RM553477](https://github.com/ThiagoAraujot)
- [Vinicius Centurion - RM554063](https://github.com/vinicenturion)

###### Clique no nome para visitar o GitHub

---

Obrigado por usar o BlueTrack! Se tiver alguma dúvida ou sugestão, entre em contato conosco.
