# C# Video Yakalama Uygulaması

Bu C# kodu, video yakalama işlemleri için bir form uygulaması oluşturmayı amaçlamaktadır. Aşağıda kodun adım adım ne yaptığını açıklamaya çalışacağım.

## Adım1: Gerekli İsim Alanlarını Tanımlanması

![](/image/Ekran Görüntüsü (211).png)

Bu kütüphane ve ad alanlarının kullanımı, video yakalama işlemlerini gerçekleştirmek ve görüntüleri işlemek için sağlanan sınıfları ve işlevleri kullanmamıza olanak sağlar.

## Adım 2: Form Sınıfının Tanımlanması

![](C:\Users\PC\Desktop\C# Video Yakalama Uygulaması\image\Ekran Görüntüsü (218).png)

- **`public partial class Form1 : Form`** satırı, `Form1` adında bir sınıfın tanımlandığını ifade eder. Bu sınıf, `Form` sınıfından türetilmiştir ve `partial` anahtar kelimesiyle işaretlenmiştir.

  `partial` anahtar kelimesi, bir sınıfın veya yapısal bir bileşenin tanımının birden fazla dosyaya bölünebileceğini belirtir. Yani, `Form1` sınıfının tanımı birden fazla dosyada yapılabilir. Bu özellik, büyük ve karmaşık sınıfların daha modüler bir şekilde yönetilmesini sağlar.  

- **`private VideoCaptureDevice videoCapture;`**  satırı, `VideoCaptureDevice` adında bir sınıfa ait `videoCapture` adında bir örneğin, `private` (özel) erişim belirleyiciyle tanımlandığını ifade eder.

  `VideoCaptureDevice`, AForge.Video.DirectShow ad alanında bulunan bir sınıftır. Bu sınıf, video yakalama cihazını temsil eder. Yani, bir video akışını yakalamak veya kaydetmek için kullanılan bir cihazı temsil eder. Bu cihaz, genellikle bir web kamerası, video yakalama kartı veya benzeri bir donanım olabilir.

- **`public static FilterInfoCollection filterInfo;`** satırı, `FilterInfoCollection` adında bir sınıfa ait `filterInfo` adında bir örneğin, `public static` (genel ve statik) erişim belirleyicileriyle tanımlandığını ifade eder.

  `FilterInfoCollection`, AForge.Video.DirectShow ad alanında bulunan bir sınıftır. Bu sınıf, video yakalama cihazlarının bilgilerini içeren bir koleksiyonu temsil eder. Yani, mevcut video giriş cihazlarının (web kameraları, video yakalama kartları vb.) bilgilerini toplu halde saklar.

  `filterInfo`, `FilterInfoCollection` sınıfının bir örneğidir ve video yakalama cihazlarının bilgilerini içeren bir koleksiyonu temsil eder. Bu örnek, uygulama içinde kullanılabilir video cihazlarının listesini tutar. Örneğin, mevcut video cihazlarının isimlerine, özelliklerine veya ayarlarına erişmek için bu örneği kullanabiliriz.

## Adım 3: Form Yapıcısı ve Başlangıç Ayarları

![](C:\Users\PC\Desktop\C# Video Yakalama Uygulaması\image\Ekran Görüntüsü (213).png)

- **`public Form1()` **metodu, `Form1` adlı sınıfa ait bir yapıcı metodun tanımını içerir. Bir sınıfın yapıcı metodu, sınıfın bir örneği oluşturulduğunda çağrılan ilk metodudur. Bu durumda `Form1` sınıfının bir örneği oluşturulduğunda `Form1()` yapıcı metodu otomatik olarak çalışır.
- **`InitializeComponent()`** metodu, Visual Studio tarafından otomatik olarak oluşturulan bir metottur ve Windows Forms uygulamalarında formun bileşenlerini (kontrolleri) oluşturur ve özelliklerini ayarlar. Bu metot, formun tasarımını yükler ve içerisinde yer alan kontrolleri hazır hale getirir.
- **`pictureBox1.SizeMode = PictureBoxSizeMode.StretchImage;`** satırı, `pictureBox1` adlı `PictureBox` kontrolünün `SizeMode` özelliğini `PictureBoxSizeMode.StretchImage` olarak ayarlar. Bu ayar, resmin PictureBox kontrolüne sığacak şekilde ölçeklendirilmesini sağlar.
- **`filterInfo = new FilterInfoCollection(FilterCategory.VideoInputDevice);` **satırı, `FilterInfoCollection` sınıfından `filterInfo` adında bir örnek oluşturur. Bu örneği, `FilterCategory.VideoInputDevice` kategorisindeki video giriş cihazlarının bilgilerini içeren bir koleksiyonla ilişkilendirir. Yani, mevcut video giriş cihazlarının bilgilerini toplu halde saklayan bir koleksiyonu temsil eder.
- **`foreach (FilterInfo item in filterInfo)`** döngüsü, `filterInfo` koleksiyonundaki her bir `FilterInfo` öğesi için döngüyü döndürür. Her döngü adımında, `item` değişkeni, koleksiyondaki bir `FilterInfo` öğesini temsil eder.
- İlk olarak, **`toolStripComboBox1`** adlı bir `ToolStripComboBox` kontrolünün `Items` özelliği kullanılarak, her bir `FilterInfo` öğesinin `Name` özelliği `toolStripComboBox1`'e eklenir. `Name` özelliği, video cihazlarının isimlerini içerir. Bu şekilde, her bir video cihazının ismi, açılır liste içerisinde bir öğe olarak gösterilir.
- Sonrasında **`toolStripComboBox1.SelectedIndex = 0;` **satırıyla, `toolStripComboBox1` kontrolünün seçili öğesi `0` (ilk öğe) olarak ayarlanır. Bu, uygulama başlatıldığında varsayılan olarak ilk video cihazının seçili olduğunu belirtir. 
- **`StartCamera();`** satırı, `StartCamera()` adlı bir metodu çağırır. Bu metodun amacı, video yakalama cihazını başlatmak ve görüntü akışını almak için gerekli işlemleri gerçekleştirmektir.

## Adım 4: Yeni Kare Olay İşleyicisi

<img src="C:\Users\PC\Desktop\C# Video Yakalama Uygulaması\image\Ekran Görüntüsü (214).png" style="zoom:80%;" />

- **`private void video_NewFrame(object sender, NewFrameEventArgs eventArgs) `** satırı; bir olay işleyicisi (`event handler`) tanımlar. Özel olarak, `video_NewFrame` adında bir olay işleyicisidir. Bu olay işleyicisi, video yakalama cihazından her yeni kare (frame) alındığında tetiklenir.

  Olay işleyicisi, iki parametre alır: `sender` ve `eventArgs`. `sender` parametresi, olayın kaynağı olan nesneyi temsil eder. `NewFrameEventArgs` ise, yeni karenin içeriğini taşıyan bir olay argümanıdır.

- Devamında bir try-catch bloğu içinde yer alan işlem kodlarını içerir. Bu kodlar, video yakalama cihazından alınan yeni bir karenin işlenmesini ve `pictureBox1` kontrolüne yüklenmesini gerçekleştirir.

  Aşağıda kodun adım adım açıklaması bulunmaktadır:

  - İlk olarak, try bloğu başlar.
  - **`using (var bitmap = (Bitmap)eventArgs.Frame.Clone()) `** satırıyla; `using` ifadesiyle, `bitmap` adında bir `Bitmap` nesnesi oluşturulur ve `eventArgs.Frame` özelliği kullanılarak bu nesneye bir kopya alınır.
  - **`BitmapImage bi = new BitmapImage(); `** satırıyla;  `BitmapImage` sınıfından bir `bi` nesnesi oluşturulur. `BitmapImage`, bir görüntünün kaynağını temsil eden ve UI (kullanıcı arayüzü) elemanlarında görüntülenmek üzere kullanılan bir sınıftır.
  - **`bi.BeginInit(); `** satırıyla; `BeginInit()` yöntemi, `BitmapImage` nesnesinin başlatılmasını işaret eder. Bu yöntem, `BitmapImage` nesnesinin bir dizi ayarını yapılandırmak için kullanılır. Örneğin, görüntünün kaynağını belirlemek, boyutlarını ayarlamak veya görüntünün yeniden boyutlandırılmasını etkinleştirmek gibi işlemler bu başlatma aşamasında gerçekleştirilebilir.
  - **`MemoryStream ms = new MemoryStream(); `** satırı; bir `MemoryStream` nesnesi olan `ms` adında yeni bir bellek akışı oluşturur.
    - `MemoryStream`, bellekteki bir veri bloğunu okuma, yazma ve taşıma işlemleri yapmak için kullanılan bir sınıftır.
    - `MemoryStream` sınıfı, verileri hafızada tutar ve bu verilere erişim sağlar.
    - `new MemoryStream()` ifadesiyle, boş bir bellek akışı oluşturulur. Bu akış, başlangıçta herhangi bir veri içermez.
    - `ms` değişkeni, oluşturulan bellek akışını temsil eder ve daha sonra bu akış üzerinde okuma ve yazma işlemleri gerçekleştirilebilir.
  - **`bitmap.Save(ms, ImageFormat.Bmp);`** satırı; `bitmap` adlı bir `Bitmap` nesnesinin içeriğini, `ms` adlı bir `MemoryStream` üzerine BMP (Bitmap) formatında kaydeder.
    - `bitmap.Save(ms, ImageFormat.Bmp)` ifadesi, `bitmap` nesnesini BMP formatında `ms` bellek akışına kaydeder.
    - `Save` yöntemi, `Bitmap` sınıfının bir yöntemidir ve görüntünün belirtilen bir akışa kaydedilmesini sağlar.
    - `ms` bellek akışı, kaydedilen görüntünün hedef bellekte depolandığı yerdir.
    - `ImageFormat.Bmp` ise kaydedilecek görüntünün formatını belirtir. BMP (Bitmap) formatı, renkli veya siyah beyaz piksel tabanlı görüntülerin kaydedilmesi için kullanılan bir dosya formatıdır.
  - **`ms.Seek(0, SeekOrigin.Begin);  `** satırı; `ms` adlı bir `MemoryStream` nesnesinin okuma/yazma konumunu başlangıç konumuna (0. byte) ayarlar.
    - `ms.Seek(0, SeekOrigin.Begin)` ifadesi, `ms` bellek akışının okuma/yazma konumunu belirli bir başlangıç konumuna taşır.
    - `Seek` yöntemi, `MemoryStream` sınıfının bir yöntemidir ve bellek akışının okuma/yazma konumunu değiştirmek için kullanılır.
    - İlk parametre olan `0`, konumu belirler ve burada 0 kullanılarak başlangıç konumuna işaret edilir.
    - İkinci parametre olan `SeekOrigin.Begin`, başlangıç konumunu belirtir ve akışın başından itibaren ölçüm yapılacağını gösterir.
  - **`bi.StreamSource = ms;  `** satırı, `ms` bellek akışını `bi` nesnesinin `StreamSource` özelliğine atayarak, `BitmapImage` nesnesinin kaynak bellek akışını belirler. Bu şekilde, `bi` nesnesi, göstermek veya işlem yapmak için `ms` bellek akışından görüntü verilerini kullanabilir.
  - `EndInit()` yöntemi çağrılarak, `BitmapImage` nesnesi sonlandırılır.
  - `Freeze()` yöntemi çağrılarak, `BitmapImage` nesnesinin dondurulması sağlanır (değişmez hale getirilir).
  - `Dispatcher.CurrentDispatcher.Invoke()` yöntemi kullanılarak, güncelleme işlemi UI (kullanıcı arayüzü) ana iş parçacığı üzerinde yapılır. Bu, `pictureBox1` kontrolüne `Image.FromStream(ms)` ile oluşturulan bir görüntünün atanmasını sağlar.
  - try bloğunun sonunda catch bloğu yer alır. Eğer herhangi bir hata oluşursa, bu hata yakalanır ve `exc` adında bir `Exception` nesnesiyle temsil edilir. Yakalanan hatanın mesajı `MessageBox.Show()` ile kullanıcıya gösterilir.

  Bu kod bloğu, video yakalama cihazından alınan yeni bir karenin bir `Bitmap` nesnesine kopyalanması ve bu kopyanın bir `BitmapImage` nesnesi üzerinde işlenmesiyle sonuçlanır. Daha sonra, `pictureBox1` kontrolüne bu işlenmiş görüntünün yüklenmesi sağlanır.

## Adım 5: Kamera Değişikliği Olay İşleyicisi

<img src="C:\Users\PC\Desktop\C# Video Yakalama Uygulaması\image\Ekran Görüntüsü (216).png" style="zoom:80%;" />

- `toolStripComboBox1` adlı bir araç çubuğu (tool strip) öğesinin `SelectedIndexChanged` olayının tetiklenmesi durumunda çalışacak olan bir olay işleyicisini tanımlar.

  İşlevi şu şekildedir:

  - Kullanıcı `toolStripComboBox1` araç çubuğunda bir öğe seçtiğinde, yani `SelectedIndexChanged` olayı tetiklendiğinde, bu olay işleyicisi devreye girer.
  - İlk olarak, **`if (videoCapture != null)` **ifadesiyle kontrol edilir. Bu, `videoCapture` nesnesinin `null` olup olmadığını kontrol eder.
  - Eğer `videoCapture ` nesnesi `null `değilse, yani bir video yakalama işlemi devam ediyorsa, aşağıdaki adımlar gerçekleştirilir:
    - **`pictureBox1.Image = null;` **ifadesi, `pictureBox1` kontrolündeki resmi temizler, yani görüntüyü kaldırır.
    - **`videoCapture.Stop();`** ifadesi, `videoCapture` nesnesini durdurur, yani video yakalama işlemini durdurur.
  - Ardından, `StartCamera()` yöntemi çağrılır. Bu yöntem, kamera görüntüsünü başlatmak için gerekli işlemleri gerçekleştirir.

  Bu kod parçası, `toolStripComboBox1` öğesinin seçilen öğe değiştikçe yapılması gereken işlemleri tanımlar. Bu işlemler arasında önceki video yakalama işlemini durdurma, `pictureBox1` kontrolündeki resmi temizleme ve yeni bir kamera görüntüsünü başlatma gibi adımlar yer alır.

## Adım 6: Kamera Başlatma Metodu

<img src="C:\Users\PC\Desktop\C# Video Yakalama Uygulaması\image\Ekran Görüntüsü (217).png" style="zoom:80%;" />

- İlk olarak, `try` bloğu içinde işlemler gerçekleştirilir.
- **`videoCapture = new VideoCaptureDevice(filterInfo[toolStripComboBox1.SelectedIndex].MonikerString);` **satırı; `filterInfo` koleksiyonundan seçilen bir video cihazını temsil eden `VideoCaptureDevice` nesnesini oluşturur. İşlevi şu şekildedir:
  - `filterInfo[toolStripComboBox1.SelectedIndex]` ifadesi, `filterInfo` koleksiyonundan `toolStripComboBox1` araç çubuğunda seçilen öğenin dizinini temsil eden bir `FilterInfo` nesnesini alır.
  - `MonikerString` özelliği, seçilen video cihazının benzersiz tanımlayıcısını (`Moniker`) temsil eder. Bu tanımlayıcı, cihazın bilgisayar sistemi tarafından kullanılmasını sağlar.
  - `new VideoCaptureDevice(...)` ifadesi, `filterInfo` koleksiyonundan alınan `MonikerString` değeri kullanılarak yeni bir `VideoCaptureDevice` nesnesi oluşturur. Bu nesne, seçilen video cihazını temsil eder.
  - `videoCapture` değişkenine bu oluşturulan `VideoCaptureDevice` nesnesi atanır, böylece ilgili video cihazıyla etkileşimde bulunabilirsiniz.

- **`videoCapture.NewFrame += video_NewFrame;`** ifadesi, `videoCapture` nesnesinin `NewFrame` olayına `video_NewFrame` yöntemini olay işleyici olarak atar. Bu sayede her yeni çerçeve geldiğinde `video_NewFrame` yöntemi çağrılır.
- **`videoCapture.Start();`** ifadesi, `videoCapture` nesnesini başlatır, yani kamera yakalama işlemini başlatır.
- Eğer herhangi bir hata oluşursa, `catch` bloğu içinde yakalanır ve bir hata iletişim kutusu (`MessageBox.Show`) görüntülenir, içinde hatanın ileti metni (`e.Message`) bulunur.

Bu kod parçası, kamera yakalama işlemini başlatmak için gerekli nesnelerin oluşturulması ve olay işleyicilerinin atamasını içerir. `videoCapture` nesnesi, seçilen video cihazının özelliklerini ve olaylarını yönetmek için kullanılır.

Bu Markdown raporu, C# video yakalama uygulamasının kodunu adım adım açıklamaktadır. Her adım, kodun ne yaptığını ve nasıl çalıştığını anlamak için önemlidir. İlgili adımları dikkatlice takip ederek kodu daha iyi anlayabilirsiniz.

