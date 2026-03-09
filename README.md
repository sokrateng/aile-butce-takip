# Aile Bütçesi Takip Uygulaması

Aile gelir ve giderlerini takip etmek için geliştirilmiş modern bir web uygulaması.

## 🌟 Özellikler

### Güvenlik
- 🔒 **Kullanıcı Girişi**: Email/Password ile güvenli giriş
- 🔐 **Veri Güvenliği**: Her kullanıcı sadece kendi verilerini görür
- 🛡️ **Row Level Security**: Supabase RLS ile korumalı veriler

### Bütçe Yönetimi
- 📊 **Dönem Bazlı Raporlama**: Aylık gelir/gider takibi ve karşılaştırma
- 👥 **Kullanıcı Yönetimi**: Aile üyelerini ekleyin, düzenleyin ve takip edin
- 📁 **Kategori Yönetimi**: Gelir ve gider kategorilerini özelleştirin
- ✏️ **İşlem Yönetimi**: İşlemleri ekleyin, düzenleyin, silin
- 🔮 **Tahmini / Kesinleşmiş Kalemler**: Henüz kesinleşmemiş gelir ve giderleri tahmini olarak işaretleyin, raporlarda ayrı görün

### Bütçe Türleri
- 📂 **Bütçe Türü Tanımlama**: "Araç Tamiri", "Ev Tadilatı" gibi özel bütçe türleri oluşturun (Ayarlar → Bütçe Türü Yönetimi)
- 🔄 **Drill-down Yapısı**: Genel Bütçe özetinden alt bütçe türlerine tıklayarak detaya inin
- 🎯 **Bütçe Türüne Özel Takip**: Her bütçe türünün gelir/gider/bakiye durumunu ayrı izleyin
- 🔐 **Erişim Kontrolü**: Her bütçe türü için hangi aile üyelerinin erişebileceğini ayarlayın
- 📋 **Ortak Kategoriler**: Tüm bütçe türleri aynı gelir/gider kategorilerini kullanır
- 🏠 **Genel Bütçe**: Tüm işlemler varsayılan olarak "Genel" bütçeye düşer, istenirse başka türe atanır

### Raporlama
- 📈 **Grafikler**: Pasta ve sütun grafikleri ile görsel raporlar
- 🔍 **Filtreleme**: Kullanıcı, dönem ve durum (kesin/tahmini) bazlı filtreleme
- 📋 **Sıralama**: Tablo kolonlarına göre sıralama
- 📊 **Aylık Karşılaştırma**: Son 4 ayın gelir/gider karşılaştırması (kesin ve tahmini ayrımıyla stacked bar grafik)
- 📉 **Bütçe Türleri Karşılaştırması**: Tüm bütçe türlerinin gelir/gider durumunu tek grafikte karşılaştırın

### Teknik
- 💾 **Bulut Veritabanı**: Supabase PostgreSQL ile veri saklama
- 🌐 **Cihazlar Arası Senkronizasyon**: Verilerinize her yerden erişin
- 🚀 **Canlı Deployment**: Vercel ile otomatik deployment

## 🚀 Kurulum

### Gereksinimler
- Node.js 16+
- Supabase hesabı
- Vercel hesabı (deployment için)

### Yerel Kurulum

```bash
# Bağımlılıkları yükleyin
npm install

# .env dosyası oluşturun
VITE_SUPABASE_URL=your_supabase_url
VITE_SUPABASE_ANON_KEY=your_supabase_anon_key

# Geliştirme sunucusunu başlatın
npm run dev

# Production build alın
npm run build
```

### Supabase Kurulumu

1. [Supabase](https://supabase.com) hesabı oluşturun
2. Yeni proje oluşturun
3. SQL Editor'da temel tabloları oluşturun (docs/supabase-setup.sql)
4. Aşağıdaki migration'ları çalıştırın:

```sql
-- Tahmini kalem desteği
ALTER TABLE transactions ADD COLUMN is_estimated boolean DEFAULT false;

-- Bütçe türleri tablosu
CREATE TABLE budget_types (
  id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  name text NOT NULL,
  auth_user_id uuid REFERENCES auth.users(id),
  created_at timestamp with time zone DEFAULT now()
);
ALTER TABLE budget_types ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Users can manage own budget types" ON budget_types
  FOR ALL USING (auth_user_id = auth.uid());

-- Bütçe türü erişim kontrolü
CREATE TABLE budget_type_users (
  id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  budget_type_id uuid REFERENCES budget_types(id) ON DELETE CASCADE,
  user_id uuid REFERENCES users(id) ON DELETE CASCADE,
  auth_user_id uuid REFERENCES auth.users(id),
  created_at timestamp with time zone DEFAULT now()
);
ALTER TABLE budget_type_users ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Users can manage own budget type users" ON budget_type_users
  FOR ALL USING (auth_user_id = auth.uid());

-- Transactions tablosuna bütçe türü ilişkisi
ALTER TABLE transactions ADD COLUMN budget_type_id uuid REFERENCES budget_types(id);
```

5. API anahtarlarını `.env` dosyasına ekleyin
6. Uygulamaya giriş yapın ve Ayarlar → Bütçe Türü Yönetimi'nden "Genel" adında bir bütçe türü ekleyin

## 🛠️ Teknolojiler

### Frontend
- React 18
- Vite
- TailwindCSS
- Recharts (Grafikler)
- date-fns (Tarih işlemleri)
- Lucide React (İkonlar)

### Backend
- Supabase (PostgreSQL)
- Supabase Auth (Authentication)
- Row Level Security (RLS)

### Deployment
- Vercel (Hosting)
- GitHub (Version Control)

## 📖 Kullanım

1. Uygulamayı açın ve kayıt olun
2. Aile üyelerini ekleyin (Ayarlar → Kullanıcı Yönetimi)
3. Gelir ve gider kategorileri oluşturun (Ayarlar → Kategori Yönetimi)
4. Bütçe türleri oluşturun (Ayarlar → Bütçe Türü Yönetimi)
   - "Genel" bütçe türünü mutlaka ekleyin (varsayılan bütçe)
   - İhtiyaca göre "Araç Tamiri", "Ev Tadilatı" gibi özel türler ekleyin
   - Her tür için erişim yetkilerini Shield ikonu ile ayarlayın
5. Dashboard'dan işlem ekleyin
   - Bütçe türünü seçin (default: Genel)
   - Henüz kesinleşmemiş kalemler için "Tahmini kalem" kutusunu işaretleyin
   - Kalem kesinleştiğinde düzenleyerek tahmini işaretini kaldırın
6. Genel Bütçe özetinde tüm bütçe türlerinin durumunu görün
   - Bir bütçe türü kartına tıklayarak detayına inin (drill-down)
   - Breadcrumb ile üst seviyeye geri dönün
7. Grafikler ve raporlarla bütçenizi takip edin
   - Özet kartlarda kesinleşmiş tutarlar ana değer, tahmini tutarlar alt bilgi olarak gösterilir
   - İşlem tablosunda Tümü / Kesin / Tahmini filtreleriyle görüntüleyin

## 🔒 Güvenlik

- Şifreler hash'lenerek saklanır
- Her kullanıcı sadece kendi verilerini görebilir
- Supabase Row Level Security ile korumalı
- Bütçe türü bazlı erişim kontrolü
- HTTPS ile güvenli bağlantı

## 📝 Lisans

MIT

## 🤝 Katkıda Bulunma

Pull request'ler kabul edilir. Büyük değişiklikler için önce issue açarak tartışalım.

## 📧 İletişim

Sorularınız için issue açabilirsiniz.
