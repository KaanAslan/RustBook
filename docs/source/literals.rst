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

Burada ise a değişkeni ``u32`` türündendir.

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

Buradaki ``0x10`` aslında 10'luk sistemdeki 16 sayısıdır. Dolayısıyla bu bağlamda bu sayı ``i8`` türünün sınırları
içerisinde olduğu için ``i8`` sabit olarak ele alınacaktır.

Tabii biz 2'lik, 8'lik ve 16'lık sistemlerde yazdığımız sabitlerin sonuna da yine tür ekleri getirebiliriz. Örneğin:

.. code-block:: rust

   let a = 0x123456789u64;

Burada artık sabit u64 türündendir. Tabii basamak ayıraçları yine bu sistemlerde yazılan sayılarda da kullanılabilir.
Örneğin 2'lik sistemdeki sayıların dörderli biçimde gruplanması bir gelenektir:

.. code-block:: rust

   let a = 0b_1010_0101;

Burada ``a`` ``i32`` türündendir.

Gerçek Sayı Sabitleri
=====================

Rust'ta sayı nokta içeriyorsa ve sabitin sonunda bir tür eki yoksa ve bu biçimde yazılan sabitler onların kullanıldığı
bağlama göre gereksinim duyulan türün sınırları içerisinde kalıyorsa o türden kabul edilmektedir. Örneğin:

.. code-block:: rust

   let f: f32 = 3.14;       // geçerli

Burada ``3.14`` sabiti ``f32`` türü ile ifade edilebilmektedir. Bu nedenle sanki ``f32`` türünden bir sabitmiş gibi
kullanılabilmiştir. Örneğin:

.. code-block:: rust

   let a: f32 = 12.3;
   let b: f32;

   b = a + 1.2;            // geçerli

Burada ``1.2`` bağlam içerisinde ``f32`` türünden bir sabit olarak ele alınacaktır. Çünkü ``1.2`` sayısı ``f32`` türü
ile ifade edilebilmektedir. Ancak bu durum tamsayılar için geçerli değildir. Örneğin:

.. code-block:: rust

   let a: f32 = 10;        // error

Burada her ne kadar ``10`` sabiti ``f32`` türü ile ifade edilebiliyorsa da tamsayı sabiti olduğu için bağlama 
geçersizdir. Noktalı bir sayı eğer ``f32`` türünün basamaksal olarak (duyarlılık olarak değil) sınırları dışında 
kalırsa bu durum derleme aşamasında error oluşturmaktadır. Örneğin:

.. code-block:: rust

   let a: f32 = 1e50;      // error!

Burada ``1e50`` sabiti basamaksal olarak ``f32`` türünün sınırları içerisinde kalmamaktadır. Bu nedenle bu sabit ataması
error ile sonuçlanacaktır. Eğer noktalı sayı basamaksal değil de mantis bakımından ``f32`` türü ile ifade edilemiyorsa 
bu durum error oluşturmaz. ``f32`` ile ifade edilebilen ona en yakın değer elde edilir. (Bu durumda ``f32`` türüne 
dönüştürme kuralı uygulanmaktadır. Bu dönüştürme kuralını tür dönüştürme operatörünü anlatırken ele alacağız.) Diğer 
pek çok dilde olduğu gibi ``'.'`` karakterinin sağına bir şey yazılmazsa sabit sanki ``.0`` gibi ele alınmaktadır. 
Örneğin:

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

Burada a değişkeni artık ``f32`` türünden olacaktır. Halbuki örneğin:

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

Gerçek sayı sabitlerinde açıkça tür belirtmek iyi bir tekniktir. Gerçek sayı sabitine ``f32`` ve ``f64`` soneki getirmiş
olsak bile eğer sabit basamaksal olarak soneke ilişkin türün sınırları dışında kalıyorsa sabit geçersiz olmaktadır. 
Örneğin:

.. code-block:: rust

   let a: f32 = 1e50;      // error! sabit f32 sınırlarını basamaksal olarak aşıyor

C'de gerçek sayı soneklerini (f ve lf) kullanabilmek için sabitin nokta içermesi gerekmektedir. Rust'ta sabit nokta
içermese de sabitin sonuna ``f32`` ve ``f64`` sonekleri getirilebilmektedir. Örneğin:

.. code-block:: rust

   let f: f64 = 1f64;       // geçerli

Üstel Biçimde Gerçek Sayı Sabitleri
===================================

Pek çok programlama dilinde olduğu gibi Rust'ta da gerçek sayı sabitleri (``f32`` ve ``f64`` türünden sabitler) üstel 
biçimde belirtilebilmektedir. Gerçek sayı sabitleri üstel biçimde belirtilirken e ya da E harfi 10'un kuvveti anlamına
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

Burada ``b'a'`` sabiti bir byte sabitidir. Byte sabitler ``u8`` türünden kabul edildikleri için biz onları ancak ``u8``
türünden bir değişkene atayabiliriz. Byte sabitlerinde tek tırnak içerisine yalnızca standart ASCII karakterlerinden 
biri yerleştirilebilir. Yani tek tırnak içine yerleştirilecek karakterin Unicode kod numarasının [0, 127] aralığında 
olması gerekir. Örneğin:

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
byte türü ve byte sabitlerinin yeri başka biçimde doldurulamamaktadır. Python'da byte sabitlerin öneki ``'b'`` ya da 
``'B'`` olabilmektedir. Halbuki Rust'ta bu önek yalnızca ``'b'`` biçimindedir.

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
edilmektedir. Java, C# ve Python gibi dillerde string'ler o dillerin ``String`` sınıfı türünden (Python'da ``str``
sınıfı türünden) değişken referansı belirtmektedir. Örneğin Java ve C#'ta iki tırnak ifadeleri o dillerin ``String``
sınıfı türünden bir değişkene atanabilmektedir.

Rust'ta iki tırnak içerisindeki string'ler ``&str`` türündendir. ``str`` türüne string dilimi (string slice) 
denilmektedir. Ancak bu tür hemen hep referanslı bir biçimde kullanılır. Dolayısıyla Rust'ta biz bir string'i ``&str``
türünden bir değişkene atayabiliriz. ``&str`` türüne *string dilim referansı* denilmektedir. Referanslar ve dilimler
ilerideki bölümlerde ele alınacaktır. Örneğin:

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

==================================
C String Sabitleri ve Fonksiyonlar
==================================

C String Sabitleri (C String Literals)
======================================

Rust'ı C Programlama Dili ile birlikte kullanabilmek için C tarzı string'ler de bulundurulmuştur. Bunlara *C string
sabitleri (C string literals)* denilmektedir. C string sabitleri iki tırnağın soluna onunla yapışık c harfi getirilerek
oluşturulmaktadır. Örneğin:

.. code-block:: rust

   let cs = c"ankara";

C String sabitleri &CStr türündendir. Yani CStr türünden bir dilim referansı belirtmektedir. Ancak CStr türü ffi isimli
modülün içerisindedir ve otomatik olarak derleyici tarafından tanınmamaktadır. (Bu durum *prelude* denilen bir konu
içerisinde ileride ayrı bir başlık halinde ele alınacaktır.) CStr türü std::ffi modülünün içerisinde tanımlanmıştır. Bu
tür std::ffi::CStr biçiminde niteliklendirme yapılarak kullanılabilir ya da ileride de göreceğimiz gibi
niteliklendirmeyi ortadan kaldırıp kolay bir kullanım oluşturmak için aşağıdaki gibi use bildirimi de yapılabilir:

.. code-block:: rust

   use std::ffi::CStr;

ffi sözcüğü *foreign function interface* sözcüklerinden kısaltılmıştır. Aslında C string sabitlerinin normal string
sabitlerinden en önemli farklılığı C string sabitlerinin sonunda ``'\0'`` (null karakter) bulunmasıdır. Yani bu
sabitlerde Rust derleyicisi yazının sonuna ``'\0'`` karakterini de yerleştirmektedir. Böylece örneğin biz C'de yazılmış
ve parametresi ``char *`` ya da ``const char *`` türünden olan fonksiyonlara C'deki beklentiyi karşılamak için ``'\0'``
ile biten bir C string sabiti gönderebiliriz. C'de yazılmış olan fonksiyonların Rust'tan, Rust'ta yazılmış olan
fonksiyonların C'den kullanılabilmesi kursumuzda ayrı bir başlık altında ele alınacaktır.

Saf C String Sabitleri
======================

C String sabitleri de *saf (raw)* biçimde kullanılabilmektedir. Bunun için iki tırnağa yapışık onun önüne cr öneki
getirilmektedir. Semantik tamamen diğer saf string'lerde olduğu gibidir. Örneğin:

.. code-block:: rust

   let cs: &std::ffi::CStr = cr"a\nb";

Burada ``\n`` karakterleri *new line* karakterini değil ayrı ayrı \\ ve n karakterlerini belirtmektedir. Yine aynı
biçimde C string sabitlerinde de yazının içerisinde iki tırnak karakterinin kolay kullanılması için iki tırnakların
başına ve sonuna aynı sayıda ``#`` karakteri getirilebilmektedir. Örneğin:

.. code-block:: rust

   let cs: &std::ffi::CStr = cr#""ankara""#;       // geçerli
   let cs: &std::ffi::CStr = cr##""#"##;           // geçerli

never Türü
==========

Rust'ta never türünden (! türünden) bir sabit ya da değer yoktur. Yani Rust'ta ! bir değer değil tür belirtmektedir.
Never türünden bir değerin olmamasının nedeni bu türün *akış buradan devam etmeyecek* anlamına gelmesindendir.

Rust Terminolojisi: Nesne, Değişken ve Değer
============================================

Rust terminolojisine yönelik önemli bir noktaya değinmek istiyoruz. C'de çokça kullanılan *nesne (object)* terimi
Rust'ta kullanılmamaktadır. Rust'ta bellekte yer kaplayan ve bir isimle erişilebilen bölgelere *değişken (variable)*,
bellekte yer kaplayan ancak ismi olmayan nesnelere ve sabitlere ise *değer (value)* denilmektedir. Eğer hem değişken hem
de değeri kastetmek istiyorsanız *ifade (expression)* terimini kullanabilirsiniz. Aşağıda C'deki terimlerin Rust
karşılıklarını veriyoruz:

=============== ===============
C               Rust
=============== ===============
Nesne           Değişken
Geçici nesne    Değer
Sabit           Sabit/Değer
İfade           İfade
=============== ===============

Örneğin biz C'de *bir yapı nesnesi* derken Rust'ta *bir yapı değişkeni* deriz. C'de *fonksiyon bir yapı nesnesine geri
döner* derken Rust'ta *fonksiyon bir yapı değerine geri döner* deriz.

Fonksiyonlar
============

Bu bölümde Rust'ta fonksiyon tanımlaması ve fonksiyonların çağrılması üzerinde duracağız. Böylece artık örneklerimizde
fonksiyonları da kullanabileceğiz.

Fonksiyon Tanımlama
-------------------

Rust'ta bir fonksiyon tanımlamanın temel genel biçimi şöyledir:

.. code-block:: text

   fn <fonksiyon_ismi>([parametre_tanımlamaları]) [ -> <geri_dönüş_değerinin_türü>] {
       //...
   }

Bu temel bir genel biçimdir. Fonksiyon tanımlamanın genel biçiminde henüz üzerinde durmayacağımız bazı ayrıntılar da
vardır. Şimdi bu temel genel biçim üzerinde duralım. Bir fonksiyon tanımlarken tanımlamanın başında fn anahtar sözcüğü
bulundurulmaktadır. Bu fn anahtar sözcüğünü fonksiyon ismi izler. Fonksiyon isminden sonra parantezlerin açılıp
kapanması gerekir. Ancak parantezlerin içerisinde fonksiyonun parametre değişkenleri tanımlanabilmektedir. Tabii
fonksiyonlar parametreye sahip olmak zorunda değildir. Bir fonksiyonun onu çağıran fonksiyona ilettiği değere *geri
dönüş değeri (return value)* denildiğini biliyorsunuz. Fonksiyonların geri dönüş değerlerinin türleri Rust'ta
fonksiyonun parametre parantezinden sonra ``->`` atomu ve sonra tür yazılarak belirtilmektedir. Örneğin:

.. code-block:: rust

   fn foo() -> i32 {
       //...
   }

Burada ``foo`` fonksiyonu parametreye sahip değildir ve geri dönüş değeri i32 türündendir. Tabii fonksiyonların geri
dönüş değerleri olmak zorunda değildir. Bu durumda parametre parantezinden sonra geri dönüş değeri belirtilmez. Örneğin:

.. code-block:: rust

   fn bar() {
       //...
   }

Burada fonksiyonun geri dönüş değeri yoktur. C'de fonksiyonun geri dönüş değerinin olmadığını belirtmek için void
anahtar sözcüğünün kullanıldığını anımsayınız. Örneğin:

.. code-block:: c

   void bar(void)
   {
       //...
   }

Birim Türü (unit type)
----------------------

Rust'ta böyle bir void anahtar sözcüğü yoktur. Ancak onun yerine ``()`` ile temsil edilen ve ismine *birim (unit)* ya da
*birim türü (unit type)* denilen bir sentaks kullanılmaktadır. Örneğin:

.. code-block:: rust

   fn bar() -> () {
       //...
   }

Fonksiyonun geri dönüş değerinin türünün belirtilmemesiyle ``-> ()`` biçiminde belirtilmesi tamamen aynı anlama
gelmektedir. Aslında Rust'ta bu birim türü boş bir *demet (tuple)* belirtmektedir. Örneğin:

.. code-block:: rust

   let s: (i32, i32) = (10, 20);

Burada s iki elemanlı bir demettir. s'in türü de (i32, i32) biçiminde ifade edilmektedir. Örneğin:

.. code-block:: rust

   let s: () = ();

Burada s elemanı olmayan (yani sıfır elemanlı) bir demettir. Bunun türü de ``()`` biçiminde ifade edilmektedir. Yani
aslında *birim (unit)* ya da *birim türü (unit type)* sıfır elemanlı bir demet belirtmektedir.

Küme Parantezi Yerleşimi
------------------------

C ve C++ programcılarının büyük çoğunluğu (fakat hepsi değil) fonksiyonların ana bloklarındaki küme parantezlerini
genellikle aşağı satırın başında açarlar. Örneğin:

.. code-block:: c

   void foo(void)
   {
       //...
   }

Fakat Rust'ta ana blok küme parantezinin aynı satırda açılması bir gelenek biçiminde yerleşmiştir. Örneğin:

.. code-block:: rust

   fn foo() {
       //...
   }

Biz Derneğimizde hangi programlama dilinde çalışıyorsa o dilin geleneklerine uygun bir kod yerleşimi kullanıyoruz.

Fonksiyon Çağırma
-----------------

Rust'ta fonksiyonlar diğer pek çok dildeki gibi çağrılmaktadır.

.. code-block:: text

   fonksiyon_ismi([argüman_listesi])

Örneğin:

.. code-block:: rust

   foo();

return Deyimi
-------------

Rust'ta fonksiyonların geri dönüş değerleri diğer pek çok dilde olduğu gibi return deyimiyle oluşturulabilmektedir.
return deyiminin genel biçimi şöyledir:

.. code-block:: text

   return [ifade];

return deyimi hem fonksiyonu sonlandırır hem de geri dönüş değerini oluşturur. Eğer fonksiyonun geri dönüş değeri yoksa
(yani geri dönüş değeri *birim (unit)* türündense) return anahtar sözcüğünün yanına bir ifade yazılmayabilir. Bu durumda
örneğin:

.. code-block:: rust

   return;

kullanımı ile aşağıdaki kullanım tamamen eşdeğerdir:

.. code-block:: rust

   return ();

Rust *katı bir tür kontrolüne sahip (strict type checking)* programlama dili olduğu için bir fonksiyonun geri dönüş
değeri hangi türdense onu aynı türden bir değişkene atamak gerekir. Örneğin:

.. code-block:: rust

   fn main() {
       let result: i32;

       result = foo();
       println!("{}", result);     // 100
   }

   fn foo() -> i32 {
       println!("foo");
       return 100;
   }

Tabii Rust'ta da fonksiyonun geri dönüş değerinin olması onu kullanmayı zorunlu hale getirmemektedir. Örneğin:

.. code-block:: rust

   fn main() {
       foo();          // geçerli, fonksiyonun geri dönüş değerini kullanmak zorunda değiliz
   }

   fn foo() -> i32 {
       println!("foo");
       return 100;
   }

Bazı *statik analiz araçları* bu tür durumlarda uyarı üretebilmektedir. Bundan kaçınmak için ``_`` ile temsil edilen ve
yer tutucu olarak kullanılan isme bağlama yapabilirsiniz. Örneğin:

.. code-block:: rust

   let _ = foo();

Öğeler ve Tanımlama Sırası
--------------------------

Rust'ta fonksiyonlar *Öğe (Item)* denilen bir sentaks grubuna ilişkindir. Genel olarak öğelerde tanımlama sırasının bir
önemi yoktur. Yani biz örneğin bir fonksiyonu aşağıda tanımlayıp onu yukarıda çağırabiliriz. Halbuki C ve C++'ta
derleyici derleme işlemini yukarıdan aşağıya doğru yapmaktadır. Bu dillerde bir değişken görüldüğünde onun daha önce
bildiriminin görülmüş olması gerekmektedir. Bu nedenle C ve C++'ta eğer çağrılan fonksiyon çağıran fonksiyonun daha
altında tanımlanmışsa çağırma noktasının yukarısında bir yerde *prototip bildiriminin* yapılması gerekmektedir. Ancak
modern dillerin hemen hepsinde bu kısıtlama artık kaldırılmıştır.

Örneğin aşağıdaki gibi bir tanımlama ve çağırma C'de ve C++'ta geçerli değildir:

.. code-block:: c

   int main(void)
   {
       foo();          // geçersiz!
   }

   void foo(void)
   {
       //...
   }

Çünkü derleyici yukarıdan aşağıya doğru kodu derlerken ``foo`` çağrısına geldiğinde henüz ``foo`` hakkında bir bilgi
edinememiştir. (C90'da bu kuralda bazı ayrıntıların olduğunu anımsayınız.) Halbuki bunun eşdeğeri Rust'ta geçerlidir:

.. code-block:: rust

   fn main() {
       foo();          // geçerli
   }

   fn foo() {
       //...
   }

Parametre Değişkenleri
----------------------

Rust'ta fonksiyonların parametre değişkenleri parametre parantezinin içerisinde bir argüman listesi biçiminde
tanımlanmaktadır:

.. code-block:: text

   <değişken_ismi>: <tür>, <değişken_ismi>: <tür>, ....

Örneğin:

.. code-block:: rust

   fn add(a: i32, b: i32) -> i32 {
       return a + b;
   }

Burada ``add`` fonksiyonunun iki parametre değişkeni vardır. Fonksiyon da iki parametresinin toplamını geri
döndürmektedir.

Aslında Rust'ta fonksiyonların parametre değişkenleri yalnızca bir değişken biçiminde değil genel olarak *çürütülemez
(irrefutable)* kalıplar biçiminde de oluşturulabilmektedir. Kalıplar (patterns) Rust'ta oldukça ayrıntılı bir konudur.
Bu konu kursumuzda çeşitli bölümlerde ele alınacaktır.

Parametre Değişkenlerinin Değiştirilebilirliği
----------------------------------------------

Rust'ta fonksiyonların parametre değişkenleri yine default durumda *değiştirilemez (immutable)* biçimdedir. Örneğin:

.. code-block:: rust

   fn foo(a: i32) {
       //...
       a = 10;         // error!
       //...
   }

Burada a parametre değişkeni değiştirilemez durumda olduğu için error oluşacaktır. Eğer parametre değişkeninin
değiştirilmesi isteniyorsa mut yapılmalıdır. Örneğin:

.. code-block:: rust

   fn foo(mut a: i32) {
       //...
       a = 10;         // geçerli
       //...
   }

Ancak Rust'ta parametre değişkeninin mut yapılması genel olarak iyi bir teknik kabul edilmemektedir. Bu durum
fonksiyonun imzasını değiştirmekte ve yeni başlayanları tereddüt içerisinde bırakabilmektedir. Rust'ta parametre
değişkenini mut yapma yerine *gölgeleme (shadowing)* ile aynı isimli yeni bir mut yerel değişken oluşturma tercih
edilmektedir. Örneğin:

.. code-block:: rust

   fn foo(a: i32) {
       let mut a = a;          // doğru teknik
       //...

       a = 10;
       //...
   }

İfadesel Diller ve Blok Deyimleri
=================================

İfadesel Diller (Expression Languages)
--------------------------------------

if gibi, switch gibi deyimlerin de birer ifade gibi kullanılabildiği programlama dillerine *ifadesel diller (expression
languages)* denilmektedir. C ve C++, Java ve C# gibi diller bu bağlamda ifadesel diller değildir. İfadesel dillerde
deyimler birer değer de üretmektedir. Dolayısıyla bu dillerde deyimlerin ürettikleri değerler başka ifadelerde
kullanılabilmekte ve değişkenlere atanabilmektedir. Örneğin:

.. code-block:: text

   x = if (koşul) {
       //...
   }
   else {
       //..
   }

Böyle bir kullanım C'de ve C++'ta geçerli değildir ancak ifadesel dillerde geçerlidir. Yeni ve modern dillerin çoğu
ifadesel özelliklere sahip olarak tasarlanmaktadır. Örneğin Swift, Kotlin gibi yeni diller, Ruby, Haskell, Scheme gibi
eski diller bu özelliklere sahiptir. İşte bu bağlamda Rust da ifadesel bir dildir. Dolayısıyla Rust'ta biz deyimlerin
ürettiği değerleri yukarıdaki örnekte olduğu gibi kullanabiliriz.

Blok Deyimleri
--------------

C'de ve C++'ta olduğu gibi Rust'ta da bloklar bir deyim belirtir. Rust'ta bloklara *blok deyimi (block statement)* ya da
*block ifadesi (block expression)* denilmektedir. Rust'ta bir blok deyimi çalıştırılırken bu blok içerisindeki deyimler
sırasıyla tek tek çalıştırılır, blok deyimi de bloğun sonundaki ifadenin (expression) değerini üretir. İfade
(expression) kavramı izleyen bölümlerde ele alınacağı gibi ``';'`` içermemektedir. Örneğin:

.. code-block:: rust

   let x: i32;

   x = {
       println!("one");
       println!("two");
       println!("three");
       100
   };

Burada x'e ``100`` değeri atanacaktır. Blok içerisindeki deyimlerin ``';'`` ile sonlandırıldığına ancak bloğun sonundaki
ifadenin ``';'`` ile sonlandırılmadığına dikkat ediniz. Rust'ta eğer bloğun sonunda bir ifade yoksa o blok *birim
türünü* üretir. Örneğin:

.. code-block:: rust

   let x = {
       println!("one");
       println!("two");
       println!("three");
   };

Burada bloğun sonunda bir ifade yoktur, dolayısıyla x değişkeni ``()`` türündendir. Anımsanacağı gibi Rust'ta birim türü
*olmamayı, boş olmayı* temsil etmektedir.

Biz ifadeler ve deyimleri izleyen bölümlerde zaten ele alacağız. Burada ifadesel dil kavramına bir giriş yapmış olduk.

Fonksiyonun Geri Dönüş Değeri Olarak Blok İfadesi
-------------------------------------------------

Aslında Rust'ta bir fonksiyon bir blok deyiminden oluşmaktadır. Fonksiyonun çağrılması da o blok deyiminin
çalıştırılması anlamına gelmektedir. Bu nedenle Rust'ta (ve diğer pek çok ifadesel dilde) aslında fonksiyonun ana
bloğunun sonundaki ifade aynı zamanda fonksiyonun geri dönüş değerini oluşturmaktadır. Örneğin:

.. code-block:: rust

   fn foo() -> i32 {
       println!("foo");
       100
   }

Burada fonksiyonun geri dönüş değerini oluşturmak için return deyimi yerine doğrudan blok deyiminin ürettiği değer olan
``100`` ifadesi yerleştirilmiştir. Tabii aslında yukarıdaki ile aşağıdaki arasında işlevsel bir farklılık yoktur:

.. code-block:: rust

   fn foo() -> i32 {
       println!("foo");
       return 100;
   }

Şimdi size sanki Rust'ta return deyimi gereksizmiş gibi gelebilir. Ancak fonksiyonun ortasında yani bir bloğun
içerisinde fonksiyonu sonlandırmak istediğimizde mecburen yine return deyimini kullanmak zorunda kalırız.

==========================================================
İç İçe Fonksiyonlar, İfade ve Deyimler, Operatörlere Giriş
==========================================================

İç İçe (Nested) Fonksiyonlar
============================

Rust'ta da bazı programlama dillerinde olduğu gibi *iç içe (nested)* fonksiyonlar tanımlanabilmektedir. İç içe fonksiyon
tanımlamaları C'de ve C++'ta yoktur. Eski dillerden Pascal'da iç içe fonksiyon tanımlamaları vardı. Nispeten yeni
tasarlanan Swift, Kotlin gibi programlama dillerinde de bu özellik bulunmaktadır. C# gibi bazı dillere ise bu özellik
sonradan eklenmiştir. Örneğin:

.. code-block:: rust

   fn foo() {
       //...
       fn bar() {
           //...
       }
       //...
   }

Burada ``bar`` fonksiyonu ``foo`` fonksiyonun içerisinde tanımlanmıştır. Bir fonksiyon başka bir fonksiyonun içerisinde
tanımlanmışsa o fonksiyon ancak tanımlandığı fonksiyonun içerisinden çağrılabilir. Yani dışarıdan iç bir fonksiyon
çağrılamamaktadır. Örneğin:

.. code-block:: rust

   fn main() {
       foo();
       bar();      // error!
   }

   fn foo() {
       println!("foo");

       fn bar() {
           println!("bar");
       }

       bar();      // geçerli
   }

Bu örnekte ``bar`` fonksiyonunun ``main`` fonksiyonu içerisinden çağrılamadığına ``foo`` fonksiyonun içerisinden
çağrılabildiğine dikkat ediniz.

Rust'ta fonksiyonlar için prototip bildiriminin bulunmadığını ve zaten buna gerek de olmadığını belirtmiştik. Genel
olarak Rust'ta aşağıda tanımlanan bir fonksiyon daha yukarıda çağrılabilmektedir. Dolayısıyla iç bir fonksiyonu aşağıda
tanımlayıp daha yukarıda çağırabiliriz. Örneğin:

.. code-block:: rust

   fn foo() {
       println!("foo");
       bar();      // geçerli

       fn bar() {
           println!("bar");
       }
   }

Tabii iç fonksiyonun da geri dönüş değeri söz konusu olabilir. Örneğin:

.. code-block:: rust

   fn foo() {
       let val: i32;

       val = bar(10);
       println!("{}", val);

       fn bar(a: i32) -> i32 {
           a * a
       }
   }

İç içe fonksiyon tanımlamalarının yapıldığı pek çok dilde iç fonksiyonlar dış fonksiyonların yerel değişkenlerini
doğrudan kullanabilmektedir. Ancak Rust'ta bu mümkün değildir. Örneğin:

.. code-block:: rust

   fn foo() {
       let val = 123;

       fn bar() {
           println!("{}", val);        // error!
       }
       //...
   }

Burada ``bar`` içerisinden ``foo`` fonksiyonun yerel değişkenlerini kullanamayız.

Peki iç içe fonksiyon tanımlamalarına neden gereksinim duyulmaktadır? İşte bunun en önemli nedeni iç fonksiyonun
dışarıdan gizlenmek istenmesidir. Bir fonksiyon dışarıda tanımlanmışsa kodu inceleyen kişiler o fonksiyonun daha genel
bir işlem yaptığını düşünürler. Ancak bir fonksiyon başka bir fonksiyonun içerisinde tanımlanmışsa kodu inceleyen
kişiler iç fonksiyonun *genel bir işlem değil yalnızca dış fonksiyon için anlamlı bir işlem yaptığını* düşünürler. Bu
düşünce de kodu inceleyenin algısını güçlendirir ve mantıksal karmaşıklığı azaltır. O halde biz eğer bir fonksiyonu
yalnızca başka bir fonksiyonun yazımını kolaylaştırmak için tanımlayacaksak onu dışarıda değil ilgili fonksiyonun
içerisinde tanımlayabiliriz. Örneğin ``do_something`` isimli bir fonksiyon belli bir işin parçalarını ``foo``, ``bar``
ve ``tar`` fonksiyonlarına yaptırıyor olsun. Bu örnekteki ``foo``, ``bar`` ve ``tar`` fonksiyonlarının dışarıdan
kullanımları anlamlı değildir. Bunlar ``do_something`` fonksiyonun yazımını kolaylaştırmak amacıyla tanımlanmıştır. O
halde bizim bu ``foo``, ``bar`` ve ``tar`` fonksiyonlarını ``do_something`` fonksiyonun içerisinde tanımlamamız daha
uygun olur. Örneğin:

.. code-block:: rust

   fn do_something() {
       fn foo() {
           //...
       }

       fn bar() {
           //...
       }

       fn tar() {
           //...
       }

       //...
       foo();
       //...
       bar();
       //...
       tar();
       //...
   }

Peki okunabilirlik sağlamak için iç fonksiyonu dış fonksiyonun neresinde tanımlamak uygun olur? Genellikle Rust
programcıları iç fonksiyonu dış fonksiyonun hemen başında tanımlarlar. Ancak bazı programcılar bunun tam tersini de
yapmaktadır. Yani iç fonksiyonu dış fonksiyonun sonunda tanımlamaktadır. Bazı programcılar ise iç fonksiyonu
kullanıldığı yerin hemen yukarısında tanımlamayı tercih etmektedir.

İfade (Expression) ve Deyim (Statement) Kavramları
==================================================

Bu bölümde Rust'ta *ifade (expression)* ve *deyim (statement)* kavramları üzerinde duracağız.

İfade (Expression)
------------------

Programlama dillerinde *değişkenlerin, sabitlerin ve operatörlerin her bir bileşimine ifade (expression)* denilmektedir.
Örneğin:

.. code-block:: text

   a
   10
   a + 10
   a[i] + b + 10
   10 + 20

birer ifadedir. Tek başına bir sabitin ifade belirttiğine, tek başına bir değişkenin ifade belirttiğine ancak tek başına
bir operatörün ifade belirtmediğine dikkat ediniz. Bir ifade alt ifadelerden oluşabilir. Yani bir ifade başka ifadelerin
operatörlerle bir araya getirilmesinden oluşabilir. Örneğin ``a * b + c * d`` ifadesi ``a * b`` ifadesi ile ``c * d``
ifadesinin ``+`` operatörü ile birleştirilmesiyle oluşturulmuştur.

Yer Belirten ve Değer Belirten İfadeler
---------------------------------------

Rust'ta ifadeler iki sınıfa ayrılmaktadır:

1) Yer belirten ifadeler (place expression)
2) Değer belirten ifadeler (value expression)

