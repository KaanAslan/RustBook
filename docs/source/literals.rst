========
Sabitler
========

Statik tür sistemine sahip olan programlama dillerinde yalnızca değişkenlerin değil sabitlerin de (genel olarak tüm
ifadelerin (expression) de) bir türü vardır. Sabitlerin türleri onların yazım biçimlerine göre ve niceliklerine göre
(yani değerlerine göre) değişebilmektedir. Bir programlama dilini öğrenirken ilk öğrenilecek konulardan birisi de o
programlama dilindeki sabitlerin nasıl oluşturulduğu ve türlerinin nasıl belirlendiğidir. Biz de bu bölümde Rust
dilindeki sabitlerin nasıl oluşturulduğunu ve türlerinin nasıl belirlendiğini ele alacağız.

Program içerisinde doğrudan yazılmış olan değerlere *sabit (literal)* denilmektedir. Örneğin:

.. code-block:: rust

   a = b + 10;

Burada a ve b birer değişken 10 ise bir sabittir. Biz burada b'nin içerisindeki değerle doğrudan 10'u topluyoruz.

Katı Tür Kontrolü
=================

Rust katı tür kontrolüne (strict type checking) sahip bir programlama dilidir. Atama sırasında kaynak değerin türüyle
hedef değerin türünün aynı olması gerekir. Örneğin:

.. code-block:: rust

   let a: i8 = 10;
   let b: i32;

   b = a;      // error!

Burada her ne kadar i8 türündeki her değer i32 türüyle temsil edilebiliyorsa da başka bir deyişle atamada bilgi kaybının
oluşması söz konusu değilse de bu atama error ile sonuçlanacaktır. Çünkü kaynak türle hedef tür birbirinden farklıdır.
Yukarıdaki gibi atamaların C, Java ve C# gibi programlama dillerinde tamamen normal ve geçerli olduğunu anımsayınız.

Rust'ta iki operand'lı operatörlerde operand'ların türleri de aynı olmak zorundadır. Örneğin:

.. code-block:: rust

   let a: i8 = 10;
   let b: i32 = 20;
   let c: i32;

   c = a + b;      // error!

============================
Sabitlerin Yazımı ve Türleri
============================

Tamsayı Sabitleri ve Türleri
============================

Rust'ta nokta içermeyen ve sonunda tür eki bulunmayan sabitler onların kullanıldığı bağlama göre gereksinim duyulan
türün sınırları içerisinde kalıyorsa o türden kabul edilmektedir. Örneğin:

.. code-block:: rust

   let a: i8 = 127;        // geçerli

Burada 127 sabitinin sonunda bir tür eki yoktur. i8 türünün sınır değerlerinin [-128, +127] olduğuna dikkat ediniz. İşte
burada 127 değeri bu sınırlar içerisinde olduğu için sanki i8 türündenmiş gibi ele alınmaktadır. Örneğin:

.. code-block:: rust

   let a: u16 = 50000;

Burada 50000 değeri içinde bulunulan bağlamda u16 türüne atanmak amacıyla kullanılmıştır. 50000 değeri u16 sınırları
içerisinde kaldığı için sanki u16 türünden bir sabitmiş gibi ele alınmaktadır. Örneğin:

.. code-block:: rust

   let a: i8 = 10;
   let b: i8;

   b = a + 10;

İki operand'lı operatörlerin operand'larının aynı türden olması gerektiğini belirtmiştik. Buradaki a + 10 işleminde 10
sanki i8 sabit gibi ele alınacaktır. Çünkü 10 sayısı i8 türünün sınırları içerisinde kalmaktadır. Örneğin:

.. code-block:: rust

   let a: u16;

   a = 70000;      // error!

Burada 70000 değeri u16 sınırlarının içerisinde kalmadığı için u16 türünden bir sabit gibi ele alınmayacaktır.

Ancak let ile bağlama yapılırken verilen ilkdeğer tür eki içermeyen bir tamsayı ise bu durumda sabit her zaman i32
türünden kabul edilmektedir. Örneğin:

.. code-block:: rust

   let a = 3000000000;         // error!

Burada 3 milyar değeri i32 türünün sınırları dışındadır. Dolayısıyla error oluşacaktır.

Basamak Ayıracı Olarak '_' Karakteri
====================================

