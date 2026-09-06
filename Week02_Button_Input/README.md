# Week 02 - Push Button Digital Input (STM32F407 Discovery)

## 📝 Deskripsi Proyek
Praktikum ini berfokus pada implementasi pembacaan masukan digital (*digital input*) dari tombol tekan (User Button) pada pin **PA0** (*Active-High*) dan menampilkan hasil pembacaannya ke empat LED internal (*User LEDs*) di port **PD12–PD15** pada board STM32F407 Discovery.

Praktikum ini mengeksplorasi dua logika pemrograman masukan:
1. **Momentary Switch (Stateless):** LED aktif hanya saat gaya mekanis/tekanan diberikan pada tombol.
2. **Toggle State (Latching) with Debouncing:** Mengubah dan mempertahankan status nyala/padam LED dengan satu kali penekanan tombol (*Rising Edge Detection*) yang dilengkapi *software debouncing*.

---

## 🔌 Pemetaan Pin (Pinout Hardware)

| Komponen Hardware | Pin STM32F407 | Mode GPIO | Konfigurasi Logika |
| :--- | :---: | :---: | :--- |
| **User Push Button** | `PA0` | `GPIO_Input` | Active-High (`GPIO_PIN_SET` saat ditekan) |
| **LED Green (LD4)** | `PD12` | `GPIO_Output` | Push-Pull[cite: 1, 2] |
| **LED Orange (LD3)** | `PD13` | `GPIO_Output` | Push-Pull[cite: 1, 2] |
| **LED Red (LD5)** | `PD14` | `GPIO_Output` | Push-Pull[cite: 1, 2] |
| **LED Blue (LD6)** | `PD15` | `GPIO_Output` | Push-Pull[cite: 1, 2] |

---

## 💻 Logika Program C (`main.c`)

### 1. Mode Momentary Switch (Stateless)
Pembacaan status dilakukan secara langsung melalui `HAL_GPIO_ReadPin()`[cite: 2]. Jika tombol ditekan (`GPIO_PIN_SET`), seluruh LED diaktifkan[cite: 2]. Saat tombol dilepas, sinyal kembali bernilai `RESET` sehingga LED langsung padam[cite: 2].

```c
/* Dalam perulangan infinite loop while (1) */
while (1)
{
    if (HAL_GPIO_ReadPin(GPIOA, GPIO_PIN_0) == GPIO_PIN_SET) {
        // Nyalakan seluruh LED saat tombol ditahan
        HAL_GPIO_WritePin(GPIOD, GPIO_PIN_12 | GPIO_PIN_13 | GPIO_PIN_14 | GPIO_PIN_15, GPIO_PIN_SET);
    } else {
        // Matikan seluruh LED saat tombol dilepas
        HAL_GPIO_WritePin(GPIOD, GPIO_PIN_12 | GPIO_PIN_13 | GPIO_PIN_14 | GPIO_PIN_15, GPIO_PIN_RESET);
    }
}
```

### 2. Mode Toggle State & Software Debouncing (Latching)
Mode ini menggunakan teknik Rising Edge Detection (mendeteksi transisi dari RESET ke SET)[cite: 2]. Instruksi HAL_Delay(50) ditambahkan sebagai software debouncing untuk meredam gangguan getaran mekanis (bouncing) pada kontak sakelar tombol[cite: 2].

```c
/* Deklarasi variabel global di luar main() */
uint8_t lastBtnState = 0;

/* Dalam perulangan infinite loop while (1) */
while (1)
{
    uint8_t currentBtnState = HAL_GPIO_ReadPin(GPIOA, GPIO_PIN_0);

    // Deteksi transisi penekanan tombol (Rising Edge)
    if (currentBtnState == GPIO_PIN_SET && lastBtnState == GPIO_PIN_RESET) {
        // Balikkan kondisi (Toggle) seluruh LED
        HAL_GPIO_TogglePin(GPIOD, GPIO_PIN_12 | GPIO_PIN_13 | GPIO_PIN_14 | GPIO_PIN_15);
        
        // Software Debouncing untuk meredam bouncing mekanis
        HAL_Delay(50);
    }
    
    // Simpan kondisi tombol saat ini untuk iterasi berikutnya
    lastBtnState = currentBtnState;
}
```