Yer belirten ifadeler C'deki *sol taraf değeri (lvalue)*, yer belirtmeyen ifadeler ise C'deki *sağ taraf değeri
(rvalue)* ile aynı anlamdadır. *The Rust Reference* dokümanları bu konuda şöyle demektedir:

*Note: Historically, place expressions were called lvalues and value expressions were called rvalues.*

Yer belirten ifadeler (place expressions) ismi üzerinde bellekte bir yer belirtmektedir. Dolayısıyla bu ifadelere atama
yapılabilir ve bu ifadelerin adresleri alınabilir. Değer ifadeleri ise bellekte bir yer belirtmeyen ifadelerdir. Bir
operatörün operand'ı kullanıldığı bağlamda ya yer ifadesi ya da değer ifadesi durumundadır. Örneğin:

.. code-block:: rust

   a = b + 10;

Burada a ifadesi bir yer ifadesi, b ve ``10`` birer değer ifadesi belirtmektedir. *The Rust Reference* dokümanlarında
hangi operatörlerin hangi operand'larının yer belirten ifade olarak ele alınacağı belirtilmiştir. Geri kalan durumlarda
operatörlerin operand'ları değer belirten ifade olarak ele alınmaktadır.

Deyim (Statement)
-----------------

Programlama dillerindeki çalıştırma birimlerine *deyim (statement)* denilmektedir. Bir program deyimlerin
çalıştırılmasıyla çalışmaktadır. Örneğin aşağıda C'de yazılmış olan kod parçasına bakınız:

.. code-block:: c

   ...
   max = a[0];
   for (int i = 1; i < size; ++i)
       if (a[i] > max)
           max = a[i];
   printf("%d\n", max);
   ...

Bu kod parçasında üç deyim vardır. Bunları birer satır boşluk vererek belirtmek istiyoruz:

.. code-block:: c

   max = a[0];

   for (int i = 1; i < size; ++i)
       if (a[i] > max)
           max = a[i];

   printf("%d\n", max);

Burada for döngüsünün tamamının tek bir deyim olduğuna dikkat ediniz. Genel olarak programlama dillerinde for gibi, if
gibi programın akışı üzerinde etkili olan deyimlere *kontrol deyimleri (control statements)*, Rust Programlama Dilinde
ise böyle deyimlere *bloklu deyimler (expression with block)* denilmektedir.

Basit Deyimler (Simple Statements)
----------------------------------

C, C++, Java ve C# gibi imperative dillerin büyük çoğunluğunda bir ifade bir sonlandırıcı (tipik olarak ``';'``) ile
sonlandırıldığında bu ifade deyim haline gelmektedir. Bu tür deyimlere pek çok programlama dilinde *basit deyim (simple
statement)* ya da *ifadesel deyim (expression statement)* denilmektedir. Örneğin:

.. code-block:: c

   max = a[0]

biçimindeki atom yığını C'de bir ifade belirtmektedir, deyim belirtmemektedir. Ancak örneğin:

.. code-block:: c

   max = a[0];

artık burada ifadenin sonuna ``';'`` yerleştirildiği için bu atom yığını bir deyim belirtmektedir. Benzer biçimde
örneğin:

.. code-block:: c

   printf("%d\n", max)

biçimindeki atom yığını bir ifade belirtirken bunun sonuna ``';'`` yerleştirirsek artık bu bir deyim belirtir hale
gelmektedir:

.. code-block:: c

   printf("%d\n", max);

Özyineleme (Recursion)
----------------------

Bir öğenin kendisi gibi başka öğeleri içermesi durumuna *özyineleme (recursion)* denilmektedir. Doğada ve bilgisayar
bilimlerinde özyinelemeyle sıkça karşılaşılmaktadır. Örneğin bir dizin (directory) kendisi gibi dizinleri
içerebilmektedir. İşte bir deyim de başka deyimleri içerebilir. Örneğin:

.. code-block:: rust

   if koşul {
       ifade1;
       ifade2;
       loop {
           //...
       }
   }
   else {
       ifade3;
       ifade4;
   }

Burada bloklar da bir deyimdir. Ancak bu bloklar bağımsız birer deyim değil if deyiminin içerisindeki deyimlerdir. Blok
deyimlerinin içerisinde de başka deyimler vardır. Bu durumu bir kutunun içerisinde başka kutuların olduğu duruma
benzetebiliriz. Örneğin bir kutunun içerisinde birtakım nesneler ve başka kutular olsun. O kutuların içerisinde de başka
kutular olsun. Böyle bir kutuya dışarıdan baktığımızda yalnızca bir tane kutu görürüz. Çünkü diğer kutular bu kutunun
içerisindedir. İşte benzer biçimde yukarıdaki kod parçasına dışarıdan bakıldığında tek bir deyim görülecektir. Diğer
deyimler buradaki if deyiminin içerisindedir.

İfadesel Deyimler ve Rust Grameri
---------------------------------

Rust'ın bir *ifadesel dil (expression language)* olduğunu belirtmiştik. Rust'ta for gibi if gibi bloklu yapılar bir
ifade belirtmektedir. *The Rust Reference* dokümanlarında *deyim (statement)* için BNF grameri şöyle verilmiştir:

.. code-block:: text

   Statement :
         ;
       | Item
       | LetStatement
       | ExpressionStatement
       | MacroInvocationSemi

Burada bir deyimin yalnızca bir ``';'`` atomundan (C, Java ve C# gibi dillerdeki boş deyim), bir program öğesinden
(Item), bir ifadeden (ExpressionStatement) ve bir makro çağrımından (MacroInvocationSemi) oluşturulabileceği
belirtilmektedir. Biz bu gramerdeki ExpressionStatement ara sembolüne *ifadesel deyim* diyeceğiz. Gramerdeki
ExpressionStatement ise şöyle açılmaktadır:

.. code-block:: text

   ExpressionStatement :
         ExpressionWithoutBlock ;
       | ExpressionWithBlock ;?

Buradaki *ExpressionWithoutBlock ;* bir ifadenin sonuna ``';'`` getirilerek deyim oluşturulabileceğini belirtmektedir.
ExpressionWithBlock ise küme parantezlerine sahip olan sentaktik yapıları (yani diğer dillerdeki bileşik deyimleri, if
gibi, for gibi deyimleri) belirtir. Gramerdeki ExpressionWithBlock bir ifadenin parçası olmaksızın bu bloklu yapıların
tek başına kullanılması durumunu anlatmaktadır. Ayrıca bu BNF gramerine göre ExpressionWithBlock sembolünden sonra
``';'`` yerleştirilebilir ya da yerleştirilmeyebilir. Bu gramerden çıkan sonuçları birkaç örnekle açıklamak istiyoruz.
Aşağıdaki ifadesel deyime dikkat ediniz:

.. code-block:: rust

   x = if (koşul) {
       //...
   }
   else {
       //...
   };

Burada aslında bir atama işlemi söz konusudur. Dolayısıyla bu kod parçası gramerde *ExpressionWithoutBlock ;* biçiminde
açılmaktadır. Burada ``';'`` atomunun deyim oluşturmak için ifadenin sonuna yerleştirilmesi zorunludur. Aşağıdaki kod
parçasına dikkat ediniz:

.. code-block:: rust

   if (koşul) {
       //...
   }
   else {
       //...
   }

Burada bloklu sentaks yapısı kendi başına kullanılmıştır. Dolayısıyla bu sentaks gramerde *ExpressionWithBlock ;?* ile
açılır. Bu durumda ``';'`` atomunun kullanılıp kullanılmayacağı isteğe bağlıdır. Yani bu gramere göre yukarıdaki if
deyimi şöyle de yazılabilirdi:

.. code-block:: rust

   if (koşul) {
       //...
   }
   else {
       //...
   };

Ancak izleyen paragraflarda ele alınacağı gibi eğer bloklu deyimler değer üretiyorsa (*birim (unit)* dışında bir değer
üretiyorsa) onların ``';'`` ile sonlandırılması zorunludur.

Etkisiz Deyimler
----------------

Rust'ta tıpkı C'de olduğu gibi etkisiz kodlar geçersiz kabul edilmemektedir. Örneğin:

.. code-block:: rust

   a + b;
   10 + 20;
   30;
   a[i];

gibi deyimler programda bir durum değişikliği yaratmadığı halde Rust'ta geçerlidir. Tabii etkisiz deyimlerin geçerli
olduğu programlama dillerinde derleyiciler uyarı mesajı (warning) verebilmektedir. Java ve C# gibi bazı dillerde ise
etkisiz ifadelerden deyim yapılamamaktadır.

Bloklu İfadelerin Sonundaki Noktalı Virgül
------------------------------------------

Rust'ta if gibi, while gibi for gibi deyimlerin sonuna bazen açıkça ``';'`` atomunun getirilmesi gerekebilmektedir. Eğer
bloklu deyimler bir değer üretmiyorsa (yani ``()`` dışında bir değer üretmiyorsa) onların sonuna ``';'`` getirmeye gerek
yoktur. Ancak bunlar bir değer üretiyorsa onların sonuna ``';'`` getirilmesi zorunludur. Örneğin:

.. code-block:: rust

   if koşul {
       println!("doğru");
   }
   else {
       println!("yanlış");
   }

Burada if ifadesi bir değer üretmemektedir. (Yani ``()`` değeri üretmektedir). Bu durumda bu ifadeyi deyim haline
getirmek için ifadenin sonuna ``';'`` getirilmeyebilir. Ancak örneğin:

.. code-block:: rust

   if koşul {
       println!("doğru");
       10
   }
   else {
       println!("yanlış");
       20
   };

Burada if ifadesi bir değer üretmektedir. Artık bunun deyim haline getirilmesi için bunun sonuna ``';'`` getirilmesi
gerekir. Tabii yine de yukarıdaki kodda mantıksal bir sorun vardır. Bu kodda if ifadesi değer ürettiği halde bu değer
kullanılmamıştır. Bu durumda if ifadesinin bir değer üretmesinin anlamı da kalmamıştır. Örneğin:

.. code-block:: rust

   x = if koşul {
       println!("doğru");
       10
   }
   else {
       println!("yanlış");
       20
   };

Burada artık gramere göre bloklu bir ifade söz konusu değildir. Bu bir atama deyimidir. Atama deyimin solundaki ifade
bloklu bir ifadedir. Dolayısıyla buradaki son ``';'`` atama deyiminin sonundaki olması gereken ``';'`` atomudur.

Aşağıdaki gibi bir deyim söz konusu olsun:

.. code-block:: rust

   if koşul {
       //...
   }
   else {
       //...
   } [2];

Burada iki deyim mi vardır yoksa tek deyim mi vardır? İşte Rust derleyicisi burada if ifadesinin bir değer üretip
üretmediğine (yani ``()`` değerini üretip üretmediğine) bakmaktadır. Eğer if ifadesi bir değer üretmiyorsa bu if
ifadesinin sonunda ``';'`` olmak zorunda değildir, dolayısıyla burada iki ayrı deyim vardır. Ancak if ifadesi bir değer
üretiyorsa burada tek bir deyim vardır. Tabii bu durumda if ifadesinden üretilen değere ``[]`` operatörü uygulanamazsa
derleme zamanında error oluşacaktır.

Bloklu Yapılar: Deyim mi, İfade mi?
-----------------------------------

Peki Rust gramerine göre Rust'taki if gibi while gibi bloklu yapılar bir deyim midir yoksa bir ifade midir? İşte Rust
gramerine göre bu bloklu yapılar birer ifadedir. Ancak bunların deyim haline getirilmesi için eğer bu ifadeler bir değer
üretmiyorsa ``';'`` kullanmaya gerek yoktur, ancak değer üretiyorsa ``';'`` kullanılması gerekmektedir.

Metotlar
========

Rust nesne yönelimli bir programlama dili olmasa da Rust'ta metot kavramı da vardır. Bir türe ilişkin fonksiyonlara
metot denilmektedir. Rust'ta Fonksiyon kavramı türle ilişkili olmayan yordamlar için metot ise türle ilişkili olan
yordamlar için kullanılmaktadır. Bir fonksiyon doğrudan modül ve fonksiyon ismi belirtilerek çağrılmaktadır:

.. code-block:: text

   fonksiyon_ismi([argüman_listesi]);

Örneğin:

.. code-block:: rust

   foo(10, 20, 30);

Ancak metotlar ilişkili oldukları tür türünden bir değişken ya da değerle nokta operatörü kullanılarak çağrılmaktadır:

.. code-block:: text

   <değişken_ya_da_değer>.metot_ismi([argüman listesi]);

Örneğin:

.. code-block:: rust

   a.foo(10, 20, 30);

Burada a değişkeni ``foo`` metodunun içinde bulunduğu tür türündendir. Görüldüğü gibi metotlar doğrudan değil metodun
ilişkin olduğu tür türünden değişken ya da değerle çağrılmaktadır.

Metotlar konusunu yapılar (structures) konusu içerisinde ayrıntılı bir biçimde ele alacağız.

Operatörler
===========

Bir programlama dilinde ilk öğrenilmesi gereken konulardan biri de dildeki temel operatörlerdir. Bu bölümde Rust'taki
temel operatörleri ele alacağız.

Operatör Kavramı
----------------

Programlama dillerinde bir işleme yol açan ve işlem sonucunda bir değer üretilmesini sağlayan atomlara *operatör
(operator)* denilmektedir. Örneğin ``+``, ``-``, ``*``, ``/``, ``>``, ``<`` birer operatör atomdur. Tabii dillerdeki
operatörlerin sayısı fazla olduğunda artık klavyedeki özel semboller de yetmediği için anahtar sözcüklerden de
operatörler oluşturulmaktadır. Örneğin C'deki sizeof bir sembol olmadığı halde bir operatör belirtmektedir.

Operatörlerin Sınıflandırılması
-------------------------------

Operatörler konusunun iyi anlaşılması için öncelikle operatörlerin sınıflandırılması üzerinde durmak gerekir.
Operatörler üç biçimde sınıflandırılmaktadır:

1) Operatörün İşlevine Göre
2) Operand Sayılarına Göre
3) Operatör Konumuna Göre

İşlevlerine göre operatörler çeşitli biçimlerde sınıflandırılabilmektedir. Örneğin:

* **Aritmetik Operatörler (Arithmetic Operators):** ``+``, ``-``, ``*``, ``/`` gibi aritmetik işlemler yapan
  operatörlere *aritmetik operatörler* denilmektedir.

* **Karşılaştırma Operatörleri (Comparison Operators/Relational Operators):** ``>``, ``<``, ``>=``, ``<=`` gibi iki
  değeri karşılaştıran operatörlere karşılaştırma operatörleri denilmektedir.

* **Mantıksal Operatörler (Logical Operators):** Programlama dillerinde AND, OR, NOT gibi mantıksal işlemler yapan
  operatörlere mantıksal operatörler denilmektedir.

* **Bit Operatörleri (Bitwise Operators):** Değeri bir bütün olarak değil de bit bit ele alıp bitsel düzeyde işlemleri
  yapan operatörlerdir.

* **Adres Operatörleri (Pointer Operators):** Adres bilgileri üzerinde işlem yapan operatörlerdir.

* **Özel Amaçlı Operatörler (Special Purpose Operators):** Yukarıdaki amaçların dışında kullanılan operatörlerdir.

========================================================
Operatörlerin Sınıflandırılması ve Aritmetik Operatörler
========================================================

Operand Sayılarına Göre Operatörler
===================================

Bir operatörün işleme soktuğu ifadelere *operand* denilmektedir. Örneğin:

.. code-block:: text

   a + b

ifadesinde ``+`` operatörü a ve b bu operatörün operand'larıdır. Örneğin:

.. code-block:: text

   a + b * c

Bu ifadede önce ``*`` operatörü yapılacaktır. ``*`` operatörünün operand'ları b ve c'dir. ``+`` operatörünün
operand'ları ise a ve ``b * c``'dir.

Operand sayılarına göre operatörler üç gruba ayrılmaktadır:

1) İki Operand'lı operatörler (Binary Operators)
2) Tek Operand'lı Operatörler (Unary Operators)
3) Üç operand'lı Operatörler (Ternary Operators)

Operatörlerin büyük bölümü iki operand'lıdır. Örneğin biz ``*`` operatörünü kullanırken onun soluna ve sağına iki
operand yerleştiririz. ``!`` operatörü gibi, işaret ``-`` operatörü gibi operatörlerin ise tek operand'ı vardır. Örneğin
biz iki değeri mantıksal NOT işlemine sokmayız, tek bir değeri mantıksal NOT işlemine sokarız. Nihayet çok seyrek olarak
programlama dillerinde üç operand'lı operatörler de bulunabilmektedir. Örneğin C, Java ve C# gibi dillerdeki ``?:``
operatörü üç operand'lı bir operatördür. Rust'ta üç operand'lı bir operatör yoktur.

Operatörlerin Konumuna Göre Sınıflandırılması
=============================================

Operatörler konumlarına göre de üçe ayrılmaktadır:

1) Araek Operatörler (Infix Operators)
2) Önek Operatörler (Prefix Operators)
3) Sonek Operatörler (Postfix Operators)

Araek operatörler operand'larının arasına getirilerek kullanılır. Örneğin ``/`` operatörü araek bir operatördür. Biz bu
operatörü ``a / b`` biçiminde kullanırız. Operand'ının önüne getirilerek kullanılan operatörlere önek operatörler denir.
Örneğin ``!`` operatörü işaret ``-`` operatörü önek operatörlerdir. Bazı operatörler ise operand'larının sonuna
getirilerek kullanılmaktadır. Bunlara da sonek operatörler denir. Örneğin fonksiyon çağırma operatörü ve ``[]``
operatörü sonek operatörlerdir.

Operatörlerin Açıklanma Biçimi
==============================

Programlama dillerinde operatörler açıklanırken önce operatörün yukarıda belirttiğimiz üç sınıflandırmada da nereye
düştüğü belirtilir. Sonra operatörün ne yaptığı ve diğer özellikleri açıklanır. Örneğin */ operatörü iki operand'lı
araek (binary infix) bir operatördür, soldaki operand'ın sağdaki operand'a bölüm değerini üretir* gibi. Örneğin *işaret
- operatörü tek operand'lı önek (unary prefix) bir operatördür, operand'ının negatif değerini üretir* gibi.

Operatörlerin Öncelik İlişkisi
==============================

Aynı ifade içerisinde birden fazla operatör kullanıldığında bunlar birbirlerine göre belli sırada işleme sokulurlar.
Buna *operatörler arasındaki öncelik ilişkisi (operator precedence)* denilmektedir. Örneğin:

.. code-block:: text

   a = b + c * d

ifadesinde önce ``*`` işlemi sonra toplama işlemi sonra atama işlemi yapılır:

.. code-block:: text

   İ1: c * d
   İ2: b + İ1
   İ3: a = İ2

Aynı öncelikli operatörler kendi aralarında soldan sağa ya da sağdan sola öncelikli yapılmaktadır. Örneğin ``+`` ve
``-`` operatörleri soldan sağa önceliklidir:

.. code-block:: text

   a = b - c + d - e

Burada işlemler şu sırada yapılır:

.. code-block:: text

   İ1: b - c
   İ2: İ1 + d
   İ3: İ2 - e
   İ4: a = İ3

Ancak örneğin işaret ``-`` operatörü sağdan sola önceliklidir:

.. code-block:: text

   a = ---b;

.. code-block:: text

   İ1: -b
   İ2: -İ1
   İ3: -İ2
   İ4: a = İ3

Operatörler arasındaki öncelik ilişkisi *operatörlerin öncelik tablosu* denilen bir tabloyla betimlenmektedir. Bu tablo
satırlardan oluşur. Üst satırdaki operatörler alt satırdaki operatörlerden daha yüksek önceliklidir. Aynı satırdaki
operatörler eşit önceliklidir. Bu satırların sağında *Soldan Sağa (Left to Right)* ya da *Sağdan Sola (Right to Left)*
ibaresi bulunur. Buna İngilizce *Associativity* denilmektedir. Associativity eşit ölçekli operatörlerin hangisinin önce
yapılacağını belirtir. *Soldan Sağa* öncelik *o satırdaki operatör aynı ifadede birlikte bulunuyorsa ifade içerisinde
hangisi daha soldaysa önce o yapılır* anlamına gelmektedir. *Sağdan Sola* öncelik de *ifade içerisinde sağda olan
operatör daha önce yapılır* anlamına gelir.

Bir operatörü diğer operatörden daha önce işleme sokmak istiyorsanız parantezlerle ona öncelik kazandırmalısınız.
Örneğin:

.. code-block:: text

   a = (b + c) * d;

.. code-block:: text

   İ1: b + c
   İ2: İ1 * d
   İ3: a = İ2

Operatörlerin öncelik tablosunun en yalın hali şöyle oluşturulabilir:

============== ==============
Operatörler    Associativity
============== ==============
``()``         Soldan Sağa
``* /``        Soldan Sağa
``+ -``        Soldan Sağa
``=``          Sağdan Sola
============== ==============

Burada en tepedeki ``()`` hem fonksiyon çağırma operatörünü hem de öncelik parantezini temsil etmektedir. Biz bu yalın
tabloyu temel alacağız. Operatörleri gördükçe bu tabloya ekleyeceğiz.

Operatörlerin öncelik tablosu pek çok programlama dilinin standart dokümanlarında ya da referans kitaplarında
verilmemektedir. Çünkü zaten öncelik çoğu durumda BNF gramerinden doğal olarak elde edilmektedir. Yani BNF grameri zaten
öncelik tablosunu da oluşturmaktadır. Ancak *The Rust Reference* dokümanlarında operatörler arasındaki öncelik ilişkisi
ayrı bir başlık altında bir tabloyla açıklanmıştır.

İşlem Öncesi Otomatik Tür Dönüştürmesi Yoktur
=============================================

Rust'ta daha önceden de belirttiğimiz gibi iki operand'lı operatörlerin operand'ları aynı türden olmak zorundadır.
Dolayısıyla Rust'ta *işlem öncesi otomatik tür dönüştürmesi (usual arithmetic conversion)* yoktur. Benzer biçimde
Rust'ta *işlem öncesinde int türüne dönüştürme (integral promotion)* biçiminde de bir kural da yoktur. Örneğin C'de biz
``int`` ile ``long`` değeri aritmetik işleme sokarsak ``long`` türünden bir değer elde ederiz. Ancak Rust'ta ``i64`` 
ile ``i32`` türlerini zaten işleme sokamayız. Rust'ta operand'lar aynı türden olmak zorunda olduğu için işlemin sonucu
da aynı türden olur. Örneğin Rust'ta iki ``i8`` türünden değeri toplarsak sonuç i8 türünden olur. Fakat örneğin C'de
iki char türünden değer toplanırsa *int türüne yükseltme kuralı* gereği sonuç int türden elde edilir.