Rust'ta tıpkı Python'da olduğu gibi büyük sayıların daha kolay algılanabilmesi için basamaklar ``'_'`` karakteriyle
ayrıştırılabilmektedir. Örneğin:

.. code-block:: rust

   let a: u64 = 3_000_000_000;     // geçerli

Burada basamakları ayıran ``'_'`` karakterinin sayı üzerinde bir etkisi yoktur. Tabii ``'_'`` karakteri ile üçerli
ayrıştırma yapmak zorunlu değildir. Ayrıştırma herhangi bir basamakta bir ya da birden fazla ``'_'`` karakteri
kullanılarak yapılabilir. Örneğin:

.. code-block:: rust

   a = 3_0_0_0_0_0_0_0_0_0;        // geçerli

Sayının sonunda da ``'_'`` karakteri bulunabilir, ancak başında bulunamaz. Yukarıda da belirttiğimiz gibi birden fazla
``'_'`` karakterinin peşi sıra kullanılmasına da izin verilmiştir. Örneğin:

.. code-block:: rust

   a = 3___0_0_0_0_0_0_0_0_0__;        // geçerli

Tabii bu ``'_'`` karakterlerinin amacı okunabilirliği artırmak olduğu için bunlar 10'luk sistemde tipik olarak binli
basamakları ayrıştırmak için kullanılmaktadır.

Basamak ayıraçları eski dillerin yeni sürümlerine de zamanla eklenmiştir. Örneğin C++'a C++14 ile C'ye de C23 ile bu
özellik eklenmiştir.

Tamsayı Sabitlerinde Tür Sonekleri
==================================

Tamsayı sabitlerinin sonlarına onların türlerini belirten ekler getirilebilmektedir. Bu sonekler tamsayı türlerinin
isimlerinden oluşmaktadır:

.. list-table::
   :widths: 10 10

   * - ``i8``
     - ``u8``
   * - ``i16``
     - ``u16``
   * - ``i32``
     - ``u32``
   * - ``i64``
     - ``u64``
   * - ``i128``
     - ``u128``

Biz tamsayı sabitlerinin sonuna yapışık bir biçimde bu sonekleri getirirsek artık sabitler kesinlikle bu türlerden olur.
Örneğin:

.. code-block:: text

   123i32
   3000000000u32
   10i8
   20u64

Örneğin:

.. code-block:: rust

   let a: u64;

   a = 10u64;

Burada 10 sabitinin açıkça u64 türünden olduğu belirtilmiştir. Örneğin:

.. code-block:: rust

   let a = 10;

Burada a değişkeni i32 türündendir. Fakat örneğin:

.. code-block:: rust

   let a = 10u32;

Burada ise a değişkeni u32 türündendir.

Basamakları ayrıştırmak için kullanılan ``'_'`` karakterlerinin sayının sonunda da bulunabileceğini belirtmiştik. Tür
soneklerinin okunabilirliği bozmasını engellemek amacıyla soneklerden önce ``'_'`` karakteri de kullanılabilir. Örneğin:

.. code-block:: rust

   let a = 10_u32;

Bazı Rust programcıları soneklerden önce ``'_'`` karakterini kullanmaktadır. Bazı programcılar ise yalnızca büyük
sabitlerde kullanmayı tercih etmektedir.

2'lik, 8'lik ve 16'lık Sistemde Tamsayı Sabitleri
=================================================

Tamsayı sabitleri 2'lik (binary) sistemde, 8'lik (octal) sistemde ve 16'lık (hexadecimal) sistemde yazılabilmektedir.
Bunun için sabitin önüne 2'lik sistem için 0b, 8'lik sistem için 0o ve 16'lık sistem için 0x önekleri getirilmektedir.
(Bu öneklerin 0B, 0O ve 0X biçiminde büyük harfli biçimleri yoktur.) Örneğin aşağıdaki sabitler geçerlidir:

.. code-block:: text

   0b1010
   0b_1_0_1_0_
   0b1010_1010
   0o712_564
   0xFC_16_FD_32

C'de 0o biçiminde bir önek bulunmadığını sayının başındaki 0'ın 8'lik sistem anlamına geldiğini anımsayınız. Oysa
Rust'ta sayının başındaki 0 rakamı 8'lik sistem anlamına gelmemektedir. 8'lik sistem için zaten 0o kullanılmaktadır.
C'de 2'lik sistemde sabit belirtilemiyordu. Ancak C'nin en yeni standardı olan C23 sürümünde bu özellik eklenmiştir.

