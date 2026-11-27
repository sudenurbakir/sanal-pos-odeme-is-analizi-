## 2.3. Unhappy Path — Zaman Aşımı (Timeout) ve Reversal Akışı

    sequenceDiagram
    autonumber
    actor M as Müşteri
    participant U as Üye İş Yeri
    participant P as Sanal POS / Ödeme Altyapısı
    participant B as Banka

    M->>U: Ödemeyi onaylar
    U->>P: Ödeme isteği gönderir (işlem anahtarı ile)
    P->>B: Yetkilendirme isteği gönderir
    Note over P,B: Belirlenen sürede yanıt alınamaz (Timeout)
    P->>P: İşlemi "Timeout / Durum Belirsiz" statüsüne çeker
    P->>B: Güvenlik için ters işlem (Reversal) isteği gönderir
    B-->>P: İptal / durum yanıtı döner
    P-->>U: Zaman aşımı ve iptal/kontrol bilgisini iletir
    U-->>M: "İşlem yanıtı zaman aşımına uğradı, tekrar denemeden önce kontrol ediniz" mesajı gösterir

