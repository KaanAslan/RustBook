============
Temel Türler
============

Bir programlama dili öğrenilirken ilk öğrenilecek şeylerden biri o programlama dilinin temel türleridir. Biz de bu bölümde
Rust'ın temel türlerini açıklayacağız.

Rust'ın Tür Kategorileri
========================

*The Rust Reference* dokümanlarına göre Rust'taki türler şu kategorilerden oluşmaktadır:

* **Primitive types**

  * Boolean
  * Numeric
  * Textual
  * Never

* **Sequence types**

  * Tuple
  * Array
  * Slice

* **User-defined types**

  * Struct
  * Enum
  * Union

* **Function types**

  * Functions
  * Closures

* **Pointer types**

  * References
  * Raw pointers
  * Function pointers

* **Trait types**

  * Trait objects
  * Impl trait

Biz burada önce *temel türleri (primitive type)* sonra ilerleyen bölümlerde de *demet (tuple)*, *dizi (array)* ve *dilim
(slice)* gibi *dizilim türlerini (sequence types)* ve kullanıcı tanımlı türleri göreceğiz.

Tamsayı Türleri
===============

Rust'ta **sayısal türler (numeric types)** kendi aralarında **tamsayı türleri integer types)**, **gerçek sayı türleri (floating-point types)** 
ve **makine bağımlı tamsayı türleri (machine-dependent integer types)** olmak üzere üçe ayrılmaktadır: biçiminde üçe ayrılmaktadır. 
Tamsayı türleri de kendi aralarında **işaretli tamsayı türleri (signed integer types)** ve **işaretsiz tamsayı türleri (unsigned integer types)** 
olmak üzere ikiye ayrılmaktadır:

.. figure:: _static/rust-numeric-type-categories.png
   :align: center
   :width: 75%

Rust'ta işaretli tamsayılar ikiye tümleyen aritmetiği ile tutulmaktadır. (Halbuki örneğin C ve C++ standartlarında her 
ne kadar tüm derleyiciler ikiye tümleyen aritmetiğini kullanıyor olsa da işaretli tamsayıların ikiye tümleyen aritmetiği 
ile tutulacağı konusunda bir garanti verilmemiştir.) Rust'ta tamsayı türlerinin isimleri oldukça kısa bir biçimde oluşturulmuştur. 
*N* bit sayısını belirtmek üzere işaretli tamsayı türlerinin isimleri *iN* biçiminde, işaretsiz tamsayı türlerinin isimleri de 
*uN* biçimindedir. Rust'ın tüm tamsayı türlerinin isimleri aşağıda verilmiştir:

**İşaretli Tamsayı Türleri**

- ``i8``
- ``i16``
- ``i32``
- ``i64``
- ``i128``

**İşaretsiz Tamsayı Türleri**

- ``u8``
- ``u16``
- ``u32``
- ``u64``
- ``u128``

**Makine Bağımlı Tamsayı Türleri**

- ``isize``
- ``usize``

Anımsanacağı gibi 2'ye tümleyen aritmetiğinde N bitlik bir işaretli tamsayının sınır değerleri [-2\ :sup:`N-1`, +2\ :sup:`N-1` - 1] 
aralığındadır. Rust'ta 128 bitlik tamsayı türünün de olduğuna dikkat ediniz. Henüz 128 bitlik işlemciler kullanılmıyor olsa da 
bazı durumlarda (özellikle bit işlemlerinde) 128 bitlik tamsayılar daha verimli kod yazımını sağlayabilmektedir. 
Aşağıda tansayı türlerinin sınır değerlerini bir tablo halinde veriyoruz:

.. list-table:: 
   :header-rows: 1

   * - Tür
     - Bit
     - En Küçük Değer
     - En Büyük Değer
   * - ``i8``
     - 8
     - ``-128``
     - ``127``
   * - ``i16`` 
     - 16
     - ``-32768``
     - ``32767``
   * - ``i32``
     - 32
     - ``-2147483648``
     - ``2147483647``
   * - ``i64``
     - 64
     - ``-9223372036854775808``
     - ``9223372036854775807``
   * - ``i128``
     - 128
     - ``-170141183460469231731687303715884105728``
     - ``170141183460469231731687303715884105727``
   * - ``u8``
     - 8
     - ``0``
     - ``255``
   * - ``u16``
     - 16
     - ``0``
     - ``65535``
   * - ``u32``
     - 32
     - ``0``
     - ``4294967295``
   * - ``u64``
     - 64
     - ``0``
     - ``18446744073709551615``
   * - ``u128``
     - 128
     - ``0``
     - ``340282366920938463463374607431768211455``
   * - ``isize``
     - 32/64
     - ``i32`` ya da ``i64`` ile aynı
     - ``i32`` ya da ``i64`` ile aynı
   * - ``usize``
     - 32/64
     - ``0``
     - ``u32`` ya da ``u64`` ile aynı