Sabitin kaçlık sistemde yazıldığı ile tür arasında bir ilişki yoktur. Örneğin:

.. code-block:: rust

   let a = 0x1234;

Buradaki 0x1234 değeri i32 türünün sınırları içerisindedir. Bu nedenle a da i32 türündendir. Örneğin:

.. code-block:: rust

   let a = 0x123456789;        // error!

Buradaki sayı i32 türünün sınırları dışındadır. Dolayısıyla bu bağlama error oluşturacaktır. Örneğin:

.. code-block:: rust

   let a: i8 = 100;
   let b: i8;

   b = a + 0x10;           // geçerli

Buradaki 0x10 aslında 10'luk sistemdeki 16 sayısıdır. Dolayısıyla bu bağlamda bu sayı i8 türünün sınırları içerisinde
olduğu için i8 sabit olarak ele alınacaktır.

Tabii biz 2'lik, 8'lik ve 16'lık sistemlerde yazdığımız sabitlerin sonuna da yine tür ekleri getirebiliriz. Örneğin:

.. code-block:: rust

   let a = 0x123456789u64;

Burada artık sabit u64 türündendir. Tabii basamak ayıraçları yine bu sistemlerde yazılan sayılarda da kullanılabilir.
Örneğin 2'lik sistemdeki sayıların dörderli biçimde gruplanması bir gelenektir:

.. code-block:: rust

   let a = 0b_1010_0101;

Burada a i32 türündendir.

Gerçek Sayı Sabitleri
=====================

Rust'ta sayı nokta içeriyorsa ve sabitin sonunda bir tür eki yoksa ve bu biçimde yazılan sabitler onların kullanıldığı
bağlama göre gereksinim duyulan türün sınırları içerisinde kalıyorsa o türden kabul edilmektedir. Örneğin:

.. code-block:: rust

   let f: f32 = 3.14;       // geçerli

Burada 3.14 sabiti f32 türü ile ifade edilebilmektedir. Bu nedenle sanki f32 türünden bir sabitmiş gibi
kullanılabilmiştir. Örneğin:

.. code-block:: rust

   let a: f32 = 12.3;
   let b: f32;

   b = a + 1.2;            // geçerli

Burada 1.2 bağlam içerisinde f32 türünden bir sabit olarak ele alınacaktır. Çünkü 1.2 sayısı f32 türü ile ifade
edilebilmektedir. Ancak bu durum tamsayılar için geçerli değildir. Örneğin:

.. code-block:: rust

   let a: f32 = 10;        // error

Burada her ne kadar 10 sabiti f32 türü ile ifade edilebiliyorsa da tamsayı sabiti olduğu için bağlama geçersizdir.
Noktalı bir sayı eğer f32 türünün basamaksal olarak (duyarlılık olarak değil) sınırları dışında kalırsa bu durum derleme
aşamasında error oluşturmaktadır. Örneğin:

.. code-block:: rust

   let a: f32 = 1e50;      // error!

Burada 1e50 sabiti basamaksal olarak f32 türünün sınırları içerisinde kalmamaktadır. Bu nedenle bu sabit ataması error
ile sonuçlanacaktır. Eğer noktalı sayı basamaksal değil de mantis bakımından f32 türü ile ifade edilemiyorsa bu durum
error oluşturmaz. f32 ile ifade edilebilen ona en yakın değer elde edilir. (Bu durumda f32 türüne dönüştürme kuralı
uygulanmaktadır. Bu dönüştürme kuralını tür dönüştürme operatörünü anlatırken ele alacağız.) Diğer pek çok dilde olduğu
gibi '.' karakterinin sağına bir şey yazılmazsa sabit sanki .0 gibi ele alınmaktadır. Örneğin:

.. code-block:: rust

   let a: f64 = 1.;        // geçerli

Ancak gerçek sayı sabitlerinde noktanın solu boş bırakılamaz. Örneğin:

.. code-block:: rust

   let a: f64 = .10;       // error!

Tabii noktanın solundaki ve sağındaki sayılarda istenildiği kadar ``'_'`` ile ayrıştırma yapılabilir. Örneğin:

.. code-block:: rust

   let a: f64 = 123_456.789_012;       // geçerli