Operand'ların Ele Alınma Sırası
===============================

C'de ve C++17'ye kadar C++'ta aritmetik operatörlerin operand'larının ele alınış sırası belirsiz (unspecified)
bırakılmıştır. C ve C++ programcılarının önemli bir bölümü operatörlerin öncelik tablosunun etkisi altında kalarak bu
kuralı yanlış bilmektedir. Çünkü operatörlerin öncelik tablosu pratik bir anlatım sağlamak için oluşturulmuştur; durumu
kesin betimlemek için yetersiz kalmaktadır. Örneğin:

.. code-block:: rust

   result = foo() + bar();

Pek çok C programcısı burada önce ``foo`` fonksiyonun çağrılacağını sanmaktadır. Derleyicilerin hemen hepsi böyle yapsa
da aslında C standartlarına göre önce ``bar`` fonksiyonu da çağrılabilir. Çünkü ``+``, ``-``, ``*``, ``/`` gibi
operatörlerin soldaki operand'ının mı önce ele alınacağı yoksa sağdaki operand'ının mı önce ele alınacağı konusunda bir
belirleme yapılmamıştır (unspecified). Tabii öncelik tablosuna bakanlar ``()`` operatörünün yanında *Soldan Sağa*
ibaresini görünce önce ``foo`` fonksiyonun çağrılacağını sanmaktadır. Zaten bu nedenden dolayı C'de aşağıdaki gibi bir
ifade tanımsız davranış oluşturmaktadır:

.. code-block:: c

   result = a + ++a;

Çünkü burada ``+`` operatörünün sol tarafı önce yapılırsa farklı bir sonuç sağ tarafı önce yapılırsa farklı bir sonuç
elde edilir. Tabii kural *önce soldaki operand ele alınır, sonra sağdaki operand ele alınır* biçiminde olsaydı hiçbir
karışıklık oluşmazdı. Bu durumda kesinlikle ilk örnekte önce ``foo`` fonksiyonu sonra ``bar`` fonksiyonu çağrılırdı.
İşte temel sentaksı ve semantiği büyük ölçüde C'den alınmış olan Java ve C# gibi dillerde kesinlikle soldaki operand
önce ele alınmaktadır. Bu nedenle bu dillerde ``a + ++a`` gibi bir ifadeden ne elde edileceği bellidir. Örneğin a'nın
içerisinde ``3`` değeri varsa bu dillerde bu ifade ``3 + 4`` değerini üretir. (İfade ``++a + a`` biçiminde olsaydı ``4 +
4`` değerini üretecekti.) C++17'de radikal bir değişiklik yapılarak operand'ların ele alınma sırası *önce soldaki sonra
sağdaki* biçiminde değiştirilmiştir. Dolayısıyla artık C++'ta da ``a + ++a`` ve ``++a + a`` gibi ifadeler tanımsız
davranış olmaktan çıkmıştır. İşte Rust'ta da aritmetik operatörlerin kesinlikle önce soldaki operand'ı sonra sağdaki
operand'ı ele alınmaktadır.

Aritmetik Operatörler
=====================

Dört İşlem Operatörleri ve Taşma (Overflow)
-------------------------------------------

``*``, ``/``, ``+`` ve ``-`` operatörleri iki operand'lı araek aritmetik operatörlerdir. Klasik dört işlemi yaparlar.
Öncelik tablosunda ``*`` ve ``/`` operatörleri, ``+`` ve ``-`` operatörlerinden daha yüksek öncelikli grupta
bulunmaktadır:

============== ==============
Operatörler    Associativity
============== ==============
``()``         Soldan Sağa
``* /``        Soldan Sağa
``+ -``        Soldan Sağa
``=``          Sağdan Sola
============== ==============

Aritmetik operatörlerde operand'lar aynı türden olmak zorunda olduğuna göre ya sonuç bu türün sınırlarının dışına
çıkarsa ne olacaktır? Bu duruma programlama dillerinde *taşma (overflow)* denilmektedir. C ve C++'ta işaretli tamsayı
türlerinde taşma oluşursa *tanımsız davranış (undefined behavior)* oluşmaktadır. Ancak işaretsiz tamsayı türlerinde
taşma oluşursa yüksek anlamlı bitler atılmakta başka bir deyişle *sarma (wrapping)* yapılmaktadır. C#'ta checked ve
unchecked bağlam kavramı vardır. Rust'ta ise taşma olduğunda default durumda panic oluşmaktadır. (panic bir programın
umulmadık bir durumla karşılaştığında çökmesi anlamına gelmektedir. Bu nesne yönelimli dillerdeki exception'a benzer bir
kavramdır. panic durumu Rust'ta programcı tarafından ``panic`` fonksiyonuyla da oluşturulabilmektedir.) Ayrıca Rust
derleyicisi taşmayı derleme aşamasında belirleyebiliyorsa error de oluşturabilmektedir. Ancak bu durum *The Rust
Reference* dokümanlarında derleyicileri yazanların isteğine bırakılmıştır. *The Rust Reference* dokümanları şöyle
söylemektedir:

*Outside of const contexts, whether overflow is detected at compile time or not is unspecified and may change across
compiler versions.*

Örneğin:

.. code-block:: rust

   fn foo(a: i8, b: i8) {
       let c: i8;

       c = a + b;
       println!("{}", c);
   }

Biz bu fonksiyonu aşağıdaki gibi çağırmış olalım:

.. code-block:: rust

   let a: i8 = 127;
   let b: i8 = 1;

   foo(a, b);

Bu çağrıda panic oluşacaktır. Örneğin:

.. code-block:: rust

   let a:u8 = 255;
   let b: u8 = 1;
   let c: u8;

   c = a + b;      // bu satırda error oluşabilir

Burada ``a + b`` işlemindeki taşmanın derleme aşamasında kontrol edilip edilmeyeceği derleyicileri yazanların isteğine
bırakılmıştır. Kursun yapıldığı zamanda kullanılan Rust derleyicisi bu satırda error oluşturmaktadır.

Rust'ta temel türler sanki bir yapıymış gibi de kullanılmaktadır. Bu nedenle i32, f64 gibi temel türler bazı metotlara
da sahiptir. Eğer taşma durumunda *sarma (wrapping)* yapılması isteniyorsa bu türlerin ``wrapping_ttt`` isimli metotları
kullanılabilir. Burada ``ttt`` işlemin türünü belirtmektedir. Örneğin:

.. code-block:: rust

   fn foo(a: i8, b: i8) {
       let c: i8;

       c = a.wrapping_add(b);
       println!("{}", c);
   }

Şimdi fonksiyonu şöyle çağırmış olalım:

.. code-block:: rust

   let a: i8 = 127;
   let b: i8 = 1;

   foo(a, b);

Artık sarma yapılacağı için ``+127``'den sonraki işaretli sayı ``-128`` olduğu için ``-128`` elde edilecektir. Temel
türlerin taşma için yalnızca ``wrapping_xxx`` metotları yoktur, başka metotları da vardır. Ancak biz bu bağlamda diğer
metotlar üzerinde durmayacağız.

Rust'ta gerçek sayı türlerindeki taşmalarda ``+inf`` ya da ``-inf`` değerleri üretilmektedir. Geçersiz bir değere sahip
işlemlerden de ise ``NaN`` değeri elde edilmektedir. *The Rust Reference* dokümanları bu konuda şöyle demektedir:

*Overflow in floating-point operations does not cause a panic. Instead, the result is ±∞ or NaN, following IEEE 754
rules.*

Kalan Operatörü
---------------

``%`` operatörü iki operand'lı araek aritmetik operatördür. Tıpkı C'deki gibi soldaki operandın sağdaki operand'a
bölümünden elde edilen kalan değerini üretir. Öncelik tablosunda ``*`` ve ``/`` ile aynı öncelik grubunda bulunmaktadır:

============== ==============
Operatörler    Associativity
============== ==============
``()``         Soldan Sağa
``* / %``      Soldan Sağa
``+ -``        Soldan Sağa
``=``          Sağdan Sola
============== ==============

Aslında programlama dillerinde ``a % b`` gibi bir işlemin sonucu şöyle oluşturulmaktadır:

.. code-block:: text

   a - a / b * b

Bu durumda örneğin C, Java, C# ve Rust'ta ``-10 % 4`` işleminin sonucu ``-2``'dir:

.. code-block:: text

   -10 - -10 / 4 * 4 = -2

Ancak bu dillerde ``10 % -4`` işleminin sonucu ``2``'dir:

.. code-block:: text

   10 - 10 / -4 * -4 = 2

Python'da tamsayılı bölmenin ``//`` operatörüyle yapıldığını ve bu operatöre *floordiv* dendiğini anımsayınız. floor
işlemi kendisinden *küçük ilk tamsayı anlamına* gelmektedir. Örneğin ``-2.5`` değerinin floor sonucu ``-3``'tür. Bu
durumda Python'da ``-10 % 4`` işleminin sonucu ``2`` olur:

.. code-block:: text

   -10 - -10 // 4 * 4 = 2

C ve C++ dillerinde ``%`` operatörünün iki operandının da tamsayı türlerine ilişkin olması zorunludur. Ancak Rust'ta
tıpkı C#, Java ve Python'da olduğu gibi bu operatörün operand'ları gerçek sayı türlerine ilişkin de olabilir. Örneğin:

.. code-block:: rust

   let a: f64 = 10.5;
   let result: f64;

   result = a % 2.0;
   println!("{}", result);     // 0.5

=======================================================
Aritmetik, Karşılaştırma, Mantıksal ve Bit Operatörleri
=======================================================

İşaret Operatörü
================

Rust'ta işaret ``-`` operatörü vardır ancak gereksiz olduğu nedeniyle işaret ``+`` operatörü dile sokulmamıştır. İşaret
``-`` operatörü tek operand'lı önek (unary prefix) bir operatördür ve operand'ının negatif değerini (ikiye tümleyen
değerini) üretir. Ancak Rust'ta işaret ``-`` operatörü işaretsiz tamsayı türlerine uygulanamamaktadır. Yani bu
operatörün operand'ı işaretli tamsayı türünden ya da gerçek sayı türünden olmak zorundadır. Halbuki C'de ve C#'ta işaret
``-`` operatörü işaretsiz tamsayı türlerine de uygulanabilmektedir.

İşaret ``-`` operatörü öncelik tablosunda ``*``, ``/`` ve ``%`` operatörlerinin yukarısında sağdan sola grupta
bulunmaktadır:

======================== ==============================
Operatörler              Associativity
======================== ==============================
``()``                   Soldan Sağa
``-``                    Sağdan Sola
``* / %``                Soldan Sağa
``+ -``                  Soldan Sağa
``=``                    Sağdan Sola
======================== ==============================

Örneğin:

.. code-block:: rust

   a = ---3;

Burada işlemler şu sırada yapılacaktır:

.. code-block:: text

   İ1: -3 => -3
   İ2: -İ1 => 3
   İ3: -İ2 => -3
   İ4: a = İ3 => ()

Artırma ve Eksiltme Operatörleri Yoktur
=======================================

C'de, Java'da ve C#'ta bulunan ``++`` ve ``--`` operatörleri Rust'ta yoktur. Aslında fonksiyonel programlama modelini
(functional programming paradigm) destekleyen yeni programlama dillerinin bazılarına da bu operatör sokulmamıştır.
Örneğin bu operatörler Python'da da yoktur. Swift'te eskiden vardı ancak 2.0 versiyonu ile birlikte dilden çıkartıldı.
Bu operatörlerin ifade içerisinde yan etkiye yol açması fonksiyonel programlamaya uygun değildir.

Karşılaştırma Operatörleri
==========================

Rust'ta diğer dillerin çoğunda da var olan altı karşılaştırma operatörü bulunmaktadır. Bu operatörlerin hepsi iki
operand'lı araek (binary infix) operatörlerdir. Karşılaştırma operatörleri Rust'ta bool türden değer üretmektedir.
Halbuki C'de karşılaştırma operatörlerinin int türden değer ürettiğini anımsayınız. C++ gibi, Java, C# ve Python gibi
bool türünün olduğu programlama dillerinde de karşılaştırma operatörleri bool değer üretmektedir. Örneğin:

.. code-block:: rust

   let a:i32 = 10;
   let b:i32 = 20;
   let result: bool;

   result = a < b;
   println!("{}", result);     // true

C'de ve C++'ta karşılaştırma operatörleri de kombine edilerek kullanılabilmektedir. Örneğin:

.. code-block:: c

   result = a == b > c;

Böyle bir ifade C'de geçerlidir. Burada önce ``b > c`` işlemi yapılır. Buradan int türden ``1`` ya da ``0`` değeri elde
edilir. Bu int değer a ile karşılaştırılır. Ancak Rust'ta bu operatörler parantezsiz biçimde birbirleriyle kombine
edilerek kullanılamamaktadır. C'de ``>``, ``<``, ``>=``, ``<=`` operatörlerinin öncelik tablosunda ``==``, ``!=``
operatörlerinden daha yüksek öncelikte bulunduğunu anımsayınız. Rust'ta zaten bunlar parantezsiz biçimde kombine
edilemediği için bunların aralarında da bir öncelik ilişkisi de yoktur. Python gibi bazı dillerde karşılaştırma
operatörlerinin kombine edilmesi ise tamamen başka bir anlama gelmektedir. Örneğin:

.. code-block:: python

   result = a == b > c

Bu ifade Python'da aşağıdaki ile eşdeğerdir:

.. code-block:: python

   result = a == b and b > c

Programlama dillerinin büyük çoğunluğunda karşılaştırma operatörleri aritmetik operatörlerden düşük önceliktedir.
Rust'ta da durum böyledir. Örneğin:

.. code-block:: rust

   result = a + b > c + d;

Burada ``a + b`` ile ``c + d`` karşılaştırılmaktadır.

======================== ==============================
Operatörler              Associativity
======================== ==============================
``()``                   Soldan Sağa
``-``                    Sağdan Sola
``* / %``                Soldan Sağa
``+ -``                  Soldan Sağa
``< > >= <= == !=``      Parantezsiz Kombine Edilemez
``=``                    Sağdan Sola
======================== ==============================

Tüm iki operand'lı aritmetik operatörlerde olduğu gibi karşılaştırma operatörlerinin de operand'larının aynı türden
olması gerekir. Aksi takdirde error oluşur. Örneğin:

.. code-block:: rust

   let a: i32 = 10;
   let b: i16 = 20;
   let result: bool;

   result = a > b;     // error! operand'lar farklı türlerden

Rust'ta karşılaştırma operatörlerinin operand'ları bool türden de olabilmektedir. Karşılaştırma ``true`` değerinin
``false`` değerinden büyük olduğu kabulü ile yapılmaktadır. Örneğin:

.. code-block:: rust

   let mut result: bool;

   result = true > false;
   println!("{}", result);     // true

   result = true == true;
   println!("{}", result);     // true

Karşılaştırma operatörlerinin parantezsiz kombine edilemediğini belirtmiştik. Ancak parantezler kullanılarak bir
karşılaştırma operatörünün ürettiği bool değer diğer bir karşılaştırma operatörüne operand yapılabilmektedir. Örneğin:

.. code-block:: rust

   let result: bool;

   result = (true > false) == true;
   println!("{}", result);     // true

Ancak böylesi kullanımlara çok seyrek gereksinim duyulmaktadır.

Mantıksal Operatörler
=====================

Rust'ta da tıpkı C, Java ve C#'ta olduğu gibi üç mantıksal operatör vardır:

============ ==========================
Operatör     Açıklama
============ ==========================
``!``        Mantıksal NOT operatörü
``&&``       Mantıksal AND operatörü
``||``       Mantıksal OR operatörü
============ ==========================

``&&`` ve ``||`` operatörleri iki operand'lı araek (binary infix), ``!`` operatörü ise tek operand'lı önek (unary
prefix) operatörlerdir. ``&&`` ve ``||`` operatörleri öncelik tablosunda karşılaştırma operatörlerinden daha düşük
öncelikli, ``!`` operatörü ise aritmetik operatörlerden daha yüksek önceliklidir:

======================== ==============================
Operatörler              Associativity
======================== ==============================
``()``                   Soldan Sağa
``- !``                  Sağdan Sola
``* / %``                Soldan Sağa
``+ -``                  Soldan Sağa
``< > >= <= == !=``      Parantezsiz Kombine Edilemez
``&&``                   Soldan Sağa
``||``                   Soldan Sağa
``=``                    Sağdan Sola
======================== ==============================

Bu operatörlerin operand'ları bool türden olmak zorundadır. Bu operatörler bool türden değer üretirler.

Kısa Devre (Short Circuit) Özelliği
-----------------------------------

``&&`` ve ``||`` operatörlerinin diğer dillerde olduğu gibi *kısa devre (short circuit)* özelliği vardır. Bu
operatörlerin önce sol tarafındaki ifade yapılır. Eğer ``&&`` operatörünün sol tarafındaki ifade ``false`` ise, ``||``
operatörünün sağ tarafındaki ifade ``true`` ise bunların sağ tarafındaki ifade hiç yapılmaz, sonuç hemen belirlenir.
Tabii ``&&`` operatörünün solundaki ifade ``true`` ise ``||`` operatörünün sol tarafındaki ifade ``false`` ise gerçekten
bu operatörlerin sağ tarafındaki ifadeler de yapılır. Örneğin:

.. code-block:: rust

   result = foo() || bar();

Burada ``foo`` ve ``bar`` fonksiyonlarının geri dönüş değerlerinin bool türden olması zorunludur. Önce ``foo``
fonksiyonu çağrılır. Eğer bu fonksiyondan ``true`` değeri elde edilirse ``bar`` fonksiyonu hiç çağrılmaz.

``&&`` ve ``||`` operatörleri kombine edildiğinde soldaki operatörün sol tarafı önce yapılmaktadır. Bu durum kişilere
tuhaf gelebilmektedir. Örneğin:

.. code-block:: rust

   result = foo() || bar() && tar();

Her ne kadar ``&&`` operatörü ``||`` operatöründen daha yüksek öncelikliyse de burada önce ``foo`` fonksiyonu çağrılır.
Eğer ``foo`` ``true`` değere geri dönerse diğer fonksiyonlar hiç çağrılmaz. Eğer ``foo`` ``false`` değerine geri dönerse
bu durumda ``bar`` fonksiyonu çağrılır. ``bar`` da ``true`` değerine geri dönerse ``tar`` fonksiyonu çağrılır. Ancak
``bar`` ``false`` değerine geri dönerse ``tar`` fonksiyonu çağrılmaz. Bu durum size ``&&`` operatörünün ``||``
operatörüne göre öncelikli olmasıyla çelişiyormuş gibi gelebilir. Ancak aslında önce ``&&`` operatörünün yapılmasıyla
önce ``||`` operatörünün sol tarafının yapılması arasında elde edilen sonuç bakımından bir farklılık yoktur. Önce
``foo`` fonksiyonun çağrılması bu sonucun daha hızlı elde edilmesine yol açmaktadır. Biz önce ``&&`` sonra ``||``
operatörünün işletildiğini varsayalım:

.. code-block:: text

   İ1: bar() && tar()
   İ2: foo() || İ1
   İ3: result = İ2

``&&`` operatörünün ve ``||`` operatörünün önce sol tarafı yapılmak zorundadır. Bunun da tek yolu aslında önce ``foo``
fonksiyonun çağrılmasıdır. Örneğin:

.. code-block:: rust

   result = foo() && bar() || tar();

Burada da önce ``foo`` fonksiyonu çağrılır. Eğer ``foo`` ``false`` değerine geri dönerse ``bar`` fonksiyonu çağrılmaz
ama ``tar`` fonksiyonu çağrılır. Eğer ``foo`` ``true`` değerine geri dönerse ``bar`` fonksiyonu çağrılır. ``bar`` da
``true`` değerine geri dönerse ``tar`` fonksiyonu çağrılmaz.

Kısa Devre Özelliği Olmayan Mantıksal İşlemler
----------------------------------------------

Rust'ta tıpkı Java ve C#'ta olduğu gibi mantıksal işlemler kısa devre özelliği olmadan da yapılabilmektedir. Bunun için
``&`` ve ``|`` operatörleri kullanılmaktadır. Bu operatörler aslında bit düzeyinde (bitwise) işlem yapmaktadır. Ancak bu
operatörler eğer tamsayı türlerine ilişkin operand almayıp bool operand alırlarsa mantıksal işlem yaparlar. Fakat bu
durumda kısa devre özelliğine sahip olmazlar. Dolayısıyla Rust'ta ``&`` operatörü ``&&`` operatörünün kısa devre
özelliği olmayan biçimi gibi, ``|`` operatörü de ``||`` operatörünün kısa devre özelliği olmayan biçimi gibi
kullanılabilmektedir. Örneğin:

.. code-block:: rust

   result = foo() | bar() & tar();

Burada kısa devre özelliği olmadığı için ``foo``, ``bar`` ve ``tar`` fonksiyonları önce çağrılır. Sonra ``&`` işlemi ve
``|`` işlemi yapılır. Dolayısıyla mantıksal işlemler kısa devre özelliksiz yapılmış olur.

Mantıksal NOT İşlemi
--------------------

Rust'ta ``!`` operatörü mantıksal NOT işleminin yanı sıra bitsel NOT işlemi de yapmaktadır. Eğer ``!`` operatörünün
operand'ı bool türdense operatör ``true`` için ``false``, ``false`` için ``true`` değerini üretir. ``!`` operatörünün
sağdan sola öncelik grubunda olduğuna dikkat ediniz. Örneğin:

.. code-block:: rust

   result = !!!true

.. code-block:: text

   İ1: !true => false
   İ2: !İ1 => true
   İ3: !İ2 => false
   İ4: result = İ3

Aşağıdaki örneği temel alarak yukarıda anlattıklarımız üzerinde denemeler yapabilirsiniz.

.. code-block:: rust

   fn main() {
       let result: bool;

       result = foo() || bar() && tar();
       println!("{}", result);
   }

   fn foo() -> bool {
       println!("foo");
       false
   }

   fn bar() -> bool {
       println!("bar");
       false
   }

   fn tar() -> bool {
       println!("tar");
       false
   }

Bit Operatörleri
================

Rust'ta bit düzeyinde işlem yapan (bitwise) altı operatör vardır:

============ ==========================================
Operatör     Açıklama
============ ==========================================
``!``        Bit NOT operatörü
``<< >>``    Sola öteleme ve sağa öteleme operatörleri
``&``        Bit AND operatörü
``^``        Bit EXOR operatörü
``|``        Bit OR operatörü
============ ==========================================

Bu operatörlerin C'de, Java ve C#'ta da bulunduğuna dikkat ediniz. Ancak yukarıda da belirttiğimiz gibi bit düzeyinde
NOT işlemi için C'deki ``~`` operatörü yerine yine ``!`` operatörü kullanılmıştır. Bit operatörleri sayıları bir bütün
olarak değil bit bit ele alarak işlem yapmaktadır. ``!`` operatörü yukarıda da belirttiğimiz gibi tek operand'lı önek
(unary prefix) bir operatördür. Ancak diğer bit operatörlerinin hepsi iki operand'lı araek (binary infix)
operatörlerdir.

C'de (ve tabii C++'ta) ``&``, ``^`` ve ``|`` operatörleri karşılaştırma operatörlerinden daha düşük önceliklidir. Bu
durum C'de uzun süredir eleştirilmektedir. Aşağıdaki C koduna dikkat ediniz:

.. code-block:: c

   if (val & 1 == 0) {
       //...
   }
   else {
       //...
   }

Burada programcı muhtemelen val değişkeninin en düşük anlamlı bitinin ``0`` olup olmadığını kontrol etmek istemiştir.
Ancak C'de ``==`` operatörü ``&`` operatöründen daha yüksek öncelikli olduğu için burada önce ``1 == 0`` karşılaştırması
yapılıp buradan ``0`` değeri elde edilir. ``val & 0`` işleminden de ``0`` elde edileceği için niyet edilen işlemler
gerçekleşmez. C'de bu karşılaştırmanın paranteze alınarak yapılması gerekmektedir:

.. code-block:: c

   if ((val & 1) == 0) {
       //...
   }
   else {
       //...
   }

Halbuki Rust'ta bu üç operatör karşılaştırma operatörlerinden daha yüksek önceliğe sahiptir. Dolayısıyla Rust'ta bu
biçimde paranteze almaya gerek kalmamaktadır.

Rust'ta yine tıpkı C'de olduğu gibi bit operatörlerinin de operand'larının tamsayı türlerine ilişkin olması gerekir.

=================================================
Bit, Öteleme, Atama ve Bileşik Atama Operatörleri
=================================================

Bit AND, OR ve EXOR Operatörleri
================================

``&`` operatörü iki tamsayının karşılıklı bitlerini AND işlemine, ``|`` operatörü ise tamsayının karşılıklı bitlerini OR
işlemine sokmaktadır. Bu operatörlerin operand'larının yine aynı türden olması gerekir. Örneğin:

.. code-block:: rust

   let a: u8 = 0xC5;       // 1100 0101
   let b: u8 = 0x3C;       // 0011 1100
   let mut result: u8;

   result = a & b;
   println!("{:x}", result);     // 4

   result = a | b;
   println!("{:X}", result);     // FD

Genellikle programcılar bit düzeyinde AND ve OR işlemlerini işaretsiz tamsayı türler üzerinde yaparlar. Ancak işaretli
sayılar üzerinde de bu işlemler uygulanabilmektedir. Burada işaretli bir tamsayı türünden değişkene değer atarken bir
noktaya dikkatinizi çekmek istiyoruz. Aşağıdaki bağlamaya dikkat ediniz:

.. code-block:: rust

   let a: i8 = 0xC3;       // error

Bu bağlama error ile sonuçlanacaktır. Çünkü sayının kaçlık sistemde yazıldığının sayının türü üzerinde bir etkisi
yoktur. Buradaki ``0xC3`` yazmakla ``195`` yazmak arasında hiçbir farklılık yoktur. ``195`` de hedef türün sınırları
içerisinde kalmadığı için error oluşmaktadır.

Bilindiği gibi EXOR işlemi bitler aynıyken ``0`` değerini, bitler farklıyken ``1`` değerini veren bir işlemdir. EXOR
geri dönüşümlü bir işlem olduğu için özellikle kriptoloji alanında yaygın biçimde kullanılmaktadır. ``a ^ b = c`` ise,
``c ^ a = b`` ve ``c ^ b = a``'dır.

Rust'ta ``^`` operatörünün iki operand'ı da bool türdense EXOR işlemi bool düzeyde yapılmaktadır. Örneğin ``true ^
true`` işlemi geçerlidir ve burada ``false`` değeri elde edilir. ``true ^ false`` işlemi geçerlidir buradan ``true``
değeri elde edilir.

