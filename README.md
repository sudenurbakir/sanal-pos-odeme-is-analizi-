# Sanal POS ve Kartlı Ödeme Süreci İş Analizi

> **Not:** Bu dokümantasyon seti, ödeme sistemleri ve iş analizi süreçlerini öğrenmek ve kavramları uygulamalı olarak pekiştirmek amacıyla bireysel eğitim/çalışma kapsamında hazırlanmıştır.

Bu çalışma, e-ticaret sanal POS altyapısında kartlı ödeme tahsilatı, üye iş yeri tanımlama, kart verisi güvenliği, hata/zaman aşımı yönetimi ve kullanıcı kabul testi (UAT) süreçlerini uçtan uca ele alan bir iş analizi dokümantasyon setidir.

## Kapsanan Yetkinlikler
- Üye iş yeri kabulü ve **MID / TID** iş kurallarının tanımlanması
- Teknik tasarıma girmeden **PCI-DSS** kapsamındaki veri güvenliği kurallarının belirlenmesi
- **ISO 8583 / API** ödeme mesaj yaşam döngüsü ve hata senaryolarının (Timeout, Reversal, Duplicate Capture) analizi
- Başarılı (**Happy Path**) ve hatalı (**Unhappy Path**) akışlar için uçtan uca **Sequence Diyagramları**
- Sayısal komisyon/yuvarlama senaryoları, uç durum (**Edge Case**) kayıtları ve sınır değer odaklı **UAT** planı
- Gereksinim kalite incelemesi ve **Değişiklik Talepleri (Change Request - CR)** yönetimi

## Doküman Yapısı
1. [İş Kuralları, MID/TID ve PCI Kapsamı](docs/01-is-kurallari-ve-pci.md)
2. [Ödeme Mesaj Yaşam Döngüsü ve Sequence Diyagramları](docs/02-mesaj-dongusu-ve-diyagramlar.md)
3. [Sayısal Senaryolar, Uç Durumlar, UAT Planı ve Değişiklik Talepleri](docs/03-sayisal-senaryolar-uat-ve-cr.md)