Ancak noktadan hemen sonra ``'_'`` ayrıştırma karakteri kullanılamamaktadır. Örneğin:

.. code-block:: rust

   let a: f64 = 1._234;       // error!

let deyiminde değişkenin türü belirtilmezse ve verilen ilkdeğer sonek içermeyen bir noktalı sayı ise bu durumda sabit
f64 türünden kabul edilir, değişken de o türden olur. Örneğin:

.. code-block:: rust

   let a = 3.14;

Burada a değişkeni f64 türündendir.

Gerçek Sayı Sabitlerinde Tür Sonekleri
======================================

Gerçek sayı sabitlerinin sonuna da onların türünü belirten tür soneki getirilebilmektedir. Bunun için aşağıdaki iki tür
soneki kullanılmaktadır:

* f32
* f64

Örneğin:

.. code-block:: rust

   let a = 3.14f32;

Burada a değişkeni artık f32 türünden olacaktır. Halbuki örneğin:

.. code-block:: rust

   let a = 3.14;

Burada a değişkeni f64 türünden olur. Tabii yine tür sonekinden önce okunabilirliği artırmak için ``'_'`` ile ayrıştırma
tercih edilebilir. Örneğin:

.. code-block:: rust

   let a = 3.14_f32;

Gerçek sayı sabitlerinde tür soneki noktadan hemen sonra başlatılamaz. Örneğin:

.. code-block:: rust

   let a: f64 = 1.f64;         // error!
   let b: f64 = 1._f64;        // error!

Gerçek sayı sabitlerinde açıkça tür belirtmek iyi bir tekniktir. Gerçek sayı sabitine f32 ve f64 soneki getirmiş olsak
bile eğer sabit basamaksal olarak soneke ilişkin türün sınırları dışında kalıyorsa sabit geçersiz olmaktadır. Örneğin:

.. code-block:: rust

   let a: f32 = 1e50;      // error! sabit f32 sınırlarını basamaksal olarak aşıyor

C'de gerçek sayı soneklerini (f ve lf) kullanabilmek için sabitin nokta içermesi gerekmektedir. Rust'ta sabit nokta
içermese de sabitin sonuna f32 ve f64 sonekleri getirilebilmektedir. Örneğin:

.. code-block:: rust

   let f: f64 = 1f64;       // geçerli

Üstel Biçimde Gerçek Sayı Sabitleri
===================================

Pek çok programlama dilinde olduğu gibi Rust'ta da gerçek sayı sabitleri (f32 ve f64 türünden sabitler) üstel biçimde
belirtilebilmektedir. Gerçek sayı sabitleri üstel biçimde belirtilirken e ya da E harfi 10'un kuvveti anlamına
gelmektedir. Örneğin:

.. code-block:: rust

   let f = 1.23e12;

Bu sabit ``1.23 * 10^12`` anlamına gelmektedir. Örneğin:

.. code-block:: rust

   let f = 1.23e-12;

Bu sabit ``1.23 * 10^-12`` anlamına gelmektedir. Gerçek sayı sabitleri üstel biçimde belirtilirken yine basamak ayıracı
olarak ``_`` karakteri kullanılabilmektedir. Aşağıdaki sabitte her ne kadar basamak ayıraçlarının kullanılması
gereksizse de sabit geçerlidir:

.. code-block:: rust

   let f = 1_.2_3E_10;

Diğer pek çok dilde olduğu gibi gerçek sayı sabitleri üstel formatta yazılırken nokta içermese bile gerçek sayı sabiti
olarak ele alınmaktadır. Örneğin:

.. code-block:: rust

   let f: f64 = 1e5;       // geçerli
   let f: i64 = 1e5;       // error!

bool Sabitleri
==============

Rust'ta bool türden iki sabit vardır. Bunlar true ve false anahtar sözcükleridir. Örneğin:

.. code-block:: rust

   let b = true;

Burada b değişkeni bool türdendir.

Karakter Sabitleri
==================

Bir Unicode karakter tek tırnak içerisine alınırsa char türden sabit belirtir. Örneğin:

.. code-block:: rust

   let c = 'x';

Burada c değişkeni char türdendir. Rust'ta da diğer bazı dillerde olduğu gibi tek tırnağın içerisinde ters bölü karakter
sabitleri kullanılabilmektedir. Ancak Rust'ta biz C'deki tüm ters bölü karakterlerini kullanamamaktayız. Rust'ta
kullanabileceğimiz ters bölü karakterleri şunlardır:

