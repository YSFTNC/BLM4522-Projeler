# 1. Veritabanı Performans Optimizasyonu ve İzleme
*Bu belgede AdventureWorks veritabanı üzerinde yapılan sorgu optimizasyonu ve indeksleme süreçleri anlatılmaktadır.*

## 1.1. Sorunlu Sorgunun Tespiti ve İzlenmesi
AdventureWorks2022 veritabanındaki `Sales.SalesOrderDetail` tablosunda `CarrierTrackingNumber` sütunu üzerinde arama yapan yavaş bir sorgu tespit edilmiştir.
Sorgunun ilk çalışma planı (Execution Plan) incelendiğinde, sistemin hedef veriyi bulmak için **Clustered Index Scan** (tüm tabloyu okuma) yaptığı ve en yüksek maliyetin (%58) burada oluştuğu gözlemlenmiştir.

## 1.2. İndeksleme ve Optimizasyon (Çözüm)
Performansı artırmak için ilgili sütuna ve sorguda çağrılan diğer sütunları da kapsayan (INCLUDE) bir **Non-Clustered Index** oluşturulmuştur:

`CREATE NONCLUSTERED INDEX IX_SalesOrderDetail_CarrierTracking ON Sales.SalesOrderDetail (CarrierTrackingNumber) INCLUDE (ProductID, LineTotal);`

İndeks sonrası sorgu tekrar çalıştırıldığında, maliyetli tablo taraması ortadan kalkmış, yerine **Index Scan (NonClustered)** ve **Clustered Index Seek** işlemleri gelmiştir. Birleştirme operasyonu ise daha verimli olan **Nested Loops**'a dönüşmüştür.