Öteleme Operatörleri
====================

Sağa Öteleme
------------

Rust'ta da C'de (Java ve C#'ta da) olduğu gibi ``>>`` (sağa öteleme) ve ``<<`` (sola öteleme) operatörleri
bulunmaktadır. Bu operatörler de iki operand'lı araek operatörlerdir. İşaretsiz bir tamsayı sağa bir kez ötelendiğinde
tüm bitler bir sağa kaydırılır sayı en soldan ``0`` ile beslenir. Bu da aslında sayının 2'ye bölünmesi anlamına
gelmektedir. İşaretli negatif tamsayıların sağa ötelenmesi biraz kafa karıştırabilmektedir. Çünkü bu durumda eğer en
soldan ``0`` ile besleme yapılırsa negatif sayı pozitif haline gelir. Negatif sayının negatifliğinin korunması için en
soldan ``1`` ile beslenmesi gerekir. Negatif tamsayıların en soldan ``1`` ile beslenmesine makine dillerinde *arithmetic
right shift* denilmektedir. C Programlama Dilinde işaretli negatif tamsayıların sağa ötelenmesinde beslemenin en soldan
``0`` ile mi yoksa ``1`` ile mi yapılacağı derleyicileri yazanların isteğine bırakılmıştır (implementation dependent).
Ancak yaygın derleyicilerin hemen hepsi en soldan beslemeyi ``1`` ile yapmaktadır. En soldan besleme ``1`` ile
yapıldığında sayı negatifliği korunarak 2'ye bölünmüş olur. Örneğin bir byte içerisinde (yani i8 olarak) ``-10``
değerini yazalım. Bunu önce ``+10`` yazıp 2'ye tümleyenini alarak yapabiliriz:

================= ========
Bit Düzeni        Değer
================= ========
``0000 1010``     ``+10``
``1111 0110``     ``-10``
================= ========

Şimdi bu ``-10`` değerini işaret bitini koruyarak ``1`` kez sağa öteleyelim:

================= ========
Bit Düzeni        Değer
================= ========
``1111 1011``     ``-5``
================= ========

Bu sayı ``-5``'tir. Sayının ``-5`` olduğunu 2'ye tümleyenini alarak anlayabilirsiniz:

================= ========
Bit Düzeni        Değer
================= ========
``0000 0101``     ``+5``
================= ========

Ancak işaretli negatif tamsayı eğer tek ise kişileri tereddüte sevkeden bir durum oluşmaktadır. ``-5`` sayısının
aritmetik biçimde sağa ötelenmesi sonucunda ``-2`` değil ``-3`` değeri elde edilmektedir. Örneğin:

================= ========
Bit Düzeni        Değer
================= ========
``1111 1011``     ``-5``
================= ========

Bu sayıyı sağa ötelediğimizde şu bitleri elde ederiz:

================= ========
Bit Düzeni        Değer
================= ========
``1111 1101``     ``-3``
================= ========

Bu değerin ``-3`` olduğunu sayının 2'ye tümleyenini alarak anlayabiliriz:

================= ========
Bit Düzeni        Değer
================= ========
``0000 0011``     ``+3``
================= ========

Her ne kadar C'de işaretli negatif tamsayıların sağa ötelenmesi derleyiciye bağlı olarak değişebiliyorsa da Rust'ta
işaretli negatif tamsayılar sağa ötelendiğinde her zaman en soldan besleme işaret biti korunacak biçimde ``1`` ile
yapılmaktadır. Örneğin:

.. code-block:: rust

   let a: i8 = -10;            // 1100 0101
   let mut result: i8;

   result = a >> 1;
   println!("{}", result);     // -5

   result = a >> 2;
   println!("{}", result);     // -3

Sola Öteleme
------------

Bir tamsayı sola bir kez ötelendiğinde tüm bitler bir sola kaydırılır, sayı en sağdan ``0`` ile beslenir. Bu da sayıyı 2
ile çarpmak anlamına gelmektedir. Rust'ta sayı ister işaretli olsun isterse işaretsiz olsun sola öteleme aynı biçimde
yapılmaktadır ve taşma dikkate alınmamaktadır. Oysa C'de işaretli bir tamsayı sola ötelenirken taşma olursa bu durumun
*tanımsız davranışa (undefined behavior)* yol açtığını anımsayınız. C'de de işaretsiz tamsayılarda her zaman bitler bir
sola ötelenmektedir. Örneğin:

.. code-block:: rust

   let a: u8 = 0x78;            // 120
   let result: u8;

   result = a << 1;
   println!("{}", result);     // 240 = F0

Tabii işaretli bir tamsayı sola ötelenirken taşma nedeniyle işaretini de değiştirebilir. Örneğin:

.. code-block:: rust

   let a = 120;                // hex 78
   let result: i8;

   result = a << 1;
   println!("{}", result);     // -16 hex F0

Öteleme Miktarı
---------------

Rust'ta genel olarak iki operand'lı operatörlerin operand'larının aynı türden olması gerektiğini belirtmiştik. Ancak
öteleme operatörleri buna bir istisna oluşturmaktadır. Öteleme operatörlerinde ötelenecek miktar (yani sağ taraftaki
operand) herhangi bir tamsayı türünden olabilir. Ancak öteleme miktarı (yani sağdaki operand'ın değeri) negatifse ya da
ötelenecek türün bit uzunluğunu aşarsa panic oluşmaktadır. Örneğin:

.. code-block:: rust

   fn main() {
       foo(-5);     // panic oluşur!
       foo(10);     // sorun yok
   }

   fn foo(n: i32) {
       let a: u8 = 1;
       let result: u8;

       result = a << n;
       println!("{}", result);
   }

*The Rust Reference* dokümanlarına göre ötelenecek değerde (yani sağ taraftaki operand'da) yukarıda bahsettiğimiz
anomali eğer derleme aşamasında tespit edilebiliyorsa derleyicileri yazanların isteğine bağlı olarak derleyici error de
oluşturabilmektedir.

Bit NOT İşlemi
==============

Mantıksal operatörleri ele aldığımız paragrafta da belirttiğimiz gibi Rust'ta ``!`` operatörü hem mantıksal NOT hem de
bit düzeyinde NOT işlemi yapmaktadır. Yani Rust'ta C'deki bit NOT işlemi için bir ``~`` operatörü bulunmamaktadır.
Örneğin:

.. code-block:: rust

   let a: u8 = 0xC5;               // 1100 0101
   let result: u8;

   result = !a;
   println!("{:X}", result);         // 0011 1010 = 3A

Rust'ta işaretli tamsayılar üzerinde de ``!`` operatörü uygulanabilmektedir. Örneğin:

.. code-block:: rust

   let a: i8 = 0x1A;               // 0001 1010
   let result: i8;

   result = !a;
   println!("{:X}", result);         // 1110 1010 = EA
   println!("{}", result);           // -27

Bit Operatörlerinin Öncelik Tablosu
===================================

``&``, ``|`` ve ``^`` operatörleri kendi aralarında öncelik tablosunda pek çok programlama dilinde şu sırada
bulunmaktadır:

============ ==============
Operatörler  Associativity
============ ==============
``&``        Soldan Sağa
``^``        Soldan Sağa
``|``        Soldan Sağa
============ ==============

``!`` operatörünün tek operand'lı operatörlerin grubunda olduğunu görmüştük. ``<<`` ve ``>>`` operatörleri pek çok
programlama dilinde hemen aritmetik operatörlerden sonraki satırdadır. Rust'ta (tıpkı Java ve C#'ta olduğu gibi) ``&``,
``|`` ve ``^`` operatörleri karşılaştırma operatörlerinden daha yüksek önceliğe konumlandırılmıştır. Konuya girişte de
belirttiğimiz gibi C'de durum böyle değildir. Örneğin:

.. code-block:: rust

   if x & 1 == 0 {
       //...
   }
   else {
       //...
   }

Burada önce ``x & 1`` işlemi yapılıp daha sonra karşılaştırma yapılmaktadır. Halbuki C'de önce ``1 == 0`` işlemi yapılıp
bunun sonucu ``&`` işlemine sokulmaktadır.

Bu operatörleri de yerleştirdiğimizde Rust'ta görmüş olduğumuz operatör için öncelik tablosu şöyle olacaktır:

======================== ==============================
Operatörler              Associativity
======================== ==============================
``()``                   Soldan Sağa
``- !``                  Sağdan Sola
``* / %``                Soldan Sağa
``+ -``                  Soldan Sağa
``<< >>``                Soldan Sağa
``&``                    Soldan Sağa
``^``                    Soldan Sağa
``|``                    Soldan Sağa
``< > >= <= == !=``      Parantezsiz Kombine Edilemez
``&&``                   Soldan Sağa
``||``                   Soldan Sağa
``=``                    Sağdan Sola
======================== ==============================

Atama Operatörü
===============

Atama operatörü iki operand'lı araek bir operatördür. Ancak Rust'ta atama işlemi *taşıma* ya da *sahipliği devretme*
işlemlerini de yapmaktadır. Bu nedenle Rust'taki atama operatörü diğer dillerdeki atama operatöründen daha farklı bir
semantiğe sahiptir. Rust'ta yapı gibi bazı türler hedefe atandığı zaman tuttukları kaynakları taşıyabilecek biçimde bir
semantik uygulanmaktadır. Ancak atamanın yapıldığı hedef değişken üzerinde -eğer hedef değişken Copy türünden değilse-
*bırakma (drop)* işlemi gerçekleşmektedir. Rust'a özgü olan bu konu Rust için oldukça önemlidir. Biz bu konuyu ayrı
başlık halinde ele alacağız. Ancak bu konu C++'taki *taşıma atama operatör fonksiyonuna (move assignment operator
function)* oldukça benzemektedir. C++11 ile eklenen bu özelliğe İngilizce *move semantic* denilmektedir. Şimdiye kadar
gördüğümüz i32, i64, f64 gibi temel türler atama sırasında tıpkı C'de olduğu gibi kopyalanmaktadır. Temel türlerin Copy
trait'ini desteklediği varsayılmaktadır. Örneğin:

.. code-block:: rust

   a = b;

Burada örneğin eğer a ve b birer yapı türünden değişken ise atama işlemi sırasında önce a bırakılır (drop edilir) sonra
b'nin sahipliği a'ya devredilir. Yani b'nin içerisindeki bilgiler a'ya taşınır. Bu konu ileride ayrıntılarıyla zaten ele
alınacaktır.

Rust'ta atama operatörü kombine edilemez. Örneğin aşağıdaki gibi zincirli atama geçerli değildir:

.. code-block:: rust

   let a: i32;
   let b: i32;
   let c: i32 = 10;

   a = b = c;          // error!

Bu durum geçerli olsaydı c'nin sahipliği b'ye, b'nin sahipliği de a'ya aktarılırdı. Bu durumda bu işlemin ``a = c;``
işleminden bir farkı kalmazdı. Dolayısıyla ``a = b = c;`` gibi bir işlem anlamsız olduğu gerekçesiyle yasaklanmıştır.
Aslında teknik olarak Rust'ta atama operatörü birim (unit) değerini (yani ``()`` değerini) üretmektedir. Biz birim
değerini de ancak birim türünden bir değişkene atayabiliriz. Aşağıdaki atama geçerli fakat anlamlı değildir:

.. code-block:: rust

   let a: ();
   let b: i32;
   let c: i32 = 20;

   a = b = c;          // b = c işleminden () elde edilir

   println!("{:?}, {}, {}", a, b, c);          // (), 20, 20

Burada ``b = c`` işleminden birim değeri elde edildiği için a da birim türünden olduğu için ``b = c`` işleminin sonucu
a'ya atanabilmiştir. Ancak işleme bir bütün olarak baktığımızda anlamlı gözükmemektedir.

Atama operatörünün öncelik tablosunun en sonunda (her ne kadar kombine etmek anlamsızsa da) sağdan sola grupta
bulunduğuna dikkat ediniz.

Bileşik Atama Operatörleri
==========================

Rust'ta da tıpkı C, Java ve C#'ta olduğu gibi *bileşik atama operatörleri de (compound assignment operators)*
bulunmaktadır. Bunların listesi şöyledir:

.. code-block:: text

   += -= *= /= %= &= |= ^= <<= >>=

op bir operatör olmak üzere ``a op= b`` tamamen ``a = a op b`` ile eşdeğerdir. Bileşik atama operatörleri öncelik
tablosunda atama operatörüyle sağdan sola aynı gruptadır:

========================================== ==============================
Operatörler                                Associativity
========================================== ==============================
``()``                                     Soldan Sağa
``- !``                                    Sağdan Sola
``* / %``                                  Soldan Sağa
``+ -``                                    Soldan Sağa
``<< >>``                                  Soldan Sağa
``&``                                      Soldan Sağa
``^``                                      Soldan Sağa
``|``                                      Soldan Sağa
``< > >= <= == !=``                        Parantezsiz Kombine Edilemez
``&&``                                     Soldan Sağa
``||``                                     Soldan Sağa
``= += -= *= /= %= &= |= ^= <<= >>=``      Sağdan Sola
========================================== ==============================

Bileşik atama operatörlerinden de yine birim (unit) değeri (yani ``()`` değeri) elde edilmektedir. Yani bu operatörler
de kombine edilememektedir.

==================
Tür Dönüştürmeleri
==================

Tür Dönüştürmelerine Giriş
==========================

Bu bölümde Rust'ta tür dönüştürmelerini göreceğiz. Anımsanacağı gibi Rust'ta bir atama işlemi söz konusu olduğunda
kaynak türle hedef türün aynı olması gerekiyordu. (Bunun referanslar ve göstericiler bağlamında bazı istisnalar vardır.)
Tabii programlama dillerinin büyük çoğunluğunda ilkdeğer verme de, fonksiyon çağırılırken argümanlardan parametre
değişkenlerine aktarım da, fonksiyonun geri dönüş değerinin oluşturulması da bir çeşit atama işlemi gibi ele
alınmaktadır. Yani bu bağlamlarda da atama işlemindeki semantik uygulanmaktadır. Dolayısıyla Rust'ta nasıl atama
operatörünün kaynak ve hedef türleri aynı olmak zorundaysa argümanın türüyle parametre değişkeninin türünün, return
ifadesinin türüyle fonksiyonun geri dönüş değeri türünün de aynı olması gerekmektedir.

as Operatörü
============

Rust'ta farklı türleri birbirine atamak için ya da iki operand'lı operatörlerle işleme sokmak için programcının as
operatörü ile açıkça tür dönüştürmesi yapması gerekmektedir. as operatörü iki operand'lı araek (binary infix) bir
operatördür. Operatörün sol tarafındaki operand dönüştürülecek ifadeyi sağ tarafındaki operand dönüştürülecek türü
belirtmektedir. Örneğin:

.. code-block:: rust

   let a: i32 = 10;
   let b: i16;

   b = a as i16;

Burada a değişkeni açıkça (explicitly) i16 türüne dönüştürülmüştür. Tabii programlama dillerinde genel olarak tür
dönüştürmesi hep geçici nesne yoluyla yapılmaktadır. Yani biz bir ifadeyi bir türe dönüştürürken önce derleyici hedef
tür türünden geçici bir nesne oluşturur, kaynak değeri bu geçici nesneye atar, işlemde bu geçici nesneyi kullanır, 
işlem bitince de geçici nesneyi yok eder. Örneğin:

.. code-block:: rust

   b = a as i16;

Burada a'nın türü i16 olarak değiştirilmemektedir. i16 türünden geçici bir değişken yaratılıp a'nın içerisindeki değer 
o değişkene atanıp işlemde o değişken kullanılmaktadır. Tabii derleyiciler buradaki geçici değişkeni olanak elveriyorsa
CPU'nun yazmaçlarında yaratmaktadır. Yani bu geçici değişkenlerin yaratılması genellikle bir performans kaybına yol
açmaz.

Rust'ta iki operand'lı operatörlerin operand'larının da aynı türden olması gerektiğini anımsayınız. Aşağıdaki toplama
işlemi operand'lar farklı türlerden olduğu için error ile sonuçlanacaktır:

.. code-block:: rust

   let a: i32 = 10;
   let b: i16 = 20;
   let result: i32;

   result = a + b;       // error!

Bu tür durumlarda da bir operand'ın açıkça diğerinin türüne as operatörüyle dönüştürülmesi gerekmektedir. Örneğin:

.. code-block:: rust

   let a: i32 = 10;
   let b: i16 = 20;
   let result: i32;

   result = a + b as i32;          // geçerli
   print!("{} ", result);          // 30

as operatörü öncelik tablosunda aritmetik operatörlerden daha yüksek öncelikli grupta bulunmaktadır.

========================================== ==============================
Operatörler                                Associativity
========================================== ==============================
``()``                                     Soldan Sağa
``- !``                                    Sağdan Sola
``as``                                     Soldan Sağa
``* / %``                                  Soldan Sağa
``+ -``                                    Soldan Sağa
``<< >>``                                  Soldan Sağa
``&``                                      Soldan Sağa
``^``                                      Soldan Sağa
``|``                                      Soldan Sağa
``< > >= <= == !=``                        Parantezsiz Kombine Edilemez
``&&``                                     Soldan Sağa
``||``                                     Soldan Sağa
``= += -= *= /= %= &= |= ^= <<= >>=``      Sağdan Sola
========================================== ==============================

Örneğin:

.. code-block:: rust

   result = a + b as i16;

işleminde operatörler şu sırada işleme sokulacaktır:

.. code-block:: text

   İ1: b as i16
   İ2: a + İ1
   İ3: result = İ2

Eğer bir işlem sonucunu bir türe dönüştürmek istiyorsanız parantez kullanmalısınız:

.. code-block:: rust

   c = (a + b) as i32;

Bilgi Kaybına Yol Açan ve Açmayan Dönüştürmeler
===============================================

Bazı dönüştürmeler bilgi kaybına yol açmazlar. Örneğin i16 türünden bir ifadenin ``i32`` türüne dönüştürülmesi sırasında
bir bilgi kaybı oluşmaz. Genel olarak küçük türden büyük türe yapılan dönüştürmeler bilgi kaybına neden olmamaktadır. 
C gibi bazı programlama dillerinde temel türlerin her biri diğerine otomatik olarak (implicitly) dönüştürülebilmektedir. 
Java ve C# gibi bazı dillerde ise yalnızca bilgi kaybına yol açmayacak dönüştürmeler otomatik yapılabilmektedir. Örneğin
Java ve C#'ta int türünden ``long`` türüne otomatik dönüştürme olduğu için ``int`` türü ``long`` türüne atanabilmektedir.
Ancak ``long`` türünden int türüne otomatik dönüştürme olmadığı için ``long`` türü ``int`` türüne atanamamaktadır. İşte
tür dönüştürmeleri ele alınırken bilgi kaybına yol açabilecek dönüştürmelerin nasıl gerçekleştiği üzerinde durulmalıdır.
Biz de izleyen paragraflarda tek tek bu durumun üzerinde duracağız.

Tamsayı Türleri Arasındaki Dönüştürmeler
========================================

* Küçük işaretli tamsayı türünden büyük işaretli tamsayı türüne, küçük işaretsiz tamsayı türünden büyük işaretsiz
  tamsayı türüne yapılan dönüştürmelerde bilgi kaybı oluşmaz. Kaynak değer aynı biçimde hedef türle ifade edilir.

* Aynı tamsayı türünün işaretli ve işaretsiz biçimler arasında yapılan dönüştürmelerde sayının bitleri değişmez,
  yalnızca işaret bitinin anlamı değişir. Örneğin:

.. code-block:: rust

   let a: i32 = -1;            // 0xFFFFFFFF
   let b: u32;

   b = a as u32;
   println!("{}", b);      // 4294967295

``i32`` içerisindeki ``-1`` değeri tüm bitler ``1`` olan bir değerdir. Tüm bitleri ``1`` olan bir sayı ``u32`` türünden 
bir değişkenin içerisine yerleştirilirse bu değer en büyük pozitif sayı haline gelir. Örneğin:

.. code-block:: rust

   let a: u32 = 4294967294;            // 0xFFFFFFFE
   let b: i32;

   b = a as i32;
   println!("{}", b);                  // -2

* Büyük tamsayı türünden (işaretli ya da işaretsiz) küçük tamsayı türüne (işaretli ya da işaretsiz) dönüştürmelerde
  sayının yüksek anlamlı bitleri atılır, düşük anlamlı bitleri atanır. Ancak eğer kaynak türe ilişkin değer hedef türün
  sınırları içerisinde kalıyorsa bir bilgi kaybı söz konusu olmaz.

Örneğin:

.. code-block:: rust

   let a: i32 = -2;            // 0xFFFFFFFE
   let b: i16;

   b = a as i16;
   println!("{}", b);          // -2

Burada ``i32`` içerisindeki ``-2`` değeri ``0xFFFFFFFE`` bitlerine sahip olacaktır. Biz yüksek anlamlı 2 byte'ı atıp 
düşük anlamlı 2 byte'ı elde edersek ``0xFFFE`` bitleri oluşur. Bu da zaten ``i16`` türü için ``-2``'dir. Görüldüğü gibi 
``i32`` içerisindeki ``-2`` değeri ``i16`` ile temsil edilebildiğinden dolayı bilgi kaybı söz konusu olmamıştır. 
Örneğin:

.. code-block:: rust

   let a: i32 = -2;            // 0xFFFFFFFE
   let b: u8;

   b = a as u8;
   println!("{}", b);          // 254

Burada yine ``i32`` içerisindeki değerin yüksek anlamlı byte'ları atılıp düşük anlamlı byte'ları elde edilmiştir. Bu da
``0xFE`` bitleridir. Tabii bu bitler işaretsiz 1 byte'lık bir değişkenin içerisine yerleştirildiği için ``254`` olarak
ele alınacaktır.

* Küçük işaretli tamsayı türünden büyük işaretli ya da büyük işaretsiz türe dönüştürme yapılırken önce küçük işaretli
  türdeki bitlerin hepsi büyük tamsayı türünün düşük anlamlı bitlerine yerleştirilir. Sonra geri kalan bitler kaynak
  türe ilişkin değer negatif ise ``1``'lerle pozitif ise ``0``'larla doldurulur. Örneğin:

.. code-block:: rust

   let a: i8 = -1;             // 0xFF
   let b: u32;

   b = a as u32;
   println!("{}", b);          // 4294967295 = 0xFFFFFFFF

Burada kaynak türdeki değer işaretli ve negatiftir. 1 byte içerisinde ``-1`` değeri ``0xFF`` bitlerinden oluşmaktadır.
Buradaki sayı negatif olduğu için ``u32``'ye dönüştürme yapılırken geri kalan 3 byte ``1``'lerle doldurulacaktır.
Dolayısıyla sayı çok büyük pozitif bir sayı haline gelmektedir. Burada ``i8`` içerisindeki ``-1`` değerinin ``u32`` 
türüne ``+1`` olarak dönüştürülmediğine dikkat ediniz. Tabii yukarıdaki örnekte küçük türle belirtilen değer pozitif 
olsaydı doldurma ``0`` bitleriyle yapılacak ve aslında yine aynı pozitif sayı elde edilecektir. Örneğin:

.. code-block:: rust

   let a: i8 = 1;              // 0x01
   let b: u32;

   b = a as u32;
   println!("{}", b);          // 0x00000001 = 1

Aslında C'de de aynı durum söz konusudur. Yalnızca C standartlarında anlatım değişik yapılmıştır. C standartlarında
küçük işaretli tamsayı türünden büyük işaretsiz tamsayı türüne dönüştürmenin iki aşamada yapılacağı söylenmiştir: Önce
kaynak değer hedef türün işaretli biçimine sonra hedef türün işaretli biçiminden işaretsiz biçimine dönüştürülmektedir.

Gerçek Sayı Türleri ile Tamsayı Türleri Arasındaki Dönüştürmeler
================================================================

* ``f32`` ya da ``f64`` türünden bir değer tamsayı türlerine dönüştürüldüğünde noktadan sonraki kısım atılır, tam kısım 
  elde edilir. (Noktalı pozitif ya da negatif bir sayının noktadan sonraki kısmının atılmasına İngilizce *truncation 
  toward zero* denilmektedir.) Peki sayının noktadan sonraki kısmı atıldıktan sonra tam kısmı hedef türün sınırları 
  içerisinde kalmıyorsa ne olacaktır? İşte Rust'ta bu durumda eğer gerçek sayı pozitifse hedef türle ifade edilebilen 
  en büyük pozitif tamsayı, gerçek sayı negatif ise hedef türle ifade edilebilen en küçük negatif sayı elde edilmektedir.
  Örneğin:

.. code-block:: rust

   let f: f64 = 1000.12;
   let b: u8;

   b = f as u8;
   println!("{}", b);      // 255

Burada ``1000.12`` sayısının noktadan sonraki kısmı atıldıktan sonra ``1000`` değeri elde edilir. Ancak bu ``1000``
değeri hedef tür olan ``u8``'in sınırları dışında kalmaktadır. ``u8`` ile ifade edilecek en büyük pozitif sayı 
255``'tir. Örneğin:

.. code-block:: rust

   let f: f64 = -1000.12;
   let b: i8;

   b = f as i8;
   println!("{}", b);      // -128

C'de gerçek sayı türlerine ilişkin bir değerin noktadan sonraki kısmı atılıp tam kısmı elde edildiğine bu değer hala
hedef türün sınırları içerisinde kalmıyorsa *tanımsız davranış (undefined behavior)* oluşmaktadır. (Ancak C
derleyicilerinin hemen hepsi bu tür durumlarda noktadan sonraki kısmı atılmış olan sayının yüksek anlamlı byte'larını
atarak dönüştürmeyi yapmaktadır.)

* Tamsayı türlerinden ``f32`` ve ``f64`` türlerine dönüştürme yapılırken bir bilgi kaybı oluşmayabilir ya da bilgi kaybı
  oluşabilir. Bu tür durumlarda basamaksal kayıp değil mantis kaybı oluşuyorsa (yani sayının mertebesi aynı ama sayı tam
  olarak ifade edilemiyorsa) dönüştürülmek istenen tamsayıya en yakın hedef türe ilişkin gerçek sayı elde edilir. Burada
  en yakın demekle dönüştürülmek istenen tamsayıdan büyük ya da küçük olan (yani mutlak değerce) sayı kastedilmektedir.
  Örneğin ``i64`` türünden bir değer tam olarak ``f32`` türü ile ifade edilemeyebilir. Bu durumda ``i64`` değerine en 
  yakın ``f32`` değeri elde edilmektedir. (Bazen dönüştürülecek tamsayı değere en yakın ondan büyük ve ondan küçük iki 
  gerçek sayı birlikte bulunuyor olabilir. Bu durumda çift olan sayı tercih edilmektedir.) Örneğin:

.. code-block:: rust

   let a: i32 = 1234567890;
   let b: f32;

   b = a as f32;
   println!("{}", b);      // 1234568000

Burada ``1234567890`` sayısı ``f32`` türü ile tam olarak ifade edilememiştir. Ancak mantis kaybıyla ona en yakın bir sayı
elde edilmiştir.

Tamsayı türlerinden gerçek sayı türlerine dönüştürme yapılırken aynı basamakta bir sayı elde edilemiyorsa (yani hedef
gerçek sayı türünün basamaksal sınırları aşılıyorsa) bu duruma *basamaksal kayıp* denilmektedir. Programlama dillerinde
genel olarak basamaksal kayıplar tolere edilmemektedir. Rust'ta eğer dönüştürülecek tamsayı türünden değer hedef türün
sınırları içerisinde kalmıyorsa (yani basamaksal bir kayıp oluşuyorsa) bu durumda kaynak tamsayı pozitifse hedef türden
``+inf``, kaynak tamsayı negatifse hedef türden ``-inf`` değeri elde edilmektedir. Fakat böylesi bir durum ancak
``u128``'den ``f32`` yapılan dönüştürmelerde söz konusu olabilir.

*  ``f32`` türünden ``f64`` türüne dönüştürmelerde bilgi kaybı oluşmaz. Dolayısıyla sayı tam olarak hedef türle ifade 
   edilir.

* ``f64`` türünden ``f32`` türüne dönüştürme yapılırken eğer mantis kaybı oluşuyorsa ``f32`` ile temsil edilen 
   dönüştürülecek sayıya en yakın sayı elde edilir. Yine eşit uzaklıkta sayılarda çift olan tercih edilmektedir. Eğer
   basamaksal bir kayıp söz konusu olursa dönüştürülecek sayı pozitifse ``+inf``, negatifse ``-inf`` değeri elde 
   edilmektedir.

======================================================================
char ve bool Dönüştürmeleri, never Türü, Klavyeden Okuma ve if İfadesi
======================================================================

char Türünün Dönüştürülmesi
===========================

``char`` türü ``as`` operatörü ile tamsayı türlerine dönüştürülebilir, ancak ``f32`` ve ``f64`` türlerine 
dönüştürülemez. ``char`` türü tamsayı türlerine dönüştürüldüğünde ilgili karakterin Unicode UTF-32 kod numarasına
(code point) ilişkin sayı elde edilmektedir. Rust'ta ``char`` türü Unicode UTF-32 kod numaralarını tutabilen 4 byte
genişlikte olduğu için ``char`` türünün dönüştürüleceği en uygun tamsayı türü de ``u32`` türüdür. Örneğin:

.. code-block:: rust

   let c: char = 'a';
   let a: u32;

   a = c as u32;
   println!("a: {}", a);           // 97

``char`` türü ``u32`` türünden daha küçük türlere dönüştürüldüğünde yüksek anlamlı byte'lar kırpılmaktadır. Örneğin:

.. code-block:: rust

   let c: char = 'ş';
   let a: i8;

   a = c as i8;            // kırpılma olacak
   println!("a: {}", a);

Rust'ta yalnızca ``u8`` türü ``as`` operatörüyle ``char`` türüne dönüştürülebilmektedir. Diğer türlerden ``char`` 
türüne dönüştürme yoktur. Örneğin:

.. code-block:: rust

   let c: char;
   let a: u8 = 97;

   c = a as char;
   println!("a: {}", c);       // a

bool Türünün Dönüştürülmesi
===========================

``bool`` türü ``as`` operatörüyle tamsayı türlerine dönüştürülebilir. Bu durumda ``true`` için ``1``, ``false`` için 
``0`` elde edilmektedir. Ancak ``bool`` türü gerçek sayı türlerine (``f32`` ve ``f64`` türlerine) dönüştürülemez. 
Tamsayı ya da gerçek sayı türlerinden ``bool`` türüne de ``as`` operatörüyle dönüştürme yapılamamaktadır. Örneğin:

.. code-block:: rust

   let b: bool = true;
   let a: i32;

   a = b as i32 + 10;
   println!("{}", a);      // 11

Temel Türler Arası Dönüştürme Tablosu
=====================================

Şimdi de hangi temel türlerden hangi temel türlere ``as`` operatörüyle dönüştürme yapılacağını özet bir tabloyla 
belirtelim:

======================= ======================= =================
Kaynak Tür              Hedef Tür               Dönüştürme Durumu
======================= ======================= =================
Tamsayı Türleri         Gerçek Sayı Türleri     VAR
Gerçek Sayı Türleri     Tamsayı Türleri         VAR
bool Türü               Tamsayı Türleri         VAR
bool Türü               Gerçek Sayı Türleri     YOK
Herhangi Bir Tür        bool Türü               YOK
char Türü               Tamsayı Türleri         VAR
char Türü               Gerçek Sayı Türleri     YOK
u8 Türü                 char Türü               VAR
u8 Dışındaki Türler     char Türü               YOK
======================= ======================= =================

never Türü ve Geri Dönmeyen Fonksiyonlar
========================================

Temel türleri görürken ``!`` ile temsil edilen *never* isimli bir tür de görmüştük. Bu tür fonksiyonların geri dönüş
değerlerinde kullanıldığında fonksiyonların geri dönmeyeceği anlamına geliyordu. Örneğin:

.. code-block:: rust

   fn foo() -> ! {
       //...
   }

Burada ``foo`` fonksiyonu çağrıldığında akış bu fonksiyondan geri dönmeyecektir. Yani fonksiyonu yazan kişi derleyiciye
fonksiyonun geri dönmeyeceği sözünü vermektedir. Ancak programcı bu sözünde durmazsa derleme aşamasında error oluşur.
Örneğin:

.. code-block:: rust

   fn foo() -> ! {
       println!("foo");
   }

Burada programcı ``foo`` fonksiyonu çağrıldığında fonksiyonun geri dönmeyeceği sözünü vermiştir. Ancak bu sözü yerine
getirmemiştir. Buradaki ``foo`` fonksiyonu çağrıldığında fonksiyon geri dönecektir. Bu nedenle bu fonksiyonda derleme
sırasında error oluşacaktır. Örneğin:

.. code-block:: rust

   fn foo() -> ! {
       loop {
       }
   }

``loop`` deyimi izleyen paragraflarda görecek olduğumuz sonsuz döngü oluşturan bir deyimdir. Dolayısıyla burada ``foo``
fonksiyonu geri dönmeyecektir. Programcı verdiği sözü tutmuştur. Örneğin:

.. code-block:: rust

   fn foo() -> ! {
       println!("foo");
       std::process::exit(0);
   }

Burada ``foo`` içerisinde ``exit`` fonksiyonu ile program sonlandırılmıştır. ``exit`` fonksiyonu da ``!`` geri dönüş
değerine sahiptir. Dolayısıyla derleyici ``exit`` fonksiyonu geri dönmediği için ``foo`` fonksiyonun da geri
dönmeyeceğini anlar. Kod geçerli olarak derlenir.

Bir fonksiyonun geri dönmeyeceği bilgisi derleyicinin o fonksiyon için daha kısa bir kod üretmesini sağlamaktadır. C++'a
bu özellik resmi olarak C++11 ile eklenen ``[[noreturn]]`` özniteliği ile gelmiştir. Örneğin:

.. code-block:: cpp

   [[noreturn]] void foo()
   {
       //...

       exit(0);
   }

C11 ile C'ye ``_Noreturn`` anahtar sözcüğü eklenmiştir. Ancak C23 ile birlikte C'ye de C++'taki gibi öznitelikler
(attributes) eklenince bu işlem artık C'de de C++'ta olduğu gibi ``[[noreturn]]`` ile yapılabilmektedir.

Rust'ta never türü *akış geriye gelmeyecek* anlamına geldiği için never türünden herhangi bir türe otomatik dönüştürme
vardır. Örneğin:

.. code-block:: rust

   fn main() {
       let a: i32;

       a = foo();      // geçerli
       //...
   }

   fn foo() -> ! {
       println!("foo");
       std::process::exit(0);
   }

Burada ``foo`` fonksiyonun geri dönüş değeri never türündendir. Never türü de *fonksiyon geriye dönmeyecek* anlamına
gelir. Akış zaten geriye dönmeyeceğine göre ``foo()`` çağrım ifadesinin herhangi bir türe atanmasında sakınca
görülmemiştir.

Diğer Dönüştürmeler
===================

Biz yukarıda tüm temel türler arasındaki dönüştürmeleri gördük. Ancak bu dönüştürmelerin dışında ``enum`` dönüştürmeleri
ve adres dönüştürmeleri bu konuların anlatıldığı bölümlerde ele alınacaktır.

Klavyeden Okuma
===============

Şimdiye kadar klavyeden (``stdin`` dosyasından) okuma yapmadık. Maalesef Rust'ta klavyeden okuma yapmak ancak birkaç
satırlık kodla sağlanabilmektedir. Bu birkaç satırlık kod içerisinde henüz görmediğimiz birtakım konular da
kullanılmaktadır. Biz de bu bölümde ayrıntılara girmeden klavyeden okumanın kalıp olarak nasıl yapıldığı üzerinde
duracağız.

stdout Dosyasının flush Edilmesi
--------------------------------

Rust'ın ``stdout`` dosyası default olarak *satır tabanlı (line buffered)* tamponlanmaktadır. Dolayısıyla yazılanların
ekranda görünmesi için ya yazının sonuna ``\n`` karakterinin iliştirilmesi ya da ``stdout`` dosyasının flush edilmesi
gerekmektedir. ``stdout`` dosyasının flush edilmesi şöyle yapılmaktadır:

.. code-block:: rust

   use std::io::Write;

   print!("Bir sayı giriniz:");
   std::io::stdout().flush().expect("cannot flush stdout!..");

Burada ``std::io::stdout()`` çağrısı ``stdout`` dosya değişkenine erişmeyi sağlamaktadır. Bu değişkenle yapının 
``flush`` metodu çağrılmıştır. Ancak ``flush`` metodu başarısız da olabilmektedir. Burada ``expect`` çağrısının argümanı 
flush başarısız olursa ``stderr`` dosyasına (default olarak ekrana) basılacak yazıyı belirtmektedir. Bu yazı basıldıktan 
sonra panic oluşacak ve programın çalışması sonlanacaktır. Tabii ``stdout`` dosyasının flush edilmesinde bir sorun ortaya
çıkması normal koşullarda mümkün değildir. Aslında biz ``flush`` metodunun geri dönüş değerini hiç dikkate almayabilirdik:

.. code-block:: rust

   use std::io::Write;

   print!("Bir sayı giriniz:");
   std::io::stdout().flush();

Ancak bu durumda Rust derleyicisi bir uyarı verecektir.

Klavyeden Sayı Okuma
--------------------

Rust'ta klavyeden (``stdin`` dosyasından) ``i32`` gibi bir türden okuma yapmak için önce bir satır String olarak okunur,
sonra bu satır parse edilerek hedef türe (örneğin ``i32`` türüne) dönüştürülür. Bu işlem şöyle yapılmaktadır:

.. code-block:: rust

   let val: i32;
   let mut s: String = String::new();

   std::io::stdin().read_line(&mut s).expect("cannot read line!..");
   val = s.trim().parse().expect("cannot parse!..");

Bu kodu satır satır ele alıp açıklayalım:

.. code-block:: rust

   let mut s: String = String::new();

Burada ``String`` isimli bir yapı değişkeni içi boş olarak yaratılmaktadır. String yapısı Rust'ın standart kütüphanesinde
bulunan string işlemleri yapan bir yapıdır. Bu yapıyı nesne yönelimli programlama dillerindeki string sınıflarına
benzetebilirsiniz. Şimdi aşağıdaki satırı açıklayalım:

.. code-block:: rust

   std::io::stdin().read_line(&mut s).expect("cannot read line!..");

Burada önce ``std::io::stdin()`` çağrısı ile ``stdin`` dosyasına ilişkin bir yapı türünden değer elde edilmiştir. Bu 
yapı türüne ilişkin değerle yapının ``read_line`` metodu çağrılmıştır. Bu metot klavyeden (``stdin`` dosyasından) bir 
satır okuyarak okunanları String değişkeninin içine yerleştirmektedir. ``read_line`` metodunu çağırırken String 
değişkeninin adresi ``&mut`` operatörüyle alınarak metoda verilmiştir. ``read_line`` metodu başarısız olabilmektedir. 
Örneğimizde metodun başarısı ele alınmış ve eğer metot başarısız olursa (gerçi başarısız olması pek de mümkün değildir) 
``stderr`` dosyasına bir mesaj bastırılarak panic oluşturulmuştur. Şimdi de aşağıdaki satırı açıklayalım:

.. code-block:: rust

   val = s.trim().parse().expect("cannot parse!..");

``read_line`` metodu klavyeden (``stdin`` dosyasından) bir satır okuduğunda satırın sonundaki ``'\n'`` karakterini de
String değişkenine yerleştirmektedir. Dolayısıyla bu yazıyı parse etmeden önce bu ``'\n'`` karakterinden kurtulmak 
gerekir. String yapısının ``trim`` metodu yazının başındaki ve sonundaki boşluk karakterlerini (white space) atmaktadır. 
``trim`` metodu string dilimine (``&str`` türüne) geri dönmektedir. Bu string dilimi ile ``str`` yapısının ``parse`` 
metodu çağrılmıştır. İşte yazıyı ``i32`` türüne dönüştüren bu ``parse`` metodudur. Ancak ``parse`` metodu da başarısız 
olabilmektedir. Aynı gerekçelerle metodun başarısı yine yapının ``expect`` metodu ile ele alınmıştır. Eğer ``parse`` 
metodu başarısız olursa ``stderr`` dosyasına bir mesaj basılacak ve panic durumu oluşacaktır. Ancak yukarıdaki kodda 
kişilerin bir nokta kafasını karıştırmaktadır. ``parse`` metodu hangi türe dönüştürme yapacağını nereden bilmektedir? 
İşte bu konu Rust'ın *generic* mekanizmasıyla ilgilidir ve ileride ele alınacaktır. Yukarıdaki kodda derleyici ifade 
``i32`` türüne atandığı için i32'ye dönüştürme yapan ``parse`` metodunu çağırmaktadır. Örneğin biz yukarıdaki kodda 
``f64`` türünden okuma yapmak isteseydik yalnızca val değişkeninin türünü değiştirmemiz yeterli olacaktı:

.. code-block:: rust

   let val: f64;
   let mut s: String = String::new();

   std::io::stdin().read_line(&mut s).expect("cannot read line!..");
   val = s.trim().parse().expect("cannot parse!..");

Yukarıdaki işlemleri bir fonksiyona da yaptırabiliriz. Örneğin:

.. code-block:: rust

   fn getval() -> i32 {
       let mut buf: String = String::new();

       std::io::stdin().read_line(&mut buf).expect("read line failed");
       buf.trim().parse().expect("parse into number")
   }

Burada ``getval`` fonksiyonu klavyeden okunan ``i32`` türünden bir değerle geri dönmektedir. Bu fonksiyonu farklı türü
okuyacak hale getirmek için tek yapılacak şey geri dönüş değerinin türünü değiştirmektir:

.. code-block:: rust

   fn getval() -> f64 {
       let mut buf: String = String::new();

       std::io::stdin().read_line(&mut buf).expect("read line failed");
       buf.trim().parse().expect("parse into number")
   }

Artık fonksiyon ``f64`` türünden (C'deki double türünden) değerle geri dönmektedir.

Genel (Generic) Okuma Fonksiyonu
--------------------------------

Aslında yukarıdaki fonksiyonu *genel (generic)* biçimde de yazabiliriz. Biz *genel fonksiyonlar (generic functions)*
konusunu ileride göreceğiz. Ancak burada açıklama fonksiyonun genelleştirilmiş halini aşağıda veriyoruz.

.. code-block:: rust

   fn getval<T>() -> T
   where
       T: std::str::FromStr,
       <T as std::str::FromStr>::Err: std::fmt::Debug,
   {
       let mut buf = String::new();
       std::io::stdin()
           .read_line(&mut buf)
           .expect("cannot read line!..");
       buf.trim()
           .parse::<T>()
           .expect("cannot parse!..")
   }

.. code-block:: rust

   use std::io::Write;

   fn main() {
       let val: i32;

       print!("Bir sayı giriniz:");
       std::io::stdout().flush().expect("cannot flush stdout!");

       val = getval();
       print!("{}\n", val * val);
   }

   fn getval<T>() -> T
   where
       T: std::str::FromStr,
       <T as std::str::FromStr>::Err: std::fmt::Debug,
   {
       let mut buf = String::new();
       std::io::stdin()
           .read_line(&mut buf)
           .expect("cannot read line!..");
       buf.trim()
           .parse::<T>()
           .expect("cannot parse!..")
   }

Kontrol Deyimleri ve İfadeler
=============================

Bu bölümde Rust'taki ``if`` gibi, ``for`` gibi temel kontrol deyimlerini ele alacağız. Bazı kontrol deyimleri *kalıp 
uyuşumu (pattern matching)* konusu ile ilgili olduğu için ``enum``, ``struct`` gibi temel veri yapıları görüldükten 
sonra ele alınacaktır. Rust'ta deyimlerin de birer ifade gibi kullanılabildiğini dolayısıyla deyimlerin de değer 
ürettiğini belirtmiştik. Bu nedenle Rust dünyasında *if deyimi, while deyimi* gibi terimler yerine *if ifadesi*, *while
ifadesi* gibi terimler tercih edilmektedir.

Deyimlerin BNF Grameri
======================

*The Rust Reference* dokümanlarında *deyimler (statements)* için oluşturulan BNF grameri şöyledir:

.. code-block:: text

   Statement :
     ;
     | Item
     | LetStatement
     | ExpressionStatement
     | MacroInvocationSemi

Burada ExpressionStatement ara sembolü şöyle açılmaktadır:

.. code-block:: text

   ExpressionStatement :
     ExpressionWithoutBlock ;
     | ExpressionWithBlock ;?

Bu *BNF grameri* ifadesel deyimlerin iki biçimde oluşturulabileceği belirtilmektedir:

1) Bir ifadenin sonuna ``';'`` getirilerek o ifade deyim yapılabilir.
2) Blok içeren sentaktik yapılar sonunda ``';'`` olmasa da deyim belirtmektedir.