.. code-block:: text

   \n
   \r
   \t
   \\
   \'
   \"
   \0

Diğer dillerde olduğu gibi tek tırnak içerisinde Unicode kod numarası belirtilerek de herhangi bir Unicode karakterden
karakter sabiti oluşturulabilmektedir. Bunun için tek tırnak içerisinde önce ``\u`` sonra da küme parantezleri
içerisinde Unicode kod numarası girilmelidir. Örneğin:

.. code-block:: rust

   let c = '\u{6728}';

   println!("{}", c);      // 木

==================================
Byte Sabitleri ve String Sabitleri
==================================

Byte Sabitleri (Byte Literals)
==============================

Tek tırnak içerisine bir ASCII karakteri yerleştirilip başına da bununla yapışık b karakteri getirilirse bu biçimdeki
sabitlere Rust'ta *byte sabitleri (byte literals)* denilmektedir. Byte sabitleri ``u8`` türündendir. Byte sabitleri tek
tırnak içerisindeki ASCII karakterinin kod numarasına ilişkin bir sayı belirtirler. Örneğin:

.. code-block:: rust

   let b: u8;

   b = b'a';       // geçerli, b'a' u8 türünden

Burada ``b'a'`` sabiti bir byte sabitidir. Byte sabitler ``u8`` türünden kabul edildikleri için biz onları ancak ``u8`` türünden bir
değişkene atayabiliriz. Byte sabitlerinde tek tırnak içerisine yalnızca standart ASCII karakterlerinden biri
yerleştirilebilir. Yani tek tırnak içine yerleştirilecek karakterin Unicode kod numarasının [0, 127] aralığında olması
gerekir. Örneğin:

.. code-block:: rust

   let b: u8 = b'ş';                // error!

Burada ``'ş'`` karakteri bir ASCII karakter olmadığı için byte sabit geçersiz bir biçimde oluşturulmuştur. Tabii Rust'ın
yukarıda belirtmiş olduğumuz ters bölü karakter sabitleri aynı zamanda ASCII karakterleri olduğu için byte sabit olarak
kullanılabilir. Örneğin:

.. code-block:: rust

   let b: u8 = b'\n';               // geçerli

Byte sabitleri oluştururken tek tırnak içerisine ``\u {...}`` biçiminde Unicode kod numarası belirtilerek karakterler
yerleştirilemez. Örneğin:

.. code-block:: rust

   let b: u8 = b'\u{61}';           // error!

``'a'`` gibi bir sabitin char türden olduğuna ama ``b'a'`` biçiminde bir sabitin ``u8`` türünden olduğuna dikkat ediniz.

Aşağıdaki bağlamaya dikkat ediniz:

.. code-block:: rust

   let b: u8 = b'a';

Burada aslında b'nin içerisinde 97 sayısı vardır. Biz b'yi yazdırırsak 97 görürüz. Örneğin:

.. code-block:: rust

   println!("{}", b);      // 97

Peki o halde yukarıdaki bağlamanın aşağıdakinden ne farkı vardır?

.. code-block:: rust

   let b: u8 = 97;

Aslında bu iki bağlama arasında işlevsel bir farklılık yoktur. Ancak byte sabitler özellikle standart ASCII
karakterlerin kod numaralarının oluşturulması için tercih edilmektedir. Yani bağlamına göre ``b'a'`` gibi bir sabit
amaçlanan şeyi daha iyi ifade edebilecektir.

Byte sabitlerini oluştururken tek tırnak içerisine yalnızca ASCII karakterlerinin yazılabileceğini belirtmiştik. Ancak
eğer istenirse [128, 255] arasındaki kod numaraları ``'\xHH'`` (Burada HH iki hex digit belirtmektedir) biçiminde de
belirtilebilir. Örneğin:

.. code-block:: rust

   let b: u8 = b'\xFC';        // geçerli

Byte sabitleri Python'da uzun süredir bulunmaktadır. Tabii Python'da bir byte'lık başka bir tür olmadığı için oradaki
byte türü ve byte sabitlerinin yeri başka biçimde doldurulamamaktadır. Python'da byte sabitlerin öneki ``'b'`` ya da ``'B'``
olabilmektedir. Halbuki Rust'ta bu önek yalnızca ``'b'`` biçimindedir.

