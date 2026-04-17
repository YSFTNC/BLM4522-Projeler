# 🛡️ Proje 3: Veritabanı Güvenliği ve Erişim Kontrolü

Bu proje, kurumsal ölçekli sistemlerde verinin mahremiyetini ve bütünlüğünü korumak amacıyla dört katmanlı bir güvenlik mimarisi üzerine inşa edilmiştir.

### 1. Erişim Yönetimi ve Rol Tabanlı Güvenlik (RBAC)
"En Az Yetki Prensibi" (Least Privilege) uyarınca, sisteme farklı yetki seviyelerinde kullanıcılar tanımlanmıştır. `StajyerLogin` hesabı sadece veri okuma (`db_datareader`) yetkisine sahipken, `YoneticiLogin` hesabı tam kontrol (`db_owner`) yetkisiyle donatılmıştır.

### 2. SQL Injection Zafiyet Analizi ve Parametreli Sorgular
Dışarıdan gelebilecek kötü niyetli 'Dinamik SQL' saldırıları simüle edilmiştir. Bu saldırıları bertaraf etmek için `sp_executesql` mimarisi kullanılarak veriler komutlardan izole edilmiş ve parametreli sorgu disiplini getirilmiştir.

### 3. Transparent Data Encryption (TDE) - Bekleyen Veri Şifreleme
Fiziksel disk hırsızlığına veya yedek dosyalarının çalınmasına karşı "Transparent Data Encryption" uygulanmıştır. AES-256 algoritmalı sertifikalar ile veritabanı fiziksel katmanda şifrelenmiş (Encrypted) ve anahtarsız erişime kapatılmıştır.

### 4. Güvenlik Denetimi ve İzlenebilirlik (Audit Logging)
Sistem üzerindeki tüm kritik hareketler (Hassas tablo okumaları ve güncellemeler) kayıt altına alınmaya başlanmıştır. Oluşturulan `GuvenlikIzleme_Audit` mekanizması ile sistemdeki şüpheli hareketler saniye saniye loglanarak raporlanabilir hale getirilmiştir.

---
**📍 Proje Kanıtları ve Ekran Görüntüleri bu dizinde mevcuttur.**

Video link : https://youtu.be/9ZgCpNl1xbQ
