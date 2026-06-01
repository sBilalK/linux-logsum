#!/bin/bash
# Log Analizi ve Yedekleme Aracı

baslik(){ # Başlık yazdırma fonksiyonu
    echo ""
    echo "======================"
    echo "$1"
    echo "======================"
    echo ""
}

    # 1. Argüman Kontrolü
    baslik "1. Argüman ve Dizin Kontolü" #başlık yazdırma
    if [ -z "$1" ]; then #argümanı kontrol eder
        echo "HATA: lütfen dizini argüman olarak verin" #argüman yoksa hata mesajı verir
        exit 1 #scripti sonlandırır
    fi


    hedef_dizin="$1"
    if [ ! -d "$hedef_dizin" ]; then #argümandaki dizinin var olup olmadığıı kontrol edr
        echo "HATA: verilen dizin mevcut değil: $hedef_dizin" #dizin yoksa hata mesajı verir
        exit 1 #scripti sonandırır
    fi

    # 2. Çalışma Dizini Hazırlama
    baslik "2. Çalışma Dizini Hazırlama" #başlık yazdırma
    rapor_dizin="$HOME/log_rapor" #rapor dizini için değişken tanımlar

    if [ -d "$rapor_dizin" ]; then #rapor dizini var ise
        echo "Çalışma dizini zaten var, siliniyor: $rapor_dizin" #dizin var mesajı verir
        rm -rf "$rapor_dizin" #ve dizini siler
    fi

    echo "Çalışma dizini oluşturuluyor: $rapor_dizin" #çalışma dizini yoksa oluşturur varsa zaten sildiğimiz için yine oluşturur
    mkdir -p "$rapor_dizin" #dizini oluşturur

    # 3. Log Dosyalarını Sayma
    baslik "3. Log Dosyalarını Sayma" #başlık yazdırma
    log_sayisi=$(find "$hedef_dizin" -maxdepth 1 -name "*.log" | wc -l) #hedef dizindeki .log dosyalarını sayar
    
    echo "Toplam log dosyası: $log_sayisi" #.log dosya sayısını söyler

    if [ "$log_sayisi" -eq 0 ]; then #.log dosyası yoksa
        echo "HATA: Log dosyası bulunamadı." #hata mesajı verir
        exit 1 #scripti sonlandırır
    fi

    # 4. En Büyük Log Dosyası
    baslik "4. En Büyük Log Dosyası" #başlık yazdırma
    biggestlog=$(ls -lhS "$hedef_dizin"/*.log | head -n 1 | awk '{print "Adı: " $9, " | Boyutu: " $5}') #en büyük .log dosyasını bulur adını ve boyuunu yazdıır.
    echo "En Büyük Log Dosyası: $biggestlog" #en büyük .log dosyasını ekrana yazdırır

    # 5. Hata Satırı Analizi
    baslik "5. Hata Satırı Analizi" #başlık yazdırma
    top_err=0 #değişken: toplam hata sayısı
    ozet_doc="$rapor_dizin/hata_ozet.txt" #değişken: özet dosyası yolu

    for log_dosya in "$hedef_dizin"/*.log; do #her log dosyası için
        dosya_adi=$(basename "$log_dosya") #dosya adını alır
        hata_sayisi=$(grep -c "ERROR" "$log_dosya") #dosyada ERROR geçen satır sayısını bulur
        top_err=$((top_err + hata_sayisi)) #toplam hata sayısına ekler
        echo "$dosya_adi: $hata_sayisi hata satırı" >> "$ozet_doc" #dosya adı ve hata sayısını özet dosyasına yazar
    done

    echo "Hata istatistikleri başarıyla $ozet_doc dosyasına yazıldı." #işlemin tamamlandığını söyler

    # 6. Koşullu Durum Değerlendirmesi
    baslik "6. Koşullu Durum Değerlendirmesi" #başlık yazdırma
    echo "Toplam hata sayısı: $top_err" #toplam hata sayısını ekrana yazdırır
    if [ "$top_err" -gt 100 ]; then #toplam hata sayısı 100'den büyükse
        echo "DURUM: KRİTİK" #durum kritik
    else
        echo "DURUM: NORMAL" #durum normal
    fi

    # 7. Arşivleme
    baslik "7. Arşivleme" #başlık yazdırma
    tarih=$(date +%Y%m%d) #tarih değişkeni: bugünün tarihi YYYYMMDD formatında
    arsiv_dosya="$rapor_dizin/log_yedek_$tarih.tar.gz" #arşiv dosyası adı: log_yedek_TARIH.tar.gz

    cd "$hedef_dizin" || exit #hedef dizine geçer geçemezse scripti sonlandırır
    tar -czf "$arsiv_dosya" *.log 2>/dev/null #hedef dizindeki tüm .log dosyalarını arşiv olarak oluşturur
    cd - >/dev/null #orijinal dizine geri döner

    echo "Log dosyaları başarıyla arşivlendi: $arsiv_dosya" #arşivleme işleminin tamamlandığını söyler

    chmod 640 "$arsiv_dosya" #arşiv dosyasının erişim iznini 640 olarak ayarlar

    yeni_izinler=$(ls -l "$arsiv_dosya" | awk '{print $1}') #arşiv dosyasının yeni izinlerini alır


    echo "Arşiv dosyası erişim izni 640 olarak ayarlandı" #arşiv dosyasının erişim izninin başarıyla ayarlandığını söyler
    echo "Bütün işlemler tamamlandı." #işlemlerin tamamlandığını söyler
    echo "script sonlandı." #scriptin sonlandığını söyler

    