String Sabitleri (String Literals)
==================================

Rust'ta iki tırnak içerisine yazılmış olan karakterlere iki tırnaklarıyla birlikte *string sabiti (string literal)*
denilmektedir. Daha önceden de belirttiğimiz gibi Rust iki tırnak içerisindeki karakterleri Unicode UTF-8 kodlamasıyla
tutmaktadır. Bu nedenle biz iki tırnak içerisinde tüm Unicode karakterleri, Rust'ın ters bölü karakterlerini ``\u{...}``
biçiminde Unicode kod numarasıyla belirttiğimiz karakterleri kullanabiliriz. Rust'ın stringlerinin sonuna null karakter
yerleştirilmediğini de daha önce belirtmiştik. C'den geçen kişiler özellikle şu iki noktaya dikkat etmelidirler:

1) Rust'taki string'lerin içerisindeki karakterler Unicode UTF-8 kodlamasıyla tutulmaktadır.
2) Rust'ta string'lerin sonunda null karakter yoktur.

C'de string'ler ifade içerisinde kullanıldığında ``char *`` türünden, C++'te ``const char *`` türünden kabul
edilmektedir. Java, C# ve Python gibi dillerde string'ler o dillerin ``String`` sınıfı türünden (Python'da ``str`` sınıfı
türünden) değişken referansı belirtmektedir. Örneğin Java ve C#'ta iki tırnak ifadeleri o dillerin ``String`` sınıfı
türünden bir değişkene atanabilmektedir.

Rust'ta iki tırnak içerisindeki string'ler ``&str`` türündendir. str türüne string dilimi (string slice) denilmektedir.
Ancak bu tür hemen hep referanslı bir biçimde kullanılır. Dolayısıyla Rust'ta biz bir string'i ``&str`` türünden bir
değişkene atayabiliriz. ``&str`` türüne *string dilim referansı* denilmektedir. Referanslar ve dilimler ilerideki bölümlerde
ele alınacaktır. Örneğin:

.. code-block:: rust

   let s: &str;

   s = "ağrı dağı";            // geçerli

Unicode UTF-8 kodlamasıyla oluşturulan bir yazının n'inci karakterine tek hamlede (tek makine komutuyla)
erişilememektedir. Çünkü bu kodlamada daha önce belirttiğimiz gibi her karakter farklı byte uzunluklarında
olabilmektedir. Dolayısıyla biz böyle bir string'in n'inci karakterini ancak string'in başından itibaren ilerleyerek
bulabiliriz. Programlama dillerinde ``[]`` operatörü rastgele erişimlerde (O(1) erişim de denilmektedir)
kullanılmaktadır. Bu nedenle Rust'ın string'lerinin karakterlerine bu operatörle erişilemez. Örneğin:

.. code-block:: rust

   let s: &str = "ağrı dağı";
   let c = s[4];               // error!

Rust string'lerinde ``s[4]`` biçiminde bir erişim yoktur.

Rust'ta string'ler tek satıra yazılmak zorunda değildir. Yani biz string'leri birden fazla satıra yazabiliriz. Tabii
istersek \\ karakteri ve hemen ardından ``\n`` karakteri ile (yani editörde \\ karakterine bastıktan sonra ENTER tuşuna
basıp) *aşağı satırdan devam et* diyerek de aynı şeyi yapabiliriz. Örneğin:

.. code-block:: rust

   let s: &str;

   s = "Ankara Türkiye'nin         // geçerli
   başkentidir";

Ancak örneğin:

.. code-block:: rust

   s = "Ankara Türkiye'nin \       // geçerli
   başkentidir";

\\ karakteri ile farklı satırların sanki tek bir satırmış gibi birleştirilmesi özelliği C'de ve diğer bazı dillerde de
bulunmaktadır.

Saf String Sabitleri (Raw String Literals)
==========================================

Rust'ta *saf string sabitleri (raw string literals)* adında string sabitleri de vardır. Aslında Rust'taki bu saf string
sabitleri farklı isimlerle pek çok programlama dilinde de bulunmaktadır. Örneğin C#'ta bu tür string'lere *verbatim
string*, Python'da *regular string* denilmektedir. C++'a da C++11 ile birlikte bu özellik eklenmiştir. Rust gibi
nispeten yeni tasarlanan dillerde de artık doğuştan itibaren bu biçimde string sabitleri bulunmaktadır.

