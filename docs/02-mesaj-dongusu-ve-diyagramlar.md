# 2. Ödeme Mesaj Yaşam Döngüsü ve Sequence Diyagramları

## 2.1. Mesaj Yaşam Döngüsü ve Hata Yönetimi Kuralları

| Kural Kodu | Süreç / Senaryo | Açıklama |
| :--- | :--- | :--- |
| **MSG-01** | Ödeme İsteği ve Yetkilendirme (Authorization) | Müşteri ödemeyi başlattığında sistem geçerli `MID` ve `TID` kontrolünü yapar, PCI kurallarına uygun şekilde kart verisini alır ve banka/ödeme ağına yetkilendirme isteği iletir. |
| **MSG-02** | Tahsilat (Capture) ve Sonuçlandırma | Yetkilendirme onaylandığında işlem tahsilat aşamasına alınır; benzersiz işlem referans numarası üretilir ve üye iş yeri ile müşteriye başarılı sonuç döner. |
| **ERR-01** | Zaman Aşımı (Timeout) ve Ters İşlem (Reversal) | Banka yanıtı belirlenen süre içinde dönmezse işlem `"Timeout / Durum Belirsiz"` statüsüne alınır. Müşteriyi hemen ikinci denemeye yönlendirecek yanıltıcı bir red mesajı yerine durum kontrolü/iptal mesajı gösterilir ve güvenlik için ters işlem (**Reversal**) akışı tetiklenir. |
| **ERR-02** | Mükerrer Tahsilat (Duplicate Capture) Önleme | Aynı sipariş/işlem anahtarıyla kısa sürede gelen tekrar isteklerde, önceki işlem henüz sonuçlanmamış veya tamamlanmışsa ikinci bir tahsilat başlatılmaz; mevcut işlem kaydının durumu döner. |

## 2.2. Happy Path — Başarılı Ödeme Akışı

```mermaid
sequenceDiagram
    autonumber
    actor M as Müşteri
    participant U as Üye İş Yeri
    participant P as Sanal POS / Ödeme Altyapısı
    participant B as Banka

    M->>U: Ürünü seçer, sepete ekler ve ödeme adımına geçer
    M->>U: Kart bilgilerini girer ve ödemeyi onaylar
    U->>P: Ödeme isteği gönderir (MID, TID, tutar, işlem anahtarı)
    P->>B: Yetkilendirme / tahsilat mesajı iletir (CVV saklanmaz)
    B-->>P: Onay yanıtı döner
    P-->>U: Başarılı işlem sonucu ve referans numarası döner
    U-->>M: "Ödemeniz başarılı" mesajını gösterir
