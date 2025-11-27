# Defender File Events Reporter / Microsoft Defender Dosya Olayları Raporlayıcı

## Overview / Genel Bakış

### English
This repository contains a PowerShell script that automatically collects and reports file activity (created, deleted, copied, modified) on devices monitored by Microsoft Defender for Endpoint. The main objectives are:

- Track file events for the last **7 days** on a daily basis  
- Generate separate **CSV files per device**  
- Bundle all CSVs into a single **ZIP archive**  
- Send the ZIP file via email to a designated recipient using **Microsoft Graph API**

This allows security teams and IT administrators to centrally monitor file activities, quickly identify anomalies, and automate reporting.

To run the script, simply set the **environment variables** (`Tenant ID`, `Client ID`, `Client Secret`, etc.) and execute the script with **PowerShell 7+**. The outputs will be stored in the `/Reports` folder and sent via email.

---

### Türkçe
Bu repository, Microsoft Defender for Endpoint ortamında cihazlarda gerçekleşen dosya işlemlerini (oluşturma, silme, kopyalama, değiştirme) otomatik olarak toplayan ve raporlayan bir PowerShell scripti içerir. Amaçlar:

- Son **7 güne** ait dosya olaylarını günlük bazda takip etmek  
- Her cihaz için ayrı **CSV dosyaları** oluşturmak  
- Tüm CSV’leri tek bir **ZIP dosyasında** toplamak  
- ZIP dosyasını **Microsoft Graph API** ile belirlenen alıcıya e-posta olarak göndermek

Bu sayede güvenlik ekipleri ve IT yöneticileri dosya aktivitelerini merkezi olarak takip edebilir, anomalileri hızlıca tespit edebilir ve raporlamayı otomatikleştirebilir.

Script’i çalıştırmak için sadece **environment variable’ları** (`Tenant ID`, `Client ID`, `Client Secret` vb.) ayarlayın ve **PowerShell 7+** ile script’i başlatın. Çıktılar `/Reports` klasöründe oluşur ve e-posta ile gönderilir.





# Defender File Events Reporter  
### Collect DeviceFileEvents for the last 7 days & send ZIP report via Microsoft Graph API  
### Son 7 güne ait DeviceFileEvents’i toplayın ve ZIP raporunu Microsoft Graph API ile gönderin

This PowerShell script queries **Microsoft Defender for Endpoint Advanced Hunting**, collects **DeviceFileEvents** for the last 7 days for selected devices, generates CSV reports, bundles them into a ZIP file, and sends the ZIP via **Microsoft Graph API** as an email attachment.  

Bu PowerShell scripti, **Microsoft Defender for Endpoint Advanced Hunting** API’sini kullanarak seçilen cihazlar için son 7 güne ait **DeviceFileEvents** verilerini toplar, CSV raporları oluşturur, tüm CSV’leri ZIP dosyasında birleştirir ve ZIP’i **Microsoft Graph API** üzerinden e-posta ile gönderir.

---

## Features / Özellikler
- Pulls file activity logs for the last **7 days** / Son 7 güne ait dosya aktivitelerini çeker  
- Queries Defender for Endpoint using **Advanced Hunting API** / Advanced Hunting API kullanarak Defender verilerini sorgular  
- Collects events: / Olay türleri:
  - FileCreated / Dosya Oluşturma  
  - FileDeleted / Dosya Silme  
  - FileModified / Dosya Değiştirme  
  - FileCopied / Dosya Kopyalama  
- Generates **per-device CSV** files / Cihaz bazlı CSV dosyaları oluşturur  
- Bundles all CSV files into a **ZIP** / Tüm CSV’leri ZIP dosyasında toplar  
- Sends email using **Microsoft Graph API** / Microsoft Graph API ile e-posta gönderir  
- Includes **retry mechanism** for API 429 responses / 429 throttling için retry mekanizması içerir  
- Fully automated process / Tamamen otomatik süreç  

---

## Requirements / Gereksinimler

### 1. Azure App Registration (API Permissions) / Azure App Kayıt (API İzinleri)
To use Defender & Graph APIs, you must create an Azure App Registration.  
Defender & Graph API’lerini kullanmak için bir Azure App Registration oluşturmanız gerekiyor.

#### **Steps / Adımlar:**
1. Go to: `Azure Portal → Azure Active Directory → App Registrations → New Application`  
   Git: `Azure Portal → Azure Active Directory → App Registrations → Yeni Uygulama`  