Rust'ta saf string sabitleri (kısaca *saf string'ler* de diyebiliriz) içerisindeki ``'\'`` karakterleri gerçekten
``'\'`` karakteri anlamına gelmektedir. Yani bu string'ler içerisindeki ters bölü karakterleri ters bölü karakter sabiti
oluşturma anlamında ele alınmamaktadır. Örneğin:

.. code-block:: rust

   let s: &str = "ali\nveli";

Buradaki string'te ``\n`` tek bir karakterdir. Bu yazı ekrana yazdırıldığında bu ``\n`` karakteri imlecin aşağı satırın
başına geçirilmesine yol açacaktır. Eğer buradaki string saf string olsaydı bu ``\n`` karakterleri *new line* karakteri
yerine ayrı ayrı ``'\'`` karakteri ve 'n' karakteri anlamına gelecekti.

Rust'ta saf string'ler iki tırnağın başına onunla yapışık r öneki getirilerek oluşturulmaktadır. Örneğin:

.. code-block:: rust

   let s: &str = r"ali\nveli";

   println!("{}", s);          // ali\nveli

Saf string sabitleri ters bölü karakteri içeren yazıların daha kolay ve okunabilir bir biçimde oluşturulmasına olanak
sağlamaktadır. Örneğin:

.. code-block:: rust

   let path: &str = r"c:\temp\test.txt";

Eğer saf string sabitleri olmasaydı biz bu yazıyı şöyle oluşturmak zorunda kalırdık:

.. code-block:: rust

   let path: &str = "c:\\temp\\test.txt";

Peki biz iki tırnak içerisinde iki tırnak karakteri içeren bir yazıyı nasıl oluşturabiliriz? Bu Rust'ta diğer dillerde
olduğu gibi ``\"`` kullanılarak yapılabilmektedir. Örneğin:

.. code-block:: rust

   let s = "\"ankara\"";

   println!("{}",s);       // "ankara"

Tabii iki tırnak içerisinde tek tırnak karakteri kullanmanın, tek tırnak içerisinde de iki tırnak karakteri kullanmanın
bir sakıncası olmadığını biliyorsunuz.

Rust'ta saf string'ler ayrıca r önekinin yanında bir ya da birden fazla ``#`` ile de oluşturulabilmektedir. Ancak bu
durumda kapanış iki tırnak karakterinden sonra açılış iki tırnak karakterinin önündeki sayıda ``#`` karakterinin
bulundurulması gerekmektedir. Örneğin:

.. code-block:: rust

   let s: &str = r#"ankara"#;
   println!("{}", s);          // ankara

Burada r önekinden sonra iki tırnağın başına ve sonuna ``#`` karakterinin getirildiğine dikkat ediniz. ``#``'lerin
sayısı birden fazla olabilir ancak kapanışta da aynı sayıda ``#`` karakterinin bulundurulması gerekir. Örneğin:

.. code-block:: rust

   let s: &str = r###"ankara"###;
   println!("{}", s);      // ankara

Burada açılış iki tırnak karakterinin solunda üç tane ``#`` karakteri olduğu için sağında da üç tane ``#`` karakteri
bulunmak zorundadır. Peki saf string'lerde ``#`` karakterlerini bulundurmanın anlamı nedir? İşte bunun amacı iki tırnak
içerisindeki iki tırnak karakterlerinin kolay yazılmasını sağlamaktır. Örneğin biz JSON formatına benzer bir formatta
anahtar değer çiftlerini bir yazı biçiminde oluşturmak isteyelim. Ancak anahtarların da iki tırnaklı yazılardan
oluştuğunu varsayalım. Bu durumda aşağıdaki gibi bir string geçersiz olacaktır:

.. code-block:: rust

   let s: &str = "{"ali": 123, "veli": 234}";      // error!

Çünkü burada iki tırnak içerisindeki iki tırnak karakteri iki tırnağı kapatmak anlamına gelmektedir. Bu string'in başına
r öneki getirerek de bunu geçerli yapamayız:

.. code-block:: rust

   let s: &str = r"{"ali": 123, "veli": 234}";         // error!

Tabii bu string'i geçerli bir biçimde şöyle belirtebilirdik:

