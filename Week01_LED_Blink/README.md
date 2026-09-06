# Week 01 - Blinking LED

## 📝 Deskripsi Proyek
Praktikum dasar untuk memahami konfigurasi pin GPIO sebagai output digital pada board STM32F407 Discovery dan mengontrol kedip empat LED secara serentak/bersamaan menggunakan operator Bitwise OR (`|`) dan `HAL_Delay()`.

## 🔌 Pemetaan Pin (Pinout)
| Komponen | Pin STM32 | Warna LED | Mode GPIO |
| :--- | :---: | :---: | :---: |
| **LD4** | `PD12` | Hijau | GPIO_Output |
| **LD3** | `PD13` | Oranye | GPIO_Output |
| **LD5** | `PD14` | Merah | GPIO_Output |
| **LD6** | `PD15` | Biru | GPIO_Output |

## 💡 Logika Program Utama (`main.c`)

```c
while (1)
{
    /* Toggle 4 LED secara bersamaan setiap 500 ms */
    HAL_GPIO_TogglePin(GPIOD, GPIO_PIN_15 | GPIO_PIN_14 | GPIO_PIN_13 | GPIO_PIN_12);
    HAL_Delay(500);
}
```