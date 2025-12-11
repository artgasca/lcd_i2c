# 📘LCD I2C Driver

Controlador para pantallas LCD basadas en HD44780 usando un expansor I²C PCF8574 / PCF8574A, optimizado para CCS PIC C Compiler y configurable desde el main.

## 🚀 Características principales

- Compatible con LCD 16x2, 20x4 y variantes HD44780.

- Compatible con módulos I2C basados en:

- PCF8574 (dirección típica 0x4E)

- PCF8574A (dirección típica 0x7E)

Totalmente configurable desde el main.c, sin modificar la librería.

- API simple de usar:
lcd_init(), lcd_print(), lcd_print_xy(), lcd_clear(), lcd_backlight_on(), etc.

- Implementación estable para CCS, usando ROM char* para cadenas constantes.

- Mapeo estándar: D7–D4, EN, RW, RS controlados vía PCF8574.

## 📁 Archivos
```c
lcd_i2c.c       -> Driver completo del LCD vía I2C
```
## ⚙️ Configuración requerida en el main.c

Antes de incluir el driver, define los parámetros del LCD:
```c
#define LCD_I2C_ADDR   0x7E   // Dirección I2C en modo 8 bits (0x4E o 0x7E)
#define LCD_COLS       16     // columnas del LCD
#define LCD_ROWS       2      // filas del LCD

#include "lcd_i2c.c"
```
### Direcciones más comunes:
| Módulo   | Dirección 7-bit | Dirección 8-bit (CCS) |
| -------- | --------------- | --------------------- |
| PCF8574  | 0x27            | **0x4E**              |
| PCF8574A | 0x3F            | **0x7E**              |

## 🧪 Ejemplo mínimo de uso
```c
#include <18F67K40.h>
#use delay(clock=64000000)
#use i2c(Master, SDA=PIN_B0, SCL=PIN_B1, FAST=400000)

#define LCD_I2C_ADDR  0x7E   // Ajustar según módulo
#define LCD_COLS      16
#define LCD_ROWS      2

#include "lcd_i2c.c"

void main() {
    delay_ms(100);
    lcd_init();
    lcd_clear();

    lcd_print_xy(1, 1, "Galio Electronics");
    lcd_print_xy(1, 2, "LCD I2C OK");

    while(TRUE) {
        lcd_backlight_off();
        delay_ms(500);
        lcd_backlight_on();
        delay_ms(500);
    }
}
```

## 🧩 API disponible
| Función                                  | Descripción                                 |
| ---------------------------------------- | ------------------------------------------- |
| `lcd_init()`                             | Inicializa el LCD en modo 4 bits            |
| `lcd_clear()`                            | Limpia la pantalla                          |
| `lcd_home()`                             | Regresa el cursor al inicio                 |
| `lcd_gotoxy(col, row)`                   | Posiciona el cursor                         |
| `lcd_putc(char)`                         | Escribe un solo carácter                    |
| `lcd_print(ROM char *text)`              | Imprime una cadena                          |
| `lcd_print_xy(col, row, ROM char *text)` | Imprime texto en una posición               |
| `lcd_backlight_on()`                     | Enciende luz de fondo                       |
| `lcd_backlight_off()`                    | Apaga luz de fondo                          |
| `lcd_set_addr(addr)`                     | Cambia dirección I2C en tiempo de ejecución |

## 🛠️ Notas importantes

- Para cadenas constantes, CCS requiere el uso de ROM char *text.

- Si no aparece nada en el LCD:
  - Verifica la dirección I2C.

  - Revisa SDA/SCL.

  - Confirma que el módulo tiene backlight jumper instalado.

- Funciona con cualquier PIC con módulo I2C compatible de CCS.

## 📦 To-Do / Futuras mejoras

- Detección automática de dirección I2C.

- Scroll horizontal y vertical.

- Soporte para caracteres personalizados (CGRAM).

- API de alto nivel para menús.
