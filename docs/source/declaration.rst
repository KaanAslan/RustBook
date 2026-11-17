===================
Bildirim İşlemleri
===================

Bu bölümde Rust'ta değişken bildirimleri üzerinde duracağız. Rust yeni bir programlama dilidir. Bu nedenle dilin
terminolojisi de maalesef tam olarak oturtulamamıştır. Bir değişkenin oluşturulmasına yani faaliyet alanına
sokulmasına bazı dillerde *bildirim (declaration)* bazı dillerde *tanımlama (definition)* denilmektedir. Rust'ta
da değişkenin faaliyet alanına sokulmasına **bildirim (declaration)** denilmektedir. Ancak bildirim oluştururken
kullanılan ``let`` deyimi *kalıp uyuşumu (pattern matching*) kurallarına uygun çalışmaktadır. Bu nedenle Rust
programcıları bildirim yerine **değişkeni bağlamak (variable binding)** terimini de kullanmaktadır. Biz kursumuzda
bir değişkenin faaliyet alanına sokulması için bildirim (declaration) terimini kullanacağız.

let Deyimi
==========

Rust'ta bir değişkenin faaliyet alanına sokulması (yani bağlanması) ``let`` deyimiyle yapılmaktadır. let deyimi *kalıp
uyuşumu (pattern matching)* özelliği ile çalışır. Bu nedenle ``let`` deyiminin genel biçimi biraz ayrıntılıdır. Biz bu
bölümde kalıp uyuşumu özelliğini göz ardı ederek ``let`` deyiminin temel kullanımı üzerinde duracağız.

.. code-block:: text

   let <değişken_ismi> [: <tür>] [= <ilkdeğer>];

``let`` deyiminin üç temel kullanım biçimi vardır:

.. code-block:: text

   1) let <değişken_ismi>: <tür>;
   2) let <değişken_ismi> = <ilkdeğer>;
   3) let <değişken_ismi>: <tür> = ilkdeğer;

``let`` deyimi ile bir değişken bağlanırken değişken isminden sonra ``':'`` atomu ve sonra değişkenin türü belirtilebilir.
Örneğin:

.. code-block:: rust

   let a: i32;
   let b: f64;
   let c: bool;

Eğer bildirilen değişkene ilkdeğer veriliyorsa tür belirtilmeyebilir. Örneğin:

.. code-block:: rust

   let a = 10;
   let b = 3.14;
   let c = true;

Bu durumda değişkenin türü ona verilen ilkdeğerden hareketle tespit edilmektedir. ``let`` deyiminde hem tür belirtilebilir
hem de değişkene ilkdeğer verilebilir. Örneğin:

.. code-block:: rust

   let a: i32 = 10;
   let b: f64 = 3.14;
   let c: bool = true;

Rust'ta değişkene ilkdeğer ``let`` deyiminde verilmek zorunda değildir. İlkdeğer verme daha sonra atama deyimiyle de
yapılabilmektedir. Örneğin:

.. code-block:: rust

   let a;          // geçerli çünkü daha sonra ilkdeğer verilmiş

   a = 10;

Bu işlem aşağıdakiyle eşdeğerdir:

.. code-block:: rust

   let a = 10;

Ancak ``let`` ile tür belirtmeden bildirim yapıldıktan sonra değişkene ilkdeğer verilmezse bu durum error oluşturmaktadır.

Aslında verilen ilkdeğerden hareketle otomatik tür tespiti eski pek çok programlama diline zaman içerisinde sokulmuştur.
C++11 ile birlikte C++'a, C23 ile de C'ye auto anahtar sözcüğüyle bu özellik eklenmiştir. Örneğin:

.. code-block:: cpp

   auto a = 123;       // 123 int türden olduğu için a da int türden

Java ve C#'ta da aynı özellik ``var`` anahtar sözcüğü ile sağlanmaktadır. Örneğin:

.. code-block:: csharp

   var a = 10;

Pek çok programlama dilinde aynı türden birden fazla değişkenin bildirimi tek hamlede yapılabilmektedir. Örneğin C'de
aşağıdaki gibi bir bildirim geçerlidir:

.. code-block:: c

   int a = 10, b, c = 20;

Ancak Rust'taki let deyimi *kalıp uyuşumu (pattern matching)* ile çalışmaktadır. Rust'ta bu biçimde bir bağlama
yapılamamaktadır. Örneğin:

.. code-block:: rust

   let a = 10, b = 20;         // error!

Rust'ta bu işlem tipik olarak *demet (tuple) kalıbı* ile yapılmaktadır. Örneğin:

.. code-block:: rust

   let (a, b) = (10, 20);      // geçerli

Değişken İsimlendirme Kuralları ve Rust'ın Yazım Geleneği
=========================================================

