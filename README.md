# Gölge-Dark Matter: Otonom Bellek Güvenlik Mimarisi (Advanced Memory Shielding)

Dark Matter, modern yazılım dünyasında "Bellek İçi Veri Güvenliği" (In-Memory Data Security) kavramını yeniden tanımlayan bir projedir. Statik veri depolama yöntemlerinin, bellek dökümü (memory dump) veya doğrudan bellek erişimi (DMA) saldırıları karşısında zayıf olduğu gerçeğinden hareketle; veriyi "yaşayan bir organizma" gibi hareket ettiren, kendi kendini onaran ve tehditleri imha eden otonom bir koruma katmanı sunar.

---

## 🔬 Teknik Felsefe
Geleneksel güvenlik, veriyi "kilitli bir sandıkta" tutmaya çalışır. Dark Matter ise veriyi "bir okyanusun içindeki kum tanesi" gibi gizler, okyanusu sürekli hareket ettirir ve okyanusa yabancı bir el girdiğinde okyanusu tamamen yeniden inşa eder.

## 🛡️ Temel Teknolojiler ve Katmanlı Savunma

### 1. Hardware-Rooted Entropy Engine
Yazılımsal `random` kütüphanelerinin deterministik (tahmin edilebilir) yapısını reddediyoruz.
* **Mekanizma:** CPU'nun termal gürültüsünü (RDRAND/RDSEED) kullanarak saf fiziksel rastgelelik üretir.
* **Etki:** Shannon Entropisi 8.0 seviyesine ulaşılarak, verinin istatistiksel analizle (Frequency Analysis) tespiti imkansız hale getirilir.

### 2. Otonom Moving Target Defense (MTD)
Statik bellek adresi, saldırganın en büyük dostudur. Biz buna "ölümcül adres" diyoruz.
* **Mekanizma:** `ShadowMigrator` iş parçacığı, her `n` saniyede bir verinin bellek içerisindeki ofsetlerini yeniden hesaplar ve veriyi güvenli bir bölgeye taşır.
* **Etki:** Saldırganın bellek dökümü (dump) alması halinde, verinin bulunduğu adres anında geçersiz olur.

### 3. Self-Healing Veri Bütünlüğü (ECC)
Bellek bozulmaları (bit-flip) veya kasıtlı bozma girişimleri karşısında sistemin ayakta kalması için.
* **Mekanizma:** Hata düzeltme kodları (ECC) ile veriye "artıklık" (redundancy) eklenir.
* **Etki:** RAM üzerindeki gürültüden kaynaklanan veri bozulmaları dahi, veri kaybı yaşanmaksızın orijinal haline geri döndürülür.

### 4. Paranoik Aktif Savunma (Canary Guard)
Sistemin içinde sürekli devriye gezen sanal "yemler" (canaries).
* **Mekanizma:** Bellek havuzunun kritik noktalarına, sadece sistemin bildiği "imza" değerler yerleştirilir.
* **Etki:** Her yazma veya okuma işleminde bu "kancalar" kontrol edilir. Yetkisiz bir süreç bu kancalardan birine dokunduğu anda sistem tetiklenir.

### 5. Self-Destruct Protocol (Sıfır İz)
En büyük güvenlik, "hiçbir kanıt bırakmamaktır".
* **Mekanizma:** İhlal tespiti anında, sistem bellek havuzunu işlemci düzeyinde üretilen rastgele gürültü ile yeniden üzerine yazar (Overwriting).
* **Etki:** Saldırgan sistemin varlığını bile kanıtlayamaz; bellek her zaman "rastgele bir gürültü havuzu" gibi görünür.

---

## 🏗️ Mimari Şema



*Yukarıdaki şema; donanımsal entropi beslemesi, aktif MTD devriyesi ve Canary kancalarının bellek havuzu üzerindeki katmanlı yapısını göstermektedir.*

---

## ⚙️ Güvenlik Metrikleri

| Yetenek | Geleneksel Yöntem | Dark Matter |
| :--- | :--- | :--- |
| **Statik Analiz** | Vulnerable | **Impossible** |
| **Memory Dump** | Vulnerable | **Noise Only** |
| **Bit-Flip / Corruption** | Data Loss | **Self-Healing** |
| **Saldırı Tepkisi** | Passive Log | **Self-Destruct** |

---
## Görseller
<img width="1250" height="697" alt="image" src="https://github.com/user-attachments/assets/a84da81b-3aa4-45a9-982c-fc5cab81f83b" />
<img width="1249" height="702" alt="image" src="https://github.com/user-attachments/assets/d0543f34-0181-4836-84b3-e18d9ac0c221" />
<img width="1248" height="705" alt="image" src="https://github.com/user-attachments/assets/bb6b4cab-e89b-4930-894d-238667d2de88" />
<img width="1249" height="702" alt="image" src="https://github.com/user-attachments/assets/d048e0a2-6093-4642-880b-8d34a6a8f378" />
<img width="1247" height="698" alt="image" src="https://github.com/user-attachments/assets/b4e1b00d-fdde-4d31-bf79-3a18c3ab769e" />
<img width="1247" height="703" alt="image" src="https://github.com/user-attachments/assets/8c184e5f-f47e-405b-96ec-000658b77340" />
<img width="1248" height="700" alt="image" src="https://github.com/user-attachments/assets/617dc051-c6ec-413a-b2b8-5b52d18670d5" />
<img width="1248" height="700" alt="image" src="https://github.com/user-attachments/assets/d01c1f87-1066-453e-b2b6-0e7fefa89ba2" />
<img width="1246" height="700" alt="image" src="https://github.com/user-attachments/assets/1241bc7f-76a4-45b9-bb64-13adddf88d3b" />
<img width="1250" height="700" alt="image" src="https://github.com/user-attachments/assets/0b3a73da-a31c-461f-bd76-dd9902603b67" />
<img width="1248" height="702" alt="image" src="https://github.com/user-attachments/assets/b0da5113-7aaf-43d1-becc-ea44cfb2395f" />
<img width="1248" height="704" alt="image" src="https://github.com/user-attachments/assets/dac41444-49cc-484d-b92f-2dad899f1ef4" />
<img width="1248" height="700" alt="image" src="https://github.com/user-attachments/assets/d9711476-20a7-4b6b-be0a-8f48ab9beaa4" />


## ⚠️ Kullanım Alanları
* **Kripto Cüzdanları:** Anahtarların RAM'de güvenli tutulması.
* **Fintech Uygulamaları:** Hassas işlem verilerinin bellek sızıntılarına (memory leakage) karşı korunması.
* **Yüksek Güvenlikli Backend Sistemleri:** Bellek içi şifreleme ve otonom saldırı savunması.

---

*Bu proje, bellek yönetimi ve otonom güvenlik sistemleri (Autonomous Defense Systems) üzerine kapsamlı bir araştırma ve geliştirme çalışmasıdır.*
