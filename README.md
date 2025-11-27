# DefenderFileEventsReporter
Automation script for collecting Microsoft Defender file events


# DefenderFileEventsReporter

This repository contains a PowerShell script that automatically collects and reports file activity (created, deleted, copied, modified) on devices monitored by Microsoft Defender for Endpoint. The main objectives are:

- Track file events for the last 7 days on a daily basis
- Generate separate CSV files per device
- Bundle all CSVs into a single ZIP archive
- Send the ZIP file via email to a designated recipient using Microsoft Graph API

This allows security teams and IT administrators to centrally monitor file activities, quickly identify anomalies, and automate reporting.



# DefenderFileEventsReporter

Bu repository, Microsoft Defender for Endpoint ortamında cihazlarda gerçekleşen dosya işlemlerini (oluşturma, silme, kopyalama, değiştirme) otomatik olarak toplayan ve raporlayan bir PowerShell scripti içerir. Amaç:

- Son 7 güne ait dosya olaylarını günlük bazda takip etmek
- Her cihaz için ayrı CSV dosyası oluşturmak
- Tüm CSV’leri tek bir ZIP dosyasında toplamak
- ZIP dosyasını Microsoft Graph API ile belirlenen alıcıya e-posta olarak göndermek

Bu sayede güvenlik ekipleri ve IT yöneticileri, dosya aktivitelerini merkezi olarak takip edebilir, anomalileri hızlıca tespit edebilir ve raporlamayı otomatikleştirebilir.