Biz daha önceki konularda bloklu yapıların sonunda ifade varsa bloklarının sonuna ``';'`` atomunun getirilmesi
gerektiğini görmüştük. Anımsayacağınız gibi blokların sonunda ifade yoksa ya da ``()`` ifadesi varsa bloklardan sonra
``';'`` atomunun getirilmesi zorunlu değildir.

Gramerdeki ``ExpressionWithBlock`` ara sembolü de şöyle açılmıştır:

.. code-block:: text

   ExpressionWithBlock :
     OuterAttribute*†
     (
     BlockExpression
     | ConstBlockExpression
     | UnsafeBlockExpression
     | LoopExpression
     | IfExpression
     | IfLetExpression
     | MatchExpression
     )

Blok İfadeleri
==============

Daha önceden de belirttiğimiz gibi Rust'ta küme parantezi içerisindeki kod parçası da bir deyim dolayısıyla bir ifade
belirtmektedir. Küme parantezleriyle oluşturulan deyimlere blok deyimleri denilmektedir. Blok deyimi de bir değer
üretmektedir. Blok deyiminin ürettiği değer bloğun sonundaki ifadedir. Örneğin:

.. code-block:: rust

   let result: i32;

   result = {
       println!("this is a block");
       100
   };

Burada ``result`` değişkenine blok ifadesinin ürettiği değer atanmıştır. Rust gramerinde blok ifadelerinin
``ExpressionWithBlock`` ara sembolü içerisinde bulunduğuna dikkat ediniz. Daha önceden de belirttiğimiz gibi blok 
ifadeleri eğer birim dışında (yani ``()`` dışında) bir değer üretiyorsa tek başlarına kullanıldığında bloğun sonunda 
bir ``';'`` gerekmektedir. Örneğin:

.. code-block:: rust

   {
       println!("this is a block");
       100
   }               // error!

Burada blok ifadesi birim değerini üretmediği için bloğun sonunda ``';'`` bulunmak zorundadır:

.. code-block:: rust

   {
       println!("this is a block");
       100
   };               // geçerli

Tabii yukarıdaki kod geçerli olsa da mantıksal bakımdan anlamsızdır. Blok ifadesinin değeri kullanılmadıktan sonra ondan
bir değerin elde edilmesinin anlamı yoktur. Örneğin:

.. code-block:: rust

   result = {
       println!("this is a block");
       100
   };

Buradaki ``';'`` her durumda bulundurulmak zorundadır. Çünkü bu ``';'`` artık atama operatörünün sonundaki ``';'``
durumundadır.

if İfadesi
==========

Rust'ta if ifadesinin genel biçimi şöyledir:

.. code-block:: text

   if <ifade> {
       //...
   }
   [
   else {
       //...
   }
   ]

``if`` anahtar sözcüğünü ``bool`` türden bir kontrol ifadesi izlemek zorundadır. Bu kontrol ifadesinin paranteze 
alınmadığına dikkat ediniz. (C, C++, Java ve C# gibi dillerde ``if`` deyimindeki kontrol ifadesinin paranteze 
alınmasının zorunlu olduğunu anımsayınız.) ``if`` ifadesinin doğruysa ve yanlışsa kısmında bir blok ifadesinin bulunması
zorunludur. (Halbuki C, C++, Java ve C# gibi dillerde eğer ``if`` deyiminin doğruysa ve yanlışsa kısmında tek deyim 
varsa bloklama yapmak zorunlu değildir.) Diğer dillerde olduğu gibi ``if`` ifadesinin else kısmı da bulunmayabilir. 
*The Rust Reference* dokümanlarında ``if`` ifadesinin *BNF grameri* ise şöyle verilmiştir:

.. code-block:: text

   IfExpression :
     if Expression BlockExpression
     (else ( BlockExpression | IfExpression | IfLetExpression ) )?

``if`` ifadesi şöyle çalışmaktadır: Derleyici ``if`` anahtar sözcüğünün yanındaki kontrol ifadesinin değerini hesaplar.
Eğer bu değer ``true`` ise yalnızca doğruysa kısmındaki blok deyimini, ``false`` ise yalnızca yanlışsa kısmındaki blok 
deyimini çalıştırır.

``if`` anahtar sözcüğünden sonra neden parantezlerin gerekmediğini (ya da C, C++, Java ve C# gibi dillerde neden
gerektiğini) merak edebilirsiniz. Rust'ta if ifadesinin doğruysa ve yanlışsa kısmında bloklama yapmak zorunlu olduğu
için bu parantezlere gerek kalmamıştır. Halbuki C, C++, Java ve C# gibi dillerde tek deyim için bloklama zorunlu
olmadığından kontrol ifadesiyle ``if`` deyiminin doğruysa kısmındaki ifadenin ayrıştırılabilmesi için parantezler
gerekmektedir. Örneğin:

.. code-block:: c

   if (a > 0) x = 10; else  y = 20;

Burada parantezler olmasaydı kontrol ifadesiyle doğruysa kısmındaki ifade ayrıştırılamazdı. Parantezleri kaldırarak aynı
ifadeyi yeniden veriyoruz:

.. code-block:: text

   if a > 0  x = 10; else y = 20;

Bloklu deyimlerde parantezin bulunmaması Rust'a özgü değildir. Swift, Kotlin, Go, Python gibi dillerde de benzer
nedenlerden dolayı kontrol ifadesinin paranteze alınmasına gerek duyulmamaktadır.

Tabii biz yine de ``if`` ifadesindeki kontrol ifadesini paranteze alabiliriz. Fakat buradaki parantezler artık öncelik
parantezi olur. Zaten biz her ifadeyi paranteze alabiliriz. Tabii parantezleri kullanmak iyi bir teknik değildir. Rust
derleyicisi bu tür gereksiz parantezlere uyarı da vermektedir.

===================================
if İfadesi, loop ve while Döngüleri
===================================

if İfadesinin Kullanımı
=======================

Aşağıdaki örnekte klavyeden (``stdin`` dosyasından) okunan ``i32`` türünden bir değerin pozitif olup olmadığı ekrana 
(``stdout`` dosyasına) yazdırılmıştır. ``if`` ifadesi şöyle kullanılmıştır:

.. code-block:: rust

   if val > 0 {
       println!("pozitif");
   }
   else {
       println!("negatif ya da sıfır");
   }

.. code-block:: rust

   use std::io::Write;

   fn main() {
       let val: i32;

       print!("Bir sayı giriniz:");
       std::io::stdout().flush().expect("cannot flush stdout!");

       val = getval();

       if val > 0 {
           println!("pozitif");
       }
       else {
           println!("negatif ya da sıfır");
       }
   }

   fn getval() -> i32 {
       let mut buf: String = String::new();

       std::io::stdin().read_line(&mut buf).expect("read line failed");
       buf.trim().parse().expect("parse into number")
   }

else-if Merdivenleri
====================

``if`` ifadesinin ``else`` kısmında başka bir ``if`` ifadesi varsa yazım biraz karmaşık hale gelmektedir. Örneğin:

.. code-block:: rust

   if val > 0 {
       println!("pozitif");
   }
   else {
       if val < 0 {
           println!("negatif");
       }
       else {
           println!("sıfır");
       }
   }

İşte bu karmaşıklığı azaltmak için ``if`` gramerine şu özellik eklenmiştir: *Eğer else anahtar sözcüğünden sonra hemen
başka bir ``if`` ifadesi geliyorsa bloklama yapmaya gerek yoktur.* Bu kural sayesinde *else-if merdivenleri* daha kolay
yazılabilmektedir. Örneğin:

.. code-block:: rust

   if val > 0 {
       println!("pozitif");
   }
   else if val < 0 {
       println!("negatif");
   }
   else {
       println!("sıfır");
   }

Tabii if ifadesinin doğruysa kısmına başka bir ``if`` ifadesi yerleştirilecekse içteki ``if`` ifadesini blok açmadan 
yazmanın bir yolu yoktur.

if İfadesinin Değer Üretmesi
============================

``if`` bir ifade olduğu için bir değer de üretmektedir. Biz ``if`` ifadesinden elde edilen değeri doğrudan 
kullanabiliriz. Örneğin onu bir değişkene atayabiliriz:

.. code-block:: rust

   result = if val > 0 {
       //...
       100
   }
   else {
       //...
       200
   };

``if`` ifadesinin doğruysa ve yanlışsa kısmında aynı türden değerlerin oluşturulması zorunludur. Örneğin:

.. code-block:: rust

   result = if val > 0 {
       //...
       10
   }
   else {
       //...
       20.5
   };

Yukarıdaki kod error oluşturacaktır. Çünkü ``if`` ifadesinin kolları aynı türden değer oluşturmamaktadır.

