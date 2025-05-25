-Oyunun başlamasıyla birlikte "Oyuna Başla" butonunu HTML'deki giriş ekranından kaldırıp canvas (oyun ekranı) içine yerleştirmek istiyorsanız, bunu JavaScript içinde canvas'a çizerek yapmalısınız.

+"Oyuna Başla" yazısını  mouse click pozisyonu ile kullanabilir miyim?
-Harika! Şimdi canvas içindeki "Oyuna Başla" yazısını tıklanabilir hale getirelim.
<script>
var baslaButon;

function component(fontSize, fontFamily, color, x, y, type, text) {
    this.type = type;
    this.fontSize = fontSize;
    this.fontFamily = fontFamily;
    this.color = color;
    this.x = x;
    this.y = y;
    this.text = text;
    this.width = 0;
    this.height = 30; // Yaklaşık font yüksekliği

    this.update = function () {
        const ctx = myGameArea.context;
        if (this.type == "text") {
            ctx.font = this.fontSize + " " + this.fontFamily;
            ctx.fillStyle = this.color;
            ctx.fillText(this.text, this.x, this.y);
            this.width = ctx.measureText(this.text).width;
        }
    };

    this.clicked = function (mx, my) {
        return (
            mx >= this.x &&
            mx <= this.x + this.width &&
            my >= this.y - this.height &&
            my <= this.y
        );
    };
}

var myGameArea = {
    canvas: document.createElement("canvas"),
    start: function () {
        this.canvas.width = 480;
        this.canvas.height = 270;
        this.context = this.canvas.getContext("2d");
        document.body.insertBefore(this.canvas, document.body.childNodes[0]);

        this.canvas.addEventListener("click", function (e) {
            const rect = myGameArea.canvas.getBoundingClientRect();
            const x = e.clientX - rect.left;
            const y = e.clientY - rect.top;

            if (baslaButon.clicked(x, y)) {
                alert("Oyun başlıyor! 🚀");
                // Buraya oyun başlatma kodu gelecek
            }
        });
    }
};

function startGame() {
    myGameArea.start();
    document.getElementById("girisEkrani").style.display = "none";
    baslaButon = new component("30px", "Consolas", "red", 150, 150, "text", "Oyuna Başla");
    updateGameArea();
}

function updateGameArea() {
    myGameArea.context.clearRect(0, 0, myGameArea.canvas.width, myGameArea.canvas.height);
    baslaButon.update();
}
</script>
----------------------------------------------------------------------------------------------
+Canvas ile oyun ekranını nasıl hareket ettirebilirim?
-Canvas üzerinde bir oyun ekranını ortalamak için HTML ve CSS kombinasyonu ile çalışabilirsin.
<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <title>Canvas Ortalama</title>
  <style>
    body, html {
      height: 100%;
      margin: 0;
      display: flex;
      justify-content: center; /* YATAY ORTALAMA */
      align-items: center;     /* DİKEY ORTALAMA */
      background-color: #222;
    }
    canvas {
      border: 2px solid #fff;
      background-color: #000;
    }
  </style>
</head>
<body>
  <canvas id="gameCanvas" width="800" height="600"></canvas>

  <script>
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');

    // Basit bir örnek çizim
    ctx.fillStyle = 'lime';
    ctx.font = '48px sans-serif';
    ctx.fillText('Oyun Başladı!', 200, 300);
  </script>
</body>
</html>
----------------------------------------------------------------------------------------------
-display: flex ve justify-content: center + align-items: center ile tüm sayfada ortalama sağlanır.

-Canvas boyutunu (örneğin 800x600) sabit tuttuğun sürece bu yöntem gayet uygundur.

+Balon nesnesine görsel nasıl ekleyebilirim?
-Bir balonun üstüne img etiketiyle resim yüklenir.

-Bu resim, canvas'a çizilir (drawImage ile).
-Sticker olarak kullanabileceğin görsel örnekleri:
🟠 Flaticon → https://www.flaticon.com/

🟠 Emoji PNG → https://emojipedia.org/

🟠 Kendi görselin varsa → sticker.png olarak klasörüne ekle
-new Balon(x, y, yaricap, renk, "sticker.png");

----------------------------------------------------------------------------------------------
-Kendi oyununuzda kullanacağınız tüm resimlerin doğrudan .png, .jpg veya .webp formatında olması gerekir. Eğer bir simge sitesinden alıyorsanız:

Resme sağ tıklayıp "Resmi yeni sekmede aç" deyin.

URL'nin .png ile bitmesine dikkat edin.

Bu URL'yi Image().src'ye verin.
----------------------------------------------------------------------------------------------+Oyunun adını hareketli nasıl yapabilirim?
-Bir yaziX değişkeni ile yazının x konumunu tutarız.

Her oyunAlaniGuncelle() çağrısında bu yaziX değerini azaltarak yazının sola doğru kaymasını sağlarız.

Yazı ekranın dışına çıkınca en sağdan yeniden başlatırız.