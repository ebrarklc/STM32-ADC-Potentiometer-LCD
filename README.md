# STM32 ADC ile Voltaj Ölçümü ve LCD HMI

Bu proje, STM32F407 mikrodenetleyicisinin **ADC (Analog-to-Digital Converter)** birimini kullanarak analog sensör verilerinin (Potansiyometre) okunması, dijital veriye dönüştürülmesi ve 16x02 Karakter LCD ekran üzerinde kullanıcıya gösterilmesi amacıyla geliştirilmiştir[cite: 6].

Sürekli (analog) dünyadaki fiziksel büyüklüklerin (voltaj), mikrodenetleyicinin işleyebileceği dijital verilere nasıl dönüştürüldüğünü ve anlamlı bir insan-makine arayüzüne (HMI) nasıl aktarıldığını pratik bir şekilde göstermektedir[cite: 6].

## 🚀 Öne Çıkan Özellikler (Highlights)

*   **Yüksek Çözünürlüklü Analog Okuma:** STM32F407'nin sahip olduğu 12-bit çözünürlüklü ADC birimi kullanılarak, 0-3.3V arasındaki giriş gerilimi 4096 (0-4095) farklı seviyede, 0.8 mV hassasiyetle ölçülmüştür[cite: 6].
*   **Kararlı Ölçüm (Sampling Time):** ADC konfigürasyonunda `Sampling Time` parametresi 144 Cycles olarak ayarlanarak analog sinyaldeki elektriksel gürültülerin önüne geçilmiş ve okumanın daha stabil olması sağlanmıştır[cite: 6].
*   **Özel 4-Bit LCD Sürücüsü:** Standart HAL kütüphanelerinde bulunmayan 16x2 LCD ekran kontrolü için, pin seviyesinde (`lcd.h` ve `lcd.c`) özel bir donanım soyutlama katmanı yazılarak ekran 4-bit modunda sürülmüştür[cite: 6].
*   **Dinamik Arayüz (Switch Kontrolü):** Fiziksel bir Switch (`SW0` / `PE13`) kullanılarak ekranın çalışma modu dinamik olarak değiştirilmiş; kullanıcının isteğine göre 12-bitlik ham veri veya gerçek voltaj ($V = ADC \times 3.3 / 4095$) değeri gösterilmiştir[cite: 6].

## 🛠️ Donanım ve Pin Yapılandırması

Sistemdeki dış birimlerin STM32F407 üzerindeki pin atamaları ve yapılandırmaları aşağıdaki gibidir[cite: 6]:

| Bileşen | Pin Kodu | Kullanım Modu | Açıklama |
| :--- | :--- | :--- | :--- |
| **Potansiyometre** | `PC5` | ADC1_IN15 | 0-3.3V arası gerilim bölücü analog giriş[cite: 6]. |
| **Switch 0 (SW0)** | `PE13` | GPIO_Input | Voltaj hesaplaması veya ham veri görünümü seçimi[cite: 6]. |
| **LCD Kontrol (RS, EN)** | `PD2`, `PD3` | GPIO_Output | LCD komut/veri seçimi ve yetkilendirme sinyalleri[cite: 6]. |
| **LCD Veri (D4-D7)** | `PC4`, `PC13-15` | GPIO_Output | 4-bit paralel LCD Veri Hatları[cite: 6]. |

## 📂 Yazılım Mimarisi ve Matematiksel Model

*   **Matematiksel Model:** ADC'den okunan 12-bitlik (0-4095) ham değer, aşağıdaki formül kullanılarak anlamlı bir gerilim (Volt) değerine dönüştürülür[cite: 6].
    ```c
    voltaj_degeri = (float)adc_ham_deger * 3.3 / 4095.0;
    ```
*   **Ana Döngü (Main Loop):** İşlemci, ADC biriminden `HAL_ADC_PollForConversion` fonksiyonu ile veriyi okur, ardından `SW0` pininin durumuna göre hesaplama yaparak sonucu LCD ekrana yansıtır[cite: 6]. Görüntü titreşimlerini engellemek için döngü sonunda 100ms gecikme (`HAL_Delay`) uygulanmıştır[cite: 6].

## 💻 Nasıl Çalıştırılır?

1.  Projeyi `STM32CubeIDE` ile açın.
2.  Kodu derleyin (`Build`) ve ArmApp-18 üzerindeki STM32F407 kartına yükleyin[cite: 6].
3.  Sistem başlatıldığında LCD ekranda "SCU BIL MUH" ve deney bilgileri animasyonlu olarak görüntülenecektir[cite: 6].
4.  Geliştirme seti üzerindeki potansiyometreyi çevirerek 12-bitlik değer değişimini gözlemleyebilir, `SW0` anahtarını kullanarak gerçek voltaj değerini (V) ekranda görebilirsiniz[cite: 6].

<img width="1024" height="683" alt="image" src="https://github.com/user-attachments/assets/f95b8624-f61b-4d5f-b3be-5aaff3cec2e6" />