``if`` ifadesinin ``else`` kısmının olmayabileceğini belirtmiştik. Peki bu durumda aşağıdaki gibi bir atama işleminde 
ne olacaktır:

.. code-block:: rust

   result = if val > 0 {
       //...
       100
   };

Burada koşul sağlanıyorsa result değişkenine ``100`` değeri atanır. Peki ya koşul sağlanmıyorsa? İşte *The Rust
Reference* dokümanlarında *eğer if ifadesinin else kısmı yoksa sanki boş bir else kısmı varmış ve oradan birim (unit)
değeri elde ediliyormuş gibi bir işlemin* uygulandığı belirtilmiştir. Dolayısıyla yukarıdaki atama işleminde ``if``
ifadesinin doğruysa ve yanlışsa kısımları farklı türlerden ifade oluşturduğu için error oluşacaktır. Tabii aşağıdaki
gibi bir atama error ile sonuçlanmaz:

.. code-block:: rust

   result = if val > 0 {
       //...
       ()
   };

Burada ``result`` değişkeninin birim türünden olması gerekir. Fakat böyle bir atamanın da mantıksal bir anlamı yoktur. 
O halde Rust'ta else kısmı olmayan ``if`` ifadesinden bir değer oluşturmaya çalışmak anlamlı değildir.

*else-if merdivenlerinde* yine her bloğun oluşturduğu değerin aynı türden olması gerekmektedir. Örneğin:

.. code-block:: rust

   result = if val == 1 {
       //...
       10
   }
   else if val == 2 {
       //...
       20
   }
   else if val == 3 {
       //...
       30
   }
   else {
       0
   };

if let İfadesi
==============

Rust'ta ``if`` ifadesinin yanı sıra ``if let`` biçiminde ``if`` ifadesine benzeyen başka bir ifade daha vardır. Ancak 
``if let`` ifadesi *kalıp uyuşumu (pattern matching)* özelliği ile çalışmaktadır. Biz kalıp uyuşumunu başka bir başlık 
altında ve çeşitli konuların içerisine yaydırarak inceleyeceğiz.

Döngü İfadeleri
===============

Bir program parçasının yinelemeli olarak çalıştırılmasını sağlayan deyimlere *döngü deyimleri (loop statements)*
denilmektedir. Örneğin pek çok programlama dilinde döngü oluşturmak için ``while`` ve ``for`` deyimleri bulunmaktadır. 
Bir süredir programlama dillerine *dolaşılabilir (iterable)* nesneleri dolaşan özel ``for`` döngüleri de sokulmuştur. 
Bu ``for`` döngülerine örneğin C++11'de *aralık tabanlı for döngüleri (range based loop)*, C#'ta ``foreach`` döngüleri 
denilmektedir. Rust'ta deyimler birer ifade gibi olduğu için biz *döngü deyimleri* terimi yerine *döngü ifadeleri (loop
expressions)* terimini kullanacağız.

Döngü İfadelerinin BNF Grameri
==============================

Rust'taki döngü ifadeleri şunlardır:

* loop döngüleri
* while döngüleri
* while let döngüleri
* for döngüleri

*The Rust Reference* dokümanlarında döngü ifadeleri için BNF grameri şöyle verilmiştir:

.. code-block:: text

   LoopExpression :
     LoopLabel? (
       InfiniteLoopExpression
       | PredicateLoopExpression
       | PredicatePatternLoopExpression
       | IteratorLoopExpression
       | LabelBlockExpression
     )

Burada ``InfiniteLoopExpression`` ara sembolü ``loop`` döngülerini, ``PredicateLoopExpression`` ara sembolü ``while``
döngülerini, ``PredicatePatternLoopExpression`` ara sembolü ``while let`` döngülerini, ``IteratorLoopExpression`` ara 
sembolü de ``for`` döngülerini belirtmektedir. ``LabelBlockExpression`` ara sembolü aslında bir döngü belirtmez. Ancak 
gramer olarak buraya yerleştirilmiştir.

loop İfadesi
============

Sonsuz Döngü
------------

Rust'ta ``loop`` ifadesi sonsuz döngü oluşturmak için kullanılmaktadır. Genel biçimi şöyledir:

.. code-block:: rust

   loop {
       //...
   }

``loop`` anahtar sözcüğünden sonra bir blok ifadesi bulunmak zorundadır. Döngü sürekli olarak buradaki deyimleri 
çalıştırır. Örneğin:

.. code-block:: rust

   fn main() {
       let mut i = 0;

       loop {
           println!("{}", i);
           i += 1;
       }
   }

Burada bir sonsuz döngü içerisinde ``i`` değişkeni sürekli artırılıp değeri yazdırılmıştır. Tabii ileride göreceğimiz 
gibi sonsuz döngülerden ``break`` ifadesi ile çıkabiliriz.

loop ve never Türü
------------------

``loop`` ifadesi içerisinde eğer ``break`` ifadesi kullanılmamışsa ``loop`` ifadesi never türünden (``!`` türünden) 
değer üretir. Never türünün herhangi bir türe doğrudan dönüştürülebildiğini anımsayınız. Bu durumda biz ``loop`` ile 
oluşturduğumuz sonsuz döngüyü herhangi bir türe atayabiliriz. Ancak bunun bir anlamı olmaz. Örneğin:

.. code-block:: rust

   let result: f64;

   result = loop {
       //...
   }

Bu kod parçası geçerli olsa da anlamsızdır. Zaten ``loop`` döngüsünden çıkılamadığına göre onun sonucunu değişkene 
atamanın da bir anlamı yoktur. Rust terminolojisinde bir akışın bir biçimde geri dönmemesi durumuna *divergent* ya da 
*diverging* denilmektedir. Eğer ``loop`` döngüsünden break ile çıkılırsa (yani ``loop`` döngüsü *divergent* değilse) bu 
durumda loop ifadesi ``break`` anahtar sözcüğünün yanındaki ifadenin değerini oluşturur. Örneğin:

break ile Değer Üretme
----------------------

.. code-block:: rust

   fn main() {
       let mut i = 0;
       let result: i32;

       result = loop {
           println!("{}", i);
           i += 1;
           if i == 10 {
               break 100;
           }
       };

       println!("{}", result);       // 100
   }

Burada ``loop`` içerisinden ``break`` ifadesi ile çıkılmıştır. Artık ``loop`` never değil ``100`` değerini 
oluşturmaktadır. Eğer ``break`` anahtar sözcüğünün yanında bir ifade kullanılmazsa bu durumda sanki birim (unit) 
ifadesi kullanılmış gibi işlem yapılır. Örneğin:

.. code-block:: rust

   result = loop {
       println!("{}", i);
       i += 1;
       if i == 10 {
           break;
       }
   };

Burada result birim (unit) türünden olmak zorundadır. Tabii bu durumda ``loop`` ifadesinin bir değişkene atanmasının da
anlamı kalmamaktadır. ``loop`` ifadesinde birden fazla ifadeli break kullanılıyorsa bütün ``break`` ifadelerinin de aynı 
türden olması gerekir. Örneğin:

.. code-block:: rust

   result = loop {
       //...
       if ifade1 {
           break 10;
       }
       //...
       if ifade2 {
           break 3.14;
       }
   }

Burada eğer ``result`` değişkeni tamsayı türlerine ilişkinse error ikinci break ifadesinde rapor edilecektir.

``loop`` ifadesinin bir blok ifadesi içerdiğini belirttik. Ancak ``loop`` ifadesindeki bloğun sonuna bir ifade 
yerleştirilemez. (Halbuki genel olarak blok ifadelerinin sonuna bir ifade yerleştirilebilmektedir.) Örneğin:

.. code-block:: rust

   result = loop {
       //...
       100             // anlamsız, error oluşacaktır!
   };

Burada ``loop`` bloğunun sonuna yerleştirilen ifade error oluşturacaktır. Ancak burada ilginç bir durumdan bahsetmek
istiyoruz. Aşağıdaki gibi bir döngüde Rust derleyicisi error oluşturmamaktadır:

.. code-block:: rust

   result = loop {
       //...
       ()
   };

Birim (unit) Türü ve loop İfadesi
---------------------------------

Aslında Rust'ta ``()`` atomları hem bir ifade hem de bir tür belirtmektedir. Zaten bunun için ``()`` atomlarına *birim
(unit)* de denilmektedir, *birim türü (unit type)* da denilmektedir. Örneğin aşağıdaki gibi bir bağlama geçerlidir:

.. code-block:: rust

   let a: () = ();

İşte Rust'ta ``()`` atomlarının bir tür mü yoksa bir değer mi (yani ifade mi) belirttiğine içinde bulunulan bağlama göre
karar verilmektedir. Yukarıdaki ``loop`` ifadesinin sonundaki ``()`` ifade olamayacağı için Rust derleyicisi tarafından 
bir tür tanımlaması olarak ele alınmaktadır. Bu da Rust'ta geçerlidir. Yani aşağıdaki gibi bir ``loop`` deyimi nasıl 
geçerliyse yukarıdaki de aynı biçimde geçerlidir:

.. code-block:: rust

   result = loop {
       //...
       struct Point {
           x: i32,
           y: i32
       }
   };

Dolayısıyla sonu ``()`` ile biten ``loop`` ifadesi divergent biçimdedir ve yine never türüne ilişkindir.

while İfadesi
=============

Rust'ta diğer pek çok programlama dilinde olduğu gibi ``while`` döngüleri de bulunmaktadır. ``while`` döngülerinin 
genel biçimi şöyledir:

.. code-block:: rust

   while <ifade> {
       //...
   }

``while`` anahtar sözcüğünden sonra yine parantez kullanmaya gerek yoktur. Ancak yinelenen deyimler blok içerisinde 
bulunmak zorundadır. ``while`` döngülerinde kontrol ifadesinin ``bool`` türden olması zorunludur.

``while`` döngüsü şöyle çalışmaktadır: Kontrol ifadesinin değeri elde edilir. Eğer bu değer ``true`` ise döngü deyimleri
(yani blok ifadesi) çalıştırılır ve başa dönülür. Kontrol ifadesi ``false`` ise döngüden çıkılır. Yani ``while`` 
döngüleri *koşul doğru olduğu sürece* yinelenmeye yol açmaktadır.

Rust'ta kontrolün sonda yapıldığı ``while`` döngüleri (yani do-while döngüleri) bulunmamaktadır. Eğer bu tarzda bir
algoritmik yapı ile karşılaşırsanız (zaten seyrek karşılaşılmaktadır) bunu kontrolün başta yapıldığı ``while`` döngüsü 
ile sağlamalısınız.

Aşağıda ``while`` döngüsünün örnek bir kullanımını görüyorsunuz:

.. code-block:: rust

   fn main() {
       let mut i = 0;

       while i < 10 {
           println!("{}", i);
           i += 1;
       }
   }

Rust'ta atama operatörü birim değerini (yani ``()`` değerini) ürettiği için C gibi dillerde çokça yaptığımız *önce ata,
sonra atanmış değeri karşılaştır* işlemini yapamamaktayız. Örneğin:

.. code-block:: rust

   while (val = getval()) != 0 {           // Rust'ta error oluşturur!
       //...
   }

Bu durumu sağlamanın bir yolu döngüyü sonsuz döngü biçimine dönüştürmektir:

.. code-block:: rust

   loop {
       val = getval();
       if val == 0 {
           break;
       }
       //...
   }

Ya da aynı şey koşul parantez içerisindeki ifadenin iki kez yazılmasıyla da yapılabilirdi:

.. code-block:: rust

   val = getval();
   while val != 0 {
       //...
       val = getval();
   }

==============================================
while İfadesi, for Döngüleri ve Range Yapıları
==============================================

while İfadesinin Ürettiği Değer
===============================

Rust'ta ``while`` ifadesi her zaman birim değerini (yani ``()`` değerini) üretmektedir. Bu nedenle biz ``while`` 
ifadesinde bloğun sonuna bir ifade yerleştiremeyiz. (Bloğun sonuna ``()`` ifadesi yerleştirilirse bu bir tür 
tanımlaması (item) olarak ele alınmaktadır.) Bu durumda Rust'ta ``while`` ifadesini bir değişkene atamanın da pratik 
bir faydası yoktur. Örneğin:

.. code-block:: rust

   result = while i < 10 {
       println!("{}", i);
       i += 1;
   };
   println!("{:?}", result);           // ()

Örneğin:

.. code-block:: rust

   result = while i < 10 {
       println!("{}", i);
       i += 1;
       100             // error!
   };

``while`` bloğunun sonuna bir ifade yerleştirmenin bir anlamı olmadığı için bu sentaks yasaklanmıştır.

``while`` döngülerinde de ``break`` ifadesi kullanılabilir. Yani ``while`` döngüleri de ``break`` ile sonlandırılabilir.
Örneğin:

.. code-block:: rust

   while true {
       println!("{}", i);
       i += 1;
       if i == 10 {
           break;
       }
   };

Fakat ``while`` içerisinde kullanılan ``break`` ifadesinin yanına bir ifade yerleştirilemez. İfadeli ``break`` yalnızca 
``loop`` döngüsünde kullanılabilmektedir. Örneğin:

.. code-block:: rust

   result = while true {
       println!("{}", i);
       i += 1;
       if i == 10 {
           break 100;      // error!
       }
   };

``while`` ifadesi her zaman birim değerini (yani ``()`` değerini) üretmektedir. Dolayısıyla ``while`` içerisinde ifadeli 
``break`` kullanmak geçerli değildir.

Klavyeden Okunan Değerin Karesini Yazdırma
==========================================

Klavyeden (``stdin`` dosyasından) bir döngü içerisinde ``getval`` isimli bir fonksiyonla bir sayı okuyup o sayının 
karesini yazdırmak isteyelim. Ancak ``0`` girildiğinde döngüyü sonlandırılacak olsun. Yukarıda da belirttiğimiz gibi 
bu işlem C, C++, Java ve C# gibi dillerde pratik bir biçimde aşağıdaki gibi yapılabilmektedir:

.. code-block:: c

   while ((val = getval()) != 0)
       printf("%d\n", val * val);

Ancak Rust'ta atama operatörü birim değerini (yani ``()`` değerini) ürettiği için bu işlem yukarıdaki gibi
yapılamamaktadır. Bu tür durumlarda ``loop`` döngüsünü tercih edebilirsiniz:

.. code-block:: rust

   loop {
       val = getval();
       if val == 0 {
           break;
       }
       println!("{}", val * val);
   };

.. code-block:: rust

   use std::io::Write;

   fn main() {
       let mut val;

       loop {
           print!("Bir sayı giriniz:");
           std::io::stdout().flush();
           val = getval();
           if val == 0 {
               break;
           }
           println!("{}", val * val);
       };
   }

   fn getval() -> i32 {
       let mut buf: String = String::new();

       std::io::stdin().read_line(&mut buf).expect("read line failed");
       buf.trim().parse().expect("parse into number")
   }

while let İfadesi
=================

Rust'ta nasıl ``if`` ifadesinin kalıp uyuşumuyla çalışan ``if let`` isminde bir biçimi varsa ``while`` ifadesinin de 
``while let`` isminde kalıp uyuşumuyla çalışan bir biçimi de vardır. Biz ``while let`` döngüsünü kalıp uyuşumu (pattern 
matching) konusundan sonra ele alacağız.

Dolaşım Mekanizmasıyla Çalışan for Döngüleri
============================================

Rust'ta dolaşım (iteratör) mekanizmasıyla çalışan bir ``for`` döngüsü de bulunmaktadır. Bu ``for`` döngüsü ancak dolaşım
(iterator) türlerine ilişkin değişken ya da değerlerle kullanılabilmektedir. Biz kursumuzda dolaşım (iterator)
yapılarının nasıl oluşturulduğunu sonraki bölümlerde göreceğiz. Ancak burada genel olarak Rust'ın ``for`` döngüsünü ele
alacağız.

Dolaşım mekanizmasıyla çalışan döngüler Rust gibi C# gibi yeni programlama dillerinde doğuştan bulunmaktadır. Eski bazı
dillere ise bu döngüler belli zamandan sonra eklenmiştir. Örneğin C++'a dolaşım yoluyla çalışan döngüler *aralık tabanlı
for döngüleri (range based for loops)* ismi ile C++11 standartlarında eklenmiştir. Java'ya ise bu döngüler 2004 yılında
*Java Language Specification 5* ile eklenmiştir. C#'ta bu döngülere ``foreach`` döngüleri, Java'da ise *genişletilmiş for
döngüleri (enhanced for loops)* denilmektedir.

Rust'ta dolaşım mekanizmasıyla çalışan ``for`` döngülerinin genel biçimi şöyledir:

.. code-block:: rust

   for <kalıp> in <dolaşım_ifadesi> {
       //...
   }

``for`` anahtar sözcüğünden sonra bir kalıp ifadesi gelmektedir. Burada tipik olarak bir değişken ismi (variable pattern)
kullanılabilir. Daha sonra bu kalıbı ``in`` anahtar sözcüğü ve bir dolaşım ifadesi (iterator expression) izler. Dolaşım
ifadesi dolaşım türünden (iterator type) bir değişken ya da değer belirtmektedir.

Dolaşım türünden bir ifade dolaşıldığında dolaşan kişiye değerler verilmektedir. Dolaşım ifadesi her dolaşım sırasında
bir değer verir, en sonunda verecek değeri kalmadığında dolaşım sonlanır. Rust'taki bazı yapılar zaten dolaşım
özelliğine sahiptir. Dolayısıyla onlar türünden değişkenleri ve değerleri ``for`` döngüleriyle dolaşabiliriz.

Rust'ta ``for`` döngüleri kabaca şöyle çalışmaktadır: Dolaşım ifadesi bir değer verir. Bu değer kalıpla belirtilen
değişkenlere atanır. Döngünün bloğu çalıştırılır. Sonra dolaşım ifadesi yeni bir değer verir bu değer de kalıpla
belirtilen değişkenlere atanır ve yine döngünün bloğu çalıştırılır. En sonunda dolaşım ifadesi artık değer vermez duruma
gelir. Çünkü verecek değeri kalmamıştır. Döngü de sonlanır.

Rust'ın ``for`` döngülerinin bazı dillerdeki ``for (i = 0; i < 10; ++i)`` biçimindeki ``for`` döngüleri gibi 
çalışmadığına dikkat ediniz. Rust'ın ``for`` döngülerinin iyi anlaşılabilmesi için iki konusunun bilinmesi gerekmektedir:

1) Kalıp uyuşumu (pattern matching) konusu.
2) Dolaşım ifadelerinin (dolaşım türünden değişkenlerin ve değerlerin) nasıl oluşturulduğu konusu.

Biz bu konuları ilerde ayrı bir bölümde ele alacağız. Ancak burada Rust'ın ``for`` döngülerinin temel kullanımı üzerinde
duracağız.

Dolaşım mekanizmasıyla çalışan ``for`` döngülerinin en önemli avantajı *farklı veri yapılarının aynı biçimde 
dolaşılmasına* olanak sağlamasıdır. Biz bu tür ``for`` döngüleriyle her türden dolaşım (iterator) ifadelerini aynı 
biçimde dolaşabilmekteyiz. Örneğin:

.. code-block:: rust

   fn main() {
       let a: [i32;5] = [1, 2, 3, 4, 5];

       for val in a {
           print!("{} ", val);
       }
       println!("");

       let v: Vec<i32> = vec![1, 2, 3, 4, 5];

       for val in v {
           print!("{} ", val);
       }
       println!("");
   }

Burada bir dizi ve bir ``Vec`` türünden aynı biçimde dolaşılmıştır. Tabii biz henüz dizileri ve vektörleri görmedik.
Yalnızca bu örneği aynı biçimde dolaşıma bir örnek olarak verdik.

Range Yapısı ile for Döngüleri
==============================

Dolaşım mekanizmasıyla çalışan ``for`` döngüleri ile klasik *for (i = 0; i < 10; ++i)* biçimindeki ``for`` döngülerini
oluşturmak için Rust'ın standart kütüphanesinde ``Range``, ``RangeInclusive`` ve ``RangeFrom`` isimli yapılar 
bulundurulmuştur. Dolaşımda kullanılan bu yapılar *genel (generic)* yapılardır. Bir genel yapı kullanılırken tür 
parametreleri için türler açıkça ya da gizlice belirtilmektedir. Genel bir türün belli türler için açılmasına Rust'ta 
*monomorfize edilmesi (monomorphize)* denilmektedir. Genel türler kursumuzda ileride başka bir bölümde ele alınmaktadır.

``Range`` isimli yapı türünden bir ifade ``for`` döngüsü ile dolaşılmak istendiğinde bu ifade dolaşım sırasında start 
değerinden itibaren end değerine kadar (end değeri dahil değil) tamsayı değerleri vermektedir. Böylece klasik ``for`` 
döngüleri bu ``Range`` yapısı yoluyla oluşturulabilmektedir. Örneğin:

.. code-block:: rust

   let r: std::ops::Range<i32> = std::ops::Range {start: 0, end: 10};

   for i in r {
       //...
   }

Burada ``r`` değişkeni ``Range<i32>`` türündendir. Dolaşım sırasında bu değişken ``0``'tan itibaren ``10``'a kadar 
(``10`` dahil değil) i32 tamsayı değerlerini verecektir. Dolayısıyla bu döngü C, C++, Java ve C#'taki aşağıdaki döngüyle 
işlevsel olarak eşdeğer hale gelmektedir:

.. code-block:: c

   for (int i = 0; i < 10; ++i) {
       //...
   }

``Range`` yapısının ``std::ops`` modülünde bulunduğuna dikkat ediniz. Yazımı kolaylaştırmak için ``use`` deyimini 
kullanabilirsiniz:

.. code-block:: rust

   use std::ops::Range;

   let r: Range<i32> = Range {start: 0, end: 10};

   for i in r {
       print!("{} ", i);
   }

Tabii ``let`` deyiminde bağlama yapılırken değişken için tür de belirtilmeyebilir. Örneğin:

.. code-block:: rust

   let r = Range {start: 0, end: 10};

``Range`` değişkeninin yaratılması ``for`` döngüsünün içerisinde de yapılabilmektedir. Ancak bu durumda ifadenin 
paranteze alınması gerekir. Örneğin:

.. code-block:: rust

   for i in (Range {start: 0, end: 10}) {
       print!("{} ", i);
   }

Exclusive Range Operatörü
=========================

Klasik ``for`` döngülerine benzer döngüler çok sık oluşturulduğu için Rust'ta ``Range`` değerlerin ``..`` operatörü ile 
daha kolay bir biçimde oluşturulmasına olanak sağlanmıştır. ``..`` operatörüne İngilizce *exclusive range* operatörü
denilmektedir. Örneğin:

.. code-block:: rust

   for i in 0..10 {
       //...
   }

biçimindeki for döngüsü tamamen aşağıdakiyle eşdeğerdir:

.. code-block:: rust

   for i in (Range {start: 0, end: 10}) {
       //...
   }

``Range`` değişkenlerinde start değerinin aralığa dahil olduğunu ancak end değerinin aralığa dahil olmadığını bir kez 
daha vurgulamak istiyoruz.

