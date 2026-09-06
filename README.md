# 🚀 Embedded Systems Journey: STM32F407 Discovery

Repositori ini berisi dokumentasi dan kode sumber praktikum **Sistem Tertanam (Embedded Systems)** menggunakan board mikrokontroler **STM32F4 Discovery (STM32F407VGT6)**. Pengembangan dilakukan menggunakan **STM32CubeMX** untuk konfigurasi periferal dan **STM32CubeIDE** untuk pemrograman C berbasis **HAL (Hardware Abstraction Layer)**.

---

## 🛠️ Hardware & Software Toolchain
- **Microcontroller Board:** STM32F4 Discovery (STM32F407VGT6 - ARM Cortex-M4 @ 168 MHz)
- **Configuration Tool:** STM32CubeMX v6.18.1
- **IDE & Compiler:** STM32CubeIDE (GCC Arm Embedded Toolchain)
- **Programmer/Debugger:** ST-LINK/V2 On-Board
- **Language:** C (STM32 HAL Driver)

---

## 📚 Daftar Proyek Praktikum

| Minggu | Judul Proyek | Periferal & Konsep Utama | Link Folder |
| :---: | :--- | :--- | :---: |
| **Week 01** | [Lab 1] Blinking LED | HSE Crystal Config, GPIO Output, Hardware Delay (`HAL_Delay`) | [Lihat Kode](./Week01_LED_Blink/) |
| **Week 02** | [Lab 2] Push Button Digital Input | GPIO Input (Active-High), Debouncing, Edge Detection, Toggle State | [Lihat Kode](./Week02_Button_Input/) |

---

## 📌 Rangkuman Ringkas Praktikum

### Week 01 - Blinking LED di Board STM32F407 Discovery
- **Konfigurasi System Clock:** Mengaktifkan *High Speed Clock* (HSE) menggunakan *Crystal/Ceramic Resonator*.
- **Pinout:** PD12 (LD4 - Green), PD13 (LD3 - Orange), PD14 (LD5 - Red), dan PD15 (LD6 - Blue) sebagai `GPIO_Output`.
- **Logika Program:** Mengubah kondisi logika pin secara bergantian dengan `HAL_GPIO_TogglePin()` dan jeda waktu 500 ms via `HAL_Delay(500)`.

### Week 02 - Input Push Button ke LED
- **Pinout:** PA0 (User Button) sebagai `GPIO_Input` (*Active-High*) dan PD12–PD15 sebagai `GPIO_Output`.
- **Momentary Switch (Stateless):** LED menyala hanya saat tombol ditahan menggunakan `HAL_GPIO_ReadPin()` dan `HAL_GPIO_WritePin()`.
- **Toggle State (Latching & Debouncing):** Mempertahankan status nyala/padam LED dengan satu kali tekan (*Rising Edge Detection*) yang dilengkapi *software debouncing* (`HAL_Delay(50)`) untuk meredam *bouncing* mekanis tombol.