Rust *büyük harf küçük harf duyarlılığı olan (case sensitive)* bir programlama dilidir. Yani büyük harfler ve küçük
harfler farklı karakterler olarak ele alınmaktadır. Rust'ta değişken isimlendirmesinde Unicode tablodaki tüm dillerin
alfabetik ve nümerik karakterleri kullanılabilmektedir. Örneğin biz bir değişken ismini Türkçe karakterlerden
oluşturabiliriz:

.. code-block:: rust

   let ağrı_dağının_yüksekliği = 5137;

Her ne kadar böyle bir serbestlik varsa da diğer pek çok dilde olduğu gibi değişken isimlendirmesinde yoğun olarak
İngilizce karakterler tercih edilmektedir. Birden fazla sözcük içeren değişkenlerin harflendirilmesi (*capitalization*)
için Rust'ta genel olarak C'deki gibi *yılan notasyonu (snake case)* tercih edilmektedir. Ancak yapı, enum, trait
gibi türlere ilişkin isimler (items) genellikle Pascal tarzı, sembolik sabitler (*const* değişkenler) ise genellikle tüm harfleri
büyük olacak biçimde harflendirilmektedir. Biz de kursumuzda Rust'taki bu geleneğe uyacağız.

Rust'ta bir değişken isminin başında ``'_'`` karakteri varsa bu durum Rust derleyicisi tarafından özel bir anlama
gelmektedir. Rust derleyicisi varsayılan durumda bildirilmiş ancak kullanılmamış değişkenler için uyarı mesajı vermektedir. 
Ancak eğer değişkenin başında ``'_'`` karakteri varsa bu uyarıyı vermemektedir. Yani programcı ismini ``'_'`` ile
başlattığı değişken için derleyiciye "ben bu değişkeni kullanmayabilirim. Belki de daha sonra kullanacağım. Bunun için
bana uyarı vmesajı erme* demek istemektedir. Örneğin:

.. code-block:: rust

   let _number = 123;

Biz burada artık bu number değişkenini kullanmasak bile derleyici bunun için bize uyarı vermeyecektir.

``let`` deyiminde tek bir ``_`` karakteri kalıp uyuşumunda *joker kalıbı (wildcard pattern)* anlamına gelmektedir. Joker kalıbı
bir değişken belirtmez, değerin kullanılmayacağını vurgulamak amacıyla kullanılmaktadır. Örneğin:

.. code-block:: rust

   let _ = foo();

Burada programcı ``foo`` fonksiyonunun geri dönüş değerinden faydalanmak istemediğini derleyiciye söylemektedir. Tabii 
aslında Rust derleyicisi bu durumda ``_`` için bellekte bir yer ayırmaz. Zaten programcı da bu ``_`` karakterini programının 
içerisinde bir değişken ismi gibi kullanamamaktadır. Örneğin:

.. code-block:: rust

   let _ = 123;

   println!("{}", _);      // error!

Belli bir versiyondan sonra Rust'ta atama operatörüün soluna da ``_`` karakteri getirilebilmektedir. Bu biçimdeki ifadelere 
*alttire ifadeleri (underscore expression)"* denilmektedir. Örneğin:

.. code-block:: rust

   _ = foo();

Değişken isimleri yerine ``_`` kullanımı ile özellikle demet açımlarında sıkça karşılaşılmaktadır. Demetler konusu ileride 
ele alınacaktır.

Rust'ta değişkenlerin maksimum karakter uzunluğu konusunda *The Rust Reference* dokümanlarında bir belirlemede
bulunulmamıştır. Bu konuda bir belirlemede bulunulmamış olması bu durumun derleyicileri yazanların isteğine bırakıldığı
anlamına gelmektedir.

Rust'taki yazım geleneği **Rust Style Guideline** dokümanında ayrıntılarıyla açıklanmaktadır. Bu dokümana aşağıdaki bağlantıdan 
erişebilirsiniz:

https://doc.rust-lang.org/stable/style-guide/

Değişkenlerin Değiştirilebilirliği ve Değiştirilemezliği
========================================================

Rust'ta bir değişken bildirildiğinde değişken varsayılan durumda *değiştirilemez (immutable)* durumdadır. Yani o değişken 
C'deki const nesne gibidir. Örneğin:

.. code-block:: rust

   let a = 10;

Burada ``a`` değişkeni eğiştirilemez (*immutable*) durumdadır. Ancak değiştirilemez bir değişkene ilkdeğer
verilmesi zorunlu da değildir. Buradaki a değişkenine bildirildikten sonra da değer atanabilir. Ancak ne olursa olsun
değiştirilemez değişkenlere toplamda yalnızca bir kez değer atanabilmektedir. Örneğin:

.. code-block:: rust

   let a: i32;

   a = 10;         // geçerli
   a = 20;         // error!