.. code-block:: rust

   let s: &str = "{\"ali\": 123, \"veli\": 234}";      // geçerli

Fakat bu durumda da yazı okunabilir olmaktan çıkmaktadır. İşte ``#``'li saf string'ler bu kolaylığı sağlamaktadır:

.. code-block:: rust

   let s: &str = r#"{"ali": 123, "veli": 234}"#;      // geçerli

Peki neden artık bu string geçerli olmuştur? Çünkü string ``#"`` ile başlatıldığı ve bitimi de ``"#`` biçiminde olacağı
için bunun içerisindeki çift tırnak karakteri artık string'i bitirmek anlamına gelmemektedir. Şimdi de aşağıdaki
string'e dikkat ediniz:

.. code-block:: rust

   let s: &str = r#"ben "#" karakterini çok seviyorum"#;      // error!

Bu string geçersizdir. Çünkü burada yazı içerisinde tesadüfen bulunan ``"#`` karakterleri saf string'i bitiren
karakterler olarak ele alınacaktır. İşte bu tür durumlarda ``#``'lerin sayısını artırabiliriz:

.. code-block:: rust

   let s: &str = r##"ben "#" karakterini çok seviyorum"##;     // geçerli

Burada artık string ``##"`` ile başlatıldığı için bitimi de ``"##`` ile yapılmaktadır.

Saf string'lerin farklı bir tür belirtmediğine yalnızca yazının içeriği konusunda etkili olduğuna dikkat ediniz. Normal
string'ler de saf string'ler de &str türündendir.

Byte String Sabitleri (Byte String Literals)
============================================

Rust'ta diğer bir string sabiti de *byte string sabiti (byte string literals)* denilen sabittir. Bu sabitlere kısaca
*byte string'ler* de denilmektedir. Byte string'ler iki tırnağın önünde ona yapışık bir biçimde b öneki ile
oluşturulmaktadır. byte string'ler ``u8`` türünden bir dizi dilimi (ya da kısaca dilim de diyebiliriz) belirtmektedir.
Dolayısıyla ``&[u8]`` türünden bir değişkene atanabilirler. Örneğin:

.. code-block:: rust

   let bs: &[u8] = b"ankara";

   println!("{:?}",bs);        // [97, 110, 107, 97, 114, 97]

Tabii byte string'lerin içerisindeki karakterlerin de yine byte sabitlerde olduğu gibi yalnızca ASCII karakterlerinden
oluşturulması gerekir. Örneğin:

.. code-block:: rust

   let bs: &[u8] = b"ağrı dağı";       // error!

Burada byte string'in içerisinde Türkçe karakterler bulundurulmuştur. Ancak byte sabitlerinde olduğu gibi byte string
sabitlerinde de [128, 255] arasındaki karakterler ``\xHH`` (Burada HH iki hex digit belirtmektedir) biçiminde ifade
edilebilir. Örneğin:

.. code-block:: rust

   let bs: &[u8] = b"abc\xFF\xFCdef";       // geçerli

Saf Byte String Sabitleri (Raw Byte String Literals)
====================================================

Byte string sabitleri de *saf (raw)* biçimde oluşturulabilmektedir. Bunlara *saf byte string sabitleri (raw byte string
literals)* denilmektedir. Bunun için iki tırnağa yapışık br öneki bulundurulmaktadır. Buradaki semantik saf string
sabitlerinde olduğu gibidir. Yani iki tırnağın içerisindeki ters bölü karakterleri ters bölü karakter sabitleri olarak
değil gerçekten ters bölü karakterleri olarak ele alınmaktadır. Örneğin:

.. code-block:: rust

   let bs: &[u8] = br"a\nb";

Burada ``\n`` *new line* karakterini değil \\ ve n karakterlerini belirtmektedir. Dolayısıyla burada byte string'i 3
uzunlukta değil 4 uzunluktadır.

Yine saf byte string sabitlerinde br önekinden sonra bunlara yapışık bir ya da birden fazla ``#`` karakteri
bulundurulabilir. Saf string sabitlerinde olduğu gibi bu da iki tırnak içerisinde iki tırnak karakterinin daha kolay
kullanılmasını sağlamaktadır. Örneğin:

.. code-block:: rust

   let bs: &[u8] = br#"ankara""#;
   let bs: &[u8] = br##""#"##;

