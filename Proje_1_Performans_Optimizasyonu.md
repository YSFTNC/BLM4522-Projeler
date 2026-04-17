# 1. Veritabanı Performans Optimizasyonu ve İzleme
**Proje Sunum Videosu (Tıkla İzle):** [https://youtu.be/Hy0qcHGLNwg]


# 1. Veritabanı Performans Optimizasyonu ve İzleme
*Bu belgede AdventureWorks veritabanı üzerinde yapılan sorgu optimizasyonu ve indeksleme süreçleri anlatılmaktadır.*

## 1.1. Sorunlu Sorgunun Tespiti ve İzlenmesi
AdventureWorks2022 veritabanındaki `Sales.SalesOrderDetail` tablosunda `CarrierTrackingNumber` sütunu üzerinde arama yapan yavaş bir sorgu tespit edilmiştir.
Sorgunun ilk çalışma planı (Execution Plan) incelendiğinde, sistemin hedef veriyi bulmak için **Clustered Index Scan** (tüm tabloyu okuma) yaptığı ve en yüksek maliyetin (%58) burada oluştuğu gözlemlenmiştir.

## 1.2. İndeksleme ve Optimizasyon (Çözüm)
Performansı artırmak için ilgili sütuna ve sorguda çağrılan diğer sütunları da kapsayan (INCLUDE) bir **Non-Clustered Index** oluşturulmuştur:

`CREATE NONCLUSTERED INDEX IX_SalesOrderDetail_CarrierTracking ON Sales.SalesOrderDetail (CarrierTrackingNumber) INCLUDE (ProductID, LineTotal);`

İndeks sonrası sorgu tekrar çalıştırıldığında, maliyetli tablo taraması ortadan kalkmış, yerine **Index Scan (NonClustered)** ve **Clustered Index Seek** işlemleri gelmiştir. Birleştirme operasyonu ise daha verimli olan **Nested Loops**'a dönüşmüştür.

## 1.3. Sistem İzleme ve DMV Kullanımı
Veritabanı üzerindeki yükü analiz etmek için Dynamic Management Views (DMV) kullanılmıştır. `sys.dm_exec_query_stats` görünümü üzerinden en çok işlemci (CPU) tüketen sorgular tespit edilmiştir. Ayrıca, `sys.dm_db_index_usage_stats` görünümü ile sistemde var olan ancak hiç okunmayan (seeks/scans=0) gereksiz indeksler listelenmiş ve temizlik için raporlanmıştır.

## 1.4. SQL Profiler ile Anlık İzleme
Optimizasyon işlemleri sırasında SQL Server Profiler aracı kullanılarak, veritabanı üzerinde koşan anlık sorguların ve arka plan işlemlerinin takibi yapılmış, sistem performansı canlı olarak izlenmiştir.