2. Give your app a name (example: `DefenderFileEventsReporter`)  
   Uygulamanıza bir isim verin (ör: `DefenderFileEventsReporter`)  
3. Select: **Accounts in this organizational directory only**  
   Seçenek: **Sadece bu organizasyon dizinindeki hesaplar**  
4. After creation: / Oluşturduktan sonra:
   - Copy: / Kopyalayın:
     - **Tenant ID**  
     - **Client ID**  
5. Go to: **Certificates & Secrets → New Client Secret** / **Sertifikalar & Secrets → Yeni Client Secret**  
   - Copy the generated secret immediately / Oluşturulan secret’ı hemen kopyalayın

#### API Permissions Required / Gerekli API İzinleri:

##### Microsoft Defender:
`https://api.security.microsoft.com/.default`
- **AdvancedHunting.Read.All**
- **SecurityEvents.Read.All** (optional / opsiyonel ama önerilir)

##### Microsoft Graph:
`https://graph.microsoft.com/.default`
- **Mail.Send**
- **User.Read**
- **Mail.ReadWrite** (optional / opsiyonel)

>  After adding permissions → Click **Grant Admin Consent**.  
> İzinleri ekledikten sonra → **Admin Consent Ver** tıklayın.

---

##  Folder Structure / Klasör Yapısı

Script will automatically create:  
Script otomatik olarak aşağıdaki klasörü oluşturur:

```
C:\Reports\
   FileEvents_<device>.csv
   FileEvents_Full.zip
```

You can customize the report path in:  
Rapor yolunu değiştirmek için:

```powershell
$ReportFolder = "C:\Reports"
```

---

## Configuration / Yapılandırma

Edit these fields before running / Scripti çalıştırmadan önce düzenleyin:

```powershell
$TenantId     = "YOUR_TENANT_ID"
$ClientId     = "YOUR_CLIENT_ID"
$ClientSecret = "YOUR_CLIENT_SECRET"

$MailFrom     = "sender@example.com"
$MailTo       = "receiver@example.com"

$Devices = @("DESKTOP-01", "LAPTOP-02")
```

---

## How to Run / Nasıl Çalıştırılır

Open PowerShell **as Administrator** / PowerShell’i **Yönetici olarak** açın:

```powershell
./DefenderFileEventsReporter.ps1
```

---

## Task Scheduler Automation / Görev Zamanlayıcı ile Otomatik Çalıştırma

You can automate the script to run daily.  
Scripti günlük otomatik çalıştırmak için Task Scheduler kullanabilirsiniz.

### Steps / Adımlar:
1. Open **Task Scheduler** / Görev Zamanlayıcı’yı açın  
2. **Create Task** / Yeni Görev oluşturun  
3. **Triggers** → *Daily* at 08:00 (or any time) / **Tetikleyiciler → Günlük → 08:00 veya istediğiniz saat**  
4. **Actions** / **Eylemler**:
   - Action: *Start a Program* / Eylem: Program Başlat
   - Program:  
     ```
     powershell.exe
     ```
   - Arguments / Argümanlar:
     ```
     -ExecutionPolicy Bypass -File "C:\Scripts\DefenderFileEventsReporter.ps1"
     ```
5. **Run whether user is logged on or not** / Kullanıcı oturumda olsun olmasın çalıştır  
6. **Run with highest privileges** / Yüksek ayrıcalıklarla çalıştır  
7. Save and enter admin password / Kaydedin ve yönetici şifresini girin  

---

## Security Notes / Güvenlik Notları

- Never upload Tenant ID, Client ID, Client Secret to GitHub.  
- Tenant ID, Client ID ve Client Secret’ı **GitHub’a yüklemeyin**  
- Use placeholder values (as in the script) / Scriptteki placeholder değerleri kullanın  
- Store real values in environment variables if needed / Gerçek değerleri environment variable veya KeyVault üzerinden saklayın  

---

## Email Output / E-posta Çıktısı

The script sends an email through:  
Script aşağıdaki endpoint üzerinden e-posta gönderir:

```
POST https://graph.microsoft.com/v1.0/users/<MailFrom>/sendMail
```

Attachment / Ek:
```
FileEvents_Full.zip
```

---

## Script Information / Script Bilgileri

- Author / Yazar: **Beril Töman**  
- Version / Sürüm: **1.0.0**  
- PowerShell: **7+ recommended / önerilir**  
- OS Support: Windows 10 / Windows 11 / Windows Server with Defender MDE onboarded  

---

## Contribution / Katkı

Pull requests and improvements are welcome / Pull request ve geliştirmeler kabul edilir.