Rust'ta ayrıca çalışılan sisteme bağlı uzunlukta olan isize ve usize isimli iki tamsayı türü de vardır. Bu tamsayı
türleri çalışılan sistemdeki teorik bellek büyüklüğüne hizalanmış olan bir uzunluğa sahiptir. Dolayısıyla bu türler 32
bit sistemlerde 32 bit (4 byte), 64 bit sistemlerde 64 bit (8 byte) uzunluğundadır. Bugün artık 16 bit işlemcilerin
kullanımı oldukça azalmıştır. Ancak 16 bit işlemcilerin kullanıldığı sistemlerde bu türler 16 bit uzunluğunda olacaktır.
*The Rust Reference* dokümanlarına göre bu tür ilgili sistemdeki göstericilerin bit uzunluğu kadar olmak zorundadır.
usize türünü C'deki size_t türüne benzetebilirsiniz. C'de de size_t türünün ilgili sistemdeki teorik bellek büyüklüğüne
göre typedef edildiğini anımsayınız. Böylece sistem programcısı farklı işlemcilerin kullanıldığı sistemlerde daha
taşınabilir kodlar yazabilmektedir. İki adres bilgisini çıkardığımızda sonuç negatif de olabilir pozitif de olabilir.
İşte isize türü iki adres farkını tutabilen usize türünün işaretli biçimidir. Pek çok işletim sisteminde proseslerin
sanal bellek alanları isize büyüklüğündedir. usize ve isize türleri aynı bit uzunluğuna sahiptir. Ayrıca *The Rust
Reference* dokümanları bir yapı değişkeninin ya da dizinin bellekte kapladığı alanın en fazla isize kadar olabileceğini
söylemektedir. Yukarıda da belirttiğimiz gibi bugün yaygın sistemlerde usize ve isize ya 64 ya da 32 bit uzunluğundadır.
Yukarıda da belirttiğimiz gibi 16 bit işlemciler artık çok seyrek kullanılmaktadır. 128 bit işlemciler ise henüz
kullanıma girmemiştir. Şimdilik 128 bitlik işlemcilere gereksinim duyulmamaktadır.

Gerçek Sayı Türleri
===================

Rust'ta iki gerçek sayı türü vardır. Bunlar f32 ve f64 türleridir. *The Rust Reference* dokümanlarında bu türlerin IEEE
754 formatlarına sahip olduğu belirtilmektedir. Bugün artık matematik işlemciler ve emülasyon araçları hep bu formatı
kullanmaktadır. C Programlama Dilinin standartlarında açıkça bu formatın kullanılacağı söylenmemiştir. Oysa Rust'ta bu
durum garanti edilmektedir. Rust'ın f32 türü C'nin float türüyle, Rust'ın f64 türü C'nin double türüyle örtüşmektedir.
C'de long double isminde bir gerçek sayı türünün daha olduğunu anımsayınız. C'nin long double pek çok derleyicide double
türüyle aynı uzunluktadır. (Tabii aslında C'de float türünün 4 byte double türünün 8 olacağının da bir garantisi de
yoktur. Ancak yaygın derleyicilerin hepsinde durum böyledir.)

bool Türü
=========

Rust'ta pek çok programlama dilinde olduğu gibi true ve false değerlerinin tutulduğu bool isminde bir tür de vardır.
Rust'ta bool türünün 1 byte yer kaplayacağı garanti edilmiştir. (C ve C++'ın böyle bir garanti vermediğini anımsayınız.)
Ayrıca false değerinin bitsel olarak 0 ve true değerinin de bitsel olarak 1 olduğu da *The Rust Reference*
dokümanlarında açıkça belirtilmiştir. C ve C++ standartlarında böyle bir belirlemede bulunulmamıştır.

Yazısal Türler 
==============

Rust'ta iki yazısal tür (textual type) vardır:

* char
* str