``Range`` değişkeni yaratılırken tür parametresi herhangi bir tamsayı türü olarak belirlenebilir. Ancak tür parametresi `
``f32`` ya da ``f64`` olursa Range değişkeni dolaşım ifadesi olmaktan çıkmaktadır. Örneğin:

.. code-block:: rust

   let r: Range<u8> = Range {start: 0, end: 10};

   for i in  r {
       //...
   }

Yukarıdaki işlemi ``..`` operatörüyle aşağıdaki gibi de yapabilirdik:

.. code-block:: rust

   for i in  0u8..10u8 {
       //...
   }

Fakat biz Range yapısını ``f32`` ya da ``f64`` türüyle *monomorfize* edersek artık onu ``for`` döngüsüyle dolaşamayız. 
Çünkü değişken dolaşım ifadesi olmaktan çıkar. Örneğin:

.. code-block:: rust

   let r: Range<f64> = Range {start: 0., end: 10.};

   for i in  r {           // error! Range<f64> dolaşım ifadesi olmaktan çıktı
       //...
   }

Tabii biz ``f64`` türünden Range değerini ``..`` operatörü ile örneğin ``0.0..10.0`` biçiminde ya da ``0f64..10f64``
biçiminde de oluşturabilirdik.

Atlamalı Dolaşım (step_by)
--------------------------

Peki ``Range`` türünden bir ifade ile atlamalı (yani step değeri ile) dolaşım sağlayabilir miyiz? İşte ``Range`` 
yapısının ``step_by`` metodu parametre olarak atlama miktarını (step miktarını) bizden alır, bize başka bir dolaşım 
değeri verir. Bu dolaşım değerini dolaştığımızda artık değerleri atlamalı biçimde elde ederiz. Örneğin:

.. code-block:: rust

   let r: Range<i32> = Range {start: 0, end: 10};

   for i in  r.step_by(2) {
       //...
   }

Burada döngünün her yinelenmesinde sırasıyla ``0, 2, 4, 6, 8`` değerleri elde edilecektir. ``step_by`` metodunun
parametresi ``usize`` türündendir. Dolayısıyla biz ``Range`` sınıfı ile noktalı artırımlar (örneğin her defasında ``0.5``
artırım) yapamayız. Noktalı artırımlara pek çok programlama dilinde *sorunlu olduğu gerekçesiyle* izin verilmemiştir.
(Eğer noktalı artırım yapılabilseydi yuvarlama hataları nedeniyle bazı durumlarda döngünün yinelenme miktarı farklı
olabilirdi.) Artırımlı dolaşım için Rust'ta ``..`` operatörüyle ilgili başka pratik bir sentaks bulunmamaktadır. Bunun
en pratik yolu aşağıdaki gibidir:

.. code-block:: rust

   for i in (0..10).step_by(2) {
       //...
   }

Burada ``..`` operatörünün önceliği nedeniyle paranteze alındığına dikkat ediniz. Öncelik tablosunda ``..`` operatörü
düşük bir önceliktedir:

========================================== ==============================
Operatörler                                Associativity
========================================== ==============================
``()``                                     Soldan Sağa
``- !``                                    Sağdan Sola
``as``                                     Soldan Sağa
``* / %``                                  Soldan Sağa
``+ -``                                    Soldan Sağa
``<< >>``                                  Soldan Sağa
``&``                                      Soldan Sağa
``^``                                      Soldan Sağa
``|``                                      Soldan Sağa
``< > >= <= == !=``                        Parantezsiz Kombine Edilemez
``&&``                                     Soldan Sağa
``||``                                     Soldan Sağa
``.. ..=``                                 Parantezsiz Kombine Edilemez
``= += -= *= /= %= &= |= ^= <<= >>=``      Sağdan Sola
========================================== ==============================

Tersten Dolaşım (rev)
---------------------

Peki elimizde bir dolaşım ifadesi varsa biz onu tersten dolaşabilir miyiz? İşte bir dolaşım ifadesinin tersten
dolaşılabilirliği o dolaşım ifadesine ilişkin yapıyı yazanlar tarafından belirlenmektedir. Başka bir deyişle eğer
dolaşım ifadesine ilişkin yapıyı yazanlar izin vermişse tersten dolaşım mümkün olmaktadır. ``Range`` ifadesini tersten
dolaşabilmek için ``Range`` yapısının ``rev`` metodu kullanılmaktadır. ``rev`` metodu parametresizdir. ``rev`` metodu 
bize tersten dolaşım yapılabilecek yeni bir dolaşım nesnesi vermektedir. Örneğin:

.. code-block:: rust

   let r: Range<i32> = Range {start: 0, end: 10};

   for i in  r.rev() {
       //...
   }

Burada yine start değeri aralığa dahildir ancak end değeri aralığa dahil değildir. Yani burada ``i`` değişkenine 
sırasıyla ``9, 8, 7, 6, 5, 4, 3, 2, 1, 0`` değerleri yerleştirilecektir. Tabii biz ``rev`` metodunun verdiği dolaşım 
türünden değerin üzerinde de yeniden ``step_by`` metodunu çağırabiliriz. Örneğin:

.. code-block:: rust

   let r: Range<i32> = Range {start: 0, end: 10};

   for i in r.rev().step_by(2) {
      //...
   }

Burada ``i`` değişkeni sırasıyla ``9, 7, 5, 3, 1`` değerlerini alacaktır. Tabii biz ``rev`` metodunu ``..`` operatörü
üzerinde de uygulayabilirdik. Örneğin:

.. code-block:: rust

   for i in (0..10).rev() {
       //...
   }

Benzer biçimde ``rev`` metodunun verdiği dolaşım türünden değer üzerinde yine ``step_by`` metodunu uygulayabiliriz:

.. code-block:: rust

   for i in (0..10).rev().step_by(2) {
       //...
   }

Peki burada ``rev`` ve ``step_by`` çağrılarının sırasını değiştirirsek ne olur?

.. code-block:: rust

   for i in  (0..10).step_by(2).rev() {
       //...
   }

Burada önce ``step_by`` ile verilen dolaşım türünden değer dolaşıldığında ``0, 2, 4, 6, 8`` değerleri elde edileceğine
göre bu değerler de tersten dolaşıldığında ``i`` değişkeni sırasıyla ``8, 6, 4, 2, 0`` değerlerini alacaktır.

Inclusive Range Operatörü
=========================

Bazen dolaşıma end değerinin de dahil edilmesini isteyebiliriz. Örneğin aşağıdaki C döngüsünün Rust eşdeğerini yazmak
isteyelim:

.. code-block:: c

   for (int i = 0; i <= 10; ++i) {
       //...
   }

Burada ``i`` değişkeni ``10`` değerini de almaktadır. İşte bunu sağlamak için Rust'ın standart kütüphanesinde 
``std::ops`` modülünde ``RangeInclusive`` isimli bir yapı bulundurulmuştur. Ancak ``RangeInclusive`` yapısının start 
ve end elemanları private durumdadır. Bu nedenle ``RangeInclusive`` türünden bir değerin yaratılması yapının ``new`` 
isimli *ilişkili fonksiyonuyla (associated function)* yapılmaktadır. Tabii ``RangeInclusive`` yapısı da *genel (generic)*
bir yapıdır. Örneğin:

.. code-block:: rust

   use std::ops::RangeInclusive;

   let ri: RangeInclusive<i32> = RangeInclusive::new(0, 10);

   for i in  ri {
       //...
   }

Burada artık dolaşıma ``10`` değeri de dahil edilecektir. RangeInclusive türünden bir değeri kolay oluşturabilmek için
``..=`` operatörü de bulundurulmuştur. ``..=`` operatörüne İngilizce *inclusive range* operatörü denilmektedir. Bu
durumda yukarıdaki işlemin işlevsel eşdeğeri basit bir biçimde aşağıdaki gibi oluşturulabilmektedir. Örneğin:

.. code-block:: rust

   for i in  0..=10 {
       //...
   }

Burada dolaşıma ``10`` da dahil edilmiştir.

=================================================
Range Yapıları, Örnek Programlar ve break İfadesi
=================================================

RangeFrom Yapısı
================

``std::ops`` modülünde ``RangeFrom`` isimli bir dolaşım türü daha vardır. ``RangeFrom`` isimli yapı start değeri olan 
ancak end değeri olmayan dolaşımlar için kullanılmaktadır. Bu türden bir ifade ``for`` döngüsüyle dolaşılmaya çalışılırsa
start değerinden başlanarak artan sırada tamsayılar elde edilir. Ancak end değeri olmadığı için ifade bize sürekli olarak
değer verecektir. Böylece bir sonsuz döngü oluşacaktır. Örneğin:

.. code-block:: rust

   use std::ops::RangeFrom;

   let rf: RangeFrom<i32> = RangeFrom{start: 5};
   for i in rf {
       //...
   }

Burada ``5``'ten başlanarak sürekli artan tamsayılar elde edilecektir.

``RangeFrom`` yapısının da genel (generic) bir yapı olduğuna dikkat ediniz. Bu yapı da ``f32`` ve ``f64`` türleriyle 
*monomorfize (monomorphize)* edilirse artık elde edilen değişken ya da değer dolaşım ifadesi olmaktan çıkmaktadır.

``..`` operatöründe sol taraftaki operand belirtilir ancak sağ taraftaki operand belirtilmezse bu durum ``RangeFrom`` 
yapısı türünden bir değerin yaratılacağı anlamına gelmektedir. Örneğin:

.. code-block:: rust

   for i in 5.. {
       //...
   }

Burada ``5..`` ifadesi RangeFrom türünden bir değerin yaratılıp kullanılması anlamına gelmektedir. Yani yukarıdaki 
``for`` ifadesi aşağıdakiyle eşdeğerdir:

.. code-block:: rust

   for i in (RangeFrom {start: 5}) {
       //...
   }

RangeTo, RangeToInclusive ve RangeFull Yapıları
===============================================

RangeTo
-------

``std::ops`` modülünde dolaşım sağlayamayan ``RangeTo``, ``RangeToInclusive`` ve ``RangeFull`` isimli üç yapı daha 
vardır. ``RangeTo`` yapısı start değeri olmayan ancak end değeri olan bir range değişkeni oluşturmaktadır. Ancak 
``RangeTo`` yapısı yukarıda da belirttiğimiz gibi bir dolaşım türü (iterator type) değildir. Yani biz bir ``RangeTo``
türünden bir ifadeyi ``for`` döngüsünde kullanamayız. Bu nedenle aşağıdaki kod error oluşacaktır. Örneğin:

.. code-block:: rust

   use std::ops::RangeTo;

   let rt: RangeTo<i32> = RangeTo {end: 10};
   for i in rt  {          // error! RangeTo değişkeni bir dolaşım ifadesi değildir
       //...
   }

Yukarıda ``for`` döngüsünde ``RangeTo`` değişkeni bir dolaşım ifadesi olmadığı için error oluşacaktır. ``RangeTo`` yapısı
türünden değerler de ``..`` operatörüyle oluşturulabilmektedir. Bunun için operatörün sol tarafındaki operand'ı boş 
bırakılır, yalnızca sağ tarafındaki operand'ı belirtilir. Örneğin:

.. code-block:: rust

   use std::ops::RangeTo;

   let rt: RangeTo<i32> = ..10;

``RangeTo`` türünden değişkenlerde end değeri aralığa dahil değildir. ``RangeTo`` konusunda iki soru akla gelmektedir: 
Birincisi *start değerinin kaç olduğu* sorusudur. İkincisi de *bu türden bir ifade for döngüsüyle dolaşılmadığına göre 
ne işe yarıyor* sorusudur.

``RangeTo`` yapısında start değerinin ne olduğuna yönelik bir açıklamada bulunulmamıştır. Ancak pek çok bağlamda bu değer
``0`` olarak kabul edilmektedir. ``RangeTo`` yapısı aslında değişik bağlamlarda kullanılabilmektedir. Programcılar da 
kendi yapılarında eğer anlamlıysa ``RangeTo`` kullanımını mümkün hale getirebilmektedir. Yani ``RangeTo`` yapısı dolaşım
amaçlı değil de başka amaçlarla kullanılabilmektedir. Örneğin bir diziyi dilimlerken biz ``RangeTo`` türünden ifade 
kullanabiliriz. ``RangeTo`` türünden ifadelerin bu tür kullanımları ilgili konuların ele alındığı yerde açıklanacaktır.

RangeToInclusive
----------------

RangeTo yapısının semantiğinde end değeri aralığa dahil değildir. Eğer end değerinin de aralığa dahil olması isteniyorsa
``RangeToInclusive`` yapısı kullanılmalıdır. ``RangeToInclusive`` türünden bir değişken benzer biçimde yaratılmaktadır:

.. code-block:: rust

   let rti: RangeToInclusive<i32> = RangeToInclusive { end: 10 };

Tabii ``RangeToInclusive`` yapısı da bir dolaşım türü belirtmemektedir. Yani bu yapı türünden değişken de ``for`` 
döngüsüyle dolaşılamamaktadır. ``RangeToInclusive`` türünden değerler ``..=`` operatörü ile *sol taraftaki operand boş 
bırakılıp sağ taraftaki operand belirtilerek* de yaratılabilmektedir. Örneğin:

.. code-block:: rust

   let rti: RangeToInclusive<i32> = ..=10;

RangeFull
---------

Son range yapısı da RangeFull isimli yapıdır. RangeFull yapısı genel (generic) bir yapı değildir. Bu yapı da semantik
bakımdan start ve end değeri olmayan bir aralık oluşturmak için kullanılmaktadır. Örneğin:

.. code-block:: rust

   let rf: RangeFull = RangeFull {};

RangeFull yapısı da bir dolaşım türü belirtmez. Yani biz bu yapı türünden ifadeleri de for döngüsünde kullanamayız. Bu
yapı çeşitli bağlamlarda *bütünü temsil etmek için* kullanılmaktadır. RangeFull türünden değer ``..`` operatörü ile
*operatörün sol tarafındaki ve sağ tarafındaki operand boş bırakılarak* oluşturulabilmektedir. Örneğin:

.. code-block:: rust

   let rf: RangeFull = ..;

Range Yapılarının Özeti
=======================

Şimdi de yukarıda tek tek gördüğümüz range yapılarını bir tablo biçiminde özetleyelim:

==================== ============================ ================
Yapı İsmi            Range Operatör İle Yaratım   Dolaşılabilirlik
==================== ============================ ================
Range                ``a..b``                     Dolaşılabilir
RangeInclusive       ``a..=b``                    Dolaşılabilir
RangeFrom            ``a..``                      Dolaşılabilir
RangeTo              ``..b``                      Dolaşılamaz
RangeToInclusive     ``..=b``                     Dolaşılamaz
RangeFull            ``..``                       Dolaşılamaz
==================== ============================ ================

Range Sabitlerinde Tür Belirleme
================================

Rust'ta sonek almayan tamsayı sabitlerinin bağlama göre değerlendirildiğini yani ilgili türün sınırları içerisinde
kalıyorsa o türden kabul edildiğini belirtmiştik. Bu durum range operatörlerinde de geçerlidir. Örneğin:

.. code-block:: rust

   let r: Range<i8> =1..10;

Burada ``1..10`` range ifadesindeki ``1`` ve ``10`` zaten ``i8`` türünden kabul edilecektir. Yani türlerin aşağıdaki 
gibi açıkça yazılmasına gerek yoktur:

.. code-block:: rust

   let r: Range<i8> =1i8..10i8;

Tabii bağlam belli değilse sabitlerin ``i32`` türünden olduğu varsayılır. Örneğin:

.. code-block:: rust

   let r = 1..10;

Bu bağlamanın eşdeğeri şöyledir:

.. code-block:: rust

   let r = 1i32..10i32;

Örnek: 1'den n'e Kadar Sayıların Toplamı
========================================

Aşağıda 1'den n'e kadar sayıların toplamını döngü yoluyla hesaplayan bir fonksiyon örneği verilmiştir. (Tabii
aslında 1'den n'e kadar sayıların toplamını döngüyle hesaplamaya gerek yoktur.)

.. code-block:: rust

   fn main() {
       let result: i32;

       result = get_total(100);
       println!("{}", result);
   }

   fn get_total(n: i32) -> i32 {
       let mut total = 0;

       for i in 1..=n {
           total += i;
       }
       total
   }

Örnek: Asal Sayıların Bulunması
===============================

Aşağıda bir sayının asal olup olmadığını belirleyen ``isprime`` isimli bir fonksiyon yazılmıştır. Fonksiyon ``i32`` 
türünden bir parametreye sahiptir. Eğer girilen argüman asalsa fonksiyon ``true`` değerine asal değilse ``false`` 
değerine geri dönmektedir. Fonksiyon şöyle yazılmıştır:

.. code-block:: rust

   fn isprime(val: i32) -> bool {
       let end = (val as f64).sqrt() as i32;

       if val % 2 == 0 {
           return val == 2;
       }
       for i in (3..=end).step_by(2) {
           if val % i == 0 {
               return false;
           }
       }
       true
   }

Öklit teoremine göre asal olmayan her sayının en az iki asal çarpanı vardır. Bu iki asal çarpanın ikisi de sayının
karekökünden büyük olmaz. O halde sayı asal değilse sayının kareköküne kadar (karekökü de dahil) en az bir asal çarpanı
olmak zorundadır. Yani kontrol sayının kareköküne kadar yapılabilir. Bütün çift sayıları kontrol etmeye de gerek yoktur.
En başta sayının çift olup olmadığı yalnızca bir kez kontrol edilebilir. Sonra hep tek sayılarla kontrol yapılabilir.
Rust'ta bir sayının karekökü ``f32`` ve ``f64`` türleri üzerinde ``sqrt`` metodu çağrılarak elde edilebilir. Yukarıdaki
``isprime`` fonksiyonunun parametresi ``i32`` türündendir. Bu nedenle önce bu parametre ``f64`` türüne dönüştürülüp 
``sqrt`` metodu çağrılmıştır. Tabii elde edilen değer yeniden ``i32`` türüne dönüştürülmüştür. Aşağıda ``2``'den 
klavyeden girilen sayıya kadarki tüm asal sayılar yazdırılmıştır.

.. code-block:: rust

   use std::io::{stdin, Write};

   fn main() {
       let val: i32;

       print!("Bir sayı giriniz:");
       std::io::stdout().flush().expect("Can't flush stdout");
       val = getval();

       for i in 2..=val {
           if isprime(i) {
               print!("{} ", i);
           }
       }
   }

   fn isprime(val: i32) -> bool {
       let end = (val as f64).sqrt() as i32;

       if val % 2 == 0 {
           return val == 2
       }
       for i in (3..=end).step_by(2) {
           if val % i == 0 {
               return false;
           }
       }
       true
   }

   fn getval() -> i32 {
       let mut buf: String = String::new();

       stdin().read_line(&mut buf).expect("cannot read line..");
       buf.trim().parse().expect("not a number")
   }

for Döngülerinde mut Döngü Değişkeni
====================================

``for`` döngülerinde döngü değişkeni (aslında kalıp) ``mut`` da olabilir. Örneğin:

.. code-block:: rust

   for i in 0..10 {
       //...
       i += 1;                  // error!
       //...
   }

Burada döngü değişkeni ``mut`` olmadığı için içerisindeki değer döngü içerisinde değiştirilememektedir. Eğer bu değerin
döngü içerisinde değiştirilmesini istiyorsanız. Döngü değişkenini ``mut`` yapabilirsiniz:

.. code-block:: rust

   for mut i in 0..10 {
       //...
       i += 1;                  // geçerli
       //...
   }

break İfadesi
=============

Rust'ta diğer pek çok dilde de olduğu gibi ``break`` ifadesi bulunmaktadır. Biz de zaten ``break`` ifadesini bazı 
örneklerimizde kullanmıştık. ``break`` ifadesinin genel biçimi şöyledir:

.. code-block:: text

   break [etiket] [ifade];

*The Rust Reference* dokümanlarında *break deyimi* yerine *break ifadesi* terimi kullanılmıştır. Tabii ``break`` anahtar
sözcüğünün yanına ``';'`` konulduğunda zaten bu ifade de bir deyim olmaktadır. Biz kursumuzda *break ifadesi* demeyi
tercih edeceğiz.

Etiketsiz break
---------------

``break`` ifadesi döngü deyimini sonlandırmak için kullanılmaktadır. Genel biçimden ``break`` anahtar sözcüğünün yanında 
bir etiket (label) bulunabileceği anlaşılmaktadır. Peki bu etiket ne anlama gelmektedir? İşte eğer ``break`` ifadesi 
etiketsiz kullanılırsa yalnızca içinde bulunduğu döngü deyimini sonlandırmaktadır. Örneğin:

.. code-block:: rust

   for i in 0..10 {
       for k in 0..10 {
           println!("({}, {})", i, k);
           if i == 5 && k == 5 {
               break;
           }
       }
   }

Burada ``i == 5`` ve ``k == 5`` durumunda programın akışı break ifadesini görecektir. Ancak yalnızca içteki döngü
sonlanacaktır. Anımsanacağı gibi C, C++, C# gibi dillerde iç bir döngüde tek hamlede tüm döngülerden çıkmak için ``goto``
deyimi kullanılıyordu. Ancak Java gibi Rust gibi bazı dillerde ``goto`` deyimi bulunmamaktadır. İşte bu dillerde iç 
döngüden tek hamlede çıkışı sağlamak için etiketler kullanılmaktadır. Döngü deyimlerinin BNF gramerini hatırlayalım, 
şöyleydi:

.. code-block:: text

   LoopExpression :
     LoopLabel? (
       InfiniteLoopExpression
       | PredicateLoopExpression
       | PredicatePatternLoopExpression
       | IteratorLoopExpression
       | LabelBlockExpression
     )

Etiketli break
--------------

Bu gramerde tüm döngü deyimlerinin başında isteğe bağlı olarak bir etiketin (label) bulunabileceği anlaşılmaktadır.
Rust'ta etiketler tek tırnak karakterine yapışık bir değişken isminden oluşmaktadır. Döngü etiketlerinde yine etiket
isminden sonra ``':'`` atomu da kullanılmak zorundadır. Örneğin:

.. code-block:: rust

   'lbl: loop {
       //...
   }

Burada ``'lbl`` etiketi belirtmektedir. İşte break işlemi yapılırken break anahtar sözcüğünün yanına *hangi döngünün
kırılacağını belirten* bir etiket getirilebilmektedir. Örneğin:

.. code-block:: rust

   'lbl: loop {
       i += 1;
       if i == 10 {
           break 'lbl;
       }
       println!("{}", i);
   }

Tabii iç içe döngü söz konusu olmadıktan sonra döngüye etiket iliştirmenin ve etiketli ``break`` kullanmanın anlamı 
yoktur. Fakat iç içe döngülerden tek hamlede çıkmak için etiketli ``break`` kullanmak gerekir. Örneğin:

.. code-block:: rust

   'lbl:
   for i in 0..10 {
       for k in 0..10 {
           println!("({}, {})", i, k);
           if i == 5 && k == 5 {
               break 'lbl;
           }
       }
   }

Burada ``break`` ile tek hamlede iki döngüden de çıkılmıştır. Döngü bir etikete sahipse ancak ``break`` ifadesinde etiket
kullanılmamışsa Rust derleyicisi uyarı vermektedir.

İfadeli break
-------------

Biz daha önce break ifadesinin yanında bir ifade de kullanmıştık. Rust'ta ifadeli ``break`` yalnızca ``loop`` döngüsü 
içerisinde kullanılabilmektedir. Bu durumda ``loop`` döngüsü *divergent* olmaktan çıkıp break ifadesindeki değeri 
üretiyordu. Örneğin:

.. code-block:: rust

   let mut i = 0;
   let result: i32;

   result = loop {
       println!("{}", i);
       i += 1;
       if i == 10 {
           break 1000;
       }
   };
   println!("{}", result);

Burada ifadeli ``break`` ifadesi nedeniyle ``loop`` döngüsünden ``i32`` türünden değer elde edilmiştir. Tabii ``loop`` 
döngüsü içerisinde hem etiket hem de ifade birlikte kullanılabilir. Örneğin:

.. code-block:: rust

   let mut i = 0;
   let result: i32;

   result = 'lbl: loop {
       //...
       i += 1;
       if i == 10 {
           break 'lbl 1000;
       }
   };
   println!("{}", result);

Burada ``break`` anahtar sözcüğünde önce etiket sonra ifade bulundurulmuştur. Bunları ters sırada yerleştiremeyiz. Burada
bir kez daha anımsatmak istiyoruz: ifadeli ``break`` ifadesi yalnızca ``loop`` döngüsü içerisinde kullanılabilmektedir. 
``while`` ve ``for`` döngülerinde kullanılamamaktadır. Örneğin:

.. code-block:: rust

   let result = for i in 0..100 {
       //...
       if i == 5 {
           break 1000;         // error
       }
   }

C, C++, Java ve C# gibi dillerde ``break`` ifadesi o dillerin ``switch`` deyimleri içerisinde de kullanılabilmektedir. 
Rust'taki ``switch`` benzeri deyime ``match`` deyimi denilmektedir. ``match`` deyimi içerisinde break kullanımı yoktur.

break İfadesinin Türü
---------------------

``break`` ifadesinin kendi ürettiği değer (yani deyimin yerleştirildiği yerden elde edilen değer) ``!`` türündendir. 
Yani break ifadesi divergent biçimdedir. ``!`` türünün herhangi bir türe otomatik dönüştürüldüğünü anımsayınız. Bu 
durumda örneğin anlamlı olmasa da biz break ifadesini herhangi bir değişkene atayabiliriz:

.. code-block:: rust

   loop {
       //...
       if ifade {
           x = break;      // x hangi türden olursa olsun geçerli!
       }
   }

===============================================
break Üzerine Notlar, continue ve match İfadesi
===============================================

break İfadesi ve Noktalı Virgül
===============================

Burada konuya devam etmeden önce sınıfta sorulan bir sorunun yanıtı hakkında bir açıklamada bulunmak istiyoruz. Soru
şöyle: *Neden aşağıdaki kodda break ifadesinden sonra* ``';'`` *konulmadığında error ortaya çıkmıyor?* Yani aşağıdaki
kodda neden ``';'`` bulunmaması soruna yol açmıyor?

.. code-block:: rust

   fn main() {
       let mut i = 0;
       let result: i32;

       result = loop {
           if i == 10 {
               break 1000
           }
           println!("{}", i);
           i += 1;
       };

       println!("\n{}", result);
   }

Burada break ifadesi ``if`` ifadesinin doğruysa bloğunun son ifadesi durumunda olduğu için, blokların da değer 
üretmesinden dolayı ``';'`` kullanılmasa da ifade geçerli olmaktadır. ``break`` ifadesinin ``!`` türünden değer 
ürettiğini anımsayınız. Bu durumda ``if`` ifadesinin ``else`` kısmı olmadığı için ``else`` kısmından ``()`` değerinin 
elde edildiği kabul edilmektedir. ``!`` türü de her türe otomatik dönüştürülebildiği için bu ``if`` ifadesinden ``()``
değeri elde edilecektir. Tabii burada ``if`` ifadesinin ``else`` kısmı olsaydı ve ``else`` kısmından başka türden bir
değer elde edilseydi kod anlamsız fakat yine geçerli olurdu. Örneğin:

.. code-block:: rust

   fn main() {
       let mut i = 0;
       let result: i32;

       result = loop {
           if i == 10 {
               break 1000
           }
           else {
               //...
               100         // anlamsız ama geçerli
           };
           println!("{}", i);
           i += 1;
       };

       println!("\n{}", result);
   }

Burada ``if`` ifadesinden ``i32`` türünden değer elde edilecektir. Ancak bu değer kullanılmadığı için else kısmının bir 
değer üretmesi anlamsızdır.

Tabii ``if`` ifadesinin hem doğruysa hem de yanlışsa kısmında ``break`` ifadesi varsa bu durumda ``if`` ifadesi de ``!`` 
türünden değer üretir. Örneğin:

.. code-block:: rust

   loop {
       //...
       if koşul {
           //...
           break
       }

       else {
           //...
           break
       }
   }

Burada ``if`` deyimi ``!`` türünden değer vermektedir.

Etiketli Blok İfadeleri
=======================

Rust'ta aslında bir döngü belirtmediği halde gramerde ``LoopExpression`` ara sembolünün altına yerleştirilen ve ismine
*etiketli block ifadesi (labeled block expression)* denilen bir deyim daha vardır. ``LoopExpression`` ara sembolünün
açılımını aşağıda yeniden veriyoruz:

.. code-block:: text

   LoopExpression :
     LoopLabel? (
       InfiniteLoopExpression
       | PredicateLoopExpression
       | PredicatePatternLoopExpression
       | IteratorLoopExpression
       | LabelBlockExpression
     )

Bir blok ifadesi içerisinde ``break`` ifadesi kullanılabilir. Ancak bu durumda ``break`` ifadesinde bir etiketin olması 
ve bloğun başında da bir etiketin bulunması gerekir. Örneğin:

.. code-block:: rust

   val = getval();
   'a: {
       println!("block begins");
       if val > 0 {
           break 'a;
       }
       println!("block ends");
   };

Bu blok ifadesi geçerlidir. İşte bu blok ifadesine *etiketli blok ifadesi* denilmektedir. Yukarıda da belirttiğimiz gibi
bir bloğun içerisinde ``break`` ifadesinin kullanılabilmesi için ``break`` ifadesinin etiketli olması zorunludur. 
Örneğin:

.. code-block:: rust

   val = getval();
   {
       println!("block begins");
       if val > 0 {
           break;          // error!
       }
       println!("block ends");
   };

Bu blok ifadesinde ``break`` etiketsiz kullanıldığı için error oluşacaktır. Etiketli blok ifadelerinde hiç ``break``
kullanılmaması error oluşturmasa da anlamsız olduğu gerekçesiyle uyarı mesajı oluşturmaktadır. Örneğin:

.. code-block:: rust

   'a: {
       println!("block begins");
       //...
       println!("block ends");
   };

Yukarıdaki kod geçerlidir fakat bloğa etiket iliştirilmesi anlamsız olduğu için uyarı oluşturacaktır. Tabii etiketli
blok ifadelerindeki ``break`` ifadesinde ``break`` anahtar sözcüğünün yanında hem etiket hem de ifade bulunabilir. Bu 
durumda önce etiketin sonra ifadenin bulundurulması zorunludur. Örneğin:

.. code-block:: rust

   val = getval();
   result = 'a: {
       println!("block begins");
       if a > 0 {
           break 'a 100;
       }
       println!("block ends");
       200
   };
   println!("{}", result);

continue İfadesi
================

Pek çok programlama dilinde bulunan ``continue`` ifadesi Rust'ta da bulunmaktadır. ``continue`` ifadesinin genel biçimi
şöyledir:

.. code-block:: text

   continue [etiket];

``continue`` anahtar sözcüğü de sonunda ``';'`` olmazsa bir ifade gibi kullanılabildiği için biz *break ifadesi* yerine
*continue ifadesi* demeyi tercih edeceğiz.

``continue`` ifadesi ancak döngü deyimlerinin içerisinde kullanılabilir. Etiketli blok deyimi içerisinde ``continue``
kullanılamaz. Programın akışı ``continue`` ifadesini gördüğünde akış sonraki yinelemeye geçer. Yani ``continue`` ifadesi 
döngü deyimini değil döngü deyimi içerisindeki blok ifadesini sonlandırmaktadır. ``continue`` ifadesinin döngü deyimini
sonlandırdığına ``continue`` deyiminin ise döngünün içerisindeki blok ifadesini sonlandırdığına dikkat ediniz. Döngü
içerisindeki blok ifadesi bittiğinde ne oluyorsa programın akışı ``continue`` ifadesini gördüğünde aynısı olmaktadır.
Örneğin:

.. code-block:: rust

   for i in 0..10 {
       if i % 2 == 0 {
           continue;
       }
       println!("{}", i);
   }

Burada ekrana tek sayılar basılacaktır. Programın akışı ``continue`` ifadesini gördüğünde sanki döngü içerisindeki blok
sonlanmış gibi bir etki oluşacağı için Range iteratörü de yeni bir değer verecektir.

Genel biçimden ``continue`` deyiminin de bir etikete sahip olabileceğini görüyorsunuz. Diğer pek çok dilde iç içe 
döngülerde ``continue`` yalnızca kullanıldığı döngünün deyimini sonlandırmaktadır. Ancak Rust'taki bu etiket sayesinde
iç içe döngülerde dıştaki döngü üzerinde de ``continue`` yapılabilmektedir. Örneğin:

.. code-block:: rust

   'a: for k in 0..10 {
       for i in 0..10 {
           //...
           if ifade {
               continue 'a;
           }
       }
       //...
   }

Burada artık ``continue`` ifadesi içteki döngünün blok ifadesini değil dıştaki döngünün blok ifadesini sonlandırmaktadır.

Biz yukarıdaki ``continue`` ifadesinin çalışmasını açıklayacak örnekler verdik. Bu örneklerde aslında ``continue`` 
kullanılmadan da hedeflenen işlemler yapılabilirdi. Peki ``continue`` ifadesine hangi durumlarda gereksinim 
duyulmaktadır? Bazı algoritmalarda (özellikle ``switch`` içeren kodlarda) ``continue`` ifadesinin kullanılması kodu 
kısaltabilmektedir. Ancak ``continue`` ifadesi çoğu kez döngü içerisinde büyük ``if`` bloklarını elimine etmek için
kullanılmaktadır. Aşağıdaki gibi kod
parçası olsun:

.. code-block:: rust

   loop {
       val = getval();
       if val > 0 {
           //...
       }
   }

Burada döngü içerisindeki ``if`` ifadesinin çok uzun olduğunu varsayalım. Bu ``if`` ifadesinin içerisinde de pek çok
kontrol deyimi kullanılmış olsun. Biz yukarıdaki döngüde bu geniş ``if`` bloğunu ``continue`` sayesinde elimine 
edebiliriz.

.. code-block:: rust

   loop {
       val = getval();
       if val <= 0 {
           continue;
       }
       //...
   }

``continue`` ifadesi de tıpkı break ifadesi gibi ``!`` değerini üretmektedir. Yani continue ifadesi de *divergent* 
durumdadır. ``continue`` ifadesini biz herhangi türden bir değişkene atayabiliriz. Örneğin yukarıdaki ``if`` ifadesinde
``else`` kısım herhangi bir türden olabilirdi:

.. code-block:: rust

   loop {
       val = getval();
       if val <= 0 {
           continue;
       }
       else {
           100         // geçerli ama anlamsız!
       };
       //...
   }

match İfadesi
=============

match İfadesine Giriş
---------------------

Bu bölümde göreceğimiz son kontrol deyimi ``match`` deyimidir. Rust'taki ``match`` deyimi C, C++, Java ve C# gibi 
dillerdeki ``switch`` deyimine benzemektedir. Ancak Rust'taki ``match`` deyimi bu dillerdeki ``switch`` deyiminden 
çok daha fazla olanaklara sahip bir deyimdir. Dolayısıyla ``match`` deyiminin Rust'ta kullanımı bu dillerdeki ``switch``
deyiminin kullanımından çok daha yoğundur.

Rust'ta ``match`` deyimi *kalıp uyuşumu (pattern matching)* kurallarıyla çalışmaktadır. Kalıp uyuşumu konusu ise oldukça
ayrıntılıdır. Bu konunun henüz görmediğimiz dizilerle, demetlerle, enum türleriyle, yapılarla bağlantısı vardır. Bu
nedenle biz bu bölümde ``match`` deyimini henüz geldiğimiz noktaya kadarki konular eşliğinde ele alacağız. Sonra bu
konuların anlatıldığı yerde çeşitli eklemeler yapacağız. En sonunda da kalıp uyuşumu konusunu bütünsel olarak yeniden
gözden geçireceğiz.

Aslında kalıp uyuşumuyla çalışan ``switch`` ya da ``match`` deyimleri Rust'a özgü değildir. Ruby gibi, Swift gibi, Kotlin
gibi, hatta Python gibi dillerde de kalıp uyuşumuyla çalışan ``match`` benzeri deyimler bulunmaktadır.

match İfadesinin Genel Biçimi
-----------------------------

Rust'ta ``match`` ifadesinin genel biçimi şöyledir:

.. code-block:: text

   match <ifade> {
       <kalıp1> => ifade1,
       <kalıp2> => ifade2,
       <kalıp3> => ifade3,
       ....
   }

``match`` anahtar sözcüğünü bir ifade izlemek zorundadır. Buradaki ifade herhangi bir türe ilişkin olabilir. ``match``
ifadesinin aslında çalışması oldukça basittir. Derleyici ``match`` anahtar sözcüğünün yanındaki ifadeyi inceler. Sonra
match kalıplarını yukarıdan aşağıya doğru ele alır. Kalıbın uyuştuğu ilk ifadeyi çalıştırır. ``match`` ifadesinden bu
çalıştırılan ifadenin değeri elde edilir. ``match`` ifadesinde diğer bazı dillerdeki ``case`` anahtar sözcüğünün
bulunmadığına dikkat ediniz. ``match`` ifadesinde ``break`` ifadesi kullanılırsa bu ``break`` ifadesinin ``match``
ifadesi ile bir ilgisi yoktur. ``match`` ifadesinin içinde bulunduğu döngü ile ilgisi vardır. ``match`` deyiminde C 
gibi, Java gibi dillerdeki *aşağıya düşme (fall through)* mekanizması da yoktur. Dolayısıyla Rust'taki ``match`` 
deyiminde yalnızca uyuşan ilk kalıbın ifadesi işletilmektedir.

*The Rust Reference* dokümanlarında ``match`` deyimindeki ``=>`` içeren her kısma *match kolu (match arm)* denilmektedir.
``match`` deyiminde akışın hangi kola aktarılacağını anlayabilmek için kalıp uyuşumu konusunun bilinmesi gerekmektedir. 
Biz burada geldiğimiz noktaya kadarki kalıp uyuşumları üzerinde duracağız. Rust'ta ``match`` deyiminde mutlaka bir match
kolunun kalıp uyuşumunu sağlaması gerekir. Aksi takdirde derleme sırasında error oluşmaktadır. Bu duruma Rust'ta 
``match`` deyiminin *exhaustive* olması denilmektedir.

Kalıpların BNF Grameri
----------------------

*The Rust Reference* dokümanlarında kalıpların (patterns) BNF grameri şöyle verilmiştir:

.. code-block:: text

   Pattern :
     |? PatternNoTopAlt ( | PatternNoTopAlt )*

   PatternNoTopAlt :
       PatternWithoutRange
     | RangePattern

   PatternWithoutRange :
       LiteralPattern
     | IdentifierPattern
     | WildcardPattern
     | RestPattern
     | ReferencePattern
     | StructPattern
     | TupleStructPattern
     | TuplePattern
     | GroupedPattern
     | SlicePattern
     | PathPattern
     | MacroInvocation

Görüldüğü gibi her kalıp bir isimle belirtilmektedir.

Rust'ta kalıp uyuşumu ile ilgili olan tek deyim ``match`` deyimi değildir. Aşağıdaki deyimlerde de kalıp uyuşumu etkili
olmaktadır:

* let deyimi
* if let deyimi
* while let deyimi
* for deyimi

Ayrıca fonksiyonların parametrelerinde de genel olarak kalıplar kullanılabilmektedir.

Sabit Kalıbı (Literal Pattern)
------------------------------

En basit kalıp *sabit kalıbı (literal pattern)* denilen kalıptır. İsmi üzerinde sabit kalıbında kalıp ifadesi
sabitlerden oluşturulmaktadır. Bu durumda ``match`` deyimi C'deki ``switch`` deyimine benzer hale gelmektedir. ``match``
anahtar sözcüğünün yanındaki ifadeyle ilk eşleşen sabit kalıbının ifadesi çalıştırılacaktır. ``match`` deyiminin 
*exhaustive* olmasını sağlamak için ``_`` kalıbı kullanılır. Bu kalıba *wildcard kalıbı (wildcard pattern)* da 
denilmektedir. Wildcard kalıbı her ifadeyle uyuşum sağlamaktadır. Bu nedenle wildcard kalıbı genellikle ``match``
ifadesini *exhaustive* hale getirmek için ``match`` ifadesinin son kolunda bulundurulur. Örneğin:

.. code-block:: rust

   match val {
       1 => println!("bir"),
       2 => println!("iki"),
       3 => println!("üç"),
       _ => println!("hiçbiri"),
   }

Burada ``val`` ifadesinin çeşitli değerleri için farklı ifadeler yapılmaktadır. Tabii bu örnekte ``println!`` makrosu
birim değeri ürettiği için match ifadesi de birim değerini üretmektedir.

``match`` ifadesinin son kolunun ``','`` atomu ile bitirilmesi zorunlu değildir. Ancak Rust programcılarının çoğu son
kola da ``','`` atomunu yerleştirmektedir. Örneğin yukarıdaki ``match`` ifadesi aşağıdaki gibi de olabilirdi:

.. code-block:: rust

   match val {
       1 => println!("bir"),
       2 => println!("iki"),
       3 => println!("üç"),
       _ => println!("hiçbiri")
   }

.. code-block:: rust

   use std::io::Write;

   fn main() {
       let val: i32;

       print!("Bir sayı giriniz:");
       std::io::stdout().flush().expect("cannot flush stdout!..");
       val = getval();
       match val {
           1 => println!("bir"),
           2 => println!("iki"),
           3 => println!("üç"),
           _ => println!("hiçbiri"),
       }
   }

   fn getval() -> i32 {
       let mut buf: String = String::new();

       std::io::stdin().read_line(&mut buf).expect("read line failed");
       buf.trim().parse().expect("parse into number")
   }

Farklı Türlerde Sabit Kalıpları
-------------------------------

Sabit kalıbında (literal pattern) kullanılacak sabit herhangi bir türe ilişkin olabilir. Örneğin:

.. code-block:: rust

   match val {
       'a' => println!("a"),
       'b' => println!("b"),
       'c' => println!("c"),
       _ => println!("hiçbiri"),
   }

C, C++, Java ve C# gibi dillerde ``switch`` anahtar sözcüğünün yanındaki kontrol ifadesinin tamsayı türlerine ilişkin
olması gerekmektedir. Ancak Rust'ta böyle bir zorunluluk yoktur. Ancak ne olursa olsun yuvarlama hatalarından dolayı
sabit kalıplarında gerçek sayı sabitlerini kullanmak kötü bir tekniktir. Örneğin:

.. code-block:: rust

   match val {
       1.0 => println!("bir"),
       2.0 => println!("iki"),
       3.0 => println!("üç"),
       _ => println!("hiçbiri"),
   }

Sabit kalıbında sabitlerin match anahtar sözcüğünün yanındaki kontrol kalıbındaki tür türünden olması gerekmektedir.

Sabit kalıbında eğer ``match`` anahtar sözcüğünün yanındaki ifade ``&str`` türündense sabitler string de olabilmektedir.
Örneğin:

.. code-block:: rust

   let city: &str = "eskişehir";
   //...

   match city {
       "ankara" => println!("6"),
       "izmir" => println!("35"),
       "adana" => println!("1"),
       "eskişehir" => println!("26"),
       _ => println!("hiçbiri"),
   }

Burada da görüldüğü gibi eğer ``match`` kollarında sabit kalıbı olarak string sabiti kullanılacaksa ``match`` anahtar 
sözcüğünün yanındaki ifadenin de ``&str`` türünden olması gerekir.

================================================================
match İfadesi (Devam): Kollar, Veya, Range ve Değişken Kalıpları
================================================================

Komut Satırı Örneği
===================

Aşağıda basit bir komut satırı örneği verilmiştir. Bu örnekte bir döngü içerisinde ``stdin`` dosyasından komutlar alınmış
ve ``match`` ifadesi ile bu komutlar işlenmiştir. Programdaki ``match`` deyiminin nasıl oluşturulduğuna dikkat ediniz:

.. code-block:: rust

   match getstr().as_str() {
       "" => continue,
       "dir" => println!("dir command"),
       "copy" => println!("copy command"),
       "rename" => println!("rename command"),
       "quit" => break,
       _ => println!("unknown command"),
   }

Tabii burada komutların işlenmesi için fonksiyonlar oluşturulup her ``match`` kolunda da ilgili fonksiyon çağrılabilirdi.
Örneğin:

.. code-block:: rust

   match getstr().as_str() {
       "" => continue,
       "dir" => dir_proc(),
       "copy" => copy_proc(),
       "rename" => rename_proc(),
       "quit" => break,
       _ => println!("unknown command"),
   }

.. code-block:: rust

   use std::io::{self, Write};

   fn main() {
      loop {
         print!("CSD>");
         io::stdout().flush().expect("cannot flush stdout!");

         match getstr().as_str() {
            "" => continue,
            "dir" => println!("dir command"),
            "copy" => println!("copy command"),
            "rename" => println!("rename command"),
            "quit" => break,
            _ => println!("unknown command"),
         }
      }
   }

   fn getstr() -> String {
      let mut buf: String = String::new();

      std::io::stdin().read_line(&mut buf).expect("read line failed");
      buf.trim().to_string()
   }

Birden Fazla Deyim İçeren match Kolları
=======================================

Bir ``match`` kolunda birden fazla deyim bulundurulmak isteniyorsa blok ifadesi oluşturulmalıdır. Örneğin:

.. code-block:: rust

   match val {
       1 => {
           //...
       },
       2 => {
           //...
       },
       3 => {
           //...
       },
       _ => ()
   }

match İfadesinin Değer Üretmesi
===============================

Tabii ``match`` ifadesi de diğer deyimlerde olduğu gibi bir değer de üretmektedir. ``match`` ifadesinden elde edilen 
değer çalıştırılan ``match`` kolundaki ifadenin değeridir. Tabii bu durumda ``match`` ifadesinin bütün kollarından 
elde edilen değerlerin aynı türden olması gerekir. Örneğin:

.. code-block:: rust

   loop {
       print!("CSD>");
       io::stdout().flush().expect("cannot flush stdout!");
       let result: i32;

       result = match getstr().as_str() {
           "" => continue,
           "quit" => break,
           "dir" => {
               println!("dir command");
               1
           },
           "copy" => {
               println!("copy command");
               2
           },
           "rename" => {
               println!("rename command");
               3
           },
           _ => {
               println ! ("unknown command");
               0
           }
       };
       println!("result: {}", result);
   }

Burada ``match`` ifadesinin her kolu ``i32`` türünden değer üretmektedir. ``break`` ve ``continue`` ifadelerinin never 
türünden (! türünden) değer ürettiğini anımsayınız. Never türü diğer her türe otomatik dönüştürülebildiği için diğer her
türle bu bağlamda bir arada bulunabilmektedir.

Peki aşağıdaki gibi bir ``match`` ifadesindeki ``result`` değişkeninin ``match`` kolundaki kullanımı geçerli midir?

.. code-block:: rust

   let result: i32;

   result = match val {
       1 => {
           println!("{}", result);         // error!
           10
       }
       //...
       _ => 0
   };

Bu örnekte ``result`` değişkeni match ifadesi bittiğinde değer alacaktır. Dolayısıyla bu kullanım *içerisinde henüz değer
atanmamış değişkenin içerisindeki değerin* kullanılması nedeniyle error oluşturacaktır. Tabii biz baştan ``result``
değişkenine değer atamış olsaydık bu durum soruna yol açmayacaktı:

.. code-block:: rust

   let mut result: i32 = 0;

   result = match val {
       1 => {
           println!("{}", result);         // geçerli
           10
       }
       //...
       _ => 0
   };

wildcard Kolu ve Uyuşan Kollar
==============================

Peki ``match`` ifadesinin son koluna wildcard kalıbı getirildiğinde bu wildcard kalıbındaki ifade ne olmalıdır? ``match``
ifadesinin wildcard kolu diğer kollarla aynı değeri üretmelidir. Eğer diğer kollar unit değeri üretiyorsa bu durumda
wildcard ifadesinde de bir şey yapmak istemiyorsanız oradaki ``()`` biçiminde ya da eşdeğer olarak ``{}`` biçiminde
oluşturabilirsiniz. Örneğin:

.. code-block:: rust

   match val {
       1 => println!("bir"),
       2 => println!("iki"),
       3 => println!("üç"),
       _ => (),
   }

Daha önceden de belirttiğimiz gibi ``match`` ifadesinde ``match`` kollarına yukarıdan aşağıya doğru sırasıyla 
bakılmaktadır. Rust'ta birden fazla ``match`` kolunda kalıp uyuşumu sağlanabilir. Bu durum error oluşturmamaktadır. 
Bu durumda yukarıdan aşağıya doğru ilk uyuşan kalıbın ``match`` ifadesi işletilmektedir. Tabii bazen derleyici sabit
kalıplarında ya da diğer bazı kalıplarda bir durum tespiti yapıp uyarı mesajı verebilmektedir. Örneğin:

.. code-block:: rust

   match val {
       1 => println!("bir"),
       1 => println!("yine bir"),
       _ => (),
   }

Burada val değeri ``1`` ise ilk koldaki ifade çalıştırılacaktır. Bu durumda açıkça diğer ikinci kol *erişilemeyen
(unreachable)* durumda olur. Rust derleyicisi bunu tespit edip bir uyarı mesajı oluşturacaktır.

Veya Kalıbı (Or Pattern)
========================

Bazen farklı değerler için aynı şeylerin yapılması istenebilir. Bunun için ``match`` ifadesinde *veya kalıbı (or 
pattern)* kullanılmaktadır. Veya kalıbı ``|`` atomuyla oluşturulmaktadır. Örneğin:

.. code-block:: rust

   match val {
      1 | 2 | 3 => println!("bir ya da iki ya da üç"),
      4 => println!("dört"),
      5 => println!("beş"),
      _ => println!("diğer bir sayı")
   }

Burada birinci match kolu aşağıdaki gibidir:

.. code-block:: rust

   1 | 2 | 3 => println!("bir ya da iki ya da üç"),

Bu kalıp ``1`` ya da ``2`` ya da ``3`` anlamına gelmektedir. Örneğin daha önce yazmış olduğumuz komut satırı
uygulamasında biz döngüden *quit* ya da *exit* komutu girildiğinde çıkmak isteyebiliriz:

.. code-block:: rust

   loop {
       //...
       match getstr().as_str() {
           "" => continue,
           "dir" => println!("dir command"),
           "copy" => println!("copy command"),
           "rename" => println!("rename command"),
           "quit" | "exit" => break,
           _ => println!("unknown command"),
       }
   }

Tabii veya kalıbındaki kalıplar sabit kalıplarıysa bunların aynı türden olması gerekir.

Range Kalıbı
============

``match`` ifadelerinde çok kullanılan diğer bir kalıp da range kalıbıdır. range kalıbı bir değerin bir aralık içerisinde
olup olmadığını belirlemekte kullanılmaktadır. Range kalıbını oluşturabilmek için yine daha önce görmüş olduğumuz ``..``
ve ``..=`` operatörleri kullanılmaktadır. Range kalıbında kullanılabilecek range ifadeleri şunlardır:

.. code-block:: text

   a..b
   a..=b
   ..b
   ..=b
   a..

Bu range ifadelerinin ne anlama geldiğini daha önce görmüştük. Bir kalıp olarak kullanıldığında bu range kalıpları şu
anlamlara gelmektedir (``a`` ve ``b``'nin aynı tamsayı türlerine ilişkin değerler olduğunu varsayıyoruz):

* ``a..b``: ``a``'dan ``b``'ye ``b`` dahil olmamak üzere (exclusive range) tamsayıların herhangi biri

* ``a..=b``: ``a``'dan ``b``'ye ``b`` dahil olmak üzere (inclusive range) tamsayıların herhangi biri

* ``..b``: ``b``'ye kadar olan fakat ``b`` dahil değil (range to) tamsayıların herhangi biri (negatif değerler de dahil)

* ``..=b``: ``b``'ye kadar olan fakat ``b`` de dahil (range to inclusive) tamsayıların herhangi biri (negatif değerler de
  dahil)

* ``a..``: a'dan başlayan tamsayıların herhangi biri (range from)

Örneğin:

.. code-block:: rust

   let val = getval();

   match val {
       0 => break,
       1 => println!("1"),
       2..5 => println!("2..5"),
       5 => println!("5"),
       6..=8 => println!("6..=8"),
       ..10 => println!("..10"),
       ..=20 => println!("..=20"),
       21.. => println!("21.."),
   }

Burada çeşitli kalıplar birlikte kullanılmıştır. Bu örnekte ilginç olan bir durum hiç wildcard kalıbının kullanılmamış
olmasıdır. Çünkü bu örnekte zaten ``match`` *exhaustive* durumdadır. Yani ``i32`` türünün her değeri için çalıştırılacak
bir ``match`` kolu bulunmaktadır.

.. code-block:: rust

   use std::io::{self, Write};

   fn main() {
      loop {
         let val: i32;

         print!("Bir değer giriniz:");
         io::stdout().flush().expect("cannot flush stdout!");

         val = getval();
         match val {
            0 => break,
            1 => println!("1"),
            2..5 => println!("2..5"),
            5 => println!("5"),
            6..=8 => println!("6..=8"),
            ..10 => println!("..10"),
            ..=20 => println!("..=20"),
            21.. => println!("21.."),
         }
      }
   }

   fn getval() -> i32 {
      let mut buf: String = String::new();

      std::io::stdin().read_line(&mut buf).expect("read line failed");
      buf.trim().parse().expect("parse into number failed")
   }

Range Kalıbında Türler
----------------------

Range kalıplarında ``match`` anahtar sözcüğünün yanındaki ifadenin türü ile range ifadesindeki türlerin aynı olması
gerekmektedir. Örneğin:

.. code-block:: rust

   let val: i8;

   val = getval();
   match val {
       1..10 => println!("1..10"),
       _ => (),
   }

Burada ``val`` ifadesi ``i8`` türündendir. ``1..10`` range kalıbı bağlama göre ``1i8..10i8`` gibi ele alınmaktadır.
Dolayısıyla buradaki range kalıbı geçerlidir. Örneğin:

.. code-block:: rust

   let val: i32;

   val = getval();
   match val {
       1i8..10i8 => println!("1i8..10i8"),            // error!
       _ => (),
   }

Bu örnekte ``val`` değişkeni ``i32`` türündendir. Ancak range kalıbı ``i8`` türüne ilişkindir. Bu nedenle error 
oluşacaktır.

Tabii range kalıbında çok seyrek olsa da ``f32`` ve ``f64`` türleri kullanılabilmektedir. Örneğin:

.. code-block:: rust

   let val: f64;

   val = getval();
   match val {
       1.0..3.0 => println!("1.0..3.0"),
       _ => ()
   }

Burada val ``f64`` türündendir. Range kalıbı da ``f64`` türüne ilişkindir. Yani örneğimizde val değeri ``1`` ile ``3``
arasında herhangi bir gerçek sayı değeri ise uyuşum sağlanacaktır.

Range Kalıbı ve Range Değişkeni
-------------------------------

Range kalıbında kalıp range operatörleriyle oluşturulmalıdır. Eğer biz bir match koluna Range türünden bir değişken
yerleştirirsek bu bir range kalıbı olmaz. Sonraki paragrafta göreceğimiz gibi bir değişken kalıbı olur. Örneğin:

.. code-block:: rust

   let a = 10;
   let r = 1..100;

   match a {
       r => println!("{}", r),     // dikkat! bu bir range kalıbı değil
       //...
   }

``match`` kolundaki ``r`` yeni bir değişkendir ve bu değişken izleyen paragrafta da görüleceği üzere ``a``'nın türünden, 
yani örneğimizde ``i32`` türünden olacaktır.

Değişken Kalıbı (Identifier Pattern)
====================================

Diğer ilginç bir kalıba da *değişken kalıbı (identifier pattern)* denilmektedir. Değişken kalıbında kalıp bir değişken
olur. Bu durumda uyuşum her zaman sağlanır. Değişken kalıbında kalıpla uyuşan değer kalıpta belirtilen değişkene
yerleştirilir. Örneğin:

.. code-block:: rust

   val = getval();
   match val {
       a => println!("a = {}", a),
   }

Burada bu değişken kalıbı tüm val değerleriyle uyuşum sağlayacaktır. Uyuşum sağlandığında ``val`` değişkeninin 
içerisindeki değer ``a`` değişkenine atanacaktır. ``a`` değişkeninin yukarıda ``let`` ile bağlanmış olması gerekmez.
Çünkü zaten ``match`` kolunda yeni bir değişken bağlaması yapılmaktadır. Bu kalıpta değişkene bir tür bilgisi 
iliştirilmez. Değişkenin türünün ``match`` anahtar sözcüğünün yanındaki ifadenin türüyle aynı olduğu kabul edilir.
Örneğin yukarıdaki ``match`` ifadesinde ``a`` değişkeninin türü ``val`` değişkeninin türü ile aynıdır. Yani ``a``
değişkeni de ``i32`` türündedir. Burada ayrıca ``match`` ifadesinin *exhaustive* olduğuna dikkat ediniz. Dolayısıyla
wildcard kalıbı gerekmemektedir. Kalıpta belirtilen değişken yalnızca o match kolunda kullanılabilmektedir.

