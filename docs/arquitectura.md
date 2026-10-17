# Arquitectura del proyecto

Dos sketches de Arduino que se comunican por ESP-NOW, el protocolo de Espressif para enviar paquetes entre placas
ESP sin router ni conexión Wi-Fi a una red.

## Piezas
| Archivo | Placa | Rol |
|---|---|---|
| `Master_ESP8266_modificado.ino` | LOLIN D1 mini clone (también ESP-01) | Controlador: envía el mensaje |
| `ESP32_Esclavo.ino` | ESP32 | Esclavo: recibe el mensaje y maneja los LEDs |

## Mensaje
Ambos lados definen la misma estructura, que viaja tal cual por el aire:

```cpp
struct { char palabra[6]; int numero; }
```

`palabra` es la clave que se verifica y `numero` es un dato adicional que el esclavo sólo imprime.

## Flujo
1. El maestro inicia ESP-NOW (rol `CONTROLLER`), registra al esclavo como peer por su MAC (`broadcastAddress`) y envía
   el mensaje una vez dentro de `setup()` (`palabra = "39552"`, `numero = 5`). El `loop()` está vacío.
2. El callback `OnDataSent` informa por serie si el envío fue "Ok" o "Fallo" y cuenta los envíos.
3. El esclavo inicia ESP-NOW y registra `OnRecv` como callback de recepción.
4. En `OnRecv` copia los bytes a la estructura, imprime la palabra y el número, y busca la palabra en `myKeys`
   (`"18079"` y `"395523"`).
5. Si hay coincidencia, enciende rojo, verde, amarillo y azul de a uno, 250 ms cada uno. Si no, no hace nada.

## Cifrado
El sketch del maestro declara una clave `LMK_KEY_STR` pensada para el cifrado de ESP-NOW, pero el peer se registra sin
clave (`esp_now_add_peer(..., NULL, 0)`), así que los mensajes viajan sin cifrar.

## Pines del esclavo
| LED | Pin |
|---|---|
| Rojo | 32 |
| Verde | 33 |
| Amarillo | 18 |
| Azul | 21 |

## Ver también
- [README](../README.md)
- Documentación de ESP-NOW: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/network/esp_now.html
