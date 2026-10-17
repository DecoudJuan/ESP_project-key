# CLAUDE.md

Trabajo práctico de Técnicas Digitales (Universidad Austral): maestro ESP8266 y esclavo ESP32 que se comunican por
ESP-NOW. Los commits originales son de Santiago-Garrote.

## Stack
- C++ de Arduino (`.ino`), sin dependencias externas más allá de los cores de ESP32 y ESP8266.
- Protocolo ESP-NOW.

## Estructura
- `Master_ESP8266_modificado.ino`: maestro (LOLIN D1 mini clone o ESP-01). Envía el mensaje una vez en `setup()`.
- `ESP32_Esclavo.ino`: esclavo (ESP32). Valida la palabra recibida contra `myKeys` y enciende cuatro LEDs.
- `docs/arquitectura.md`: flujo, estructura del mensaje y pines.

## Comandos
No hay build ni tests por línea de comandos. Se compila y se carga desde el Arduino IDE (o `arduino-cli`) eligiendo la
placa que corresponda; monitor serie a 115200 baudios.

## Convenciones
- La estructura del mensaje (`palabra[6]` + `int numero`) tiene que ser idéntica en ambos sketches.
- Comentarios en español.
- Antes de cargar el maestro, revisar que `broadcastAddress` sea la MAC del ESP32 real.
- Las claves del esclavo (`myKeys`) y `LMK_KEY_STR` están escritas en el código: no sumar claves reales de otros sistemas.

## Reglas del repo
- Commits y PRs sin atribución a Claude: nada de `Co-Authored-By: Claude` ni "Generated with Claude Code".
- Sin emojis de color en documentos ni mensajes.
