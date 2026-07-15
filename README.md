# Paper Portfolio, elinle kontrol edilen video

Kağıt estetikli bir portfolyo sayfası. Sağdaki video kendi kendine oynamaz: tarayıcı kameradan elini izler, avucunu açtıkça video ilerler, yumruk yaptıkça geri sarar.

**Canlı demo:** https://enesayd.in/paper/

**Nasıl yapıldığının adım adım anlatımı:** https://enesayd.in/paper/nasil-yapildi.html

Orijinal tasarım [divyashrma18/paper-portfolio](https://github.com/divyashrma18/paper-portfolio) reposundan alındı. Bu repo, videonun scrub için optimize edilmiş ve kişiselleştirilmiş halidir.

## Nasıl çalışır

1. **MediaPipe HandLandmarker** (CDN'den yüklenir) her karede elin 21 noktasını verir.
2. Parmak uçlarının bileğe ortalama uzaklığı avuç boyutuna bölünür, 0 ile 1 arası bir "açıklık" değeri çıkar. Değer EMA ile yumuşatılır ve aralık kullanıcının eline göre kendini kalibre eder.
3. Açıklık değeri doğrudan `video.currentTime`'a yazılır. Yumruk = videonun başı, açık avuç = sonu.
4. Kamera istemeyenler için ok tuşları da videoyu sarar.

## Kritik detay: video encode

Video sürekli ileri geri sarıldığı için sık keyframe şart, yoksa her seek takılır:

```bash
ffmpeg -i kaynak.mp4 -c:v libx264 -g 1 -crf 21 -preset fast -pix_fmt yuv420p -an -movflags +faststart assets/me.mp4
```

## Kurulum

```bash
git clone https://github.com/Eneslexi/paper-portfolio.git
cd paper-portfolio
python3 -m http.server 8000
```

Derleme yok, framework yok. Üç dosya ve bir video. Kamera izni sadece `localhost` ve `https` üzerinde çalışır, canlıda SSL şart.
