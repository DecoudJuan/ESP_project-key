<!-- repo-check: sin-deploy -->
# ESP_project-key

Trabajo práctico de la materia "Técnicas Digitales" (primer año de Ingeniería Informática, Universidad Austral): un
maestro ESP8266 envía una clave por ESP-NOW a un esclavo ESP32, que enciende una secuencia de LEDs si la clave es válida.

Los commits del repositorio son de Santiago-Garrote (compañero de trabajo).

## Cómo funciona
- **Maestro** (`Master_ESP8266_modificado.ino`): placa LOLIN D1 mini clone (también funciona con un ESP-01). Al
  arrancar arma un mensaje con una palabra (`"39552"`) y un número (`5`) y lo envía por ESP-NOW a la MAC del esclavo.
  Registra un callback que imprime por el monitor serie si el envío fue exitoso.
- **Esclavo** (`ESP32_Esclavo.ino`): ESP32 en modo estación Wi-Fi con ESP-NOW. Al recibir un mensaje lo imprime por
  serie y compara la palabra con su lista de claves válidas (`myKeys`). Si coincide, enciende en secuencia, 250 ms cada
  uno, los LEDs rojo (pin 32), verde (pin 33), amarillo (pin 18) y azul (pin 21).
- Ambos programas comparten la misma estructura de mensaje (`char palabra[6]` y `int numero`), que tiene que ser
  idéntica de los dos lados.

## Stack
- C++ de Arduino (sketches `.ino`).
- Protocolo ESP-NOW de Espressif, con las librerías `esp_now.h` y `WiFi.h` (ESP32) y `espnow.h` y `ESP8266WiFi.h`
  (ESP8266).
- Hardware: una placa ESP8266 (LOLIN D1 mini clone o ESP-01), una placa ESP32 y cuatro LEDs.

## Cómo correrlo en local
1. Instalar el Arduino IDE y agregar el soporte de las placas ESP32 y ESP8266 desde el gestor de placas.
2. Cargar `ESP32_Esclavo.ino` en el ESP32 y abrir el monitor serie a 115200 baudios.
3. Cambiar en `Master_ESP8266_modificado.ino` el arreglo `broadcastAddress` por la dirección MAC del ESP32 y cargarlo
   en el ESP8266.
4. Cablear los LEDs del esclavo a los pines 32, 33, 18 y 21 (con su resistencia) y alimentar ambas placas. El maestro
   envía el mensaje una vez al arrancar; para repetirlo, reiniciar la placa.

## Deploy
Este proyecto no tiene deploy: es un trabajo práctico de electrónica digital que corre en placas ESP físicas, sin
servidor ni sitio publicado. Se despliega cargando los sketches en las placas desde el Arduino IDE.

## Documentos
- [Arquitectura del proyecto](docs/arquitectura.md): protocolo, estructura del mensaje y flujo entre maestro y esclavo.
- [CLAUDE.md](CLAUDE.md): guía para trabajar el repo con Claude Code.

## Licencia
Distribuido bajo licencia MIT, ver [LICENSE](LICENSE).