char türü bir karakterin Unicode UTF-32 kod numarasını (code point) tutmaktadır. Dolayısıyla char türü 4 byte
uzunluğundadır. Biz char türden bir değişkene tek tırnak içerisine aldığımız bir Unicode karakteri atayabiliriz.
Buradaki *Unicode UTF-32* teriminin anlaşılmasını sağlamak için karakter tablolama sistemleri hakkında bazı temel
bilgiler üzerinde durmak istiyoruz.

Karakterlerin bilgisayardaki temsili için oluşturulan tablolara *karakter tabloları (character sets)* denilmektedir.
Dünyanın bilgisayar alanında kullanılan ilk karakter tablosu ASCII (American Standard Code Information Interchange)
denilen tablodur. Bu tablo 60'ların başlarında tasarlanmaya başlamış ve 1963 yılında ANSI (American National Standard
Institute) tarafından standardize edilmiştir. ASCII tablosunun dışında EBCDIC gibi WISCII gibi başka karakter tabloları
da geliştirilmiştir. Bunlar arasında en yaygın kullanılanı ASCII tablosu olmuştur. ASCII tablosu 7 bitlik bir tabloydu.
Yani 128 karakter içeriyordu. Sonra zamanla 8'inci bit değişik ülkeler tarafından değişik biçimlerde organize edildi ve
*code page* varyasyonları ortaya çıkmaya başladı. Örneğin bugün Türkçe için ilk yarısı standart ASCII tablosunun aynısı
olan ikinci yarısı değişik düzenlenmiş olan üç farklı code page varyasyonu bulunmaktadır: IBM 857 (eski DOS
sistemlerinde kullanılıyordu), Microsoft 1254 (Windows'ta kullanılıyor) ve ISO 8859-9 (ISO standardı, Microsoft 1254'e
çok benziyor). Bu karakter tablolarının hepsi temelde 8 bit tablolardır. Yani en fazla 256 farklı karakteri
barındırabilmektedir. Eskiden belleklerin çok kıt olduğu dönemlerde karakterlerin az bitle ifade edilmesi anlamlıydı.
Ancak bellekler bollaşınca artık bunun önemi de kalmamıştır. 8 bitlik code page'ler dünyanın tüm dillerini ve çeşitli
işaret dillerini temsil edememektedir. Ayrıca farklı dillerdeki yazıların aynı doküman içerisinde tutulduğu durumlarda 8
bitlik code page'ler sorunlara yol açmaktadır. İşte son 30 yıldır her bir karakterin kabaca 1 byte ile değil 2 byte ile
kodlandığı ismine *Unicode* denilen bir tablo oluşturulmuştur. Unicode tablo zamanla programlama dillerini de etkisi
altına almış ve diller de bu tabloyu destekleyecek biçimde evrim geçirmişlerdir. Unicode karakter tablosu ISO tarafından
*ISO/IEC 10646* kod numarasıyla da standardize edilmiştir. Unicode tablodaki karakterlerin kod numaraları (code points)
için ilk zamanlar 2 byte yeterli oluyordu. Ancak zamanla Unicode'un çeşitli versiyonlarında tabloya karakterler eklendi.
Bugün Unicode tabloda yaklaşık 150,000 karakter bulunmaktadır. Yani artık Unicode karakterler için 2 byte da
yetmemektedir.

Karakter tablolarını anlayabilmek için bazı terimlerin anlamlarının bilinmesi gerekir. Bir karakter tablosundaki
karakterlerin görüntülerine İngilizce *glyph* denilmektedir. Karakter tablosundaki her karaktere 0'dan başlanarak bir
artan sırada bir sayı karşılık getirilir. Karakterin bu sıra numaralarına *kod numarası (code point)* denilmektedir.
Örneğin ASCII tablosunda 'a' karakterinin kod numarası 97'dir. Bir karakter tablosundaki kod numaralarının 2'lik
sistemde nasıl ifade edileceğine de *karakter kodlaması (character encoding)* denilmektedir.

Unicode tablonun ilk 128 kod numarasına ilişkin karakterleri standart ASCII karakterleriyle aynıdır. Unicode tablonun
128 ile 255 arasındaki kod numaralarına ilişkin karakterler de ASCII Latin-1 (ISO 8859-1) code page'i ile aynıdır.
Türkçe özel karakterlerin Unicode tablodaki kod numaraları 200'lü ve 300'lü sıralardadır ve bunların sıraları Türk
alfabesindeki sıra ile uyumlu değildir.