Değişkenlerin varsayılan durumda değiştirilemez olması Rust'a özgü bir özellik değildir. Swift gibi Kotlin gibi yeni
programlama dillerinde de Erlang ve Haskell gibi fonksiyonel dillerde de değişkenler varsayılan durumda değiştirilemez
biçimdedir. Rust'ta değişkenin değiştirilebilir (*mutable*) olmasını sağlamak için değişken isminin soluna *mut* anahtar sözcüğü
getirilmektedir. Örneğin:

.. code-block:: rust

   let mut a: i32 = 10;

   a = 20;         // geçerli
   //...
   a = 30;         // geçerli

Örneğin:

.. code-block:: rust

   let mut a: i32;

   a = 10;                         // geçerli
   println!("{}", a);              // 10
   a = 20;                         // geçerli
   println!("{}", a);              // 20

İlkdeğer Verilmemiş Değişkenlerin Kullanımı
===========================================

Java, C# ve yeni tasarlanmış olan bazı dillerde olduğu gibi Rust'ta da henüz değer atanmamış bir değişkenin kullanılması
(içerisindeki değerin elde edileceği bir ifadede kullanılması) derleme aşamasında error oluşturmaktadır. Örneğin:

.. code-block:: rust

   let a: i32;

   println!("{}", a);       // derleme aşamasında error oluşur!

C'de ve C++'ta içerisine henüz değer atanmamış nesnelerin kullanılabildiğini ancak bu durumun *tanımsız davranışa (undefined
behavior)* yol açtığını anımsayınız.

Değişkenlerin Gölgelenmesi
==========================

Rust'ta C, C++, Java, C# gibi dillerde bulunmayan ismine *gölgeleme (shadowing)* denilen bir faaliyet alanı (*scope*)
özelliği vardır. Rust'ta aynı faaliyet alanında aynı isimli değişkenler bildirilebilir. Bu durumda ikinci bildirime kadar i
lk değişken faaliyet göstermeye devam eder. İkinci bildirimden sonra artık ilk değişken kullanılamaz, ancak ikinci değişken 
kullanılabilir. Örneğin:

.. code-block:: rust

   let a: i32 = 10;

   println!("{}", a);      // 10

   let a: f64 = 3.14;      // geçerli, gölgeleme (shadowing) deniyor

   println!("{}", a);      // 3.14

Burada aynı fonksiyon içerisinde aynı isimli değişken ``let`` deyimi ile birden fazla kez bağlanmıştır. Bu tür bildirimlerin
C, C++, Java ve C# gibi pek çok dilde geçerli olmadığını anımsayınız.

*The Rust Reference* dokümanlarına göre gölgeleme sırasında türler aynı olsa bile farklı bir değişken yaratılır.
Örneğin:

.. code-block:: rust

   let a: i32 = 10;
   //...
   let a: i32 = 20;

Burada iki a'nın adresi alındığında farklı adresler görülecektir. Tabii kodun gözlemlenebilen davranışı değişmedikten
sonra derleyiciler kodu daha hızlı çalışacak ya da daha az yer kaplayacak biçimde yeniden düzenleyebilirler. Bu sürece
derleyicilerin *kod optimizasyonu (code optimization)* dendiğini anımsayınız. Aşağıdaki kodu inceleyiniz:

.. code-block:: rust

   let a: i32 = 10;

   println!("{}", a);      // 10

   let a: i32 = 20;

   println!("{}", a);      // 20

Bu kodda biz ``a`` için ikinci kez farklı bir yer ayrılıp ayrılmadığını anlayamayız. Çünkü kodumuzda bu gözlemi yapabilecek
bir durum oluşmamıştır. Dolayısıyla derleyici burada tek bir ``a`` oluşturup kodu optimize etmeye çalışabilir.

Rust'ta Katı Tür Kontrolü
=========================

Rust *katı tür kontrolüne (strict type checking)* sahip bir programlama dilidir. Atama sırasında kaynak değerin türüyle
hedef değerin türünün aynı olması gerekir. Örneğin:

.. code-block:: rust

   let a: i8 = 10;
   let b: i32;

   b = a;      // error!

Burada her ne kadar i8 türündeki her değer i32 türüyle temsil edilebiliyor olsa da başka bir deyişle atamada bilgi kaybının
oluşması söz konusu değilse de bu atama error ile sonuçlanacaktır. Çünkü kaynak türle hedef tür birbirinden farklıdır.
Yukarıdaki gibi atamaların C, Java ve C# gibi programlama dillerinde tamamen normal ve geçerli olduğunu anımsayınız.

Rust'ta iki operand'lı operatörlerde operand'ların türleri de aynı olmak zorundadır. Örneğin:

.. code-block:: rust

   let a: i8 = 10;
   let b: i32 = 20;
   let c: i32;

   c = a + b;      // error!

C, C# ve Java gibi dillerde bu tür durumlarda işlem öncesinde *otomatik tür dönüştürmesinin (implicit type conversion)* 
uygulandığını anımsayınız.