Unicode tablonun üç temel encoding'i bulunmaktadır: UTF-8, UTF-16 ve UTF-32. Unicode'un UTF-8 kodlaması multibyte bir
kodlamadır. Bu kodlamada çok kullanılan karakterler az byte ile az kullanılan karakterler çok byte ile kodlanmaktadır.
Örneğin standart ASCII karakterlerine ilişkin kod numaralarının hepsi UTF-8 kodlamasında 1 byte ile, Türkçe karakterler
2 byte ile, Japonca ve Çince'deki kanji karakterleri ise 3 byte ile ve çok az kullanılan karakterler de 4 byte ile
kodlanmaktadır. Bu nedenle Unicode UTF-8 kodlanmış bir yazının byte sayısına bakarak onun kaç karakter içerdiği
anlaşılamaz. Unicode UTF-16 kodlamasında her karakter 2 byte ile kodlanır. Eskiden Unicode tablonun tüm karakterleri
böyle 2 byte ile kodlanabiliyordu. Ancak Unicode tablo büyüyünce artık bazı karakterler UTF-16 encoding'inde iki ayrı
iki byte ile kodlanmaktadır. Zaten tüm dillerin karakterlerinin kod numaraları 65535 içerisindedir. İşte UTF-32 de her
karakterin sabit 4 byte ile kodlandığı encoding'tir. Yukarıda Unicode tablonun ilk 128 karakterinin kod numaraları ile
ASCII tablosundaki karakterlerin kod numaralarının aynı olduğunu belirtmiştik. Bu durumda standart ASCII ile kodlanmış
bir yazı ile Unicode UTF-8 kodlanmış bir yazı arasında bir farkın kalmadığına dikkat ediniz.

Java ve C# dillerindeki char türü Unicode UTF-16 encoding'ini kullanmaktadır. Zaten bu diller Unicode tablonun daha
büyük kod numaralarını doğrudan desteklememektedir. Python'daki CPython yorumlayıcısı da Unicode UTF-16 encoding'ini
kullanmaktadır. C++'a UTF-8, UTF-16 ve UTF-32 desteği C++11 standartları ile, C'ye de C11 standartları ile eklenmiştir.

İşte Rust'ta char türü en genel Unicode encoding'i olan UTF-32 encoding'ini kullanmaktadır. Bu nedenle char türü Rust'ta
4 byte uzunluktadır.

Rust'ta str türü iki tırnak içerisindeki string'leri temsil etmektedir. Bu türe Rust'ta *string dilimi (string slice)*
de denilmektedir. Ancak bu tür genel olarak referanslı bir biçimde yani &str biçiminde kullanılır. &str türüne ise
*string dilimi referansı (string slice reference)* denilmektedir. Biz Rust'ta iki tırnak içerisindeki yazıları str
türünden değişkenlere değil &str türünden değişkenlere atayabiliriz. Referans kavramının ve dilim kavramının Rust'ta ne
anlam ifade ettiği kursumuzun ilerleyen bölümlerinde ayrıntılarıyla ele alınacaktır. Burada şimdiden bir kavramı da
açıklamak istiyoruz. Rust'ta bazı türlere *dinamik uzunluğa sahip türler (dynamically sized types)* ya da kısaca
*uzunluğu bilinmeyen türler (unsized types)* denilmektedir. Dinamik uzunluğa sahip türler türünden değişkenler
bildirilememekte ancak referanslar bildirilebilmektedir.

Rust'ta iki tırnak içerisindeki yazılar Unicode UTF-8 encoding'iyle tutulmaktadır. Yukarıda da belirttiğimiz gibi
Unicode UTF-8 encoding'i ile kodlanmış yazıların karakter uzunlukları yazının kapladığı byte uzunluğuna bakarak
anlaşılamamaktadır.

Rust'ta iki tırnak içerisindeki yazıların sonunda C'de ve C++'ta olduğu gibi bir null karakter (``'\0'`` karakteri)
bulunmamaktadır. Bu nedenle Rust'ta yazının sonu özel bir karakterle değil yazının uzunluğundan hareketle
belirlenmektedir.

never Türü
==========

Rust'ta ! ile temsil edilen ismine *never türü* denilen bir tür de vardır. Bu never türü *akışın o noktadan devam
etmeyeceği* anlamına gelmektedir. Never türü özellikle fonksiyonların geri dönüş değerlerinde kullanılmaktadır.

