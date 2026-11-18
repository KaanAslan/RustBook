========
Sabitler
========

Statik tür sistemine sahip olan programlama dillerinde yalnızca değişkenlerin değil sabitlerin de -genel olarak tüm
ifadelerin (*expression*) de- bir türü vardır. Bir programlama dilini öğrenirken ilk öğrenilecek konulardan birisi de o
programlama dilindeki sabitlerin nasıl oluşturulduğu ve türlerinin nasıl belirlendiğidir. Sabitlerin türleri onların 
yazım biçimlerine göre ve niceliklerine göre (yani değerlerine göre) belirlenmektedir.Biz de bu bölümde Rust dilindeki 
sabitlerin nasıl oluşturulduğunu ve türlerinin nasıl belirlendiğini ele alacağız.

Program içerisinde doğrudan yazılmış olan değerlere *sabit (literal)* denilmektedir. Örneğin:

.. code-block:: rust

   a = b + 10;

Burada ``a`` ve ``b`` birer değişken ``10`` ise bir sabittir. Biz burada ``b``'nin içerisindeki değerle doğrudan ``10``'u 
topluyoruz.

Tamsayı Sabitleri 
=================

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
ayrıntılıdır. Bu konunun henüz görmediğimiz dizilerle, demetlerle, ``enum`` türleriyle, yapılarla bağlantısı vardır. Bu
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

====================================================
Kalıp Uyuşumu (Devam), if let / while let ve Diziler
====================================================

Kalıp Uyuşumu (Pattern Matching)
================================

Değişken Kalıbı
---------------

``match`` kollarına yukarıdan aşağıya doğru bakıldığını anımsayınız. Bu durumda biz değişken kalıbını örneğin sabit
kalıplarından sonraya yerleştirebiliriz:

.. code-block:: rust

    val = getval();
    match val {
        10 => println!("this is literal pattern 10"),
        x => println!("{}", x),
    }

Burada eğer ``val`` ``10`` ise birinci kol, ``val`` ``10`` değilse ikinci kol işletilecektir.

Değişken kalıbı size biraz anlamsız gelebilir. Ne de olsa match edilen değer zaten ``match`` anahtar sözcüğünün
yanındaki değerdir. Yani örneğin yukarıdaki örnek işlevsel olarak aşağıdakiyle eşdeğerdir:

.. code-block:: rust

    val = getval();
    match val {
        10 => println!("this is literal pattern 10"),
        _ => println!("{}", val),
    }

Peki o zaman gerçekten değişken kalıbı gereksiz midir? Yanıt hayır. Biz henüz pek çok kalıbı görmedik. Başka kalıplarda
uyuşum sağlayan değerlerin değişkenlere atanmasının önemi ortaya çıkmaktadır.

Değişken kalıbında ayrıca değişkenin önüne ``mut`` anahtar sözcüğü de getirilebilir. Bu durumda değişken *mutable*
olur. Örneğin:

.. code-block:: rust

    val = getval();
    match val {
        mut x => {
            println!("{}", x);
            x = 20;
            println!("{}", x);
        }
    }

Eğer burada ``x``'i ``mut`` yapmasaydık blok içerisinde ``x``'in değerini değiştiremezdik.

Match Koruması (Match Guard)
----------------------------

``if`` anahtar sözcüğü ile kalıplara bir koşul da eklenebilir. Bu durumda kalıp şu hale gelmektedir:

.. code-block:: text

    <kalıp> if <koşul>

Burada uyuşum ancak ilgili koşul sağlandığında sağlanmaktadır. Örneğin:

.. code-block:: rust

    val = getval();
    match val {
        x if x > 0 => println!("positive: {}", x),
        _ => println!("zero or negative"),
    }

Buradaki ``if`` anahtar sözcüğünün ``else`` kısmı yoktur. ``if`` anahtar sözcüğünü bir koşul izlemek zorundadır. Tabii
yukarıdaki örnekte böyle bir koşul eklendiğinde artık ``match`` ifadesi *exhaustive* olmaktan çıkmaktadır. Değişken
kalıbına ``if`` kullanma durumuna *match koruması (match guard)* denilmektedir. Yukarıda da belirttiğimiz gibi tipik
olarak match guard değişken kalıplarında kullanılıyor olsa da istenirse diğer kalıplarda da kullanılabilmektedir.

Bağlama (At Kalıpları)
----------------------

``match`` kollarındaki kalıplarda *bağlama (binding)* da oluşturulabilmektedir. Bağlama ``@`` atomu ile yapılır. Bu
atomun soluna bir değişken sağına ise bir kalıp getirilir. Bağlama yapılmış kalıplara *at kalıpları (at patterns)* da
denilmektedir. Eğer ``@`` atomunun sağındaki kalıp uyuşum sağlarsa uyuşan değer soldaki değişkene yerleştirilmektedir.
Örneğin:

.. code-block:: rust

    val = getval();
    match val {
        x @ 1..20 => println!("{}", x),
        _ => println!("zero or negative"),
    }

Burada at kalıbının kullanıldığı kola dikkat ediniz:

.. code-block:: rust

    x @ 1..20 => println!("{}", x),

Eğer ``match`` anahtar sözcüğünün yanındaki ifadenin değeri ``1..20`` aralığındaysa uyuşum sağlanacak ve bu değer
``x``'e atanacaktır. Bağlama işleminde ``@`` atomunun sağındaki kalıbın uyuşumu dikkate alınmaktadır. Uyuşum
sağlanırsa ilgili değer ``x`` değişkenine bağlanmaktadır. Tabii yukarıdaki örnekte siz yine *bu durumda x'e bağlamaya
ne gerek var, zaten ilgili değer val değişkeni içerisinde bulunuyor* diye düşünebilirsiniz. Ancak ileride göreceğimiz
başka kalıplarda bu *at kalıbının* daha anlamlı kullanımlarıyla karşılaşacağız. Burada önemli olan nokta bu at
kalıbında ``match`` yanındaki değerin değil uyuşan değerin değişkene yerleştirilmesidir. Bu konu hakkında ileride
açıklamalar yapılacaktır.

Henüz Ele Alınmayan Kalıplar
----------------------------

Biz bu bölümde ``match`` kalıplarının şimdilik bu kadarını gördük. Başka konular işlendikçe o konulara ilişkin
kalıplar ayrıca ele alınacaktır. Henüz ele almadığımız tipik kalıplar şunlardır:

- ``enum`` kalıpları
- Yapı kalıpları
- Demet kalıpları
- Dizi kalıpları
- ``ref`` ve ``ref mut`` kalıpları

İç İçe match İfadeleri
----------------------

``match`` deyimi aynı zamanda bir ifade belirttiğine göre bir ``match`` kolunda başka bir ``match`` ifadesi
bulunabilir. Örneğin:

.. code-block:: rust

    match val1 {
        1..20 => match val2 {
            //...
        },
        _ => (),
    }

let Deyiminde Kalıp Uyuşumu
---------------------------

Aslında Rust'ta ``let`` deyimi de ``match`` gibi kalıp uyuşumu ile bağlama yapmaktadır. Yani ``match``
ifadesindeki kalıpların hepsi olmasa da birçoğu aslında ``let`` deyiminde de kullanılabilmektedir. Örneğin:

.. code-block:: rust

    let a = 10;

Bu ``let`` deyimini biz ``match`` ifadesinde gördüğümüz *değişken kalıbı (identifier pattern)* biçiminde ele
alabiliriz. Örneğin:

.. code-block:: rust

    let (a, b) = (10, 20);

Burada aslında kaynak değer bir demettir. ``let`` deyiminde de *demet kalıbı (tuple pattern)* kullanılmıştır.
Örneğin:

.. code-block:: rust

    let a = [10, 20, 30];
    let [x, y, z] = a;

Burada biz aslında ``x``, ``y`` ve ``z`` değişkenlerini oluşturmuş olmaktayız. ``x``'in içerisine ``10``,
``y``'nin içerisine ``20`` ve ``z``'nin içerisine ``30`` yerleştirilecektir. Bu kalıba *dilim kalıbı (slice pattern)*
denilmektedir. Bu kalıpları ileride ilgili konuların içerisinde göreceğiz.

if let ve while let İfadeleri
-----------------------------

if let İfadesi
~~~~~~~~~~~~~~

Biz deyimler kısmında Rust'ta ``if let`` ve ``while let`` isimli iki deyimin daha bulunduğunu belirtmiştik. Ancak bu
deyimler hakkında bir şey söylememiştik. Şimdi bu deyimlerin çalışma biçimini açıklayacağız. Aslında bu deyimler
özellikle ``enum`` denilen türlerle (yani ``enum`` kalıplarıyla) kullanılmaktadır. ``enum`` türlerini ileride ayrı bir
başlık altında inceleyeceğiz. Biz kontrol deyimlerinde olduğu gibi ``if let`` ve ``while let`` deyimleri için
``if let`` ifadesi ve ``while let`` ifadesi terimlerini kullanacağız.

``if let`` ifadesi *kalıp uyuşuyorsa doğruysa kısmın, kalıp uyuşmuyorsa yanlışsa kısmın çalıştırıldığı* ilginç bir
``if`` deyimidir. ``if let`` ifadesinin genel biçimi şöyledir:

.. code-block:: text

    if let <kalıp> = <ifade> {
        //...
    }
    [ else {
        //...
    }
    ]

``if let`` anahtar sözcüklerinin yanında bir kalıp bulunur, bu kalıbı ``=`` atomu ile bir ifade izler. Eğer kalıp
uyuşumu sağlanırsa ``if let`` ifadesinin doğruysa kısmındaki blok deyimi, kalıp uyuşumu sağlanmazsa yanlışsa
kısmındaki blok deyimi çalıştırılır. ``if let`` ifadesi de ``if`` ifadesi gibi bir değer üretmektedir. Örneğin:

.. code-block:: rust

    val = getval();
    if let 1..10 = val {
        //...
    }
    else {
        //...
    }

Bu örnekte kalıp ``1..10`` biçiminde belirtilmiş olan ``range`` kalıbıdır. Eğer ``val`` değeri bu aralıktaysa bu
kalıp uyuşumu sağlanacak dolayısıyla ``if`` deyiminin doğruysa kısmı çalıştırılacaktır. Eğer ``val`` bu aralıkta
değilse ``if`` deyiminin yanlışsa kısmı çalıştırılacaktır. Tabii biz ``match`` deyiminde gördüğümüz ve henüz
görmediğimiz kalıpları ``if let`` ifadesinde de kullanabiliriz. Örneğin:

.. code-block:: rust

    val = getval();
    if let x @ 1..10 = val {
        println!("matched 1..10, x = {}", x);
    }
    else {
        println!("pattern cannot match!");
    }

Burada at kalıbı (``@`` kalıbı) uygulanmıştır. Bağlanan ``x`` değeri yalnızca ``if`` ifadesinin doğruysa kısmında
kullanılabilmektedir.

.. code-block:: rust

    use std::io::Write;

    fn main() {
        let val: i32;
        print!("Bir değer giriniz:");
        std::io::stdout().flush().expect("Can't flush stdout");

        val = getval();
        if let Some(x) = val {
            println!("matched 1..10, x = {}", x);
        }
        else {
            println!("pattern cannot match!");
        }
    }

    fn getval() -> i32 {
        let mut buf: String = String::new();

        std::io::stdin().read_line(&mut buf).expect("read line failed");
        buf.trim().parse().expect("parse")
    }

``if let`` ifadesinde de ``else if`` kısmı ya da ``else if let`` kısmı kullanılabilir. Benzer biçimde ``if``
ifadesinde de ``else if`` kısmının yanı sıra ``else if let`` kısmı da bulundurulabilmektedir. Örneğin:

.. code-block:: rust

    val = getval();
    if let 0..10 = val {
        println!("matched");
    }
    else if val % 2 == 0 {
        println!("val is even");
    }
    else {
        println!("other...");
    }

if let İfadesinin match Eşdeğeri
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Aslında ``if let`` ifadesi tamamen iki kollu ``match`` ifadesi ile karşılanabilir. Örneğin:

.. code-block:: rust

    if let pattern = expression {
        //...
    }
    else {
        //...
    }

ifadesinin ``match`` eşdeğeri şöyledir.

.. code-block:: rust

    match expression {
        pattern => {
            //...
        }
        _ => {
            //...
        }
    }

Tabii bu tür durumlarda ``if let`` ifadesini okunabilirlik bakımından tercih edebilirsiniz. ``match`` ifadesi çok kollu
durumlar için tercih edilmelidir.

while let İfadesi
~~~~~~~~~~~~~~~~~

``while let`` döngüsü de *kalıp uyuştuğu sürece yinelenen* bir döngüdür. ``while let`` ifadesinin genel biçimi
şöyledir:

.. code-block:: text

    while let <pattern> = <ifade> {
        //...
    }

Burada da ``match`` deyiminde görmüş olduğumuz ve henüz görmemiş olduğumuz kalıplar kullanılabilmektedir. Döngü kalıp
uyuştuğu sürece yinelenmektedir. Örneğin:

.. code-block:: rust

    i = 0;
    while let 0..10 = i {
        print!("{} ", i);
        i += 1;
    }

Burada bir ``range`` kalıbı kullanılmıştır. Bu kalıp uyuşumu ``i``'nin ``0`` ile ``9`` arasındaki değerleri için
sağlanacaktır. Ancak ``i = 10`` olduğu zaman kalıp uyuşumu artık sağlanmayacak ve döngüden çıkılacaktır. Örneğin:

.. code-block:: rust

    while let x @ 1..=10 = getval() {
        //...
    }

Burada klavyeden (``stdin`` dosyasından) girilen değer ``1`` ile ``10`` arasında olduğu sürece ``x``'e yerleştirilip
blok deyimi çalıştırılacaktır. Eğer girilen değer bu aralıkta değilse kalıp uyuşmayacağı için döngü sonlanacaktır.

``if let`` ve ``while let`` ifadelerinin daha uygun kullanımları ile kursumuzun ilerleyen aşamalarında karşılaşacağız.
Burada biz bu deyimlerin kalıp uyuşumu ile nasıl çalıştığını açıkladık.

Diziler
=======

Dizilere Giriş
--------------

Diziler elemanları aynı türden olan ve bellekte ardışıl bir biçimde bulunan veri yapılarıdır. Genellikle programlama
dilleri tarafından *built-in* biçimde desteklenmektedir. Biz de bu bölümde Rust'ta diziler üzerinde duracağız.

Yukarıda belirttiğimiz gibi diziler aynı türden elemanlara sahiptir ve dizi elemanları bellekte fiziksel bakımdan
ardışıl bir biçimde tutulmaktadır. Dizi elemanlarının ardışıl biçimde tutulması iki bakımdan önemlidir:

1) Bu sayede dizinin bir elemanına çok hızlı (buna *rastgele erişim (random access)* ya da *O(1) karmaşıklıkta erişim*
   de denilmektedir) bir biçimde erişilir. İşlemciler bir adresten belli bir uzaklıktaki nesnelere tek bir makine
   komutuyla erişebilmektedir.

2) Dizi elemanlarının ardışıl olması nedeniyle diziler fonksiyonlara başlangıç adresleriyle aktarılabilmektedir.

Dizi Türleri ve İlkdeğer Verme
------------------------------

Rust'ta dizi türleri ``[tür; uzunluk]`` biçiminde ifade edilmektedir. Örneğin:

.. code-block:: rust

    let a: [i32; 5];

Yazım biçimi olarak noktalı virgülden sonra boşluk bırakmak daha yaygındır. Ancak boşluk bırakmayan Rust programcıları
da vardır. Biz kursumuzda buradaki noktalı virgülden sonra boşluk bırakacağız. Dizi bildirirken uzunluğun sabit ifadesi
biçiminde belirtilmesi zorunludur. Örneğin:

.. code-block:: rust

    let n = 5;
    let a: [i32; n];        // error! uzunluk sabit ifadesi biçiminde belirtilmemiş

Yukarıdaki dizi bildirimi error oluşturacaktır. Çünkü dizi uzunluğu sabit ifadesi ile belirtilmemiştir.

Rust'ta bir dizi oluşturulurken tipik olarak dizi elemanlarına ilkdeğerleri verilmektedir. Bu işlem köşeli
parantezlerle yapılmaktadır. (Dizi elemanlarına C'de küme parantezleri içerisinde ilkdeğer verildiğini anımsayınız.)
Örneğin:

.. code-block:: rust

    let a: [i32; 5] = [10, 20, 30, 40, 50];

Tabii verilen her ilkdeğerin dizi türünden olması gerekir. Örneğin:

.. code-block:: rust

    let a: [i32; 5] = [10, 20, 30, 40, 50.5];       // error!

Rust'ta dizi elemanlarına ilkdeğer verilirken verilen ilkdeğerlerin tam olarak dizi uzunluğu kadar olması zorunludur.
Örneğin:

.. code-block:: rust

    let a: [i32; 5] = [10, 20, 30, 40];      // error!

Burada ``a`` dizisi ``5`` elemanlı olduğu halde biz bu diziye ``4`` tane ilkdeğer verdik. Bu durum error
oluşturacaktır. Halbuki örneğin C ve C++'ta dizinin az sayıda elemanına ilkdeğer verilebilmektedir. Bu durumda diğer
elemanlara ``0`` yerleştirilmektedir. Rust'ta böyle bir durum söz konusu değildir. Rust'ta da dizi elemanlarına verilen
ilkdeğerlerin sabit ifadesi olması zorunlu değildir. Örneğin:

.. code-block:: rust

    let (x, y, z) = (10, 20, 30);
    let a: [i32; 3] = [x, y, z];      // geçerli

Rust'ta dizi elemanlarına ilkdeğer verirken köşeli parantez içerisindeki değerlerin hepsinin aynı türden olması gerekir.

Tür Çıkarımı
------------

Dizilere ilkdeğer verilirken ``let`` deyiminde tür belirtilmezse derleyici tür çıkarımını aşağıda belirtildiği gibi
yapmaktadır:

- Dizi uzunluğu köşeli parantez içerisindeki eleman sayısı kadardır.
- Dizinin türü verilen ilkdeğerlerin türünden olur.
- Verilen ilkdeğerler sonek içermeyen tamsayı sabitlerinden oluşuyorsa dizi ``i32`` türünden kabul edilir.
- Verilen ilkdeğerler sonek içermeyen gerçek sayı sabitlerinden oluşuyorsa dizi ``f64`` türünden kabul edilir.
- Verilen ilkdeğerlerin sonek içermeyen sabitlerden oluşması durumunda ya bu sabitlerin hepsinin tamsayı sabiti olması
  ya da gerçek sayı sabiti olması gerekir.
- Verilen ilkdeğerlerden en az biri sonek içeren bir sabit ise diğerlerinin de ya bu soneki içermesi ya da bu sonekin
  ilişkin olduğu türün sınırları içerisinde olması gerekir.

Örneğin:

.. code-block:: rust

    let a = [10i16, 20i16, 30i16];

Bu ilkdeğer verme geçerlidir. Dizi ``[i16; 3]`` türündendir. Örneğin:

.. code-block:: rust

    let a = [10i16, 20, 30, 40, 50];

Bu ilkdeğer verme de geçerlidir. Sonek almayan tamsayı sabitleri ``i16`` türünün sınırları içerisinde kaldığı için
``i16`` türünden kabul edilir. Örneğin:

.. code-block:: rust

    let a = [10, 20.2, 30];          // error!

Bu ilkdeğer verme error ile sonuçlanır. Çünkü verilen ilkdeğerlerden bir kısmı tamsayı sabitlerinden bir kısmı gerçek
sayı sabitlerinden oluşmaktadır. Örneğin:

.. code-block:: rust

    let a = [10, 20.2f64, 30];       // error

Bu ilkdeğer verme de aynı gerekçelerle error oluşturacaktır. Örneğin:

.. code-block:: rust

    let a = [10.2, 20.3, 30.4];

Bu ilkdeğer verme geçerlidir. Dizi ``[f64; 3]`` türündendir. Örneğin:

.. code-block:: rust

    let a = [10.2, 20.3f32, 30.4];

Bu ilkdeğer verme de geçerlidir. Çünkü ``10.2`` ve ``30.4`` ``f32`` türüyle temsil edilebilmektedir.

Dizi bağlanırken tür belirtildiğinde tüm ilkdeğerlerin aynı türden olması gerekir. Örneğin:

.. code-block:: rust

    let a: [i8; 3] = [10i8, 20, 30];

Bu bağlama geçerlidir. Çünkü tüm sonek almayan ilkdeğerler ``i8`` türü ile temsil edilebilmektedir. Örneğin:

.. code-block:: rust

    let a: [f64; 3] = [3.14, 10, 20];       // error!

Burada ilkdeğer olarak verilen sabitlerin tamsayı sabiti bir kısmı da gerçek sayı sabitidir. Dolayısıyla bu ilkdeğer
verme geçersizdir. Ancak aşağıdaki ilkdeğer verme geçerlidir:

.. code-block:: rust

    let a: [f64; 3] = [3.14, 10f64, 20f64];

Dizi Yaratma Sentaksı
---------------------

Aslında Rust'ta köşeli parantezler içerisinde ifade listesi yazıldığında bu sentaks *dizi yarat* anlamına gelmektedir.
Bu biçimde yaratılan dizilerin türünün tespiti *tür belirtilmeyen let deyimindeki dizi türünün belirlenmesi*
kurallarıyla yapılmaktadır. Yani örneğin ``[10, 20, 30]`` biçiminde yaratılan dizinin türü daha önce görmüş olduğumuz
*let deyimiyle tür belirtilmeden yapılan bağlamadaki tür tespiti* kurallarına göre yapılmaktadır. Örneğin:

.. code-block:: rust

    let a = [10, 20, 30]

Biz daha önce bu dizinin türünün ``[i32; 3]`` biçiminde belirlendiğini görmüştük. İşte ``[10, 20, 30]`` ifadesi de
``[i32; 3]`` biçiminde bir dizinin yaratılmasına yol açmaktadır. Rust'ta tamsayı ve gerçek sayı sabitlerinin
türlerinin bağlama göre değerlendirildiğini de anımsayınız. Örneğin:

.. code-block:: rust

    let a: [i8; 3];

    a = [10, 20, 30];       // geçerli

Burada artık ``[10, 20, 30]`` dizisi hedef tür dikkate alınacağı için ``[i8; 3]`` türündenmiş gibi ele alınacaktır.

Mademki Rust'ta etkisiz kodlar error oluşmasına yol açmamaktadır o halde aşağıdaki gibi anlamsız bir kod parçası da
geçerlidir:

.. code-block:: rust

    [10, 20, 30];

Burada ``[i32; 3]`` türünden bir dizi oluşturulmuş ancak hiçbir değişkene atanmamıştır. Tabii Rust derleyicisi bu
durumda bir uyarı verecek ve optimizasyon sürecinde tamamen koddan atılacaktır.

Yukarıdaki örnekten de gördüğünüz gibi Rust'ta bir dizi türünden değişkene istenildiği zaman aynı dizi türünden bir
başka değişken atanabilir. Bu bakımdan Rust C'ye benzememektedir. Örneğin:

.. code-block:: rust

    let mut a: [i32; 5] = [10, 20, 30, 40, 50];
    //...
    a = [100, 200, 300, 400, 500];      // geçerli

Halbuki bunun C karşılığı C'de geçersizdir:

.. code-block:: c

    int a[5] = {10, 20, 30, 40, 50};
    //...
    a = {100, 200, 300, 400, 500};      // geçersiz!

C'de dizi isimleri ifade içerisinde kullanıldığında dizinin başlangıç adresine dönüştürülmektedir. Zaten küme
parantezleri C'de yalnızca ilkdeğer vermede ve C99'da bileşik sabit oluşturmakta kullanılmaktadır.

Rust'ta bu durumun nasıl geçerli olduğunu merak edebilirsiniz. C'de aynı türden iki yapı nesnesinin atanabildiğini, bu
durumda yapının karşılıklı elemanlarının atandığını anımsayınız. Örneğin:

.. code-block:: c

    struct SAMPLE {
        int a[5];
    };

    struct SAMPLE s = {{1, 2, 3, 4, 5}};
    struct SAMPLE k;

    k = s;            // geçerli!

Rust'ta bir dizi türünden değişkene daha sonra aynı dizi türünden değişken atandığında yine C'deki yapılarda olduğu
gibi dizinin karşılıklı elemanları birbirine atanmaktadır. Ancak Rust'ta bir diziye aynı türden bir dizi atanırken
aslında Rust'a özgü başka bir süreç daha devreye girmektedir. ``a`` ve ``b``'nin dizi türünden olduğunu varsayalım:

.. code-block:: rust

    a = b;

Rust'ta burada eğer bu dizilerin eleman türleri ``Copy`` ise (yani ``Copy`` trait'ini destekliyorsa, temel türler
``Copy`` trait'ini desteklemektedir) atama işleminde bir sahiplik devredilmesi ve gerçek anlamda bir ``drop`` işlemi
söz konusu olmaz. Ancak dizinin eleman türleri ``Copy`` trait'ini desteklemiyorsa (yani ``Copy`` türünden değilse) bu
durumda atama işleminde soldaki dizi ``drop`` edilir, sağdaki dizinin sahipliği de devredilir. Tabii biz henüz bu
konuları görmedik. Sahiplik ve ödünç alma konuları ayrı bir bölümde ele alınacaktır.

[değer; uzunluk] Sentaksı
-------------------------

Bir dizinin bütün elemanlarına ilkdeğer atamadan dizi kullanılamamaktadır. Ancak bazen diziler çok büyük olabilir ve
dizi elemanlarına ilkdeğer atamak çok zahmetli hale gelebilir. İşte Rust'ta elemanları belli bir değerden oluşan bir
dizi yaratmak için pratik bir sentaks bulundurulmuştur. Rust'ta ``[değer; uzunluk]`` biçiminde bir sentaks *bu
uzunlukta dizi yarat ve dizinin her elemanına buradaki değeri yerleştir* anlamına gelmektedir. Örneğin:

.. code-block:: rust

    let a: [i32; 100] = [0; 100];

Burada ``100`` elemanlı bir dizi oluşturulmuş ve dizinin her elemanına ``0`` yerleştirilmiştir. (Bu sentakta ``;``
atomunun solundaki ve sağındaki öğeleri yanlış yerleştirebilirsiniz. Doğru biçimin ne olduğunu hatırlamanın basit bir
yolu şudur: *Dizi türlerinde uzunluk her zaman ';' atomunun sağında belirtilmektedir. O halde bu sentaksta ';'
atomunun solunda doldurulacak değer olmalıdır.*) Tabii önceki paragrafta da belirttiğimiz gibi bir diziye bildirildikten
sonra da aynı türden bir diziyi atabiliriz:

.. code-block:: rust

    let a: [i32; 100];

    a = [0; 100];

Peki ``[değer; uzunluk]`` sentaksıyla yaratılan dizinin türü ne olacaktır? Burada dizinin uzunluğunu zaten biz vermek
zorundayız. Doldurulacak değerin türü de dizinin türünü belirtmektedir. Örneğin ``[0; 100]`` dizisi ``[i32; 100]``
türündendir. Örneğin ``[1u8; 10]`` dizisi ``[u8; 10]`` türündedir. Aşağıdaki atama işlemi türler farklı olduğu için
geçersizdir:

.. code-block:: rust

    let a: [i32; 100] = [3.14; 100];     // error!

Burada ``3.14`` sabiti ``f64`` türünden olduğu için burada yaratılan dizinin türü de ``[f64; 100]`` biçiminde
olacaktır.

``[değer; uzunluk]`` sentaksında yine uzunluğun sabit ifadesi biçiminde belirtilmesi zorunludur. Ancak değer sabit
ifadesi olmak zorunda değildir. Örneğin:

.. code-block:: rust

    let val: u8 = 1;
    let a: [u8; 1000]= [val; 1000];     // geçerli

Fakat *The Rust Reference* dokümanlarına göre ``[değer; uzunluk]`` sentaksındaki değerin bir ``Copy`` türünden olması
(yani ``Copy`` trait'ini destekliyor olması) ya da sabit olması zorunludur. Örneğin Rust'ın standart kütüphanesindeki
``String`` yapısı ``Copy`` türünden olmadığı için bu sentaksla kullanılamaz. Örneğin:

.. code-block:: rust

    let x = ["ankara".to_string(); 5];      // error!

Dizi Elemanlarına Erişim ve Sınır Kontrolü
------------------------------------------

Rust'ta dizi elemanlarına erişmek için yine ``[]`` operatörü kullanılmaktadır. Köşeli parantezlerin içerisinde
``usize`` türünden indeks belirten bir ifade bulunmak zorundadır. Örneğin:

.. code-block:: rust

    let a: [i32; 5] = [10, 20, 30, 40, 50];
    let i: i32 = 2;
    let val: i32;

    val = a[i];             // error!
    println!("{}", val);

Burada ``i`` değişkeni ``i32`` türünden olduğu için error oluşacaktır. Örneğin:

.. code-block:: rust

    val = a[2];     // geçerli

``2`` sabiti ``usize`` türünün sınırları içerisinde kaldığı için ``usize`` olarak da ele alınmaktadır. Bu nedenle
``a[2]`` gibi bir indekslemeyle geçerlidir. Örneğin:

.. code-block:: rust

    let a: [i32; 5] = [10, 20, 30, 40, 50];

    for i in 0..5 {
        println!("a[{}] = {}", i, a[i]);
    }

Burada her ne kadar ``0..5`` ifadesi ``Range<i32>`` türündense de döngü içerisinde ``i`` değişkeni dizi indeksi olarak
kullanıldığı için derleyici tarafından ``usize`` olarak ele alınmaktadır. Bu Rust'ın tür çıkarımı (type inference)
konusunun ayrıntılarıyla ilgilidir. Bu konunun ayrıntıları ileride ayrı bir başlık halinde ele alınacaktır.

Rust'ta dizi elemanlarına erişimde her zaman *sınır kontrolü (bounds check)* uygulanmaktadır. Eğer erişim geçerli bir
indekse yapılmıyorsa ``panic`` durumu oluşturulmaktadır. (Halbuki C'de ve C++'ta böyle bir kontrolün yapılmadığını
anımsayınız. Java, C# ve Python gibi dillerde de sınır kontrolü programın çalışma zamanı sırasında uygulanmaktadır.)

Rust'taki bu sınır kontrolü Rust programlarını eşdeğer C programlarına göre yavaşlatabilir mi? Evet bu kontrol Rust
programlarını eşdeğer C programlarına göre yavaşlatabilmektedir. Ancak modern derleyiciler çok gelişmiş kod
optimizasyonlarına sahiptir. Rust derleyicileri kod optimizasyonu sırasında eğer bir sınır ihlali tehlikesi yoksa sınır
kontrolünü aslında hiç yapmamaktadır. Örneğin:

.. code-block:: rust

    let a: [i32; 5] = [10, 20, 30, 40, 50];

    for i in 0..5 {
        println!("a[{}] = {}", i, a[i]);
    }

Buradaki kodda dizinin sınırları dışına erişim mümkün değildir. O halde Rust derleyicisi burada aslında herhangi bir
sınır kontrolü yapmayabilir. Başka bir deyişle Rust derleyicisi yalnızca aslında potansiyel bir tehlike varsa sınır
kontrolü uygulamaktadır. Bu da performans kaybını azaltmaktadır.

Rust'ta ayrıca dizilerin sınır kontrolü yapmadan erişimi sağlayan ``get_unchecked`` ve ``get_unchecked_mut`` isimli
metotları da bulunmaktadır. Ancak bu metotlar yalnızca ``unsafe`` bloklar içerisinde kullanılabilmektedir.
``get_unchecked`` metodu bize değiştirilemez (immutable) bir referans, ``get_unchecked_mut`` metodu ise
değiştirilebilir (mutable) bir referans vermektedir. Biz henüz Rust'ta referanslar ve göstericiler konusunu görmedik.
Ancak yine de aşağıdaki örneği vermek istiyoruz:

.. code-block:: rust

    let a: [i32; 5] = [10, 20, 30, 40, 50];
    let mut val: &i32;

    for i in 0..5 {
        unsafe {
            val = a.get_unchecked(i);
            println!("a[{}] = {}", i, *val);
        }
    }

Dizi elemanları üzerinde değişiklik yapmak istersek bu durumda ``get_unchecked_mut`` metodunu kullanmalıyız:

.. code-block:: rust

    let mut a: [i32; 5] = [10, 20, 30, 40, 50];
    let mut val: &i32;

    for i in 0..5 {
        unsafe {
            val = a.get_unchecked_mut(i);
            println!("a[{}] = {}", i, *val);
        }
    }

*The Rust Reference* dokümanlarında dizi erişimlerinde sınır ihlali eğer derleme zamanında tespit edilebiliyorsa
derleyicilerin error mesajını derleme zamanında verebileceği (optional özellik) belirtilmiştir. Örneğin:

.. code-block:: rust

    let mut a: [i32; 5] = [10, 20, 30, 40, 50];

    a[100] = 100;           // error!

Burada sınır ihlali açık bir biçimde yapılmıştır. Derleyici bu erişimi derleme zamanında tespit edebildiği için
derleme zamanında error oluşturabilmektedir.

Dizilerin Değiştirilebilirliği
------------------------------

Diziler de normal diğer türden değişkenler gibi default olarak değiştirilemez (immutable) durumdadır. Dizi elemanları
üzerinde değişiklik yapılmak isteniyorsa dizinin ``mut`` olması gerekir. Örneğin:

.. code-block:: rust

    let mut a: [i32; 5] = [10, 20, 30, 40, 50];

    for i in 0..5 {
        a[i] = a[i] * a[i];
    }
    println!("a = {:?}", a);

İlkdeğer Verilmemiş Diziler
---------------------------

Rust'ta bir dizinin tüm elemanlarına ilkdeğer vermeden dizinin herhangi bir elemanına atama amaçlı bile olsa
erişemeyiz. Örneğin:

.. code-block:: rust

    let mut a: [i32; 100];

    for i in 0..100 {
        a[i] = i as i32;            // error!
    }

Burada dizi elemanlarına henüz ilkdeğer verilmeden ``[]`` operatörü ile elemanlara değer atanmak istenmiştir. Bu durum
error oluşturacaktır. Bu biçimdeki işlemler ileride göreceğimiz ``std::mem`` modülündeki ``MaybeUninit`` türü
kullanılarak yapılabilmektedir.

Dizi Uzunluğu: len Metodu
-------------------------

Bir dizinin uzunluğu ``len`` isimli metotla elde edilebilmektedir. (Tabii dizi yaratılırken dizi uzunluğu sabit ifadesi
biçiminde verildiğine göre dizi uzunluğunu zaten programcı bilmektedir.) ``len`` metodu bize dizi uzunluğunu ``usize``
türünden vermektedir. ``usize`` türünün çalışılan sisteme göre o sistemin teorik bellek büyüklüğü ile ilgili olduğunu
anımsayınız. Örneğin:

.. code-block:: rust

    let a: [i32; 5] = [10, 20, 30, 40, 50];

    for i in 0..a.len() {
        print!("{} ",a[i]);
    }
    println!("");

``len`` metodu aslında dizilerin değil dilimlerin (slices) metodudur. Rust'ta dizi üzerinde bir metot çağrıldığında
eğer metot diziye ilişkin değilse dizi referansı otomatik olarak dilim referansına dönüştürülüp dilimin metodu
çağrılmaktadır. Bu konu ileride ele alınacaktır.

Örnekler
--------

Aşağıdaki örnekte bir dizinin en büyük elemanı bulunmuştur.

.. code-block:: rust

    fn main() {
        let a: [i32; 10] = [5, 67, 23, 45, 11, 9, 56, 34, 22, 27];
        let mut max: i32;

        max = a[0];
        for i in 1..a.len() {
            if a[i] > max {
                max = a[i];
            }
        }
        println!("{}", max);
    }

Aşağıdaki örnekte *kabarcık sıralaması (bubble sort)* algoritması ile bir dizi sıraya dizilmiştir.

.. code-block:: rust

    fn main() {
        let mut a: [i32; 10] = [5, 67, 23, 45, 11, 9, 56, 34, 22, 27];
        let mut temp: i32;

        for i in 0..a.len() - 1 {
            for k in 0..a.len() - 1 - i {
                if a[k] > a[k + 1] {
                    temp = a[k];
                    a[k] = a[k + 1];
                    a[k + 1] = temp;
                }
            }
        }
        for i in 0..a.len() {
            print!("{} ", a[i]);
        }
        println!("");
    }

Dizilerin Dolaşılması
---------------------

into_iter ile Dolaşım
~~~~~~~~~~~~~~~~~~~~~

Bir dizi ``for`` döngüsüyle dolaşılabilir. Rust'ta dolaşım işlemi için dizilerin iteratör veren metotları
kullanılmaktadır. Diziyi dolaşmak için bize iteratör veren metotlardan biri ``into_iter`` isimli metottur. Örneğin:

.. code-block:: rust

    let a: [i32; 10] = [5, 67, 23, 45, 11, 9, 56, 34, 22, 27];

    for x in a.into_iter() {
        //...
    }

Bu döngü her yinelendikçe dizinin bir elemanı ``x``'e yerleştirilmektedir. Dizi elemanları bitince döngü de
sonlanmaktadır. Aslında Rust'ta dizilerin kendisi doğrudan iterator değildir. Ancak diziler ``IntoIterator``
trait'ini desteklediği için bu doğrudan ``for`` döngülerinde de kullanılabilmektedir. Yukarıdaki dolaşım tamamen
eşdeğer biçimde aşağıdaki gibi de yapılabilir:

.. code-block:: rust

    for x in a {
        //...
    }

Dizileri bu biçimde dolaşırken dizi elemanları döngü değişkenine atandığı için eğer dizi türü ``Copy`` değilse dizinin
sahipliği de kaybedilmektedir. (Buna İngilizce *dizinin consume edilmesi* de denilmektedir.) Örneğin:

.. code-block:: rust

    let names: [String; 3] = ["ali".to_string(), "veli".to_string(), "selami".to_string()];

    for name in names {
        println!("{}", name);
    }
    println!("{:?}", names);        // error!

Bu örnekte dizi elemanları ``Copy`` türünden olmayan ``String`` değerlerinden oluşmaktadır. İşte biz bu diziyi bu
biçimde ``for`` döngüsüyle döndüğümüzde artık dizi elemanları taşınmış olur ve asıl dizi (örneğimizdeki ``names``)
sahipliğini kaybeder. Dolayısıyla bu ``for`` döngüsünden sonra biz bu diziyi artık kullanamayız. Tabii eğer dizi
``i32`` gibi bir ``Copy`` türünden olsaydı böyle bir error oluşmayacaktı. Örneğin:

.. code-block:: rust

    let a: [i32; 3] = [10, 20, 30];

    for x in a {
        println!("{}", x);
    }
    println!("{:?}", a);            // geçerli

``i32`` türü taşınmamakta kopyalanmaktadır. Halbuki ``String`` türü taşınmaktadır. Bu konu ileride ayrı bir bölümde
ele alınacaktır.

iter ile Dolaşım
~~~~~~~~~~~~~~~~

Dizileri ``for`` döngüsüyle dolaşmanın diğer bir yolu ``iter`` metodunu kullanmaktır. (``iter`` metodu aslında dizi
türünün değil dilim türünün bir metodudur.) ``iter`` metodu bize bir iteratör verir. Ancak biz bu iteratörle
dolaştıkça dizi elemanlarının kendisini değil referanslarını (yani adreslerini) elde ederiz. Rust'ta bir referansın
gösterdiği yerdeki değere erişmek için tek operand'lı önek (unary prefix) ``*`` operatörü kullanılmaktadır. Bu
biçimde diziyi dolaştığımızda dizi elemanları *ödünç alındığı (borrow)* için dolaşım sonrasında dizi tüketilmemiş
(consume edilmemiş) olur. Dolayısıyla dizi eleman türü ``Copy`` trait'ini desteklemiyor olsa bile biz diziyi
dolaşımdan sonra da kullanabiliriz. Örneğin:

.. code-block:: rust

    let names: [String; 3] = ["ali".to_string(), "veli".to_string(), "selami".to_string()];

    for name in names.iter() {
        println!("{}", *name);
    }
    println!("{:?}", names);            // geçerli

Buradaki ``for`` döngüsünde ``name`` değişkeni ``String`` türünden değil ``&String`` türündendir. Dolayısıyla döngü
her yinelendiğinde ``String`` değerlerinin kendisi değil adresleri ``name`` değişkenine atanmaktadır. Döngü bittikten
sonra biz ``names`` dizisini kullanabiliriz. Yukarıda da belirttiğimiz gibi eğer dizi ``Copy`` Trait'ini
destekliyorsa zaten dizinin tüketilmesi gibi bir durum söz konusu olmamaktadır. Tabii ``Copy`` trait'ini destekleyen
türlere ilişkin dizileri de biz yine ``iter`` metodu ile dolaşabiliriz. Örneğin:

.. code-block:: rust

    let a: [i32; 5] = [10, 20, 30, 40, 50];

    for x in a.iter() {
        println!("{}", *x);
    }
    println!("{:?}", a);            // geçerli

Burada ``x`` değişkeni ``i32`` türünden değil ``&i32`` türündedir. Yani içerisinde ``i32`` türünden değer bulunan dizi
elemanının adresini tutmaktadır. Biz de örneğimizde ilgili dizi elemanına ``*`` operatörü ile eriştik.

Adres Yoluyla Dolaşım
~~~~~~~~~~~~~~~~~~~~~

``for`` döngüsünde bir diziyi ``iter`` metodu ile dolaşmakla dizinin adresini alarak dolaşmak işlevsel olarak
eşdeğerdir. Aşağıdaki iki dolaşım biçimi eşdeğerdir:

.. code-block:: rust

    for x in a.iter() {
        //...
    }

    for x in &a {
        //...
    }

Her iki dolaşımda da dizinin türü ``T`` olmak üzere ``x`` değişkeni ``&T`` türündendir. Yani ``for`` döngüsünde eğer
bir diziyi adres yoluyla dolaşmak istiyorsak ``iter`` metodunu çağırmak yerine dizi nesnesinin adresini de alabiliriz.
Tabii dizi adres yoluyla dolaşıldığı için bu durumda dizinin sahipliği devredilmez. Dolayısıyla ``for`` döngüsü
bittiğinde de biz diziyi kullanabiliriz. Örneğin:

.. code-block:: rust

    let names: [String; 5] = [
        String::from("Ali"),
        String::from("Veli"),
        String::from("Selami"),
        String::from("Ayşe"),
        String::from("Fatma")
    ];

    for name in &names {
        println!("{}", *name);
    }
    println!("{:?}", names);            // geçerli

Burada belki şu anda anlamayacak olsanız da geleceğe yönelik bir ayrıntıyı açıklamak istiyoruz. Rust'ta bir dizinin
``&`` operatörüyle adresi alındığında elde edilen ürün ``&[T; N]`` türünden olur. ``&[T; N]`` türünden ``&[T]``
türüne (``&[T]`` türüne *T türünden dilim referansı* da denilmektedir) otomatik dönüştürme vardır. Rust'ta bir türün
hiç ``as`` kullanılmadan başka bir türe otomatik dönüştürülmesine İngilizce *type coercion* denilmektedir. İşte
Rust'ta aşağıdaki dönüştürme otomatik olarak yapılabilmektedir:

.. code-block:: rust

    let rnames: &[String; 5] = &names;          // geçerli burada "coercion" yok, türler aynı
    let srnames: &[String] = rnames;   // geçerli &[String; 5] türü otomatik olarak &[String] türüne dönüştürülebilir

İşte dilim referansları ``for`` döngüsünde kullanıldığında sanki dilim referansıyla ``iter`` metodu çağrılmış gibi
olmaktadır. Yani aslında:

.. code-block:: rust

    for name in &names {
        //...
    }

Bu işlemin eşdeğeri şöyledir:

.. code-block:: rust

    let temp: &[String] = &names;
    for name in temp.iter() {
        //...
    }

Buradaki ``iter`` metodu diziye ilişkin ``iter`` metodu değil, dilime ilişkin ``iter`` metodudur.

Burada yaptığımız açıklamaları anlamamış olabilirsiniz. Referanslar ve göstericiler konusu ileride ayrı bir başlık
halinde ele alınacaktır. Şu anda bilinmesi gereken özet durum bir diziyi tüketmeden (consume etmeden) adres yoluyla
dolaşımın ``iter`` metoduyla ya da dizinin adresinin alınmasıyla da yapılabileceğidir.

iter_mut ile Dolaşım
~~~~~~~~~~~~~~~~~~~~

Bir diziyi iteratör yoluyla dolaşmanın diğer bir yolu da ``iter_mut`` metodunu kullanmaktır. ``iter_mut`` metodu ile
dolaşımda yine dizi elemanlarının adresleri elde edilir. Ancak bu adresler ``mut`` adreslerdir. Dolayısıyla artık
döngü değişkeni kullanılarak dizi elemanları üzerinde değişiklikler yapılabilir. Tabii ``iter_mut`` metodunun
kullanılabilmesi için dizinin de ``mut`` olması gerekir. Örneğin:

.. code-block:: rust

    let a: [i32; 5] = [10, 20, 30, 40, 50];

    for x in a.iter_mut() {             // error!
        //...
    }

Burada dizi ``mut`` olmadığı için biz dizi ile ``iter_mut`` metodunu çağıramayız. Şimdi diziyi ``mut`` yapalım:

.. code-block:: rust

    let mut a: [i32; 5] = [10, 20, 30, 40, 50];

    for x in a.iter_mut() {             // geçerli
        //...
    }

Burada artık ``x`` ``&i32`` türünden değil ``&mut i32`` türünden olacaktır. Dolayısıyla artık biz döngü değişkenini
``*`` operatörüyle kullanarak atama yaparsak dizinin ilgili elemanına atama yapmış oluruz. Örneğin:

.. code-block:: rust

    let mut a: [i32; 5] = [10, 20, 30, 40, 50];

    for x in a.iter_mut() {
        *x = *x * *x;
    }

Burada ``x`` ``&mut i32`` türündendir. Biz aslında dizinin her elemanını onun karesiyle değiştirmiş olduk.

Burada henüz tam olarak anlamlandıracak olmasanız da yine bir ayrıntıdan bahsetmek istiyoruz. Aslında Rust'ta
``iter_mut`` metodu dizinin bir metodu değildir. Dilimin (dizi diliminin) bir metodudur. Biz bir dizi ile ``iter_mut``
metodunu çağırdığımızda aslında dizi otomatik olarak dilime dönüştürülüp dilime ilişkin ``iter_mut`` metodu
çağrılmaktadır.

Diziyi ``iter_mut`` yoluyla dolaşmanın da kısa yolu vardır. Eğer dizi ``mut`` ise biz dizinin adresini değiştirilebilir
bir biçimde ``&mut`` operatörüyle elde edebiliriz. Bu adres de Rust'ta otomatik olarak ``mut`` bir dilim referansına
dönüştürülebilmektedir. Yani ``a`` dizisi ``mut`` ve ``[T; N]`` türünden olmak üzere ``&mut a`` aslında
``&mut [T; N]`` türünden bir dizi referansının elde edilmesine yol açmaktadır. Bu dizi referansı da otomatik olarak
``&mut [T]`` türüne dönüştürülmektedir. İşte bu türün de ``iter_mut`` metodu devreye girmektedir. Dolayısıyla ``T``
dizinin türünü belirtmek üzere aşağıdaki üç döngü işlevsel olarak tamamen eşdeğerdir:

.. code-block:: rust

    for x in a.iter_mut() {
        //...
    }

    for x &mut a {
        //...
    }

    let temp: &[T] = &a;
    for x &mut temp {
        //...
    }

Yazım bakımından hangisinin tercih edileceği programcıya kalmıştır. Tabii daha az tuşa basmak isteniyorsa ikinci
biçim tercih edilebilir. Ancak bazen ileride göreceğimiz gibi zincirli çağrımlar yapılması gerekebilir. Bu durumda
birinci biçimi kullanmak daha uygun olabilir. Örneğin:

.. code-block:: rust

    let mut a: [i32;5] = [10, 20, 30, 40, 50];

    for x in &mut a {
        *x = *x * *x;
    }

    for x in a {
        print!("{} ",x);
    }

.. code-block:: rust

    fn main() {
        let mut a: [i32;5] = [10, 20, 30, 40, 50];

        for x in &mut a {
            *x = *x * *x;
        }

        for x in a {
            print!("{} ",x);
        }
        println!("");
    }

Dolaşım Yöntemlerinin Özeti
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Bir diziyi iteratör yoluyla dolaşmanın üç yöntemini aşağıda özetliyoruz. ``a`` dizisinin ``T`` türünden olduğunu
varsayıyoruz:

1) Tüketici dolaşım:

.. code-block:: rust

    for x in a.into_iter() {
        //...
    }

Eşdeğeri:

.. code-block:: rust

    for x in a {
        //...
    }

Burada ``x`` ``T`` türündendir. ``into_iter`` metodu dizi türünün bir metodudur.

2) Adres yoluyla dolaşım:

.. code-block:: rust

    for x in a.iter() {
        //...
    }

Eşdeğeri:

.. code-block:: rust

    for x in &a {
        //...
    }

Burada ``x`` ``&T`` türündendir. ``iter`` metodu dilim türünün bir metodudur.

3) mut adres yoluyla dolaşım:

.. code-block:: rust

    for x in a.iter_mut() {
        //...
    }

Eşdeğeri:

.. code-block:: rust

    for x in &mut a {
        //...
    }

Burada ``x`` ``&mut T`` türündendir. ``iter_mut`` metodu dilim türünün bir metodudur.

Dizilerin Dilimlenmesi (Array Slicing)
--------------------------------------

Bir dizinin belli bir kısmının elde edilmesi işlemine *dizinin dilimlenmesi (array slicing)* denilmektedir. Dizi
dilimleme yalnızca Rust'a özgü bir özellik değildir. Örneğin Python'da, Ruby'de, Go'da, Swift'te ve Kotlin'de de dizi
dilimlemesi vardır. Rust'ta dizileri dilimleyebilmek için ``[]`` operatörünün içerisine ``range`` ifadelerinin
yerleştirilmesi gerekir. ``Range`` ifadelerinin pratik oluşturulma biçimlerini aşağıdaki özet tabloyla yeniden
anımsatmak istiyoruz:

====================  ==========================
Yapı İsmi             Range Operatör İle Yaratım
====================  ==========================
``Range``             ``a..b``
``RangeInclusive``    ``a..=b``
``RangeFrom``         ``a..``
``RangeTo``           ``..b``
``RangeToInclusive``  ``..=b``
``RangeFull``         ``..``
====================  ==========================

Biz bu ``range`` ifadelerinin hepsini ``for`` döngülerinde kullanamıyorduk. Ancak bu ``range`` ifadelerinin hepsi
dilimlemede kullanılabilmektedir. Bir dizi ``range`` ifadeleriyle dilimlendiğinde elde edilen ürüne *dizi dilimi
(array slice)* ya da kısaca *dilim (slice)* denilmektedir. Dizi dilimleri *dinamik uzunluğa sahip türler* türündendir.
Bu nedenle Rust'ta dilimler türünden değişkenler bildirilememekte ancak referanslar bildirilebilmektedir. Rust'ta
dilimler metot çağırma dışında doğrudan ifadelerde kullanılamamaktadır. Dilimlerin ifade içerisinde kullanılabilmesi
için onların ``&`` operatörüyle adreslerinin alınması gerekir. Bir dilimin adresi alındığında elde edilen ürüne *dilim
referansı (slice reference)* denilmektedir. Rust'ta yukarıda da belirttiğimiz gibi program içerisinde dilimlerin
kendileri değil referansları kullanılmaktadır. Örneğin ``a`` bir dizi belirtsin, biz de bu ``a`` dizisini ``a[3..6]``
biçiminde dilimlemiş olalım. İşte biz bu ``a[3..6]`` ifadesini ancak dilim metotlarını çağırmakta kullanabiliriz. Bu
ifadeyi bir değişkene atayamayız. Ancak bu ifadenin ``&a[3..6]`` biçiminde adresini alırsak elde ettiğimiz dizi dilim
referansını aynı türden bir dizi dilim referansına atayarak çeşitli ifadelerde kullanabiliriz.

``a`` ``T`` türünden bir dizi belirtiyor olsun. Biz de ``a`` dizisini ``a[x..y]`` biçiminde dilimlemiş olalım.
Yukarıda bu dilim ifadesinin metot çağırma dışında tek başlarına kullanılamayacağını belirtmiştik. İşte Rust'ta bu
dilim ifadesi ``[T]`` türünden kabul edilmektedir. ``[T]`` türü de *dinamik uzunluğa sahip bir türdür*. Eğer biz bu
dilim ifadesinin adresini alırsak (yani ifadeyi ``&a[x..y]`` biçimine getirirsek) artık bu ifadenin türü ``&[T]``
olmaktadır. ``[T]`` türü *T türünden bir dilim* anlamına gelir. Ancak bu türde bir uzunluk bilgisi olmadığı için bu
ifade yalnızca metot çağrılarında doğrudan kullanılabilmektedir. Fakat ``&[T]`` türü *T türünden dilim referansı*
anlamına gelir. Dilim referansları Rust'ta ifadeler içerisinde kullanılabilmektedir.

Yukarıda da belirttiğimiz gibi biz bir dilimi ``[T]`` türünden bir değişkene atayamayız. Örneğin ``a`` dizisi ``i32``
türünden olsun:

.. code-block:: rust

    let s:[i32] = a[..];           // error

Burada ``a[..]`` ifadesi bir dilim belirtmektedir ve ``[i32]`` türündendir. Ancak biz bu dilimi ``[i32]`` türünden bir
değişkene atayamayız. Ancak dilimler doğrudan dilim metotlarının çağrılmasında kullanılabilmektedir. Örneğin Rust'ta
``a[..].len()`` ifadesi geçerlidir. Bu tür durumlarda Rust derleyicisi dilimin adresini otomatik olarak kendisi
almaktadır. Metot çağırma sırasında uygulanan bu dönüştürmeleri başka bir bölümde ele alacağız.

Terminoloji
~~~~~~~~~~~

Burada terminolojiye ilişkin bir açıklama yapmak istiyoruz. Dilim referansları *bellekte ardışıl biçimde bulunan
değerlerin başlangıç adreslerini ve uzunluklarını belirten* genel bir terimdir. Diziler bellekte ardışıl olarak
tutulduğu için dilimler de diziler gibi ardışıl değerlerin belli bir kısmını belirtmektedir. Bu nedenle *dilim
referansı (slice reference)* yerine *dizi dilimi referansı (array slice reference)* da denilebilmektedir. Ancak
Rust'ın resmi dokümanlarında *dizi dilimi* yerine yalnızca *dilim*, *dizi dilim referansı* yerine de yalnızca *dilim
referansı* terimi kullanılmaktadır. Biz de kursumuzda genellikle *dilim* ve *dilim referansı* terimlerini
kullanacağız.

Dilim Referansının Yapısı
~~~~~~~~~~~~~~~~~~~~~~~~~

Yukarıdaki anlatımlar ışığında ``&a[x..y]`` ifadesini biz ancak ``&[T]`` türünden bir değişkene atayabiliriz.
Örneğin:

.. code-block:: rust

    let a:[i32; 10] = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100];
    let sr: &[i32];

    sr = &a[3..6];      // geçerli!

Peki böylesi bir atamada ``sr`` referansının içerisine ne yerleştirilmektedir? İşte dilim referansları aslında iki
elemanlı bir yapı gibi bellekte yer kaplamaktadır. Yani buradaki ``sr`` değişkeni aslında iki parçadan oluşmaktadır.
Birinci parça dilimlenen yerin başlangıç adresini, ikinci parça da dilimlenen yerin eleman uzunluğunu tutmaktadır.
Rust'ta bu biçimde hem adres hem de uzunluk tutan referanslara (referanslar da bir çeşit göstericidir) *şişman
göstericiler (fat pointers)* da denilmektedir. Bir dilim referansını şekilsel olarak şöyle gösterebiliriz:

.. code-block:: text

    Dilim Referansı
    ┌─────────────┐
    │  gösterici  │
    ├─────────────┤
    │   uzunluk   │
    └─────────────┘

Peki bu biçimdeki bir dizi dilim referansı bellekte kaç byte yer kaplayacaktır? İşte anımsanacağı gibi adres bilgileri
çalışılan sistemin kaç bitlik olduğuna bağlı olarak değişebilmekteydi. Dizi dilim referansının uzunluk tutan kısmı
``usize`` türündendir. Referansın bu kısmı da sistemin kaç bitlik olduğuna göre değişebilmektedir. O halde ``32`` bit
sistemlerde bu referans toplam ``4 + 4 = 8`` byte, ``64`` bit sistemlerde ``8 + 8 = 16`` byte yer kaplayacaktır.

=======================================================================
Dilimler, Çok Boyutlu Diziler, Dilim Kalıpları ve Demetler (Tuples)
=======================================================================

Dizi Dilimleri ve Fonksiyonlar
==============================

Dilimleme Biçimleri
-------------------

Dizileri dilimlerken daha önce görmüş olduğumuz tüm ``range`` ifadeleri kullanılabilmektedir. Örneğin ``a`` bir dizi
olsun. Aşağıdaki dilimlemelerin hepsi geçerlidir:

==================  ====================================================================================
Dilimleme Biçimi    Anlamı
==================  ====================================================================================
``a[x..y]``         x'inci indeksten y'inci indekse kadar dizi elemanları, y'inci indeks dahil değil
``a[x..=y]``        x'inci indeksten y'inci indekse kadar dizi elemanları, y'inci indeks dahil
``a[..y]``          Baştan itibaren y'inci indekse kadar dizi elemanları, y'inci indeks dahil değil
``a[..=y]``         Baştan itibaren y'inci indekse kadar dizi elemanları, y'inci indeks dahil
``a[..]``           Dizinin tüm elemanları
==================  ====================================================================================

Daha önceden de belirttiğimiz gibi (bu konunun ayrıntıları ileride ayrı bir başlık halinde ele alınacaktır) bir dizinin
adresi otomatik olarak dizi dilim referansına dönüştürülebilmektedir. Bu durumda bir dizinin tamamından dilim elde
etmenin iki yolu vardır (dizinin ``T`` türünden olduğunu varsayıyoruz):

1) ``let sr: &[T] = &a[..];``
2) ``let sr: &[T] = &a;``

Dizilerin Fonksiyonlara Aktarılması
-----------------------------------

Rust'ta diziler fonksiyonlara iki yöntemle aktarılmaktadır:

1) Dizinin tamamının kopyalanarak fonksiyona aktarılması yöntemi: Rust'ta dizinin tüm elemanlarını fonksiyona
   kopyalayarak bu biçimde aktarabiliriz. Böyle bir aktarım yöntemi C'de yoktur. Bu yöntemde fonksiyonun parametre
   değişkeni dizi ile aynı türden bir dizi olur. Fonksiyon da aynı türden bir dizi ile çağrılır. Örneğin:

   .. code-block:: rust

       fn main() {
           let a:[i32; 10] = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100];

           foo(a);
       }

       fn foo(a: [i32; 10]) {
           //...
       }

   Zaten biz Rust'ta bir diziyi aynı türden bir diziye atayabiliyorduk. Fonksiyon çağrıldığında argümanlardan parametre
   değişkenlerine atama yapıldığını anımsayınız. Bu biçimde aktarımda eğer dizi türü ``Copy`` değilse (yani taşınabilen
   bir türse) bu durumda aktarımdan sonra dizinin sahipliği kaybedilir. Dizi çağıran fonksiyonda artık kullanılamaz.
   Örneğin:

   .. code-block:: rust

       fn main() {
           let names:[String; 5] = [
               "ali".to_string(),
               "veli".to_string(),
               "selami".to_string(),
               "ayşe".to_string(),
               "fatma".to_string()
           ];

           foo(names);

           for name in &names {                // error!
               println!("{}", *name);
           }
       }

       fn foo(names: [String; 5]) {
           for name in &names {
               println!("{}", *name);
           }
       }

   Bu örnekte standart kütüphanedeki ``String`` türü bir ``Copy`` türü olmadığı için sahiplik devredilmiştir. Tabii
   dizilerin bu biçimde fonksiyonlara aktarılması özel bir durum söz konusu değilse genellikle tercih edilmez.
   Dolayısıyla kötü bir tekniktir. (Bu durumu C'deki yapı nesnelerinin fonksiyona adres yoluyla değil de kopyalama
   yoluyla aktarılmasına benzetebilirsiniz.)

2) Dizinin dilimlenerek fonksiyona aktarılması yöntemi: Bu yöntemde fonksiyonun parametre değişkeni bir dilim
   referansı olur. Fonksiyon da dizi elemanlarına ilişkin dilim referansıyla çağrılır. Örneğin:

   .. code-block:: rust

       fn main() {
           let a:[i32; 10] = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100];

           foo(&a);        // foo(&a[..]);
       }

       fn foo(a: &[i32]) {
       //...
       }

   Bu yöntemde dizi ne kadar büyük olursa olsun aslında fonksiyona yalnızca dizinin başlangıç adresi ve uzunluğu
   aktarılmaktadır.

Dilim Referansı ile Dizi Elemanlarına Erişim
--------------------------------------------

Peki elimizde bir dilim referansı varsa biz bu referansla dizi elemanlarına nasıl erişebiliriz? İşte Rust'ta dilim
referansı ile yine ``[]`` operatörünü kullanarak dizinin elemanlarına (referansın gösterdiği yerdeki dizinin
elemanlarına) erişebiliriz. Başka bir deyişle dilim referansları sanki diziymiş gibi kullanılabilmektedir. Örneğin:

.. code-block:: rust

    fn main() {
        let a:[i32; 10] = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100];

        foo(&a);        // foo(&a[..]);
    }

    fn foo(sr: &[i32]) {
        for i in 0..sr.len() {
            print!("{} ", sr[i]);
        }
        println!("");
    }

Tabii yine erişimde sınır kontrolü (bounds check) yapılmaktadır. ``foo`` fonksiyonunun yazımına dikkat ediniz:

.. code-block:: rust

    fn foo(sr: &[i32]) {
        for i in 0..sr.len() {
            print!("{} ", sr[i]);
        }
        println!("");
    }

Burada dilimin ``len`` metodu kullanılmıştır. (Aslında dizilerin ``len`` metodu yoktur. Zaten dizilerle ``len``
metodunu çağırdığımızda dilimin ``len`` metodunu çağırmış oluruz.) Dilim referansının içerisinde zaten uzunluk
bilgisinin bulunduğunu anımsayınız. ``len`` metodu doğrudan referansın içerisindeki bu uzunluk bilgisini bize
vermektedir.

Daha önceden de belirttiğimiz gibi aslında biz bir diziyle ``iter`` metodunu kullandığımızda dizinin ``iter`` metodu
değil diliminin ``iter`` metodu çağrılmaktadır. Aynı durum ``iter_mut`` metodu için de geçerlidir. Yani aslında
dizilerin ``iter`` ve ``iter_mut`` metotları yoktur. Biz dizilerle bu metotları çağırdığımızda zaten dilimlerinin
``iter`` ve ``iter_mut`` metotları çağrılmaktadır. O halde elimizde bir dilim referansı varsa biz doğrudan bu referans
ile ``iter`` ve ``iter_mut`` metotlarını çağırabiliriz.

Dilim referanslarıyla ``iter`` metodu çağrıldığında yine referansın gösterdiği diziye ilişkin dizi elemanlarının
adreslerini veren bir iteratör elde edilmektedir. Örneğin:

.. code-block:: rust

    fn foo(sr: &[i32]) {
        for val in sr.iter() {
            print!("{} ", *val);
        }
        println!("");
    }

Burada ``val`` değişkeni ``i32`` türünden değildir, ``&i32`` türündendir. Yani dizi elemanlarının adresini tutan bir
gösterici gibidir. Biz de ``*val`` ifadesi ile dizi elemanına erişmiş olduk. Rust'ta zaten dilimler bir iteratör
belirtmektedir. Dolayısıyla yukarıdaki döngünün eşdeğeri hiç ``iter`` metodunu çağırmadan dilim referansının kendisi
kullanılarak da oluşturulabilmektedir. Örneğin:

.. code-block:: rust

    fn foo(sr: &[i32]) {
        for val in sr {
            print!("{} ", *val);
        }
        println!("");
    }

Her iki biçim de tamamen eşdeğer etkiye yol açmaktadır. ``iter`` metodunu çağırmak yerine dilim referansının
kendisinin kullanılabilmesinin nedeni dizi dilimlerinin ``IntoIterator`` trait'ini desteklemelerinden
kaynaklanmaktadır. Bu konu ileride başka bir bölümde ele alınacaktır.

Yukarıdaki gibi dilim referansıyla dizi elemanlarına erişirken ``val`` referansı değiştirilemez (immutable) bir
referanstır. (Yani C'deki gösterdiği yer ``const`` olan gösterici gibidir.) Eğer dolaşımın ``mut`` bir referansla
yapılması isteniyorsa ``iter_mut`` metodu kullanılmalıdır. Bu metodun verdiği iteratör diziler konusunda da gördüğümüz
gibi ``mut`` referanslar vermektedir. Tabii bu durumda dilim referansının da ``mut`` olması gerekir. ``mut`` bir dilim
referansına atama yapabilmek için dizi de ``mut`` olmalıdır. Örneğin:

.. code-block:: rust

    fn main() {
        let mut a:[i32; 10] = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100];

        foo(&mut a);        // foo(&mut a[..]);

        for val in a {
            print!("{} ", val);
        }
        println!("");
    }

    fn foo(sr: &mut [i32]) {
        for val in sr.iter_mut() {
            *val = *val * 2;
        }
        println!("");
    }

Burada fonksiyonun parametresi olan dilim referansının ``mut`` bir referans olduğuna ve asıl dizinin de ``mut`` bir
dizi olduğuna dikkat ediniz. Aslında bir dilim referansı ``mut`` ise zaten bu referansı ``for`` döngüsünde
kullandığımızda ``iter_mut`` metodunun çağrılmasıyla aynı etki oluşmaktadır. Yani yukarıdaki ``foo`` fonksiyonunu şöyle
de yazabilirdik:

.. code-block:: rust

    fn foo(sr: &mut [i32]) {
        for val in sr {
            *val = *val * 2;
        }
        println!("");
    }

Biz dizilerle ``into_iter`` metodunu çağırdığımızda dizi elemanlarının adreslerini değil kopyalarını elde ediyorduk.
İşte dilimlerin de aslında ``into_iter`` metotları vardır. Çünkü dilimler de ``IntoIterator`` isimli trait'i
desteklemektedir. ``into_iter`` metodu da bu trait'ten gelmektedir. Ancak biz dilim referanslarıyla ``into_iter``
metodunu çağırdığımızda dizi elemanlarının kendisini elde etmeyiz. Yine referans ``mut`` değilse ``mut`` olmayan bir
referans, referans ``mut`` ise ``mut`` bir referans elde ederiz. Özetle aşağıdaki üç dolaşım biçimi de işlevsel olarak
eşdeğerdir:

.. code-block:: rust

    fn foo(sr: &[i32]) {
        for val in sr.iter() {
            //...
        }
    }

    fn foo(sr: &[i32]) {
        for val in sr.into_iter() {
            //...
        }
    }

    fn foo(sr: &[i32]) {
        for val in sr {
            //...
        }
    }

Burada üç dolaşım biçiminde de ``val`` değişkeni ``&i32`` türündendir. Şimdi de referansın ``mut`` olduğunu
varsayalım. Yine aşağıdaki üç dolaşım biçimi işlevsel olarak aynıdır:

.. code-block:: rust

    fn foo(sr: &mut [i32]) {
        for val in sr.iter_mut() {
            //...
        }
    }

    fn foo(sr: &mut [i32]) {
        for val in sr.into_iter() {
            //...
        }
    }

    fn foo(sr: &mut [i32]) {
        for val in sr {
            //...
        }
    }

Burada üç dolaşım biçiminde de ``val`` değişkeni ``&mut i32`` türündendir.

Aşağıda bir dizinin en büyük elemanına geri dönen bir fonksiyon örneği verilmiştir. Örneğimizde biz bu fonksiyonu
dizinin tamamına ilişkin bir dilim referansıyla çağırdık:

.. code-block:: rust

    maxval = getmax(&a);

Tabii aslında dizinin belli bir kısmının en büyük elemanını da elde edebilirdik. Örneğin:

.. code-block:: rust

    maxval = getmax(&a[..5]);

Burada dizinin ilk beş elemanının en büyüğü elde edilmiştir.

.. code-block:: rust

    fn main() {
        let a:[i32; 10] = [45, 23, 12, 45, 34, 26, 87, 42, 8, 53];
        let maxval: i32;

        maxval = getmax(&a);
        println!("Max: {}", maxval);
    }

    fn getmax(sr: &[i32]) -> i32 {
        let mut maxval: i32;

        maxval = sr[0];
        for i in 0..sr.len() {
            if sr[i] > maxval {
                maxval = sr[i];
            }
        }
        maxval
    }

mut Referanstan mut Olmayan Referansa Dönüştürme
------------------------------------------------

Nasıl C'de ``const`` olmayan bir nesnenin adresi ``const`` bir göstericiye atanabiliyorsa aynı gerekçelerle Rust'ta
``mut`` bir dizi referansı ``mut`` olmayan bir dilim referansına atanabilmektedir. Başka bir deyişle ``mut`` bir
referanstan ``mut`` olmayan referansa Rust'ta otomatik dönüştürme (type coercion) vardır. Örneğin:

.. code-block:: rust

    let rs1: &mut [i32];
    let rs2: &[i32];
    //...

    rs2 = rs1;      // geçerli

Bu durumda örneğin bir fonksiyonun parametre değişkeni ``mut`` olmayan bir dilim referansı ise biz bu fonksiyonu
``mut`` bir dizi diliminin adresiyle çağırırsak bir sorun oluşmaz.

Dizi ve Dilim Metotları
-----------------------

Bir dizi ile bir metot çağrılırken dizi adresi dilim referansına otomatik dönüştürülebildiği için aslında bir dizi ile
yalnızca dizinin metotları değil dilimin metotları da çağrılabilmektedir. İşte daha önceden de belirttiğimiz gibi
aslında dizilerle kullandığımız ``len``, ``iter`` ve ``iter_mut`` metotları dizilere ilişkin değil dilimlere ilişkin
metotlardır. Bir metodun dizinin metodu mu yoksa diliminin metodu mu olduğunu anlamanın pratik bir yolu yoktur. Bunun
için *The Rust Standard Library* dokümanlarına başvurmanız gerekir. Bu dokümanlara ilişkin bağlantıyı yeniden vermek
istiyoruz:

https://doc.rust-lang.org/std/

Bu dokümanlarda *Primitive Types* başlığı altındaki *array* ve *slice* kısımlarını gözden geçirebilirsiniz.

Rust'ta dizilerin ve dilimlerinin aslında pek çok metodu vardır. Bu metotların bazılarını kullanabilmek için bazı
konuları bilmemiz gerekir. Ancak bazı metotları kursumuzun bu noktasında gözden geçirebiliriz. Yalnızca dizilerde olan
ancak dilimlerde olmayan metotların önemli olanları şunlardır:

- ``map``
- ``try_map``
- ``into_iter`` (dilimlerde de var ama işlevi aynı değil)
- ``Default`` (izleyen paragraflarda açıklayacağız)

Dizilerde olmayan fakat dilimlerde olan metotların önemli olanları da şunlardır:

- ``contains(&item)``
- ``starts_with(slice)``
- ``ends_with(slice)``
- ``binary_search(&item)``
- ``binary_search_by(|probe| ...)``
- ``windows(n)``
- ``chunks(n)``
- ``rchunks(n)``
- ``chunks_exact(n)``
- ``split(|x| condition)``
- ``splitn(n, |x| condition)``
- ``rsplit(...)`` / ``.rsplitn(...)``
- ``as_ptr()``
- ``as_mut_ptr()``
- ``as_ref()`` / ``as_mut()``
- ``get(index)`` / ``get_mut(index)``
- ``get_unchecked(index)`` (unsafe)
- ``first()`` / ``.last()``
- ``split_at(mid)``
- ``split_at_mut(mid)``
- ``as_chunks()`` (1.63+)
- ``sort()``
- ``sort_by(...)``
- ``sort_by_key(...)``
- ``reverse()``
- ``rotate_left(n)`` / ``rotate_right(n)``
- ``fill(value)`` (1.50+)
- ``copy_from_slice(slice)``
- ``iter()``
- ``iter_mut()``
- ``windows(n).map(...)``
- ``chunks(n).map(...)``
- ``iter().enumerate()``

Şimdi bu metotların bazılarını gözden geçirelim. Bir dizi ile hem dizinin hem de dilimin metotlarını
kullanabildiğimize bir kez daha dikkatinizi çekmek istiyoruz.

contains
~~~~~~~~

``contains`` metodu belli bir elemanın dizinin içerisinde olup olmadığına ilişkin ``bool`` bir değer geri
döndürmektedir. Ancak bu metot bizden parametre olarak bir referans istemektedir. Biz de ``&`` operatörünü kullanarak
argümanı oluşturmalıyız. Örneğin:

.. code-block:: rust

    fn main() {
        let a:[i32; 10] = [45, 23, 12, 45, 34, 26, 87, 42, 8, 53];
        let result: bool;

        result = a.contains(&42);
        println!("{}", result);     // true
    }

Burada sabit bir değerin adresinin alınması size biraz tuhaf gelebilir. Çünkü C'de böyle bir şey mümkün değildir.
Ancak Rust'ta bir sabitin adresi alındığında derleyici önce sabiti statik ömürlü bir değişkene yerleştirir onun adresini
alır.

starts_with ve ends_with
~~~~~~~~~~~~~~~~~~~~~~~~

``starts_with`` ve ``ends_with`` metotları dizinin ilk kısmının ve son kısmının argüman olarak verilenle aynı olup
olmadığına bakar. Bu metot bizden bir dilim referansı istemektedir. Örneğin:

.. code-block:: rust

    fn main() {
        let a:[i32; 10] = [45, 23, 12, 45, 34, 26, 87, 42, 8, 53];
        let result: bool;

        result = a.ends_with(&[42, 8, 53]);
        println!("{}", result);     // true
    }

windows
~~~~~~~

``windows`` metodu bizden argüman olarak bir ``usize`` türünden bir tamsayı değeri alır. Bu değerin ``n`` olduğunu
varsayalım. Söz konusu diziyi n'erli biçimde dilimleyerek bize dilim referanslarını veren bir iteratör verir. Örneğin:

.. code-block:: rust

    fn main() {
        let a:[i32; 10] = [45, 23, 12, 45, 34, 26, 87, 42, 8, 53];

        for sr in a.windows(3) {
            println!("{:?}", sr);
        }
    }

Buradaki ``sr`` ``&[i32]`` türündedir. Bu programdan aşağıdaki çıktı elde edilecektir:

.. code-block:: text

    [45, 23, 12]
    [23, 12, 45]
    [12, 45, 34]
    [45, 34, 26]
    [34, 26, 87]
    [26, 87, 42]
    [87, 42, 8]
    [42, 8, 53]

chunks
~~~~~~

``chunks`` metodu da ``windows`` metoduna benzemektedir. Ancak örtüşmeyen dilimlerin referanslarını verir. Örneğin:

.. code-block:: rust

    fn main() {
        let a:[i32; 10] = [45, 23, 12, 45, 34, 26, 87, 42, 8, 53];

        for sr in a.chunks(3) {
            println!("{:?}", sr);
        }
    }

Buradan şu çıktı elde edilecektir:

.. code-block:: text

    [45, 23, 12]
    [45, 34, 26]
    [87, 42, 8]
    [53]

sort
~~~~

``sort`` metodu eğer dizinin eleman türü ``Ord`` trait'ini destekliyorsa diziyi sıraya dizmektedir. Tabii dizinin
``mut`` olması gerekir. Örneğin:

.. code-block:: rust

    fn main() {
        let mut a:[i32; 10] = [45, 23, 12, 45, 34, 26, 87, 42, 8, 53];

        a.sort();

        for val in a {
            print!("{} ", val);
        }
        println!("");
    }

reverse
~~~~~~~

``reverse`` metodu diziyi ters yüz etmektedir. Örneğin:

.. code-block:: rust

    fn main() {
        let mut a:[i32; 10] = [45, 23, 12, 45, 34, 26, 87, 42, 8, 53];

        a.reverse();

        for val in a {
            print!("{} ", val);
        }
        println!("");
    }

fill
~~~~

``fill`` metodu diziyi belli bir değerle doldurmaktadır. Örneğin:

.. code-block:: rust

    fn main() {
        let mut a:[i32; 10] = [45, 23, 12, 45, 34, 26, 87, 42, 8, 53];

        a.fill(0);

        for val in a {
            print!("{} ", val);
        }
        println!("");
    }

Burada artık dizinin bütün elemanları ``0`` olacaktır.

copy_from_slice
~~~~~~~~~~~~~~~

``copy_from_slice`` metodu bir diziyi başka bir dizinin elemanlarıyla kopyalamak için kullanılmaktadır. Örneğin:

.. code-block:: rust

    fn main() {
        let mut a:[i32; 3] = [45, 23, 12];

        a.copy_from_slice(&[10, 20, 30]);
        println!("{:?}", a);        // [10, 20, 30]
    }

Bu metotta kaynak ve hedef dilimler aynı uzunlukta olmalıdır. Tabii bazen dizinin belli bir kısmına kopyalama yapmak
isteyebiliriz. Bunun için hedef diziyi de dilimlemek gerekir. Örneğin:

.. code-block:: rust

    fn main() {
        let mut a:[i32; 10] = [45, 23, 12, 5, 8, 9, 6, 2, 10, 34];

        a[3..6].copy_from_slice(&[10, 20, 30]);
        println!("{:?}", a);        // [45, 23, 12, 10, 20, 30, 6, 2, 10, 34]
    }

as_ptr, as_mut_ptr ve as_ref
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Dilim referansının içerisindeki uzunluk bilgisinin ``len`` metoduyla elde edildiğini görmüştük. Peki dilim
referansının içerisindeki adres bilgisi nasıl elde edilebilir? İşte bunun için ``as_ptr`` isimli bir metot
kullanılmaktadır. Tabii bu metot bize bir referansa değil C'deki gibi bir gösterici vermektedir. Rust'ta C'deki gibi
göstericilere *ham göstericiler (raw pointers)* denilmektedir. Ham göstericiler de Rust'ta ancak ``unsafe`` bloklarda
kullanılabilmektedir. Bu konunun ayrıntılarını zaten ileride ayrı bir başlık halinde ele alacağız. Örneğin:

.. code-block:: rust

    let a:[i32; 5] = [10, 20, 30, 40, 50];
    let sr: &[i32] = &a[2..4];

    let pt: *const i32 = sr.as_ptr();
    unsafe {
        println!("{}", *pt);        // 30
    }

Eğer dizi ``mut`` ise ``as_mut_ptr`` metodu ile *mut bir ham gösterici* elde edilebilmektedir. Örneğin:

.. code-block:: rust

    let mut a:[i32; 5] = [10, 20, 30, 40, 50];
    let sr: &mut [i32] = &mut a[2..4];

    let pt: *mut i32 = sr.as_mut_ptr();
    unsafe {
        *pt = 100;
    }
    println!("{:?}", a);        // [10, 20, 100, 40, 50]

Ayrıca dilimlerin ``as_ref`` isimli bir metodu da vardır. Bu metot önemli bir şey yapmamaktadır. Doğrudan dilim
referansının kendisini vermektedir.

Dilimlerle Doğrudan Metot Çağırma
---------------------------------

Aslında yukarıdaki örnekten de görüldüğü gibi bir diziyi dilimler dilimlemez o dilim ile doğrudan dilimin metotlarını
çağırabiliriz. Yani onun adresini alarak metot çağırmamıza gerek yoktur. Örneğin:

.. code-block:: rust

    let a:[i32; 10] = [45, 23, 12, 5, 8, 9, 6, 2, 10, 34];
    let result: bool;

    result = a[2..6].contains(&8);
    println!("{}", result);

Buradaki ``contains`` metodunun nasıl çağrıldığına dikkat ediniz:

.. code-block:: rust

    result = a[2..6].contains(&8);

Biz ``a[2..6]`` gibi bir ifadenin bir dilim belirttiğini ve dilimlerin de doğrudan bazı durumlar dışında
kullanılamadığını belirtmiştik. Tabii eğer dizi ``mut`` ise dilim de ``mut`` kabul edilmektedir. Örneğin:

.. code-block:: rust

    let mut a:[i32; 10] = [45, 23, 12, 5, 8, 9, 6, 2, 10, 34];

    a[..5].sort();
    println!("{:?}", a);        // [5, 8, 12, 23, 45, 9, 6, 2, 10, 34]

``sort`` metodunun ``mut`` bir dilim referansı istediğini anımsayınız. ``a[..5]`` ifadesi ``a`` ``mut`` olduğu için
otomatik olarak ``mut`` bir referansa dönüştürülmektedir. Dolayısıyla yukarıdaki örnekte dizinin ilk ``5`` elemanı
sıraya dizilmiştir.

Sıfır Elemanlı Diziler
----------------------

Rust'ta herhangi türden sıfır elemanlı bir dizi oluşturulabilir. Örneğin:

.. code-block:: rust

    let a:[i32; 0];

Tabii ``0`` elemanlı bir diziye içi boş köşeli parantezlerle değer verilebilir. Örneğin:

.. code-block:: rust

    let a:[i32; 0] = [];

Bu dizinin uzunluğunu ``len`` metoduyla almak istediğimizde ``0`` değerini elde ederiz.

Çok Boyutlu Diziler
-------------------

Rust'ta çok boyutlu diziler *dizi dizileri gibi* oluşturulmaktadır. Örneğin biz ``i32`` türünden ``3`` satırlı ``2``
sütunlu bir matris oluşturmak isteyelim. Bunu Rust'ta her biri ``2`` elemandan oluşan ``3`` elemanlı bir dizi gibi
oluştururuz:

.. code-block:: rust

    let a: [[i32; 2]; 3];

Tabii bu biçimdeki dizilere iç içe köşeli parantezlerle ilkdeğer verilebilir. Örneğin:

.. code-block:: rust

    let a: [[i32; 2]; 3] = [[10, 20], [30, 40], [50, 60]];

Bu dizide elemana erişim de iki köşeli parantezle yapılmaktadır. Örneğin:

.. code-block:: rust

    let a: [[i32; 2]; 3] = [[10, 20], [30, 40], [50, 60]];

    println!("{}", a[2][1]);        // 60

Tabii diğer dillerde olduğu gibi iki boyutlu bir diziyi iç içe iki döngüyle dolaşabiliriz:

.. code-block:: rust

    let a: [[i32; 2]; 3] = [[10, 20], [30, 40], [50, 60]];

    for i in 0..a.len() {
       for j in 0..a[i].len() {
           print!("{} ", a[i][j]);
       }
       println!("");
    }

Bu koddan aşağıdaki gibi bir çıktı elde edilecektir:

.. code-block:: text

    10 20
    30 40
    50 60

Tabii biz aslında bu iki boyutlu diziyi iteratör yoluyla da dolaşabiliriz. Örneğin:

.. code-block:: rust

    let a: [[i32; 2]; 3] = [[10, 20], [30, 40], [50, 60]];

    for x in a.iter() {
        for y in x.iter() {
            print!("{} ", *y);
        }
        println!("");
    }

``iter`` metodu ile verilen iteratörün dolaşım sırasında dizi elemanlarının adreslerini verdiğini anımsayınız. O halde
yukarıdaki örnekte ``x`` değişkeni ``&[i32; 2]`` türünden olur. ``x.iter()`` çağrısı da aslında bu iç diziler
üzerinde iteratör elde etmektedir. (Burada çağrının ``x.iter`` yerine ``(*x).iter()`` biçiminde yapılması gerektiğini
düşünebilirsiniz. Ancak Rust'ta bu işlem de otomatik yapılmaktadır.) Bu durumda ``y`` değişkeninin türü de ``&i32``
olacaktır. Tabii yukarıdaki döngülerdeki ifadeler daha önce belirttiğimiz gibi şöyle de kısaltılabilirdi:

.. code-block:: rust

    let a: [[i32; 2]; 3] = [[10, 20], [30, 40], [50, 60]];

    for x in &a {
        for y in x {
            print!("{} ", *y);
        }
        println!("");
    }

Programlamada üçten fazla boyuta sahip dizilere çok seyrek biçimde gereksinim duyulmaktadır. Ancak üç boyutlu dizilerle
çokça karşılaşılabilmektedir. Rust'ta ikiden fazla boyutlu diziler benzer biçimde oluşturulmaktadır. Örneğin
``2x2x2``'lik bir diziyi şöyle oluşturabiliriz:

.. code-block:: rust

    let a: [[[i32; 2]; 2]; 2] = [[[10, 20], [30, 40]], [[50, 60], [70, 80]]];

Anımsanacağı gibi elemanlarının hepsi belli bir değerden oluşan diziler Rust'ta ``[değer; uzunluk]`` sentaksıyla
yaratılabiliyordu. İşte aynı sentaksı biz çok boyutlu dizilerde de kullanabiliriz. Örneğin:

.. code-block:: rust

    let a: [[i32; 2]; 3];

    a = [[0; 2]; 3];

Buradaki ``[[0; 2]; 3]`` ifadesine dikkat ediniz. Burada aslında ``[0; 2]`` dizisinden ``3`` tane
oluşturulmaktadır. Öte yandan ``[0; 2]`` dizisi de elemanları ``0`` olan iki elemanlı dizi oluşturmaktadır. O halde bu
ifade her elemanında ``0`` olan ``3x2``'lik bir dizi belirtmektedir.

Dizi Açımı (Destructuring) ve Dilim Kalıpları
=============================================

Dizi Açımı
----------

Bir dizinin elemanları pratik bir biçimde değişkenlere atanabilir. Buna Rust'ta dizinin *açılması (destructuring)*
denilmektedir. Dizinin açılması *The Rust Reference* dokümanlarında kalıp uyuşumu biçiminde ele alınmıştır. Bu kalıba
*dilim kalıbı (slice pattern)* ismi verilmiştir. Biz kursumuzda bu işleme amaç bağlamında bazen *açım (destructuring)*
bazen de *dilim kalıbı (slice pattern)* bazen de *dizi kalıbı (array pattern)* diyeceğiz. Açım (destructuring)
özelliği modern pek çok dilde bulunmaktadır. Örneğin bu özelliğe Python'da *unpacking*, C#'ta ve Swift'te
*deconstructing* denilmektedir.

Yukarıda da belirttiğimiz gibi açım sentaksı bir kalıp biçiminde oluşturulmaktadır. Bu kalıba *dilim kalıbı (slice
pattern)* denilmektedir. Dilim kalıbında köşeli parantezler içerisinde elemanlar yerine kalıplar bulundurulmaktadır.

``let`` deyiminde kalıp olarak köşeli parantezler içerisinde değişken listesi kullanılırsa (variable patterns) dizinin
elemanları sırasıyla bu değişkenlere atanır. Örneğin:

.. code-block:: rust

    let a: [i32; 3] = [10, 20, 30];
    let [x, y, z] = a;

    println!("x = {}, y = {}, z = {}", x, y, z);        // x = 10, y = 20, z = 30

Burada aslında biz ``x``, ``y``, ve ``z`` isminde üç tane değişken yaratıp bu değişkenlere de dizi elemanlarını
sırasıyla atamış olduk. Yukarıda yapılan işlemin tamamen eşdeğeri şöyledir:

.. code-block:: rust

    let a: [i32; 3] = [10, 20, 30];
    let x = a[0];
    let y = a[1]
    let z = a[2];

``let`` deyimi ile dizi açımı sırasında ayrıca tür de belirtilebilir. Ancak bunun bir faydası olmadığı için Rust
programcıları gereksiz tür belirtmekten kaçınmaktadır. Örneğin:

.. code-block:: rust

    let a: [i32; 3] = [10, 20, 30];
    let [x, y, z]: [i32; 3] = a;       // geçerli ama açım sırasında tür belirtmenin bir faydası yok

Dizi açımı dolaylı bir atama anlamına geldiği için ``Copy`` türünden olmayan dizilerin elemanları bu işlem sırasında
taşınmaktadır. Dolayısıyla açımdan sonra bu elemanlar kullanılamamaktadır. Örneğin:

.. code-block:: rust

    let a: [String; 3] = ["ali".to_string(), "veli".to_string(), "selami".to_string()];
    let [x, y, z] = a;

    println!("x = {}, y = {}, z = {}", x, y, z);        // x = ali, y = veli, z = selami
    println!("{}", a[2]);       // error!

Açım yaparken (yani dilim kalıbını kullanırken) değerini bir değişkene yerleştirmek istemediğimiz elemanlar için
``_`` atomunu kullanabiliriz. Örneğin:

.. code-block:: rust

    let a: [i32; 5] = [10, 20, 30, 40, 50];
    let [x, _, y, _, z] = a;

    println!("x = {}, y = {}, z = {}", x, y, z);        // x = 10, y = 30, z = 50

Buradaki açıma dikkat ediniz:

.. code-block:: rust

    let [x, _, y, _, z] = a;

Dizinin ``0``'ıncı indeksli elemanı ``x`` değişkenine, ``2``'inci indeksli elemanı ``y`` değişkenine ve
``4``'üncü indeksli elemanı da ``z`` değişkenine atanmaktadır. Buradaki ``_`` karakterleri *bu indekse karşı gelen
elemanların bir değişkene atanmasını istemiyorum* anlamına gelmektedir.

Normal olarak açım sırasında (yani dilim kalıbında) köşeli parantezlerin içerisinde kaynak dizi ile aynı uzunlukta bir
eleman listesinin bulundurulması gerekir. Örneğin:

.. code-block:: rust

    let a: [i32; 10] = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100];
    let [x, y, z] = a;    //error!

Burada dizi ``10`` eleman uzunluğunda olduğu halde kalıpta yalnızca ``3`` eleman belirtilmiştir. İşte kalıptaki ``..``
sentaksı *herhangi 0 tane ya da daha fazla elemanla eşleş* anlamına gelmektedir. Örneğin:

.. code-block:: rust

    let a: [i32; 10] = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100];
    let [x, y, z, ..] = a;      //geçerli

    println!("x = {}, y = {}, z = {}", x, y, z);        // x = 10, y = 20, z = 30

Burada açım kalıbına dikkat ediniz:

.. code-block:: rust

    let [x, y, z, ..] = a;      //geçerli

Buradaki ``..`` sentaksı *geri kalanların hepsi ama ben bunlarla ilgilenmiyorum* anlamına gelmektedir. *The Rust
Reference* dokümanlarında ``..`` biçiminde oluşturulan kalıplara *rest patterns* denilmektedir.

Aslında ``..`` sentaksı dilim kalıbında herhangi bir yerde bulunabilir. Yani sonda bulunmak zorunda değildir. Ancak bu
sentaks dilim kalıbında yalnızca toplamda bir kez kullanılabilmektedir. Örneğin:

.. code-block:: rust

    let a: [i32; 10] = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100];

    let [x, .., y, z] = a;
    println!("x = {}, y = {}, z = {}", x, y, z);        // x = 10, y = 90, z = 100

    let [.., x, y, z] = a;
    println!("x = {}, y = {}, z = {}", x, y, z);        // x = 80, y = 90, z = 100

``..`` sentaksı *0 tane* anlamına da gelmektedir. Örneğin:

.. code-block:: rust

    let a: [i32; 3] = [10, 20, 30];
    let [x, y, z, ..] = a;      // geçerli

Burada ``..`` ile sıfır eşleşme yapılacaktır. Ancak kalıp geçerlidir.

``..`` kalıbında ``@`` ile değişken bağlaması da yapılabilmektedir. Bu durumda ilgili değişken bir dizi olur. Örneğin:

.. code-block:: rust

    let a: [i32; 10] = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100];

    let [x, others @ .., y, z] = a;

    println!("x = {}, y = {}, z = {}", x, y, z);        // x = 10, y = 90, z = 100
    println!("others: {:?}", others);                   // others: [20, 30, 40, 50, 60, 70, 80]

Buradaki açıma dikkat ediniz:

.. code-block:: rust

    let [x, others @ .., y, z] = a;

Kalıptaki ``..`` sentaksı ``7`` tane elemanla uyuşmaktadır. İşte biz ``others @ ..`` sentaksı ile bu ``7`` elemanı da
bir dizi biçiminde elde edebilmekteyiz. Rust'ta sıfır elemanlı dizilerin neden geçerli olduğu bu bağlamdan da
anlaşılmaktadır.

Çok boyutlu diziler için açım özyinelemeli biçimde (yani kalıpta iç içe birden fazla köşeli parantez kullanılarak)
yapılabilmektedir. Örneğin:

.. code-block:: rust

    let a: [[i32; 2]; 3] = [[10, 20], [30, 40], [50, 60]];
    let [[x, y], _, [z, k]] = a;

    println!("x = {}, y = {}, z = {}, k = {}", x, y, z, k);     // x = 10, y = 20, z = 50, k = 60

Çürütülemez (irrefutable) Kalıplar
----------------------------------

Bir kalıp *her durumda zaman uyuşum gösterir* biçimdeyse bu tür kalıplara Rust'ta *çürütülemez (irrefutable)*
kalıplar denilmektedir. ``let`` deyimindeki kalıpların da *çürütülemez (irrefutable)* olması gerekmektedir. Örneğin:

.. code-block:: rust

    let a: [i32; 3] = [10, 20, 30];

    let [x, y, _] = a;          // geçerli

Buradaki dilim kalıbı *çürütülemez (irrefutable)* biçimdedir. Dolayısıyla ``let`` bağlaması geçerlidir. Ancak
örneğin:

.. code-block:: rust

    let a: [i32; 3] = [10, 20, 30];

    let [x, y, 30] = a;         // error!

Burada ``let`` deyimindeki kalıp çürütülemez (irrefutable) değildir. (Çürütülemez olmayan kalıplara *çürütülebilir
(refutable)* kalıplar da denilmektedir.) Bu nedenle yukarıdaki bağlama error ile sonuçlanacaktır. Bu örnekte kalıbın
uyuştuğunu düşünebilirsiniz. Ancak bu genel bir uyuşma değildir. Örneğin:

.. code-block:: rust

    let mut a: [i32; 3] = [10, 20, 30];
    //...
    let [x, y, 30] = a;         // error!

Burada ikinci ``let`` deyiminden önce dizinin son elemanı değiştirilirse kalıp uyuşumu sağlanmayacaktır. Çürütülemez
kalıp demek her koşul altında kesinlikle uyuşum gösteren kalıp demektir.

let else Yapısı
---------------

Rust'ın ``1.65.0`` (Kasım 2022) versiyonuyla birlikte ``let`` deyimine *irrefutable* olmayan kalıpların kullanılmasına
olanak sağlamak amacıyla isteğe bağlı bir ``else`` kısmı da eklenmiştir. ``let`` deyiminin genel biçimi şu hale
getirilmiştir:

.. code-block:: text

    let <kalıp> = ifade [ else { [deyimler] }]

``let`` deyiminin ``else`` kısmı kalıp uyuşumu sağlanamazsa çalıştırılmaktadır. Ancak ``else`` kısmındaki blok
ifadesinin ``!`` türünden olması (yani bu blok ifadesindeki akışın geri dönmemesi) gerekmektedir. Örneğin ``a``
değişkeni ``[i32; 3]`` türünden bir dizi olsun.

.. code-block:: rust

    let [10, x, y] = a;         // error! kalıp "irrefutable" değil

Buradaki dilim kalıbı *irrefutable* olmadığı için derleme işleminde error oluşacaktır. Ancak bu ``let`` deyimine
``else`` kısmı eklenirse kalıp *irrefutable* olmasa bile derleme başarılı olur:

.. code-block:: rust

    let [10, x, y] = a else { panic!("refutable pattern in let statement!") };

Burada kalıp uyuşumu sağlanamazsa ``panic`` oluşacak ve program sonlanacaktır. ``panic!`` makrosunun ``!`` türünden
olduğuna ve ``else`` kısmındaki blok deyiminin de ``!`` türünden olduğuna dikkat ediniz. Ayrıca Rust'ta bloğun son
deyimi *divergent* yani ``!`` türündense bloğun da bu değeri ürettiği kabul edilmektedir. Yani özel bir durum olarak
biz yukarıdaki bağlamada ``panic!`` makrosundan sonra ``;`` atomunu da bulundurabilirdik:

.. code-block:: rust

    let [10, x, y] = a else { panic!("refutable pattern in let statement!"); };

Burada yine bloktan elde edilen değer ``()`` türünden değil ``!`` türündendir.

.. code-block:: rust

    fn main() {
        let a = [10, 20, 30];
        let [10, x, y] = a else { panic!("refutable pattern in let statement!") };

        println!("{} {}", x, y);        // 20 30
    }

Dizi Referansı Üzerinde Dilim Kalıbı
------------------------------------

Dilim kalıplarında kaynak ifade bir dizi referansı olursa köşeli parantez içerisindeki değişkenler de birer referans
olur (Yani dizi elemanlarının adreslerini tutan gösterici gibi olur). Örneğin:

.. code-block:: rust

    let a:  = [10, 20, 30];
    let [x, y, z] = &a;

    println!("{}, {}, {}", *x, *y, *z);     // 10 20 30

Bu açımda ``x``, ``y`` ve ``z`` ``i32`` türünden değil ``&i32`` türündendir. Ancak bu biçimdeki kalıplarda köşeli
parantezlerin önüne ``&`` atomu getirilirse değişkenler artık referans olmaktan çıkar. Örneğin:

.. code-block:: rust

    let a: [i32; 3] = [10, 20, 30];
    let &[x, y, z] = &a;

Burada artık ``x``, ``y`` ve ``z`` ``i32`` türündendir.

match İfadesinde Dilim Kalıpları
--------------------------------

Dilim kalıbı (yani yukarıdaki açım kalıpları) benzer biçimde ``match`` ifadesinde de kullanılabilmektedir. ``match``
anahtar sözcüğünün yanındaki ifade bir dizi olursa ``match`` kalıpları da dilim kalıplarından oluşabilir. Ancak tabii
artık ``match`` kollarındaki dilim kalıplarında değişken yerine sabitlerin bulunması anlamlı olur. (``match``
kollarındaki kalıplarda değişken bulundurulmasına *değişken kalıbı (variable pattern)* dendiğini ve değişken
kalıplarının da her zaman uyuşum sağladığını anımsayınız.) Örneğin:

.. code-block:: rust

    let a: [i32; 3] = [10, 20, 30];

    match a {
        [10, 20, 50] => println!("[10, 20, 30]"),
        [10, _, 50] => println!("[10, _ 30]"),
        [.., 30] => println!("[.., 30]"),
        [others @ .., 50] => println!("[others @ ..,  50]"),
        _ => println!("cannot match"),
    }

Burada birinci kol ve ikinci kol uyuşum sağlamayacaktır. Ancak üçüncü kol uyuşum sağlayacaktır.

``match`` ifadesindeki dizinin türü ile ``match`` kollarındaki dilim kalıbının türünün uyuşması gerekmektedir. Örneğin
yukarıdaki ``match`` ifadesinde dizi ``3`` elemanlıdır. Burada biz ``4`` elemana sahip bir ``match`` kolu
bulunduramayız.

Match Koruması ile Dilim Kalıpları
----------------------------------

``match`` ifadesinde dilim kalıplarında da match koruması uygulanabilir. Örneğin:

.. code-block:: rust

    let a: [i32; 3] = [10, 20, 30];

    match a {
       [x, y, _] if x + y < 50 => println!("[x, y, _] if x + y < 50"),
        _ => println!("cannot match"),
    }

Burada dilim kalıbından hemen sonra match korumasının getirildiğine dikkat ediniz.

Veya Kalıbı (or pattern)
------------------------

Dilim kalıplarındaki köşeli parantez içerisindeki elemanlarda ``|`` atomu ile *veya kalıbı (or pattern)* da
uygulanabilir. Örneğin:

.. code-block:: rust

    let a: [i32; 3] = [10, 20, 30];

    match a {
       [0 | 5 | 10, ..] => println!("[10|20|30, ..]"),
        _ => println!("cannot match"),
    }

Buradaki birinci ``match`` kolundaki kalıba dikkat ediniz. Bu kalıpta *dizinin ilk elemanı 0 ya da 5 ya da 10 ise*
uyuşum sağlanacaktır.

Range Kalıbı
------------

Dilim kalıplarında köşeli parantezler içerisindeki elemanlarda ``range`` kalıbı da kullanılabilmektedir. Örneğin:

.. code-block:: rust

    let a: [i32; 3] = [10, 20, 30];

    match a {
        [1..=15, ..] => println!("[1..=15, ..]"),
        _ => println!("cannot match"),
    }

Burada ``[1..=15, ..]`` kalıbı *dizinin ilk elemanı 1 ile 15 arasındaysa* uyuşum sağlayacaktır.

if let ve while let ile Dilim Kalıpları
---------------------------------------

Tabii biz dilim kalıbını da kalıp uyuşumu ile çalışan ``if let`` ve ``while let`` ifadelerinde kullanabiliriz.
Örneğin:

.. code-block:: rust

    let a: [i32; 3] = [10, 20, 30];

    if let [10, ..] = a {
        println!("Ok")
    }
    else {
        println!("Not ok")
    }

Tabii dilim kalıbındaki köşeli parantezler içerisindeki elemanlarda değişken kullanırsak ilgili eleman koşulunun her
zaman sağlandığı kabul edilir. Biz de ``if let`` deyiminin doğruysa kısmında bu değişkeni kullanabiliriz. Örneğin:

.. code-block:: rust

    if let [x, 20, 30] = a {
        println!("{}", x);
    }
    else {
        println!("Not ok")
    }

Burada eğer ``a`` dizisinin son iki elemanı ``20`` ve ``30`` ise uyuşum sağlanacak ve ilk eleman da ``x`` değişkenine
yerleştirilecektir. Tabii dilim kalıbı ``while let`` ifadesinde de benzer biçimde kullanılabilmektedir. Örneğin:

.. code-block:: rust

    while let [x, 10, 20] = a {
        //...
    }

Burada kalıp uyuşumu sağlandığı sürece döngü yinelenecektir.

Fonksiyon Parametrelerinde Kalıplar
-----------------------------------

Rust'ta fonksiyon tanımlanırken parametrelerde kalıplar da kullanılabilmektedir. Çünkü aslında Rust'ta fonksiyon
parametreleri genel bir kalıp biçiminde oluşturulabilmektedir. Tabii fonksiyon parametreleri için oluşturulan
kalıpların da *çürütülemez (irrefutable)* olması gerekir. Örneğin:

.. code-block:: rust

    fn main() {
        let a: [i32; 3] = [10, 20, 30];

        foo(a);
    }

    fn foo([x, _, _]: [i32; 3])  {
        println!("{}", x);          // 10
    }

Burada ``foo`` fonksiyonunun tanımlanmasında parametre değişkeni olarak kullanılan kalıp *çürütülemez (irrefutable)*
bir kalıptır. Aşağıdaki fonksiyon tanımlamasında parametrede belirtilen kalıp *çürütülebilir (refutable)* olduğu için
derleme error ile sonuçlanacaktır:

.. code-block:: rust

    fn foo([x, _, 30]: [i32; 3])  {        // error! parametre olarak kullanılan kalıp çürütülemez değil
        println!("{}", x);
    }

Parametre olarak kalıp kullanılması ile dizi elemanlarının açılarak fonksiyona aktarılması sağlanabilmektedir.
Örneğin:

.. code-block:: rust

    fn main() {
        let a: [i32; 3] = [10, 20, 30];

        foo(a);
    }

    fn foo([x, y, z]: [i32; 3])  {
        println!("{}, {}, {}", x, y, z);
    }

Fonksiyon parametrelerinde kalıp oluştururken tür belirtilmesi zorunludur. Örneğin aşağıdaki gibi bir fonksiyon
tanımlaması geçerli değildir:

.. code-block:: rust

    fn foo([x, y, z])  {                // error! tür bilgisi yok
        println!("{}, {}, {}", x, y, z);
    }

Daha önceden de belirttiğimiz gibi fonksiyon parametresi bir dizi türünden referans ise dilim kalıbındaki değişkenler
de dizi elemanlarının adreslerini tutan referanslar haline gelecektir. Örneğin:

.. code-block:: rust

    fn foo([x, y, z]: &[i32; 3]) {
        //...
    }

Burada ``x``, ``y`` ve ``z`` ``i32`` türünden değil ``&i32`` türündendir. Tabii köşeli parantezlerin de önüne ``&``
atomunu getirirsek artık değişkenler referans olmaktan çıkacaktır:

.. code-block:: rust

    fn foo(&[x, y, z]: &[i32; 3]) {
        //...
    }

Burada ``x``, ``y`` ve ``z`` artık ``i32`` türündendir.

Dilim ve Dilim Referansı ile Dilim Kalıpları
--------------------------------------------

Dilim kalıplarında kalıba karşı gelen ifade bir dizi yerine bir dilim referansı ya da bizzat bir dilim de olabilir.
Örneğin:

.. code-block:: rust

    let a: [i32; 5] = [10, 20, 30, 40, 50];
    let sr: &[i32] = &a[..3];

    match sr {
        [1..200, 20, 30] => println!("[1..200, 20, _]"),
        _ => println!("cannot match")
    }

Burada ``match`` ifadesi ``&[i32]`` türünden bir dilim referansıdır. Derleyici programın çalışma zamanı sırasında kalıp
uyuşumunu denetleyecek kodları oluşturmaktadır. Yukarıdaki ``match`` deyiminin ilk kolu uyuşum sağlayacaktır.

Dilimlerin kendi başlarına Rust'ta kullanımlarının çok sınırlı olduğunu, onların hemen her zaman referanslarla
kullanıldığını belirtmiştik.

Dilim kalıplarında ifadeler doğrudan dilim de olabilmektedir. Örneğin:

.. code-block:: rust

    let a: [i32; 5] = [10, 20, 30, 40, 50];

    match a[..3] {
        [x, y, z] => println!("x = {}, y = {}, z = {}", x, y, z),
        _ => println!("cannot match")
    }

Burada ``a[..3]`` bir dilim referansı değil bir dilimdir. Ancak ``match`` deyiminde dilimler de dilim kalıplarıyla
kullanılabilmektedir. Tabii ``if let`` ve ``while let`` deyimlerinde de dilimlerin dilim kalıpları eşliğinde
kullanılması mümkündür:

.. code-block:: rust

    if let [x, y, z] = a[..3] {
        println!("x = {}, y = {}, z = {}", x, y, z);
    }
    else {
        println!("cannot match");
    }

Tabii bu durumda ``sr`` bir dilim referansı olmak üzere ``*sr`` bir dilim belirteceğine göre ``sr`` de ``*sr`` de
``match`` ve ``if let``, ``while let`` gibi deyimlerdeki dilim kalıplarında kullanılabilir. Ancak gereksiz biçimde ``*``
operatörünü kullanmanın da bir anlamı yoktur. Örneğin:

.. code-block:: rust

    fn main() {
        let a: [i32; 5] = [10, 20, 30, 40, 50];

        foo(&a[..3]);
    }

    fn foo(sr: &[i32]) {
        match *sr {         // geçerli
            [x, y, z] => println!("x = {}, y = {}, z = {}", x, y, z),
            _ => println!("cannot match")
        }
    }

Burada biz ``match`` ifadesini zaten ``sr`` biçiminde de belirtebilirdik.

Demetler (Tuples)
=================

Demetlere Giriş
---------------

Şimdi de Rust'ta *demet (tuple)* denilen veri yapısını göreceğiz. Demetler eski dillerin bazılarında yeni dillerin ise
hemen hepsinde bulunan bir veri yapısıdır. Bazı diller ise demetleri standart kütüphaneleri yoluyla desteklemektedir.
Demetlerin built-in desteklendiği bazı programlama dilleri şunlardır:

- Python
- Haskell
- Erlang
- C# (7.0 ile 2017 yılında)
- Java (*record* ismi altında Java 14 ile 2020 yılında)
- C++ (built-in değil standart kütüphane yoluyla)
- Swift
- Go (kısıtlı biçimde)
- Rust

Demet Türü ve Demet Yaratma
---------------------------

Bir demet *elemanları farklı türlerden olabilen sıralı bir grup değeri* belirtmektedir. Bazı dinamik tür sistemine
sahip olan dillerde elemanlar arasındaki sırasal ilişki onların bir dizi kullanılmasına olanak vermektedir. Demetler bir
bakıma yapılara (structures) benzemektedir. Yapılarda da elemanlar farklı türlerden olabilmektedir. Her ne kadar
aralarında bir benzerlik varsa da demetlerle yapılar hem sentaks hem de semantik bakımdan farklı veri yapılarıdır.

Rust'ta demetler normal parantezlerle temsil edilmektedir. Bir demetin türü *parantezler içerisinde sırasıyla
elemanların türleri belirtilerek* ifade edilmektedir. Örneğin:

.. code-block:: rust

    let t: (i32, f64, i32);

Burada ``t`` değişkeni üç elemanlı bir demet belirtmektedir. Bu değişkenin türü ``(i32, f64, i32)`` biçiminde ifade
edilmektedir.

Demet yaratmak (yani demetler türünden değerler yaratmak) oldukça kolaydır. Tek yapılacak şey normal parantezler
içerisinde elemanları aralarına ``,`` atomu getirerek belirtmektir. Örneğin:

.. code-block:: rust

    let t: (i32, f64, i32);

    t = (10, 3.14, 20);

Burada ``t`` demetine onunla aynı türden olan bir demet atanmıştır. Tabii bu işlem ilkdeğer verme sentaksıyla da
yapılabilmektedir:

.. code-block:: rust

    let t: (i32, f64, i32) = (10, 3.14, 20);

``let`` deyiminde tür belirtilmeden de bir demet oluşturulabilir. Bu durumda tür çıkarımı daha önce diziler
konusunda açıkladığımız kurallara göre yapılmaktadır. Normal parantezler içerisinde sonek almamış sabitler varsa ve bu
sabitler tamsayı belirtiyorsa ``i32`` türünden, noktalı sayı belirtiyorsa ``f64`` türünden kabul edilmektedir. Örneğin

.. code-block:: rust

    let t = (10, 20, 30.5),

Burada ``t`` demeti ``(i32, i32, f64)`` türündedir. Tabii eğer parantezler içerisindeki değerlerin türleri zaten
biliniyorsa tür tespiti bu türler dikkate alınarak yapılmaktadır. Örneğin:

.. code-block:: rust

    let t = (10u8, 20f32, 30i64);

Burada ``t`` demeti ``(u8, f32, i64)`` türündendir.

Tabii parantezlerle demet yaratılırken demet değerlerinin sabit ifadesi biçiminde belirtilmesi gerekmez. Demet
elemanlarının değerleri herhangi bir ifade biçiminde oluşturulabilir. Örneğin:

.. code-block:: rust

    t = (10 + a, x + y, foo())

Burada bu ifadelerin değerleri hesaplanıp demet bu değerlerle yaratılacaktır. Örneğin:

.. code-block:: rust

    let x = 10;
    let y = 3.14;

    let t = (x, y);

Burada ``t`` demeti ``(i32, f64)`` türündendir.

Demetlerin Atanması
-------------------

Aynı türden iki demet birbirine atanabilir. Ancak farklı türden demetler birbirine atanamazlar. Örneğin:

.. code-block:: rust

    let t: (i32, f64) = (10, 3.14);
    let k: (i32, f64);

    k = t;      // geçerli

Burada her iki demet de aynı türden olduğu için atama geçerlidir. Ancak örneğin:

.. code-block:: rust

    let t: (i32, f32) = (10, 3.14);
    let k: (i32, f64);

    k = t;      // error!

Burada iki demetin türleri farklı olduğu için atama error ile sonuçlanacaktır.

Tabii normal parantezlerle demet yaratılırken sabitler sonek almamışsa Rust'ta yine *bağlama dayalı tür çıkarımı*
yapılmaktadır. Örneğin:

.. code-block:: rust

    let t: (i8, i32, f32);

    t = (10, 20, 3.14);     // geçerli

Burada ``10`` sabiti ``i32`` türünden değildir. Bağlamda buna karşı gelen eleman ``i8`` türünden olduğu için ve ``10``
sabiti ``i8`` türüyle ifade edilebildiği için ``i8`` türünden kabul edilmektedir. Aynı durum ``20`` ve ``3.14``
sabitleri için de geçerlidir. Fakat örneğin:

.. code-block:: rust

    let t: (i8, i32, f32);

    t = (10i32, 20, 3.14);     // error!

Burada ilk demet elemanı açıkça sonek belirtilerek ``i32`` türünden yapılmıştır. Bağlam yalnızca sonek almayan
sabitler için dikkate alınmaktadır. Bu atama demetler farklı türlerden olduğu için error ile sonuçlanacaktır.

Demet Elemanlarına Erişim
-------------------------

Rust'ta bir demetin elemanlarına nokta operatörüyle elemanın indeks numarası belirtilerek erişilmektedir. Örneğin
demet türünden değişkenin ismi ``t`` olmak üzere bu demetin elemanlarına ``t.0``, ``t.1``, ``t.2``, ``t.3``
ifadeleriyle erişilir:

.. code-block:: rust

    let t = (10, 3.14, 20);

    println!("{}", t.0);        // 10
    println!("{}", t.1);        // 3.14
    println!("{}", t.2);        // 20

Buradaki ``0``, ``1``, ``2``, ``3`` gibi indeks belirten sabitlere tür iliştirilemez. Buradaki indeksler yerine
değişkenler kullanılamaz. Örneğin:

.. code-block:: rust

    let t = (10, 3.14, 20);
    let i = 0;

    println!("{}", t.i);        // error!

Birim Türü (unit type)
----------------------

Sıfır elemanlı bir demet söz konusu olabilir. Anımsayacağınız gibi sıfır elemanlı demetlere *birim (unit)*
deniliyordu. Rust'ta ``()`` sentaksının hem ``0`` elemanlı bir demet türünü hem de sıfır elemanlı bir demet türünden
değeri belirttiğini anımsayınız. Bu nedenle Rust'ta ``()`` ifadesine bazen *birim (unit)* bazen de *birim türü (unit
type)* denilmektedir. Örneğin:

.. code-block:: rust

    let t: () = ();         // geçerli

Burada ``t`` değişkeni ``()`` türündedir. Bu türden bir değişkene yalnızca ``()`` ifadesi ya da içerisinde bu değer
olan bir demet atanabilir.

Tek Elemanlı Demetler
---------------------

Tek elemanlı demetler oluşturulurken ek bir ``,`` atomunun kullanılması zorunludur. Örneğin:

.. code-block:: rust

    let t = (10);

Burada ``(10)`` ifadesi tek elemanlı bir demet anlamına gelmemektedir. Buradaki parantezler demet parantezi değil
öncelik parantezidir. Biz her ifadeyi gereksiz olsa da parantezlere alabiliriz. Fakat örneğin:

.. code-block:: rust

    let t = (10, );

Burada artık ekstra ``,`` atomundan dolayı parantezler demet parantezi olarak ele alınmaktadır. Tabii tek elemanlı
demetlere aslında gerçek anlamda bir gereksinim yoktur.

Tek elemanlı demet türü belirtilirken de ekstra ``,`` atomunun bulundurulması gerekmektedir. Örneğin:

.. code-block:: rust

    let a: (i32) = (10, );      // error!

Rust'ta her ne kadar gereksiz ve anlamsız olsa da tür ifadeleri de paranteze alınabilmektedir. Dolayısıyla buradaki
``a`` değişkeni ``i32`` türündendir. Bir demet belirtmemektedir. Tabii aşağıdaki bildirim geçerlidir:

.. code-block:: rust

    let a: (i32, ) = (10, );      // geçerli

İç İçe Demetler
---------------

Demetin elemanları başka bir demet olabilir. Örneğin:

.. code-block:: rust

    let t: ((i32, i32), f64);

Burada ``t`` değişkeni iki elemanlı bir demet belirtmektedir. Bu demetin ilk elemanı da (yani ``.0`` elemanı da) bir
demet belirtmektedir. Örneğin:

.. code-block:: rust

    t = ((10, 20), 3.14);

Tabii aslında biz ``let`` deyiminde tür belirtmeden de aynı değişkeni bildirebilirdik:

.. code-block:: rust

    let t = ((10, 20), 3.14);

Demet içerisindeki demetin elemanlarına birden fazla noktalı indekslemeyle erişilmektedir. Örneğin yukarıdaki demetin
ilk elemanının ilk elemanına ``t.0.0`` ifadesiyle erişilir:

.. code-block:: rust

    let t: ((i32, i32), f64) = ((10, 20), 3.14);

    println!("{}", t.0.0);      // 10
    println!("{}", t.0.1);      // 20
    println!("{}", t.1);        // 3.14

Bir demetin elemanı bir dizi de olabilir. Örneğin:

.. code-block:: rust

    let t: ((i32, [i32; 3]), f64) = ((10, [20, 30, 40]), 3.14);

Demet Dizileri
--------------

Demet dizileri de söz konusu olabilir. Örneğin:

.. code-block:: rust

    let a: [(i32, i32); 5];

Burada ``a`` her elemanı ``(i32, i32)`` türünden olan ``5`` elemanlı bir dizidir. Dizi elemanlarına ilkdeğerlerini
şöyle verebiliriz:

.. code-block:: rust

    let a: [(i32, i32); 5] = [(10, 20), (30, 40), (50, 60), (70, 80), (90, 100)];

Biz bu diziyi ``for`` döngüsüyle dolaşırsak demetleri elde ederiz:

.. code-block:: rust

    let a: [(i32, i32); 5] = [(10, 20), (30, 40), (50, 60), (70, 80), (90, 100)];

    for t in a {
        println!("{} {}", t.0, t.1);
    }

Örneğin:

.. code-block:: rust

    fn main() {
        let people: [(&str, i32); 5] = [("ali", 10), ("veli", 20), ("selami", 30), ("ayşe", 40), ("fatma", 50)];

        for person in people {
            println!("{} {}", person.0, person.1);
        }
    }

Demetlerin Atanması ve Sahiplik
-------------------------------

Aynı türden iki demet birbirine atanırken eğer demetin tüm elemanları ``Copy`` türünden değilse yine hedef değişken
``drop`` edilmekte ve sahiplik devredilmektedir. Eğer demetin eleman türlerinin hepsi ``Copy`` türünden değilse
atamadan sonra sahiplik devredildiği (değer taşındığı için) için artık kaynak değişkeni kullanamayız. Örneğin:

.. code-block:: rust

    let people: [(String, i32); 5] = [
        (String::from("ali"), 10),
        (String::from("veli"), 20),
        (String::from("selami"), 30),
        (String::from("ayşe"), 40),
        (String::from("fatma"), 50)
    ];

    let others: [(String, i32); 5];

    others = people;

    println!("{}", people[0].0);        // error!

Demetlerin Bellek Organizasyonu
-------------------------------

Peki demetlerin bellek organizasyonu nasıldır? *The Rust Reference* dokümanlarında açıkça bir bellek organizasyonu
belirtilmemiştir. Rust'ta değişkenlerin ve değerlerin bellek organizasyonları aslında *tür temsili (type
representation)* konusuyla ilgilidir. Bu organizasyon *demetsel yapılarda (tuple struct)* değiştirilebilmektedir. Fakat
default durumda Rust'ta demet elemanlarının bellek organizasyonu hakkında açıkça bir belirlemede bulunulmamıştır. Ancak
demetlerin fonksiyonlara adres yoluyla aktarılabildiği dikkate alındığında demet elemanlarının mecburen bellekte
ardışıl bir dizilime sahip olması gerektiği sonucu çıkartılabilir. (Tabii elemanlar arasında hizalama (alignment)
amaçlı kontrollü boşluklar da bulundurulabilmektedir.) Rust derleyicileri optimizasyon amacıyla elemanların sırasını da
değiştirebilmektedir.

Demetlerin Fonksiyonlara Aktarılması
------------------------------------

Demetler fonksiyonlara dizilerde olduğu gibi iki yolla aktarılabilmektedir:

1) Kopyalama ya da taşıma yoluyla
2) Adres yoluyla (ödünç alma yoluyla)

Kopyalama ya da taşıma yoluyla aktarımda fonksiyonun parametre değişkeni bir demet olur. Fonksiyon da aynı türden bir
demet ifadesiyle (adresiyle değil) çağrılır. Yukarıda da belirttiğimiz gibi böyle bir aktarımda eğer demetin her
elemanı ``Copy`` türünden değilse sahiplik devredilmekte yani demet değerleri taşınmaktadır. Örneğin:

.. code-block:: rust

    fn main() {
        let a: (String, i32) = (String::from("Ali"), 123);

        foo(a);
    }

    fn foo(t: (String, i32)) {
        println!("{}, {}", t.0, t.1)
    }

Bu çağrımdan sonra artık biz ``main`` fonksiyonu içerisinde ``a`` demetini kullanamayız. Çünkü ``String`` türü bir
``Copy`` türü değildir, sahiplik devredilmiştir.

Adres yoluyla aktarımda ise fonksiyonun parametre değişkeni demet türünden bir referans olur. Fonksiyon da aynı demet
türünden bir değişkenin adresi alınarak çağrılır. Örneğin:

.. code-block:: rust

    fn main() {
        let a: (String, i32) = (String::from("Ali"), 123);

        foo(&a);
    }

    fn foo(t: &(String, i32)) {
        //...
    }

Burada ``t`` parametre değişkeninin türüne dikkat ediniz. ``t`` artık bir referanstır (yani bir göstericidir.)
Buradaki aktarım kopyalama ya da taşıma yoluyla değil adres yoluyla yapılmaktadır. Yani ``foo`` fonksiyonu
içerisinde aslında ``t`` bir göstericidir. Onun gösterdiği yerde de -ardışıl biçimde- iki elemanlı bir demet
bulunmaktadır. Rust'ta C'de olduğu gibi gösterici yoluyla erişimlerde ``->`` gibi operatörün olmadığını
anımsayınız. Bu ``t`` referansının gösterdiği yerdeki demetin elemanlarına yine ``t.0`` ve ``t.1`` ifadeleriyle
erişilmektedir. Örneğin:

.. code-block:: rust

    fn foo(t: &(String, i32)) {
        println!("{}, {}", t.0, t.1)
    }

Peki hangi aktarımı tercih etmeliyiz? Adres yoluyla aktarım çoğu zaman daha etkindir. Çünkü aktarılan yalnızca bir adres
bilgisidir. Fakat küçük demetler özellikle eleman türleri ``Copy`` ise değerle de aktarılabilir. Örneğin:

.. code-block:: rust

    fn main() {
        let pt = (10, 20);

        foo(pt);
    }

    fn foo(pt: (i32, i32)) {
        println!("{}, {}", pt.0, pt.1)
    }

Tabii adres yoluyla aktarımda asıl demeti fonksiyonda değiştirmek istiyorsak referansın ve değişkenin ``mut`` olması
gerekir. Örneğin:

.. code-block:: rust

    fn main() {
        let mut pt = (10, 20);

        println!("{}, {}", pt.0, pt.1);
        foo(&mut pt);
        println!("{}, {}", pt.0, pt.1);
    }

    fn foo(pt: &mut (i32, i32)) {
        pt.0 = 30;
        pt.1 = 40;
    }

=======================================================
Demetler (Devam) ve Sayımlama Türleri (enum)
=======================================================

Demetler (Devam)
================

Fonksiyonların Demetle Geri Dönmesi
-----------------------------------

Fonksiyonların geri dönüş değerleri demet biçiminde olabilir. Bu sayede birden fazla değerle geri dönen fonksiyonların
yazımı kolaylaşmaktadır. Örneğin:

.. code-block:: rust

    fn foo() -> (i32, f64) {
        //...
    }

Bu fonksiyon ``(i32, f64)`` türünden bir demetle geri dönmektedir. C'de demet olmadığını anımsayınız. (C++'ın standart
kütüphanesinde demet işlevselliğini sağlayan ``tuple`` isimli bir sınıf vardır.) C'de yukarıdaki fonksiyonun eşdeğeri
ancak bir yapı ile oluşturulabilir. Tabii bir fonksiyonun birden fazla değerle geri dönmesi eğer değer sayısı çok
fazlaysa bir zaman kaybı oluşturabilmektedir. Ancak fonksiyonların birkaç elemanlı demetlerle geri döndürülmesindeki
zaman kaybı genellikle önemsenmemektedir.

Peki fonksiyonların demetlerle geri dönmesi durumunda başarı kontrolü nasıl yapılabilir? Örneğin ikinci derece bir
denklemin köklerini geri döndüren bir fonksiyonun parametrik yapısı aşağıdaki gibi olabilir mi?

.. code-block:: rust

    fn get_roots(a: f64, b: f64, c: f64) -> (f64, f64) {
        //...
    }

Buradaki sorun denklemin kökünün olmaması durumunda fonksiyonun neyle geri döndürüleceğidir. Rust'ta bu tür
durumlarda genellikle henüz görmediğimiz ``Option`` ve ``Result`` isimli sayımlama (enumeration) türleri tercih
edilmektedir. Bu sayımlama türleri asıl değerlerin yanı sıra başarısızlığın da iletilmesini sağlamaktadır. Tabii biz
başarı durumunu da bir demet elemanı ile iletebiliriz. Bu durumda fonksiyonumuzun parametrik yapısı şöyle olacaktır:

.. code-block:: rust

    fn get_roots(a: f64, b: f64, c: f64) -> (f64, f64, bool) {
        //...
    }

Bu yöntemin de sorunu başarısızlık durumunda gereksiz bir biçimde iki kök için değer kullanılmayacağı halde değer
girilmesidir. Örneğin:

.. code-block:: rust

    fn get_roots(a: f64, b: f64, c: f64) -> (f64, f64, bool) {
        let delta: f64;

        delta = b * b - 4.0 * a * c;
        if delta < 0.0 {
            (0., 0., false)
        }
        else {
            let x1 = (-b + delta.sqrt()) / (2. * a);
            let x2 =  (-b - delta.sqrt()) / (2. * a);

            (x1, x2, true)
        }
    }

Burada ``delta < 0`` olduğu durumda kullanılmayacak olduğu halde iki kök değeri de girilmek zorunda kalınmıştır.

.. code-block:: rust

    fn main() {
        let result: (f64, f64, bool);

        result = get_roots(1., 0., -4.);
        if result.2 {
            println!("{} {}", result.0, result.1);
        }
        else {
            println!("No roots!");
        }
    }

    fn get_roots(a: f64, b: f64, c: f64) -> (f64, f64, bool) {
        let delta: f64;

        delta = b * b - 4.0 * a * c;
        if delta < 0.0 {
            (0., 0., false)
        }
        else {
            let x1 = (-b + delta.sqrt()) / (2. * a);
            let x2 =  (-b - delta.sqrt()) / (2. * a);

            (x1, x2, true)
        }
    }

Demetin Adres Yoluyla Doldurulması
----------------------------------

Yukarıdaki örnek için alternatif bir çözüm de şöyle oluşturulabilir: Fonksiyon dışarıdan adresiyle aldığı bir demetin
içini doldurabilir. Geri dönüş değeri de başarı ya da başarısızlığı belirtebilir. Bu durumda fonksiyonun parametrik
yapısı şöyle olacaktır:

.. code-block:: rust

    fn get_roots(a: f64, b: f64, c: f64, result: &mut (f64, f64)) -> bool {
            //...
    }

Fonksiyon da aşağıdaki gibi çağrılacaktır:

.. code-block:: rust

    let mut result: (f64, f64) = (0., 0.);

    if get_roots(1., 0., -4., &mut result) {
        println!("x1 = {}, x2 = {}", result.0, result.1);
    }
    else {
        println!("No roots!...")
    }

Aşağıdaki örnekte bu çözüm uygulanmıştır.

.. code-block:: rust

    fn main() {
        let mut result: (f64, f64) = (0., 0.);

        if get_roots(1., 0., -4., &mut result) {
            println!("x1 = {}, x2 = {}", result.0, result.1);
        }
        else {
            println!("No roots!...")
        }
    }

    fn get_roots(a: f64, b: f64, c: f64, result: &mut (f64, f64)) -> bool {
        let delta: f64;

        delta = b * b - 4.0 * a * c;
        if delta < 0.0 {
            false
        }
        else {
            result.0 = (-b + delta.sqrt()) / (2. * a);
            result.1 = (-b - delta.sqrt()) / (2. * a);
            true
        }
    }

Option ile Geri Dönüş
---------------------

Daha önceden de belirttiğimiz gibi yukarıdaki örnektekine benzer durumlarda aslında Rust'ta en iyi yöntem fonksiyonun
``Option<(f64, f64)>`` ``enum`` türüyle (bazı durumlarda da ``Result`` sayımlama türüyle) geri döndürülmesidir:

.. code-block:: rust

    fn get_roots(a: f64, b: f64, c: f64) -> Option<(f64, f64)> {
        //...
    }

Biz ``enum`` türlerini henüz görmedik. Sonraki bölümde sayımlama türlerini göreceğiz. Aşağıda henüz bu konuları görmemiş
olsak da Rust'ça böyle bir fonksiyonun nasıl tanımlanıp kullanıldığına ilişkin somut bir örnek verilmiştir.

.. code-block:: rust

    fn main() {
        if let Some(result) = get_roots(1., 0., -4.) {
            println!("x1 = {} x2 = {}", result.0, result.1);
        }
        else {
            println!("No roots!");
        }
    }

    fn get_roots(a: f64, b: f64, c: f64) -> Option<(f64, f64)> {
        let delta: f64;

        delta = b * b - 4.0 * a * c;
        if delta < 0.0 {
            None
        }
        else {
            let x1 = (-b + delta.sqrt()) / (2. * a);
            let x2 =  (-b - delta.sqrt()) / (2. * a);

            Some((x1, x2))
        }
    }

Demet Açımı (Destructuring)
---------------------------

Demetler de tıpkı diziler gibi açılabilmektedir. Daha önceden de belirttiğimiz gibi aslında açım (destructuring)
işlemi bir kalıp uyuşumu ile yapılmaktadır. Demet açımı için kullanılan kalıplara *demet kalıpları (tuple patterns)*
denilmektedir. Yani açım (destructuring) işlemi aslında kalıp uyuşumu (pattern matching) sürecinin özel bir durumudur.

Demet açımında normal parantezler kullanılmaktadır. Örneğin:

.. code-block:: rust

    let t: (i32, i32) = (10, 20);
    let (x, y) = t;             // demet açımı

Burada ``t`` demetinin ilk elemanı ``x`` değişkenine, ikinci elemanı da ``y`` değişkenine atanacaktır. Dizilerde
olduğu gibi bu tür açımlarda ayrıca türün belirtilmesine gerek yoktur. Ancak yapılsa da bir sorun oluşturmaz. Örneğin:

.. code-block:: rust

    let t: (i32, i32) = (10, 20);
    let (x, y): (i32, i32) = t;     // tür belirtmeye gerek yok, ancak belirtilse de bir sorun oluşmaz

Rust programcıları bu biçimdeki gereksiz tür belirtmelerini tercih etmemektedir.

Açım işlemi tıpkı dizilerde olduğu gibi fonksiyon çağrısı sırasında da yapılabilmektedir. Fonksiyonların parametre
değişkenlerinin bir kalıp biçiminde oluşturulabildiğini anımsayınız. Örneğin:

.. code-block:: rust

    fn main() {
        let t: (i32, i32) = (10, 20);

        foo(t);
    }

    fn  foo((x, y): (i32, i32))  {
        println!("x = {}, y = {}", x, y);
    }

Burada ``t`` demeti fonksiyona açılarak gönderilmektedir. Tabii fonksiyon parametrelerinde mutlaka türün
belirtilmesi gerekmektedir.

Dilim kalıplarında olduğu gibi demet kalıplarında da demetin adresi alınırsa kalıptaki değişkenler artık referans
haline gelmektedir. Örneğin:

.. code-block:: rust

    fn  foo((x, y): &(i32, i32))  {
        println!("x = {}, y = {}", *x, *y);
    }

Burada ``x`` ve ``y`` değişkenleri ``i32`` türünden değil ``&i32`` türündendir. Tabii fonksiyon da demetin adresiyle
çağrılmalıdır. Tıpkı dilim kalıplarında olduğu gibi kalıptaki parantezlerin önüne ``&`` atomu getirilebilir. Bu
durumda kalıptaki değişkenler referans olmaktan çıkar. Örneğin:

.. code-block:: rust

    fn  foo(&(x, y): &(i32, i32))  {
        println!("x = {}, y = {}", x, y);
    }

Burada ``x`` ve ``y`` artık ``i32`` türündendir.

.. code-block:: rust

    fn main() {
        let t: (i32, i32) = (10, 20);

        foo(&t);
    }

    fn  foo((x, y): &(i32, i32))  {
        println!("x = {}, y = {}", *x, *y);
    }

Geri Dönüş Değerinin Açılması
-----------------------------

Demetlerin kullanıldığı programlama dillerinde fonksiyonların geri dönüş değerleri demet ise bunların açılarak elde
edilmesiyle biçimindeki kodlarla sık karşılaşılmaktadır. Örneğin:

.. code-block:: rust

    fn main() {
        let (x, y, z) = foo();
        println!("{}, {}, {}", x, y, z);
    }

    fn  foo() -> (i32, i32, i32) {
        (10, 20, 30)
    }

Burada ``foo`` fonksiyonunun geri dönüş değeri açılarak elde edilmiştir:

.. code-block:: rust

    let (x, y, z) = foo();

Aşağıdaki örnekte ikinci derece denklemin köklerine geri dönen fonksiyonun geri dönüş değeri açılarak elde
edilmiştir.

.. code-block:: rust

    fn main() {
        let (x1, x2, result) = get_roots(1., 0., -4.);

        if result {
            println!("x1 = {}, x2 = {}", x1, x2);
        }
        else {
            println!("No roots!");
        }
    }

    fn get_roots(a: f64, b: f64, c: f64) -> (f64, f64, bool) {
        let delta: f64;

        delta = b * b - 4.0 * a * c;
        if delta < 0.0 {
            (0., 0., false)
        }
        else {
            let x1 = (-b + delta.sqrt()) / (2. * a);
            let x2 =  (-b - delta.sqrt()) / (2. * a);

            (x1, x2, true)
        }
    }

İç İçe (Özyinelemeli) Açım
--------------------------

Demet içerisinde bir demet ya da dizi özyinelemeli biçimde açılabilmektedir. Örneğin:

.. code-block:: rust

    let t: (i32, (i32, i32, i32), i32) = (10, (20, 30, 40), 50);
    let (x, y, z) = t;

    println!("{}", x);          // 10
    println!("{:?}", y);        // (20, 30, 40)
    println!("{}", z);          // 50

Burada ``y`` değişkeni bir demet olarak açılacaktır. Çünkü ``y`` değişkenine karşı gelen demet elemanı bir demettir.
Ancak biz özyinelemeli açım da yapabiliriz. Örneğin:

.. code-block:: rust

    let t: (i32, (i32, i32, i32), i32) = (10, (20, 30, 40), 50);
    let (x, (y, z, k), m) = t;

    println!("{}", x);          // 10
    println!("{:?}", y);        // 20
    println!("{}", z);          // 30
    println!("{}", k);          // 40
    println!("{}", m);          // 50

Bu örnekte iç demet de açılmıştır. Demetin elemanları dizi ise ya da dizinin elemanları demet ise yine açım benzer
biçimde yapılmaktadır. Örneğin:

.. code-block:: rust

    let t: (i32, [i32; 3], i32) = (10, [20, 30, 40], 50);
    let (x, [y, z, k], m) = t;

    println!("{}", x);          // 10
    println!("{:?}", y);        // 20
    println!("{}", z);          // 30
    println!("{}", k);          // 40
    println!("{}", m);          // 50

Burada bir noktaya dikkatinizi çekmek istiyoruz. Biz şimdiye kadarki örneklerimizde genellikle ``let`` deyiminde
değişkenin türünü türlerin daha iyi kavranabilmesi için açıkça belirttik. Aslında Rust programcıları bu tür gereksiz
tür ifadelerini hiç kullanmamaktadır. Biz de kursumuz ilerledikçe artık açık tür ifadelerini gerekmedikçe
kullanmayacağız.

Demet Kalıpları
---------------

Şimdi de demet kalıpları üzerinde ayrıca duralım. Demet kalıpları dilim kalıplarına (yani dizi kalıplarına) oldukça
benzemektedir. Ancak köşeli parantezlerle değil normal parantezlerle oluşturulmaktadır. Burada biz çeşitli örneklerle
demet kalıplarının hepsini gözden geçireceğiz.

Genel olarak demet kalıplarında parantezler içerisindeki demet elemanlarının her biri bir kalıp ifadesi olabilir.
Örneğin demet kalıbı aşağıdaki gibi sabitlerden oluşturulabilir:

.. code-block:: rust

    let t: (i32, i32, i32) = (10, 20, 30);

    match t {
        (10, 20, 30) => println!("(10, 20, 30) matched"),
        _ => println!("No match!"),
    }

Burada ``(10, 20, 30)`` kalıbında demetin her elemanı bir sabittir.

Yine kalıpta değişken kullanılırsa her zaman uyuşum sağlanır. Değişkene karşı gelen demet değerleri değişkenlere
yerleştirilir. Örneğin:

.. code-block:: rust

    let t: (i32, i32, i32) = (10, 20, 30);

    match t {
        (x, 20, y) => println!("x = {}, y = {}", x, y),
        _ => println!("No match!"),
    }

Burada ``(x, 20, y)`` kalıbı ile uyuşum sağlanırsa yeni ``x`` ve ``y`` değişkenleri yaratılıp ``t``'nin ilgili
elemanları bu değişkenlere yerleştirilecektir.

Demet kalıplarında da demet elemanlarının bir ya da birden fazlasında ``_`` atomu (wildcard pattern) kullanılabilir. Bu
durumda bu atoma karşı gelen demet elemanlarının uyuştuğu kabul edilir. Örneğin:

.. code-block:: rust

    let t: (i32, i32, i32) = (10, 20, 30);

    match t {
        (_, 100, _) => println!("(_, 100, _) matched"),
        (_, 20, 30) => println!("(_, 20, 30) matched"),
        _ => println!("No match!"),
    }

Burada ``match`` ifadesinin ikinci kolu uyuşum sağlayacaktır. Tabii yine kollardan birden fazlası uyuşum
sağlayabilir. Ancak ilk uyuşum sağlayan kol işleme sokulur. Yine demet kalıbındaki parantez içerisindeki elemanlarda
``..`` atomu kullanılırsa *diğerlerinin hepsinin uyuşum sağladığı* kabul edilmektedir. Örneğin:

.. code-block:: rust

    let t: (i32, i32, i32) = (10, 20, 30);

    match t {
        (10, ..) => println!("(_, ..) matched"),
        (10, 20, 30) => println!("(_, 20, 30) matched"),
        _ => println!("No match!"),
    }

Burada ``(10, ..)`` kalıbı ilk elemanı ``10`` olan diğer elemanları herhangi değerlere sahip olan demetlerle uyuşum
sağlamaktadır. Bu örnekte birden fazla kolun uyuşum sağladığına da dikkat ediniz. Örneğimizde uyuşumu sağlayan birinci
kol çalıştırılacaktır. Tabii yine ``..`` atomu demet elemanlarında yalnızca bir kez kullanılabilmektedir. Demet
kalıplarında da ``..`` atomu demetin sonunda bulunmak zorunda değildir. Örneğin:

.. code-block:: rust

    let t: (i32, i32, i32) = (10, 20, 30);

    match t {
        (.., 20) => println!("(.., 20) matched"),
        (10, 20, 30) => println!("(_, 20, 30) matched"),
        _ => println!("No match!"),
    }

Burada ikinci kol uyuşum sağlayacaktır.

Kalıptaki demet elemanlarında ``range`` kalıpları da kullanılabilir. Örneğin:

.. code-block:: rust

    let t: (i32, i32, i32) = (10, 20, 30);

    match t {
        (10, 15..25, 30) => println!("(10, 25..25, 30) matched"),
        (10, 20, 30) => println!("(_, 20, 30) matched"),
        _ => println!("No match!"),
    }

Buradaki ``(10, 15..25, 30)`` kalıbına dikkat ediniz. Bu demet kalıbının ikinci elemanında ``range`` kalıbı
kullanılmıştır. Demet kalıplarında da kalıptan sonra ``if`` anahtar sözcüğü ile *koruma (guard)* oluşturulabilmektedir.
Örneğin:

.. code-block:: rust

    let t: (i32, i32, i32) = (10, 20, 30);

    match t {
        (x, y, z) if x + y > 100 => println!("(x, y, z) if x + y > 100 matched"),
        (10, 20, 30) => println!("(_, 20, 30) matched"),
        _ => println!("No match!"),
    }

Burada ``(x, y, z) if x + y > 100`` kalıbına dikkat ediniz. Her ne kadar sanki demet kalıbındaki elemanlarda değişken
kalıpları kullanıldığı için birinci kolun uyuşum sağlayacağı sanılabiliyorsa da koruma yüzünden birinci kalıp uyuşum
sağlamamaktadır.

Dilim kalıbında (dizi kalıbında) ``@`` ile dizinin bir kısmı değişkene bağlanabiliyordu. Bu işlem demetlerde geçerli
değildir. Örneğin:

.. code-block:: rust

    let t: (i32, [i32; 3], i32) = (10, [20, 30, 40], 50);
    let (10, others @ .., 50) = t;      // error!

if let ve while let ile Demet Kalıpları
----------------------------------------

Tabii demet kalıpları da yalnızca ``let`` ve ``match`` deyimlerinde değil ``if let`` ve ``while let`` deyimlerinde de
kullanılabilmektedir. Örneğin:

.. code-block:: rust

    let t: (i32, i32) = (10, 20);

    if let (x, 20) = t {
        println!("matched! x = {}", x);
    }
    else {
        println!("not matched!");
    }

Demetlerin for Döngüsüyle Dolaşılması
-------------------------------------

Peki demetler ``for`` döngüsüyle dolaşılabilir mi? Başka bir deyişle demetler bir iteratör belirtmekte midir? Statik
tür sistemine sahip programlama dillerindeki bu konudaki temel sorun demet elemanlarının türleri farklı olabildiği için
dolaşım sırasında elde edilecek değişkenin türünün ne olacağıdır. Python gibi Ruby gibi dinamik tür sistemine sahip
programlama dillerinde böyle bir sorun yoktur. Ancak Rust gibi statik tür sistemine sahip dillerde genel olarak bu
sorun yüzünden böyle bir dolaşım mümkün değildir.

Sayımlama Türleri (enum)
========================

Giriş
-----

Kursumuzun bu bölümünde Rust'ın *sayımlama (enumeration)* türleri üzerinde duracağız. Sayımlama türleri pek çok
programlama dilinde bulunmaktadır. Ancak o dillerin çoğundaki sayımlama türleri Rust'taki kadar merkezi bir öneme sahip
değildir. Bu nedenle o dillerin eğitiminde bu konuyu önem derecesini dikkate alarak daha ilerilere öteliyoruz. Fakat
Rust'ta sayımlama türleri merkezi bir önemdedir. Bu nedenle biz kursumuzda henüz yapıları görmeden sayımlama türlerini
ele alacağız. Kursumuzda *sayımlama türleri* terimi yerine ``enum`` türleri terimini de kullanacağız.

Sayımlama Türlerine Neden Gereksinim Duyulmaktadır?
---------------------------------------------------

Programlama dillerinde sayımlama türlerine (enumeration types) ve sayımlama sabitlerine (enumerator) neden gereksinim
duyulmaktadır? Bunu C# üzerinden bir örnekle açıklayalım. Örneğin bir oyundaki bir topu sola, yukarı, sağa ve aşağı bir
birim ilerleten ``Move`` isimli bir fonksiyon (C#'ta her fonksiyon bir sınıfın içerisinde bulunmaktadır. Bu nedenle
bunlara metot denilmektedir) yazmak isteyelim. Böyle bir fonksiyonun parametresi ne olmalıdır? Buradaki yön bilgisi
dört seçenekten biri olabilir. İlk akla gelecek seçenek parametreyi ``int`` türden yapmak ve parametreyi bir ``switch``
içerisine sokarak ele almaktır:

.. code-block:: csharp

    public static void Move(int direction)
    {
        switch (direction) {
            case 0:             // left
                //...
                break;
            case 1:             // up
                //...
                break;
            case 2:             // right
                //...
                break;
            case 3:             // down
                //...
                break;
        }
    }

Bu durumda fonksiyonu (metodu) şöyle çağırırız:

.. code-block:: csharp

    Move(0);        // left
    //...
    Move(1);        // up
    //...
    Move(2);        // right
    //...
    Move(3);        // down

Parametrenin yukarıdaki gibi bir tamsayı türünden alınmasının şu dezavantajları vardır:

1) Okunabilirlik zayıf olur. Koda bakan kişi topun hangi yöne götürülmek istendiğini çabuk anlayamaz.
2) Fonksiyona (metoda) yanlış değer geçilirse hata derleme aşamasında değil çalışma zamanı sırasında sinsice ortaya
   çıkar.

Peki fonksiyonun parametresi ``string`` yapılsa nasıl olur? Örneğin:

.. code-block:: csharp

    public static void Move(string direction)
    {
        switch (direction) {
            case "left":
                //...
                break;
            case "up":
                //...
                break;
            case "right":
                //...
                break;
            case "down":
                //...
                break;
        }
    }

Bu durumda fonksiyon da şöyle çağrılacaktır:

.. code-block:: csharp

    Move("left");
    //...
    Move("up");
    //...
    Move("right");
    //...
    Move("down");

Bu tasarımda biz okunabilirlik problemini çözmüş olduk ancak hala bu tasarımın iki dezavantajı vardır:

1) Fonksiyona (metoda) yanlış argüman geçilirse hata yine derleme aşamasında değil programın çalışma zamanı sırasında
   sinsice ortaya çıkacaktır.
2) String'lerin karşılaştırılması sayıların karşılaştırılmasına göre çok daha yavaştır. (İki string'in
   karşılaştırılmasının bir döngü içerisinde string'lerin karşılıklı karakterlerinin karşılaştırılması yoluyla
   yapıldığını anımsayınız.)

İşte sayımlama türleri bu tür durumlar için düşünülmüştür. Birtakım olgular ya da seçenekler hem yazısal biçimle ifade
edilsin hem de arka planda aslında sayısal biçimde işleme sokulsun ve geçersiz değerler derleme aşamasında teşhis
edilsin isteniyorsa sayımlama türleri tercih edilmelidir. Yine C# üzerinden örnek verelim:

.. code-block:: csharp

    enum Direction
    {
        Left, Up, Right, Down
    }
    //...

    public static void Move(Direction d)
    {
        switch (d)
        {
            case Direction.Left:
                //...
                break;
            case Direction.Up:
                //...
                break;
            case Direction.Right:
                //...
                break;
            case Direction.Down:
                //...
                break;
        }
    }

Çağırma şöyle yapılabilir:

.. code-block:: csharp

    Move(Direction.Left);
    //...
    Move(Direction.Up);
    //...
    Move(Direction.Right);
    //...
    Move(Direction.Down);
    //...

Burada ``enum`` elemanları aslında tamsayısal bir değer belirtmektedir. Artık biz bu fonksiyonu (metodu) olmayan bir yön
ile çağırırsak hata derleme aşamasında ortaya çıkacaktır. İşte programlama dillerinde *haftanın günleri*, *yılın
ayları*, *renkler*, *protokoller* gibi seçenekler sayımlama türleriyle ifade edilmektedir.

Rust'taki enum Türleri ve Birlikler (union)
-------------------------------------------

Rust'taki sayımlama türleri amaç bakımından diğer dillerdeki sayımlama türlerine benziyor olsa da Rust'taki sayımlama
türlerinin diğer dillerdeki sayımlama türlerinden semantik bakımdan fazlalıkları ve farklılıkları vardır. Rust'taki
sayımlama türleri yalnızca basit sembolik sabitler oluşturmak için değil aynı zamanda birlik (union) benzeri oluşturmak
için de kullanılmaktadır.

Anımsanacağı gibi C/C++ gibi dillerde *birlikler (unions)* elemanları çakışık olarak yerleştirilen veri
yapılarıdır. Bir birliğin tüm elemanları aynı adresten itibaren yerleştirilir. Dolayısıyla bir birliğin bir elemanına
değer atandığında diğer elemanların da değerleri değişecek ve anlamsızlaşacaktır. Örneğin C'de aşağıdaki gibi bir
birlik söz konusu olsun:

.. code-block:: c

    union sample {
        int a;
        sdhort b;
        double c;
    };

Bu birlik türünden bir değişken tanımlayalım:

.. code-block:: c

    union Sample s;

Burada ``s`` değişkeni için birliğin en büyük elemanının uzunluğu kadar yer ayrılacaktır. Çünkü elemanlar çakışık
yerleştirilmektedir:

.. code-block:: text

    Birliğin bellek yerleşimi (her sütun bir byte)

    Offset      0     1     2     3     4     5     6     7
              ┌─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┐
    a         │ a0  │ a1  │ a2  │ a3  │ ??  │ ??  │ ??  │ ??  │
              ├─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┤
    b         │ b0  │ b1  │ ??  │ ??  │ ??  │ ??  │ ??  │ ??  │
              ├─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┤
    c         │ c0  │ c1  │ c2  │ c3  │ c4  │ c5  │ c6  │ c7  │
              └─────┴─────┴─────┴─────┴─────┴─────┴─────┴─────┘

Örneğin:

.. code-block:: c

    s.c = 3.14;

Artık ``s.a`` ve ``s.b`` elemanlarının değerleri anlamsız olacaktır. Birlikler *farklı bilgilerin yalnızca bir
tanesinin bulundurulması durumunda* yer kazancı sağlamaktadır. Ancak elimizde bir birlik varsa bizim onun hangi
elemanının set edilmiş olduğunu bilmemiz gerekir. Örneğin:

.. code-block:: c

    void foo(union Sample *ps)
    {
        //...
    }

Burada biz birliğin hangi elemanının set edilmiş olduğunu bilmezsek içerisindeki değeri kullanamayız. O halde bizim bir
biçimde birliğin hangi elemanının set edilmiş olduğu bilgisini de yukarıdaki fonksiyona parametre olarak geçirmemiz
gerekir. Tabii bunu daha organize bir biçimde yapı içerisinde birlik kullanarak sağlayabiliriz:

.. code-block:: c

    struct sample {
        union {
            int a;
            short b;
            double c;
        } info;
        int type;       // 0 => a, 1 => b, 2 => c
    };

    struct sample s;

    s.info.c = 3.14;
    s.type = 2;

    foo(&s);

Bu durumda ``foo`` fonksiyonu da şöyle olacaktır:

.. code-block:: c

    void foo(struct sample *ps)
    {
        swich (ps->type) {
            case 0:             // a elemanına başvur
                //...
                break;
            case 1:             // b elemanına başvur
                //...
                break;
            case 2:             // c elemanına başvur
                //...
                break;
        }
    }

Bu örnek ve açıklama Rust'taki ``enum`` türlerini anlamamızı kolaylaştıracaktır.

Item Grubu
----------

Rust'ta sayımlama türleri sentaktik olarak *Item* grubundadır. *The Rust Reference* dokümanlarında *Item* ara
sembolünün BNF açılımı şöyledir:

.. code-block:: text

    Item:
        OuterAttribute*
        VisItem
        | MacroItem

    VisItem:
       Visibility?
      (
          Module
        | ExternCrate
        | UseDeclaration
        | Function
        | TypeAlias
        | Struct
        | Enumeration
        | Union
        | ConstantItem
        | StaticItem
        | Trait
        | Implementation
        | ExternBlock
    )

    MacroItem:
        MacroInvocationSemi
    | MacroRulesDefinition

Item'lar *global alanda ya da bir fonksiyonun içerisinde tanımlanabilen sentaktik öğeleri belirtmektedir*. Item
öğelerinin sonunda ``;`` bulundurulmadığını anımsayınız. Daha önceden de belirttiğimiz gibi *Item* öğeleri kodun
aşağısında bulundurulsa bile kodun yukarısından kullanılabilmektedir. Fonksiyonların da sentaktik olarak *Item* grubu
altında bulunduğuna dikkat ediniz.

enum Türü Tanımlama ve Varyant Çeşitleri
----------------------------------------

Bir sayımlama türü (``enum`` türü) tanımlamanın genel biçimi şöyledir:

.. code-block:: text

    enum <isim> {
        [enum_elemanları]
    }

Görüldüğü gibi ``enum`` anahtar sözcüğünü bir *enum ismi* izlemektedir. Küme parantezlerinin içerisine de ``enum``
elemanları yerleştirilmektedir. ``enum`` elemanları ``,`` atomuyla birbirinden ayrılmaktadır. Rust'ta ``enum`` 
elemanlarına *varyant (variant)* da denilmektedir.

``enum`` varyantları üç gruba ayrılmaktadır:

- Birimsel (unit-like) ``enum`` varyantları
- Demetsel (tuple-like) ``enum`` varyantları
- Yapısal (struct-like) ``enum`` varyantları

En basit ``enum`` varyantları *birimsel (unit-like)* denilen ya da *birim varyant* denilen ``enum`` varyantlarıdır. 
Bu ``enum`` varyantları yalnızca bir isimden oluşmaktadır. Yalnızca birimsel varyantlardan oluşan ``enum`` türleri 
C/C++, C#, Java gibi dillerdeki ``enum`` türlerine oldukça benzemektedir. Örneğin:

.. code-block:: rust

    enum MyEnum {
        A, B, C
    }

Burada ``A``, ``B`` ve ``C`` varyantları ``MyEnum`` isimli ``enum`` türünün birimsel varyantlarıdır. Rust programcıları
okunabilirlik bakımından genellikle her ``enum`` varyantını ayrı bir satırda yazmayı tercih etmektedir. Örneğin:

.. code-block:: rust

    enum Direction {
        Left,
        Up,
        Right,
        Down
    }

Sayımlama türlerinin demetsel ve yapısal elemanlarını izleyen paragraflarda açıklayacağız.

enum Türünden Değişkenler ve Varyanta Erişim
--------------------------------------------

Pek çok programlama dilinde olduğu gibi Rust'ta da enum'lar türünden değişkenler bildirilebilmektedir. Örneğin:

.. code-block:: rust

    enum Direction {
        Left,
        Up,
        Right,
        Down
    }
    //...

    let d: Direction;

Burada ``d`` değişkeni ``Direction`` türündendir.

Anımsanacağı gibi C/C++, Java ve C# gibi dillerde ``enum`` elemanlarına *enumerator* deniyordu. Ancak yukarıda da
belirttiğimiz gibi Rust'ta ``enum`` elemanlarına genellikle *varyant* denilmektedir. Bir ``enum`` varyantına erişmek 
için (yani onu kullanabilmek için) *enum ismi* ile varyant ismi ``::`` operatörü (path separator) ile birleştirilir. 
``::`` operatörü iki operand'lı araek bir operatördür. Örneğin ``Direction`` ``enum`` türünün elemanlarına 
``Direction::Left``, ``Direction::Right`` biçiminde erişilmektedir.

Rust'ta ``enum`` türünün varyantları ilgili ``enum`` türündendir. Örneğin:

.. code-block:: rust

    enum Direction {
        Left,
        Up,
        Right,
        Down
    }

Burada ``Direction::Left``, ``Direction::Up``, ``Direction::Left`` ve ``Direction::Down`` varyantları ``Direction``
türündedir. Rust'ta atama işleminde kaynak ve hedef türlerin aynı olması gerektiğini anımsayınız. O halde bir ``enum``
varyantı ancak o ``enum`` türünden bir değişkene atanabilir. Enum türlerinden diğer türlere, diğer türlerden ``enum`` 
türlerine *otomatik dönüştürme (coercion)* yoktur. Örneğin:

.. code-block:: rust

    let d: Direction;

    d = Direction::Down;        // geçerli, d ile Direction::Down aynı türden

Örneğin:

.. code-block:: rust

    enum Fruit {
        Banana,
        Apple,
        Ornage,
        Cherry
    }

    enum Company {
        Microsoft,
        Apple,
        Google,
        Oracle
    }
    //...

    let f: Fruit;

    f = Company::Apple;      // error! f ile Company::Apple farklı türlerden

Demetsel Varyantlar
-------------------

Enum türlerinin demetsel varyantları bir isim ve parantezler içerisinde tür listesiyle oluşturulmaktadır. Bu sentaks
demet sentaksına benzediği için bu varyantlara demetsel varyantlar (tuple-like variants) denilmiştir. Örneğin:

.. code-block:: rust

    enum MyEnum {
        A,              // birimsel varyant
        B(i32, f64),    // demetsel varyant
    }

Burada ``MyEnum`` ``enum`` türünün ``B`` isimli varyantı demetsel bir varyanttır. ``enum`` türünün demetsel varyantları
kullanılırken tıpkı bir demet oluşturuluyormuş gibi demet türleri için değerler belirtilir. Örneğin:

.. code-block:: rust

    let s: MyEnum;

    s = MyEnum::B(10, 3.14);

Rust'taki ``enum`` türlerinin yalnızca sembolik sabitleri barındırmadığına aynı zamanda sanki bir birlik gibi değerler 
de tuttuğuna dikkat ediniz. Bir ``enum`` türünün farklı demetsel varyantları bulunabilmektedir. Örneğin:

.. code-block:: rust

    enum IPAdress {
        V4(u8, u8, u8, u8),
        V6(String)
    }
    //...

    let ipv4 = IPAdress::V4(127, 0, 0, 1);
    let ipv6 = IPAdress::V6(String::from("2001:db8:85a3::8a2e:370:7334"));

Yapısal Varyantlar
------------------

Biz henüz yapıları görmedik. Ancak ``enum`` türlerinin yapısal varyantları sentaks bakımından yapılara (structures)
benzemektedir. Yapısal varyant oluşturmanın genel biçimi şöyledir:

.. code-block:: text

    <isim> { <eleman_ismi>: <tür>, <eleman_ismi>: <tür>, <eleman_ismi>: <tür>, ... }

Varyant isminden sonra küme parantezlerinin kullanıldığına dikkat ediniz. Örneğin:

.. code-block:: rust

    enum MyEnum {
        A,                          // birimsel varyant
        B(i32, f64),                // demetsel varyant
        C { x: i32, y: f64 }        // yapısal varyant
    }

``enum`` türünün yapısal varyantları oluşturulurken küme parantezleri içerisinde eleman isimleri, sonra ``:`` atomu ve
tür bilgisi bulundurulmaktadır.

Yapısal varyantlar bilgi taşıma yeteneği bakımından demetsel varyantlara benzemektedir. Ancak iki varyant türü arasında
sentaks dışında semantik farklılıklar da vardır. İzleyen paragraflarda bu farklılıkları açıklayacağız.

``enum`` türlerinin yapısal varyantlarını oluştururken Rust programcıları genellikle varyant isminden sonra ve küme
parantezlerinin başı ile sonunda birer SPACE karakteri bulundurmaktadır. Halbuki demetsel varyantlarda Rust
programcıları bu boşlukları bulundurmazlar. Biz de kursumuzda -her ne kadar bu yazım biçimini eleştiriyorsak da- bu
genel eğilime uyacağız.

Yapısal Varyantlara İlişkin Değerlerin Oluşturulması
----------------------------------------------------

Enum türlerinin yapısal varyantlarına ilişkin ``enum`` türünden değerler oluşturmanın genel biçimi şöyledir:

.. code-block:: text

    <enum_ismi>::<varyant_ismi> {<eleman_ismi>: değer, <eleman_ismi>: değer, <eleman_ismi>: değer, ...}

Görüldüğü gibi ``enum`` türünün yapısal varyantlarına ilişkin bir ``enum`` değeri sanki yapı türünden değer 
yaratılıyormuş gibi yaratılmaktadır. Yaratım sırasında önce varyantın ismi sonra küme parantezleri içerisinde 
alanların isimleri ve onların değerleri belirtilmektedir. Eleman ismiyle değeri arasında ``:`` atomu 
bulundurulmaktadır. Örneğin:

.. code-block:: rust

    enum MyEnum {
        A,                          // birimsel varyant
        B(i32, f64),                // demetsel varyant
        C { x: i32, y: f64 }        // yapısal varyant
    }
    //...

    let s: MyEnum = MyEnum::C { x: 100, y: 3.14 };

Yine pek çok Rust programcısı varyant isminden sonra, küme parantezlerinin başında ve sonunda birer SPACE karakteri
bulundurmaktadır.

``enum`` türlerinin demetsel varyantları ile yapısal varyantları içerdikleri bilgi bakımından birbirine oldukça
benzemektedir. Ancak yapısal varyantlardaki varyant elemanlarının (bunlara *The Rust Reference* dokümanlarında
*field* denilmektedir) isimli olduğuna dikkat ediniz. Tabii yapısal varyantları oluştururken isimlerin sırasının bir
önemi yoktur. Örneğin:

.. code-block:: rust

    let s: MyEnum = MyEnum::C { y: 3.14, x: 100 };        // geçerli

Ancak yapısal varyantlara ilişkin ``enum`` değerleri oluşturulurken varyantın tüm alanları için değer belirtilmek
zorundadır. Örneğin:

.. code-block:: rust

    let s: MyEnum = MyEnum::C { x: 100 };     // error! y için değer belirtilmemiş

Enum Yapıcı Fonksiyonları
-------------------------

Demetsel ve yapısal varyantlara ilişkin ``enum`` değerleri yaratılırken yaratım sanki bir fonksiyon çağırma işlemiymiş
gibi yapılmaktadır. Bu nedenle *The Rust Reference* dokümanlarında ve Rust programcıları arasında ``enum`` varyantlarına
ilişkin değerlerin yaratılması için kullanılan ifadelere *enum yapıcı fonksiyonları (enum constructors)* da denilmektedir.
Örneğin:

.. code-block:: rust

    enum Fruit {
        Apple,
        Banana(i32),
        Apricot { price: i32, unit: i32 }
    }

Burada ``Fruit::Banana(10)`` gibi bir ifadeye ya da ``Fruit::Apricot { price: 300, unit: 1 }`` gibi bir ifadeye
*yapıcı fonksiyon (constructor)* da denilmektedir.

Alanlar ve Alansız enum Türleri
-------------------------------

Yukarıda da belirttiğimiz gibi bir ``enum`` yapıcı fonksiyonundaki öğelere *alan (field)* denilmektedir. Örneğin:

.. code-block:: rust

    enum Fruit {
        Apple,
        Banana(i32),
        Apricot { price: i32, unit: i32 }
    }

Buradaki ``Banana`` varyantının ``i32`` öğesi bir alandır. Benzer biçimde ``Apricot`` varyantının ``x`` ve ``y``
öğeleri de birer alan belirtmektedir. Görüldüğü gibi alanlar da ``Banana`` varyantında olduğu gibi isimsiz,
``Apricot`` varyantında olduğu gibi isimli olabilmektedir.

Eğer bir ``enum`` türünde yapıcı fonksiyonların hepsi alansızsa tür ``enum`` türlerine *alansız (field-less)* ``enum``
türleri denilmektedir. Örneğin:

.. code-block:: rust

    enum City {
        NewYork,
        Paris(),
        Ankara {}
    }

Burada ``City`` isimli ``enum`` türü *alansız (field-less)* bir ``enum`` türüdür. Her ne kadar buradaki ``Paris`` 
varyantı demetsel bir varyantsa da bir alanı yoktur. Benzer biçimde her ne kadar ``Ankara`` varyantı yapısal bir 
varyantsa da bir alana sahip değildir. Dolayısıyla buradaki ``City`` ``enum`` türü *alansız (field-less)* bir ``enum``
türüdür. Tabii buradaki ``enum`` türünden ``Paris`` varyantına ilişkin ve ``Ankara`` varyantına ilişkin ``enum`` 
değerleri yaratılırken yine demet ve yapı sentaksı kullanılmaktadır. Örneğin:

.. code-block:: rust

    let x = City::Paris();
    let y = City::Ankara {};

Eğer bir ``enum`` türünün tüm elemanları birimsel (unit-like) ise bu biçimdeki ``enum`` türlerine *yalnızca birimsel
elemanlardan oluşan enum türleri (unit-only enums)* denilmektedir. Örneğin:

.. code-block:: rust

    enum Direction {
        Left,
        Up,
        Right,
        Down
    }

Buradaki ``Direction`` ``enum`` türü *yalnızca birimsel elemanlardan oluşan (unit-only)* bir ``enum`` türüdür.

enum Değerlerinin Yaratılmasında İki Özel Durum
-----------------------------------------------

Enum varyantlarına ilişkin ``enum`` değerleri yaratılırken yukarıda açıklamadığımız iki özel durum daha vardır:

1) Demetsel varyantlara ilişkin ``enum`` değerleri küme parantezleriyle de (yani sanki yapısal ``enum`` varyantında 
   olduğu gibi de) yaratılabilmektedir. Ancak bu yaratım sentaksında demetsel varyantlara ilişkin alanların isimleri
   olmadığı için indeksleri kullanılmaktadır. Örneğin:

   .. code-block:: rust

       enum Vehicle {
           Car,
           Truck(i32, f64),
           Motorcycle { x: i32, y: i32 },
       }

   Biz ``Truck`` varyantına ilişkin bir ``enum`` değerini şöyle yaratıyorduk:

   .. code-block:: rust

       let v = Vehicle::Truck(10, 5.5);

   Ancak şöyle yaratabiliriz:

   .. code-block:: rust

       let v = Vehicle::Truck { 0: 10, 1: 5.5 };

   Burada isim yerine elemanın demetteki indeks numarasının belirtildiğine dikkat ediniz. *Bu sentaksın ne anlamı
   olabilir* diye düşünebilirsiniz. Bu sayede demetsel varyantlarını yapıcı fonksiyonlarında elemanları farklı
   sıralarda belirtebilmekteyiz. Örneğin:

   .. code-block:: rust

       let v = Vehicle::Truck { 1: 5.5, 0: 10 };

2) *The Rust Reference* dokümanlarına göre ``enum`` türünün birimsel varyantlarına ilişkin ``enum`` değerleri boş küme
   parantezleriyle de yaratılabilmektedir. Anımsayacağınız gibi biz birimsel varyantlara ilişkin ``enum`` değerlerini
   doğrudan isim belirterek yaratıyorduk:

   .. code-block:: rust

       let v = Vehicle::Car;

   Bu yaratımı şöyle de yapabilmekteyiz:

   .. code-block:: rust

       let v = Vehicle::Car {};

   Ancak demet sentaksıyla birimsel varyantlara ilişkin ``enum`` değerleri yaratılamamaktadır:

   .. code-block:: rust

       let v = Vehicle::Car();         // error!

use Deyimi ile Niteliklendirmenin Kaldırılması
----------------------------------------------

Bir ``enum`` türünün varyantlarına ``enum`` ismi ve ``::`` operatörüyle erişildiğini belirtmiştik. Rust'ta ``::``
operatörüne *yol ayıracı (path separator)* denilmektedir. Eğer ``enum`` varyantlarına bu operatör olmadan doğrudan
erişilmek isteniyorsa ``use`` deyiminden faydalanılmaktadır. Biz kursumuzda ``use`` deyimini ileride ayrı bir başlık
altında ele alacağız. Ancak burada bu deyimle niteliklendirmenin nasıl elimine edileceği hakkında bir açıklama yapmak
istiyoruz.

Niteliklendirmenin elimine edilmesi için ``use`` deyimi şöyle kullanılmaktadır:

.. code-block:: text

    use <enum_ismi>::*;

Örneğin:

.. code-block:: rust

    enum Vehicle {
        Car,
        Truck(i32, f64),
        Motorcycle { x: i32, y: i32 },
    }

    use Vehicle::*;
    //...

    fn main() {
        let v = Truck(10, 5.5);     // geçerli
        //...
    }

Burada ``use`` deyiminden dolayı ``enum`` varyantı *enum ismi* kullanılmadan doğrudan belirtilmiştir. Daha önceden de
belirttiğimiz gibi ``use`` deyimi gramer olarak bir *Item* durumundadır. *Item* öğelerinin yazım sırasının bir önemi
yoktur. Yani biz önce ``use`` deyimini kullanıp sonra ``enum`` tanımlamasını yapabiliriz:

.. code-block:: rust

    use Vehicle::*;         // geçerli

    enum Vehicle {
        Car,
        Truck(i32, f64),
        Motorcycle { x: i32, y: i32 },
    }

``enum`` türünün tüm varyantları için değil tek bir varyantı için de ``use`` deyimini kullanabiliriz. Örneğin:

.. code-block:: rust

    use Vehicle::Car;

Burada yalnızca ``Car`` varyantı niteliklendirilmeden kullanılabilir durumdadır. Birden fazla varyantın
niteliklendirilmeden kullanılmasını sağlamak istiyorsanız küme parantezlerini kullanmalısınız. Örneğin:

.. code-block:: rust

    use Vehicle::{Car, Truck};

Burada ``Vehicle`` ``enum`` türünün ``Car`` ve ``Truck`` varyantları niteliklendirilmeden kullanılabilir.

enum Değişkeninin İçerisindeki Varyant
--------------------------------------

Şimdi ``enum`` türleri konusunda biraz daha ilerleyelim.

Bir ``enum`` türünden değişkenin içerisinde ``enum`` türünün yalnızca bir varyantı bulunabilir. Örneğin:

.. code-block:: rust

    enum Fruit {
        Apple,
        Banana(i32),
        Apricot { price: i32, unit: i32 }
    }

    let f: Fruit = Fruit::Banana(10);

Burada ``f`` isimli ``enum`` değişkeninin içerisinde ``Banana`` varyantı vardır. ``f`` değişkeninin içerisinde belli 
bir anda tek bir varyant bulunabilir. Ancak bu varyantı daha sonra değiştirilebilir. Örneğin:

.. code-block:: rust

    let mut f: Fruit = Fruit::Banana(10);
    //...
    f = Fruit = Fruit::Apple;

Peki Rust'ta bir ``enum`` değişkeninin içerisinde hangi varyantın bulunduğunu nasıl anlarız? Bunu anladıktan sonra o
varyantın alanlarını nasıl elde edebiliriz? İşte bu soruları yanıtlayabilmemiz için öncelikle ``enum`` varyantlarına
ilişkin *ayırıcı değer (discriminant)* kavramını gözden geçirmemiz gerekir.

Ayırıcı Değer (discriminant)
----------------------------

Bir ``enum`` türünün her varyantı bir *ayırıcı değere (discriminant)* sahiptir. Default durumda (yani programcı bir
belirleme yapmamışsa) ``enum`` türünün ayırıcı değerleri ``0``'dan itibaren tamsayılarla temsil edilmektedir. Örneğin:

.. code-block:: rust

    enum Fruit {
        Apple,
        Banana(i32),
        Apricot { price: i32, unit: i32 }
    }

Burada ``Apple`` varyantının ayırıcı değeri ``0``, ``Banana`` varyantının ``1``, ``Apricot`` varyantının ``2``'dir.
Programcı ``enum`` varyantlarının ayırıcı değerlerini bazı koşullar sağlanıyorsa açıkça da (explicitly)
belirleyebilmektedir. Eğer programcı ``enum`` varyantı için bir ayırıcı değeri açıkça belirtmişse sonraki varyantlar bu
değeri izlemektedir. (Bu durum C/C++, Java ve C# gibi dillerde de benzerdir.)

Rust'ta ``enum`` varyantları için açıkça ``= <sabit_ifadesi>`` sentaksıyla ayırıcı değer belirlemesinin yapılabilmesi 
için ``enum`` türünün aşağıda belirtilen iki koşuldan en az birini sağlıyor olması gerekmektedir:

1) Eğer ``enum`` türü yalnızca birimsel elemanlardan oluşuyorsa (yani *unit-only* ise) ``enum`` türünün varyantları 
   için açıkça ayırıcı değer belirlemesi yapılabilmektedir. Örneğin:

   .. code-block:: rust

       enum Direction {
           Left,
           Up = 10,
           Right,
           Down = 20
       }

   Burada ``Left`` varyantının ayırıcı değeri ``0``, ``Up`` varyantının ``10``, ``Right`` varyantının ``11`` ve
   ``Down`` varyantının ``20``'dir. ``enum`` türünün yalnızca birimsel elemanlardan oluştuğuna (yani *unit-only*
   olduğuna) dikkat ediniz.

2) Eğer ``enum`` türüne ``repr`` özniteliği iliştirilmişse bu durumda ``enum`` varyantları için yine açıkça ayırıcı 
   değer belirlemesi yapılabilir. Biz henüz *öznitelikler (attributes)* konusunu işlemedik. Ancak Rust'ta öznitelikler
   ``#[....]`` sentaksıyla oluşturulmaktadır. Öznitelikler çeşitli sentaktik öğelere iliştirilebilmektedir. İşte bir
   ``enum`` tanımlamasının başına (yani ``enum`` anahtar sözcüğünden önce) ``repr`` (*represent* sözcüğünden kısaltma)
   özniteliği iliştirilirse bu öznitelik ``enum`` türünün varyantlarına ilişkin ayırıcı değerlerin türünü belirtir.
   Örneğin:

   .. code-block:: rust

       #[repr(u8)]
       enum Direction {
           Left(i32),
           Up(i32) = 10,
           Right(i32),
           Down(i32) = 20
       }

   Burada ``enum`` türü yalnızca birimsel varyantlardan oluşmadığı halde ``enum`` tanımlamasına ``repr`` özniteliği
   iliştirildiği için ``enum`` varyantları için ``= <sabit_ifadesi>`` sentaksıyla ayırıcı değer belirlemesi
   yapılabilmiştir. Örneğimizdeki ``#[repr(u8)]`` özniteliğindeki ``u8`` ayırıcı değerin türünü belirtmektedir.
   Ayırıcı değerler ``repr`` özniteliğinde belirtilen türün sınırları dışında değerlere sahip olamazlar. Örneğin
   aşağıdaki ``enum`` tanımlaması error ile sonuçlanacaktır:

   .. code-block:: rust

       #[repr(u8)]
       enum Direction {
           Left(i32),
           Up(i32),
           Right(i32) = 255,
           Down(i32)               // error!
       }

   Burada ``Down`` varyantının ayırıcı değeri ``256`` olacaktır. Ancak bu değer ``u8`` türünün sınırları dışında
   kalmaktadır.

Yukarıdaki iki koşuldan en az biri sağlanmıyorsa biz ``enum`` varyantları için ayırıcı değer belirlemesi yapamayız.
Örneğin:

.. code-block:: rust

    enum Direction {            // error!
        Left(i32),
        Up(i32) = 10,
        Right(i32),
        Down(i32) = 20
    }

Bu örnekte ayırıcı değer belirleme işlemi geçerli değildir.

``enum`` tanımlamasına ``repr`` ile ayırıcı değer türü iliştirilirken bu türün tamsayı türlerine ilişkin olması
zorunludur. Örneğin:

.. code-block:: rust

    #[repr(f64)]                // error!
    enum Direction {
        Left(i32),
        Up(i32),
        Right(i32),
        Down(i32)
    }

Rust'ta ``enum`` varyantlarına ilişkin ayırıcı değerlerinin hepsi birbirinden farklı olmak zorundadır. (Halbuki C/C++,
Java ve C# gibi dillerde böyle bir koşul yoktur.) Örneğin:

.. code-block:: rust

    enum Direction {
        Left,
        Up,
        Right,
        Down = 1        // error!
    }

Burada ``Up`` varyantının ayırıcı değeri de ``Down`` varyantının ayırıcı değeri de ``1``'dir. Bu durum error
oluşturacaktır.

Mademki ``enum`` varyantlarının ayırıcı değerlerinin bir türü var ve bu türü biz ``repr`` özniteliği ile
belirleyebiliyoruz, peki bu belirlemeyi hiç yapmadığımız durumdaki ayırıcı değerlerin default türü nedir? İşte Rust'ta
açıkça ``repr`` özniteliği ile bir tür belirtilmemişse default durumda *Rust temsili (Rust representation)* geçerli
olmaktadır. Rust temsiline göre de ``enum`` varyantlarının ayırıcı değerleri ``isize`` türündendir. Ancak *The Rust
Reference* dokümanları bu ``isize`` türünün yalnızca temsil amacıyla kullanıldığını belirtmektedir. Yani default
ayırıcı değer türü ``isize`` olsa da derleyiciler isterse kod üretirken bundan daha küçük türleri de kullanabilirler.

=======================================================================
enum Türleri: Bellek Organizasyonu, Ayırıcı Değer ve Kalıp Uyuşumu
=======================================================================

enum Değişkeninin Bellek Organizasyonu
======================================

Bir ``enum`` türünden değişkenin bellek organizasyonu daha önce de belirttiğimiz gibi adeta bir *birlik (union)*
organizasyonu gibidir. ``enum`` türünden bir değişken için C'de olduğu gibi ``enum`` türünün en büyük elemanını içerecek
büyüklükte bir yer ayrılmaktadır. Ancak C'den farklı olarak Rust'taki ``enum`` değişkenleri aynı zamanda ayırıcı değeri 
de kendi içerisinde tutmaktadır. Böylece derleyici ``enum`` değişkeninde hangi varyantın tutulduğunu da bu ayırıcı 
değerden hareketle anlayabilmektedir. O halde Rust'ta bir ``enum`` türünden değişken için ayrılacak alanın uzunluğu 
şöyle ifade edilebilir:

.. code-block:: text

    enum değişkeni için ayrılacak yerin uzunluğu = ayırıcı değerin uzunluğu + enum türünün en büyük elemanının uzunluğu

Tabii ayırıcı değerler ile tutulan ``enum`` varyantı arasında hizalama amacıyla padding byte'ları bulunabilir. Aslında
*The Rust Reference* dokümanlarında ``enum`` değişkeninin uzunluğunun ne olduğu ve içsel yerleşiminin nasıl yapıldığı
hakkında bir şey söylenmemiştir. Yani bu durum derleyici yazanların isteğine bırakılmıştır. Biz yukarıda tipik olarak
bir ``enum`` değişkeninin içsel organizasyonunun nasıl olması gerektiği konusunda bir açıklama yaptık. Şimdi
açıklamalarımızı somut bir örnekle pekiştirelim. Aşağıdaki gibi bir ``enum`` tanımlanmış olsun:

.. code-block:: rust

    enum Fruit {
        Apple,
        Banana(i32, f64),
        Apricot { price: i32, unit: i32 }
    }

Burada ``Apple`` varyantı birimsel bir varyanttır. Birimsel varyantların aslında değişken içerisinde bir yer
kaplamasına gerek yoktur. Çünkü zaten birimsel varyantlar değer tutmamaktadır. Dolayısıyla aslında onlar yalnızca
ayırıcı değer yoluyla teşhis edilebilirler. Örneğimizde ``Banana`` varyantı ``4 + 8 = 12`` byte, ``Apricot`` varyantı
ise ``4 + 4 = 8`` byte yer kaplayacaktır. Bu durumda bu ``enum`` türünün en büyük varyantı ``Banana`` varyantıdır. 
O halde bu ``enum`` türünden değişken *bu varyantın uzunluğu + ayırıcı değerin (discriminant) uzunluğu* kadar yer 
kaplayacaktır. Ayırıcı değerin dil içerisindeki default türünün ``isize`` olduğunu belirtmiştik. ``isize`` türü de 
64 bit sistemlerde ``8`` byte uzunluğundaydı. Ancak yukarıda biz *her ne kadar ayırıcı değerin türü isize olarak
belirtiliyorsa da derleyicilerin ayırıcı değer için içsel olarak daha küçük türleri de kullanabileceğini*
belirtmiştik. Elimizdeki derleyicide bu ``enum`` türünden değişkenin uzunluğu ``16`` byte olarak rapor edilmektedir.
Muhtemelen derleyicimiz bu ``enum`` türünün ayırıcı değeri için ``8`` byte değil ``4`` byte ayırmıştır.

.. code-block:: rust

    fn main() {
        let f = Fruit::Apple;
        let size = std::mem::size_of_val(&f);

        println!("{}", size);       // 16
    }

    enum Fruit {
        Apple,
        Banana(i32, f64),
        Apricot { price: i32, unit: i32 }
    }

Neden En Büyük Varyant Kadar Yer Ayrılır?
-----------------------------------------

Bu konuda sıkça tereddüt edilen bir konu hakkında açıklamada bulunmak istiyoruz. Aşağıdaki ``enum`` türünden değişkene
dikkat ediniz:

.. code-block:: rust

    enum Fruit {
        Apple,
        Banana(i32, f64),
        Apricot { price: i32, unit: i32 }
    }
    //...

    let f = Fruit::Apple;

Burada neden ``f`` için ``16`` byte yer ayrılıyor? Örneğin ``4`` byte bunun için yetmez miydi? İşte tıpkı C'deki
birliklerde olduğu gibi derleyicinin ``enum`` türünden değişkeni için ``enum`` türünün en büyük elemanını tutabilecek
kadar yer ayırması gerekmektedir. Çünkü daha sonra (eğer değişken ``mut`` ise) bu değişken aynı ``enum`` türünün başka
bir varyantını da tutabilmelidir. Örneğin:

.. code-block:: rust

    let mut f = Fruit::Apple;
    //...
    f = Fruit::Banana(10, 5.5);

Burada başlangıçta ``f`` değişkeni ``Fruit::Apple`` varyantını tutarken sonra ``Fruit::Banana(10, 5.5)`` varyantını
tutar hale gelmiştir. İşte işin başında ``f`` için en kötü olasılıkla en uzun elemanı da tutabilecek uzunlukta bir yer
ayrılması gerekmektedir.

Ayırıcı Değerin Sayısal Olarak Elde Edilmesi
============================================

Peki elimizde bir ``enum`` türünden değişken varsa biz onun ayırıcı değerini sayısal olarak elde edebilir miyiz? İşte
Rust'ta aslında ayırıcı değerin sayısal biçimde elde edilmesi dilin tasarımsal prensipleriyle çelişmektedir. Yani Rust
tasarımcıları ``enum`` türünden değişkenin ayırıcı değerinin sayısal biçimde elde edilmesini pek de istememiştir. Ancak
yine de iki durumda bu ayırıcı değer açıkça sayısal biçimde elde edilebilmektedir.

1) Eğer ``enum`` türünün tüm varyantları birimsel varyantlarsa (yani ``enum`` türü *unit-only* biçimindeyse) bu durumda
   bu ``enum`` türünden bir değişken ``as`` operatörüyle herhangi bir tamsayı türüne (ancak gerçek sayı türlerine değil)
   dönüştürülerek ayırıcı değer elde edilebilir. Örneğin:

   .. code-block:: rust

       fn main() {
           let d = Direction::Right;
           let dval: i32;

           dval = d as i32;            // geçerli, enum "unit-only"
           println!("{}", dval);       // 2
       }

       enum Direction {
           Left,
           Up,
           Right,
           Down
       }

   Burada ``d`` değişkeni ``Direction`` isimli ``enum`` türündendir. Bu ``enum`` türünün tüm varyantları birimseldir. 
   Bu nedenle bu ``enum`` türünden bir değişken ya da değer ``as`` operatörü ile bir tamsayı türüne dönüştürüldüğünde 
   ayırıcı değer elde edilebilir.

2) Alansız (field-less) bir ``enum`` türünün birimsel varyantları dışındaki hiçbir varyantı için ``= <sabit_ifadesi>`` 
   ile açıkça değer belirtilmemişse bu ``enum`` türünden değişken ya da değer tamsayı türlerine dönüştürülerek ayırıcı
   değer elde edilebilir. Örneğin:

   .. code-block:: rust

       enum Color {
           Red,
           Green(),
           Blue {},
           Magenta
       }

   Burada ``Color`` alansız (field-less) bir ``enum`` türüdür. Bu ``enum`` türünün birimsel olmayan varyantları için 
   açıkça değer belirtilmediği için bu ``enum`` türünden değişken ya da değer ``as`` operatörüyle tamsayı türlerine
   dönüştürülerek ayırıcı değer elde edilebilir:

   .. code-block:: rust

       fn main() {
           let d = Color::Blue{};
           let dval: i32;

           dval = d as i32;            // geçerli
           println!("{}", dval);       // 2
       }

   Burada bir hatırlatma yapmak istiyoruz. Yukarıdaki ``enum`` türünün varyantları için ancak ``repr`` özniteliği ile
   ayırıcı değer belirtilebilmektedir. Aşağıdaki ``enum`` türüne dikkat ediniz:

   .. code-block:: rust

       #[repr(i32)]
       enum Color {
           Red = 10,
           Green(),
           Blue {},
           Magenta
       }

   Burada biz bu ``enum`` türünden bir değişken ya da değeri yine ``as`` operatörüyle tamsayı türlerine
   dönüştürebiliriz. Çünkü açıkça değeri belirtilen varyant birimsel varyanttır:

   .. code-block:: rust

       fn main() {
           let d = Color::Blue{};
           let dval: i32;

           dval = d as i32;            // geçerli
           println!("{}", dval);       // 2
       }

       #[repr(i32)]
       enum Color {
           Red = 10,
           Green(),
           Blue {},
           Magenta
       }

   Fakat aşağıdaki dönüştürme error ile sonuçlanacaktır:

   .. code-block:: rust

       fn main() {
           let d = Color::Blue{};
           let dval: i32;

           dval = d as i32;            // error!
           println!("{}", dval);
       }

       #[repr(i32)]
       enum Color {
           Red,
           Green() = 10,
           Blue {},
           Magenta
       }

   Tabii ``enum`` alansız değilse zaten hiçbir durumda dönüştürme yapılamamaktadır. Örneğin:

   .. code-block:: rust

       fn main() {
           let d = Color::Blue{};
           let dval: i32;

           dval = d as i32;            // error!
           println!("{}", dval);
       }

       enum Color {
           Red,
           Green(i32),
           Blue,
           Magenta
       }

   Burada ``Color`` alansız (field-less) bir ``enum`` değildir. Dolayısıyla bu ``enum`` türünden bir değişken ya da 
   değer tamsayı türlerine dönüştürülemez.

std::mem::discriminant Fonksiyonu
=================================

``std::mem`` modülü içerisindeki ``discriminant`` isimli fonksiyon bir ``enum`` değişkeninin adresini alır. O ``enum``
değişkeninin ayırıcı değerine ilişkin ``std::mem::Discriminant<E>`` türünden (buradaki ``E`` adresi alınan ``enum``
değişkeninin ilişkin olduğu ``enum`` türünü belirtmektedir) bir değer verir. ``std::mem::Discriminant<E>`` türü generic
bir yapı belirtmektedir. Bu yapının elemanları da ``==`` ve ``!=`` operatörleriyle karşılaştırılabilmektedir. Bu
sayede biz aynı türden iki ``enum`` değişkeninin ya da değerinin içerisinde aynı ayırıcı değerin olup olmadığını dolaylı
bir biçimde anlayabiliriz. Örneğin:

.. code-block:: rust

    enum Color {
        Red(i32),
        Green(i32),
        Blue(i32),
        Magenta(i32)
    }
    //...

    let x = Color::Blue(10);
    let y = Color::Blue(20);

Burada ``x`` ve ``y`` değişkenleri alan değerleri farklı olsa da aynı varyantı tutmaktadır. İşte biz bu ``x`` ve ``y``
değişkenlerinin içerisindeki ayırıcı değerlerin aynı olup olmadığını ``std::mem::discriminant`` fonksiyonuyla
anlayabiliriz:

.. code-block:: rust

    let disc_x: std::mem::Discriminant<Color>;
    let disc_y: std::mem::Discriminant<Color>;
    let result: bool;

    disc_x = std::mem::discriminant(&x);
    disc_y = std::mem::discriminant(&y);

    result = disc_x == disc_y;
    println!("result: {}", result);         // true

Burada ``x`` ve ``y`` içerisinde aynı varyant bulunmaktadır. Ancak bu varyantların tuttukları değer farklıdır. Fakat
``std::mem::discriminant`` fonksiyonu bu alan değerlerine değil yalnızca ayırıcı değere bakmaktadır. Tabii bu tür
karşılaştırmalar aslında daha kısa bir biçimde aşağıdaki gibi de yapılabilmektedir:

.. code-block:: rust

    use std::mem::discriminant;

    let x = Color::Blue(10);
    let y = Color::Blue(20);
    let result: bool;

    result = discriminant(&x) == discriminant(&y);
    println!("{}", result);     // true

Ya da örneğin:

.. code-block:: rust

    let x = Color::Blue(10);

    if discriminant(&x) == discriminant(&Color::Blue(10)) {
        println!("yes");
    }
    else {
       println!("no");
    }

Ancak bu ``Discriminant<E>`` değeri ile ``>``, ``<``, ``>=``, ``<=`` gibi karşılaştırmalar yapılamamaktadır.

.. code-block:: rust

    use std::mem::discriminant;

    fn main() {
        let x = Color::Blue(10);

        if discriminant(&x) == discriminant(&Color::Blue(10)) {
            println!("yes");
        }
        else {
           println!("no");
        }
    }

    enum Color {
        Red(i32),
        Green(i32),
        Blue(i32),
        Magenta(i32)
    }

enum Varyantlarına İlişkin Kalıplar
===================================

Peki bir ``enum`` türünden değişkenin içerisindeki varyant alanlarını nasıl elde edebiliriz? İşte varyant alanları ancak
Rust'taki *kalıp uyuşumu* mekanizması ile elde edilebilmektedir. Bunun için tipik olarak ``match`` ifadesi
kullanılmaktadır. ``match`` anahtar sözcüğünün yanındaki ifade bir ``enum`` türündense ``match`` kollarında bu ``enum`` 
türüne ilişkin varyant kalıpları bulundurulabilir.

Birimsel Varyant Kalıpları
--------------------------

Birimsel ``enum`` varyantları için kalıplar doğrudan varyant ismiyle belirtilmektedir. Örneğin:

.. code-block:: rust

    enum Direction  {
        Left,
        Up,
        Right,
        Down
    }
    //...

    let d = Direction::Right;

    match d {
        Direction::Left => println!("left"),
        Direction::Up => println!("up"),
        Direction::Right => println!("right"),
        Direction::Down => println!("down")
    }

Burada kalıpların oluşturulma biçimine dikkat ediniz. Kalıplar yalnızca varyant isimlerinden oluşturulmuştur. Çünkü
kalıplara ilişkin varyantlarda alanlar yoktur.

.. code-block:: rust

    fn main() {
        let d = Direction::Right;

        match d {
            Direction::Left => println!("left"),
            Direction::Up => println!("up"),
            Direction::Right => println!("right"),
            Direction::Down => println!("down")
        }
    }

    enum Direction  {
        Left,
        Up,
        Right,
        Down
    }

Demetsel Varyant Kalıpları
--------------------------

``enum`` türünün demetsel varyantları için daha önce demetler (tuples) konusunda görmüş olduğumuz demet kalıplarının
hepsi kullanılabilir. Yani bu durumda kalıp bir demet kalıbı gibi oluşturulmaktadır. Önce ``enum`` varyantının ismi
belirtilir, sonra parantezler içerisinde demet kalıbı oluşturulur. Örneğin:

.. code-block:: rust

    enum Color  {
        RGB(u8, u8, u8),
        GrayScale(u8),
        CMYK(u8, u8, u8, u8),
    }

Buradaki ``enum`` türünün varyantları demetsel biçimdedir. Biz de ``match`` deyiminde varyant isminden sonra demet
kalıplarını kullanabiliriz:

.. code-block:: rust

    match color {
        Color::RGB(r, g, b) => println!("RGB: {}, {}, {}", r, g, b),
        Color::GrayScale(c) => println!("Grayscale, {}", c),
        Color::CMYK(c, m, y, k) => println!("CMYK: {}, {}, {}, {}", c, m, y, k),
    }

Burada ``match`` kollarında uyuşma sağlandığında biz hem değişkenin hangi varyantı tuttuğunu hem de o varyantın
alanlarını elde etmiş oluruz. Yukarıdaki ``match`` deyiminin *exhaustive* olduğuna da dikkat ediniz. Bu nedenle
``match`` ifadesinin sonuna ``_`` kolu yerleştirmedik.

.. code-block:: rust

    fn main() {
        let color = Color::RGB(255, 255, 0);

        match color {
            Color::RGB(r, g, b) => println!("RGB: {}, {}, {}", r, g, b),
            Color::GrayScale(c) => println!("Grayscale, {}", c),
            Color::CMYK(c, m, y, k) => println!("CMYK: {}, {}, {}, {}", c, m, y, k),
        }
    }

    enum Color  {
        RGB(u8, u8, u8),
        GrayScale(u8),
        CMYK(u8, u8, u8, u8),
    }

Demetsel varyantlarda varyant isminden sonra yalnızca değişken kalıplarını değil daha önce görmüş olduğumuz tüm demet
kalıplarını kullanabiliriz. Örneğin:

.. code-block:: rust

    match color {
        Color::RGB(255, g, _) => println!("RGB: {}", g),
        Color::GrayScale(c) => println!("Grayscale, {}", c),
        Color::CMYK(..) => println!("CMYK"),
        _=> println!("None")
    }

Burada ilk ``match`` koluna dikkat ediniz:

.. code-block:: rust

    Color::RGB(255, g, _) => println!("RGB: {}", g),

Bu ``match`` kolunun uyuşması için ilk değerin ``255`` olması gerekir. Burada demetin üçüncü elemanı her durumda
uyuşum sağlayacaktır. Programcı demetin ikinci elemanının değerini elde etmek istemiştir. Şimdi de üçüncü ``match``
koluna dikkat ediniz:

.. code-block:: rust

    Color::CMYK(..) => println!("CMYK"),

Burada programcı ``CMYK`` varyantının her alanına uyuşum sağlamak istemiştir. Bu kalıp şöyle yazılamazdı:

.. code-block:: rust

    Color::CMYK => println!("CMYK"),            // error!

Yine demetsel varyantlarda ``..`` atomu herhangi bir elemanda bulunabilmektedir. Örneğin:

.. code-block:: rust

    Color::RGB(.., b) => println!("RGB: {}", b),

Tabii ``..`` atomu yalnızca tek bir elemanda bulunabilir. Biz yine ``@`` sentaksı ile *bağlama (binding)* yapabiliriz.
Örneğin:

.. code-block:: rust

    c @ Color::RGB(255, g, _) => println!("RGB: {:?} {}", c, g),

Burada tüm varyantın değeri ayrıca ``c`` değişkenine de yerleştirilmiştir. Tabii demetsel varyantlarda demet
elemanlarında demet kalıplarındaki diğer tüm kalıplar kullanılabilir. Örneğin biz bir alanda ``range`` kalıbını
kullanabiliriz:

.. code-block:: rust

    Color::RGB(200..=255, g, b) => println!("RGB: {} {}", g, b),

Yine demetsel varyantlara ilişkin kalıplarda koruma (guard) uygulanabilir. Örneğin:

.. code-block:: rust

    Color::RGB(_, g, b) if g + b > 100 => println!("RGB: {} {}", g, b),

Burada kalıp uyuşumunun sağlanması için ayrıca ``g + b > 100`` koşulunun da sağlanması gerekmektedir.

Örnek: İkinci Derece Denklemin Kökleri
--------------------------------------

Biz daha önce ikinci derece bir denklemin köklerini veren bir fonksiyon örneği yazmıştık. Ancak o fonksiyonda göze
hoş gelmeyen bir durum vardı. Denklemin kökü yoksa biz gereksiz bir biçimde yine bir demet oluşturuyorduk.
Fonksiyonun tasarımını anımsayınız:

.. code-block:: rust

    fn get_roots(a: f64, b: f64, c: f64) -> (f64, f64, bool) {
        let delta: f64;

        delta = b * b - 4.0 * a * c;
        if delta < 0.0 {
            (0., 0., false)
        }
        else {
            let x1 = (-b + delta.sqrt()) / (2. * a);
            let x2 =  (-b - delta.sqrt()) / (2. * a);
            (x1, x2, true)
        }
    }

İşte bu tür durumlarda aslında Rust'ta ``enum`` türleri tercih edilmektedir. ``enum`` türleri farklı içerikteki 
bilgileri çakışık biçimde tutabildiği için bu tür durumlara çok uygundur. Aşağıdaki tasarıma dikkat ediniz:

.. code-block:: rust

    enum QuadraticResult {
        Roots(f64, f64),
        None
    }

    fn get_roots(a: f64, b: f64, c: f64) -> QuadraticResult {
        let delta = b * b - 4. * a * c;

        if delta > 0. {
            let x1 = (-b + delta.sqrt()) / (2. * a);
            let x2 = (-b - delta.sqrt()) / (2. * a);
            QuadraticResult::Roots(x1, x2)
        }
        else {
            QuadraticResult::None
        }
    }

Burada artık fonksiyon bir ``enum`` değerine geri dönmektedir. Bu değer de ya ``QuadraticResult::Roots`` varyantına
ilişkin olabilir ya da ``QuadraticResult::None`` varyantına ilişkin olabilir. Şimdi fonksiyonu çağıralım:

.. code-block:: rust

    let qresult: QuadraticResult;

    qresult = get_roots(1., 0., -4.);

Bizim burada fonksiyonun köklerinin olup olmadığını belirleyip eğer kök varsa kök değerlerini elde etmemiz gerekir.
İşte bunu kalıp uyuşumu ile ``match`` ifadesini kullanarak sağlayabiliriz:

.. code-block:: rust

    match qresult {
        QuadraticResult::Roots(x1, x2) => println!("x1 = {} x2 = {}", x1, x2),
        QuadraticResult::None => println!("No root found"),
    }

Tabii biz kalıp uyuşumu ile çalışan ``if let`` deyimini de kullanabilirdik. Ne de olsa örneğimizdeki
``QuadraticResult`` ``enum`` türünün yalnızca iki varyantı vardır. Örneğin:

.. code-block:: rust

    if let QuadraticResult::Roots(x1, x2) = get_roots(1., 0., -4.) {
        println!("x1 = {}, x2 = {}", x1, x2);
    }
    else {
        println!("No root found!")
    }

Burada eğer kalıp uyuşuyorsa zaten kök var demektir. Kökün olmaması durumunda kalıp uyuşumu da sağlanmayacaktır.

.. code-block:: rust

    fn main() {
        if let QuadraticResult::Roots(x1, x2) = get_roots(1., 0., -4.) {
            println!("x1 = {}, x2 = {}", x1, x2);
        }
        else {
            println!("No root found!")
        }
    }

    fn get_roots(a: f64, b: f64, c: f64) -> QuadraticResult {
        let delta = b * b - 4. * a * c;

        if delta > 0. {
            let x1 = (-b + delta.sqrt()) / (2. * a);
            let x2 = (-b - delta.sqrt()) / (2. * a);
            QuadraticResult::Roots(x1, x2)
        }
        else {
            QuadraticResult::None
        }
    }

    enum QuadraticResult {
        Roots(f64, f64),
        None
    }

Hata Mesajını Barındıran enum Türü
----------------------------------

Yukarıdaki ``enum`` türünü hata mesajını da barındıracak biçimde aşağıdaki hale dönüştürebiliriz:

.. code-block:: rust

    enum QuadraticResult {
        Roots(f64, f64),
        Error(String),
    }

Artık ``enum`` türünün bir elemanı kökleri, diğer elemanı kökün olmaması durumundaki hata mesajı belirtmektedir. Tabii
fonksiyonu da şöyle değiştirmemiz gerekecektir:

.. code-block:: rust

    fn get_roots(a: f64, b: f64, c: f64) -> QuadraticResult {
        let delta = b * b - 4. * a * c;

        if delta > 0. {
            let x1 = (-b + delta.sqrt()) / (2. * a);
            let x2 = (-b - delta.sqrt()) / (2. * a);
            QuadraticResult::Roots(x1, x2)
        }
        else {
            QuadraticResult::Error(String::from("No roots found"))
        }
    }

Biz de artık fonksiyonun geri dönüş değerini ``match`` ifadesi ile aşağıdaki gibi ele alırız:

.. code-block:: rust

    match get_roots(1., 0., 4.) {
        QuadraticResult::Roots(x1, x2) => println!("x1 = {}, x2 = {}", x1, x2),
        QuadraticResult::Error(msg) => println!("{}", msg)
    }

.. code-block:: rust

    fn main() {
        match get_roots(1., 0., 4.) {
            QuadraticResult::Roots(x1, x2) => println!("x1 = {}, x2 = {}", x1, x2),
            QuadraticResult::Error(msg) => println!("{}", msg)
        }
    }

    fn get_roots(a: f64, b: f64, c: f64) -> QuadraticResult {
        let delta = b * b - 4. * a * c;

        if delta > 0. {
            let x1 = (-b + delta.sqrt()) / (2. * a);
            let x2 = (-b - delta.sqrt()) / (2. * a);
            QuadraticResult::Roots(x1, x2)
        }
        else {
            QuadraticResult::Error(String::from("No roots found"))
        }
    }

    enum QuadraticResult {
        Roots(f64, f64),
        Error(String),
    }

Yapısal Varyant Kalıpları
-------------------------

Şimdi de ``enum`` türlerinin yapısal (struct-like) varyantlarına ilişkin kalıplar üzerinde duralım. ``enum`` türümüz
aşağıdaki gibi olsun:

.. code-block:: rust

    enum MyEnum {
        A,
        B(i32, i32),
        C { x: i32, y: i32, z: i32 },
    }

Bu ``enum`` türünden aşağıdaki gibi bir değişken oluşturalım:

.. code-block:: rust

    let me: MyEnum = MyEnum::C { x: 10, y: 20, z: 30 };

Şimdi bu örneği kullanarak yapısal varyantlara ilişkin kalıpların nasıl oluşturulduğunu madde madde açıklayalım:

- Yapısal kalıplarda önce yapısal varyantın ismi sonra küme parantezleri içerisinde eleman isimleri ve ``:`` atomu ile
  sabit kalıpları oluşturulabilir. Örneğin:

  .. code-block:: rust

      match me {
          MyEnum::C { x: 10, y: 20, z: 30 } => println!("matched"),
          _ => println!("cannot match!")
      }

  Burada ``MyEnum::C`` kalıbına dikkat ediniz. Varyantın her alanı için sabit kalıbı kullanılmıştır.

  Tabii yapısal varyant kalıplarında alanların yapısal varyanttaki sırayı izlemesi zorunlu değildir. Örneğin:

  .. code-block:: rust

      match me {
          MyEnum::C { y: 20, z: 30, x: 10 } => println!("matched"),
          _ => println!("cannot match!")
      }

- Yapısal varyantlarda varyantın alanları için ``_`` (wildcard pattern) atomu bulundurulursa kalıp uyuşumu o alanlar
  için her zaman sağlanmaktadır. Örneğin:

  .. code-block:: rust

      match me {
          MyEnum::C { x: _, y: 20, z: _ } => println!("matched"),
          _ => println!("cannot match!")
      }

  Burada ``match`` deyiminin ilk koluna dikkat ediniz:

  .. code-block:: rust

      MyEnum::C { x: _, y: 20, z: _ } => println!("matched"),

  Bu kalıp *x herhangi bir değerde olabilir, z herhangi bir değerde olursa olsun, ancak y 20 olmalı* anlamına
  gelmektedir.

- Yapısal varyantların alanlarında da ``..`` kalıbı (rest pattern) kullanılabilmektedir. Ancak yapısal varyantlarda bu
  ``..`` kalıbı küme parantezlerinin sonunda bulunmak zorundadır. Örneğin:

  .. code-block:: rust

      match me {
          MyEnum::C { y: 20, .. } => println!("matched"),
          _ => println!("cannot match!")
      }

  ``match`` deyiminin birinci koluna dikkat ediniz:

  .. code-block:: rust

      MyEnum::C { y: 20, .. } => println!("matched"),

  Burada ``..`` kalıbı *geri kalan tüm alanlar herhangi bir değerde olabilir* anlamına gelmektedir. Aşağıdaki kalıp
  geçerli değildir:

  .. code-block:: rust

      MyEnum::C { .., y: 20 } => println!("matched"),         // error!

  Demetsel varyantlarda bu ``..`` kalıbı herhangi bir yerde bulunabilmesine karşın yapısal varyantlarda sonda
  bulunmak zorundadır.

- Yapısal varyantların alanlarında da değişken kalıbı kullanılabilir. Bu durumda alan ismini ``:`` atomu ve bunu da
  bir değişken izler. Örneğin:

  .. code-block:: rust

      match me {
          MyEnum::C { x: a, y: b, z: 30} => println!("matched: a = {}, b = {}", a, b),
          _ => println!("cannot match!")
      }

  Burada birinci ``match`` kolunda ``x`` ve ``y`` alanları her zaman uyuşum sağlamaktadır. Ancak bu alanların
  değerlerini biz ``a`` ve ``b`` değişkenlerinin içerisine yerleştirdik.

- Aslında yapısal varyantların kalıplarında yalnızca alan ismi belirtilirse (yani ``:`` atomu ve değişken ismi
  belirtilmezse) bu durum da geçerli kabul edilmektedir. *The Rust Reference* dokümanlarına göre *yalnızca alan
  isminin belirtilmesi, alan isminden sonra ':' ve alan isminin aynısının kullanılması ile aynı etkiye* sahiptir.
  Örneğin:

  .. code-block:: rust

      match me {
          MyEnum::C { x, y, z: 30 } => println!("matched: x = {}, y = {}", x, y),
          _ => println!("cannot match!")
      }

  Örneğimizdeki ``match`` deyiminin ilk koluna dikkat ediniz:

  .. code-block:: rust

      MyEnum::C { x, y, z: 30 } => println!("matched: x = {}, y = {}", x, y),

  Burada ``x`` ve ``y`` alan isimlerinden sonra ``:`` sentaksı kullanılmamıştır. Dolayısıyla bu alanların değerleri
  doğrudan ``x`` ve ``y`` isimli değişkenler yaratılarak onların içerisine yerleştirilecektir. Yani yukarıdaki kalıp
  aşağıdakiyle tamamen eşdeğerdir:

  .. code-block:: rust

      MyEnum::C { x: x, y: y, z: 3 } => println!("matched: x = {}, y = {}", x, y),

- ``match`` deyiminde yapısal varyantların alanlarında yine koruma uygulanabilir. Örneğin:

  .. code-block:: rust

      MyEnum::C {x, y, z: 30} if x + y > 30 => println!("matched: x = {}, y = {}", x, y),

  Burada uyuşumun sağlanması için aynı zamanda ``x + y > 30`` koşulunun da sağlanması gerekmektedir.

=============================================================================
enum Türlerinin Fonksiyonlara Aktarılması, Metotlar ve Generic enum Türleri
=============================================================================

enum Türlerinin Fonksiyonlara Aktarılması
=========================================

Peki ``enum`` türünden bir değişken ya da değer fonksiyonlara nasıl aktarılabilir? İşte tıpkı dizilerde olduğu gibi
``enum`` türünden değişkenler ya da değerler fonksiyonlara iki biçimde aktarılabilmektedir:

1) Kopyalama (ya da taşıma yoluyla)
2) Adres yoluyla

Bir ``enum`` türünden değişkenin ya da değerin kopyalanarak (ya da taşınarak) fonksiyona aktarılması durumunda 
fonksiyonun parametre değişkeni ``enum`` türünden bir değişken olur. Fonksiyon da aynı ``enum`` türünden bir değişken 
ya da değerle çağrılır. Bu durumda argüman olarak kullanılan ``enum`` değişkeninin ya da değerinin tüm byte'ları 
fonksiyona kopyalanarak aktarılır. Tabii bu aktarım biçiminde değişken de sahipliğini devredecektir.

Örneğin:

.. code-block:: rust

    fn main() {
        let f = Fruit::Orange { origin: String::from("Anamur"), weight: 3.56};

        foo(f);
    }

    fn foo(f: Fruit) {
        match f {
            Fruit::Apple =>println!("Apple"),
            Fruit::Banana(items) => println!("Banana: {}", items),
            Fruit::Orange {origin: o, weight: w} => println!("Orange {}, {}", o, w)
        }
    }

    enum Fruit {
        Apple,
        Banana(i32),
        Orange { origin: String, weight: f64},
    }

Burada ``f`` değişkeninin içerisindeki değer bir bütün olarak fonksiyona aktarılmıştır. Artık bu aktarımdan sonra eğer
``enum`` varyantının içerisinde ``Copy`` türünden olmayan bir alan (field) varsa ``enum`` değişkeni sahipliğini 
kaybedecektir. Çünkü değişken fonksiyona taşınmıştır. Eğer ``enum`` türü bellekte az yer kaplıyorsa bu biçimde 
değişkenin ya da değerin kopyalanarak (ya da taşınarak) fonksiyona aktarılması amaç bakımından da uygunsa kötü bir 
teknik olarak değerlendirilmez. Ancak değişkenin ya da değerin taşınması istenmiyorsa ya da ``enum`` türü büyük olduğu 
için değişkenin ya da değerin kendisinin taşınması maliyetli olacaksa bu durumda aktarım adres yoluyla yapılmalıdır.

Adres yoluyla aktarımda fonksiyonun parametre değişkeni ``enum`` türünden bir referans (referans ``mut`` da olabilir)
olur. Fonksiyon da ``enum`` türünden değişkenin ya da değerin adresiyle çağrılır. Her ne kadar biz henüz *referanslar ve
göstericiler konusunu* görmemiş olsak da buna bir örnek vermek istiyoruz:

.. code-block:: rust

    fn main() {
        let f = Fruit::Orange { origin: String::from("Anamur"), weight: 3.56};

        foo(&f);
    }

    fn foo(f: &Fruit) {
        match f {
            Fruit::Apple =>println!("Apple"),
            Fruit::Banana(items) => println!("Banana: {}", *items),
            Fruit::Orange {origin: o, weight: w} => println!("Orange {}, {}", *o, *w)
        }
    }

    enum Fruit {
        Apple,
        Banana(i32),
        Orange { origin: String, weight: f64},
    }

``match`` deyiminde ``match`` anahtar sözcüğünün yanındaki ifade bir referans ise kalıp uyuşumundaki alan belirten
değişkenler de birer referans olur. ``foo`` fonksiyonu içerisindeki ``match`` deyimine dikkat ediniz:

.. code-block:: rust

    match f {
        Fruit::Apple =>println!("Apple"),
        Fruit::Banana(items) => println!("Banana: {}", *items),
        Fruit::Orange {origin: o, weight: w} => println!("Orange {}, {}", *o, *w)
    }

Burada ``f`` değişkeni ``&Fruit`` türündendir. Yani ``Fruit`` isimli ``enum`` türünden bir referanstır. Bu durumda
kalıptaki alanlarda kullanılan değişkenler de birer referans olur. (Örneğin ``items`` değişkeni ``&i32`` türünden,
``o`` değişkeni ``&String`` türünden ve ``w`` değişkeni de ``&f64`` türündendir.) Bu konu referanslarla ilgili olduğu
için bu tür kalıplar üzerinde *göstericilerin ve referansların* anlatıldığı bölümde daha ayrıntılı bir biçimde
duracağız. Burada bir noktaya daha değinmek istiyoruz. Yukarıdaki örnekte ``match`` ifadesinde ``f`` yerine ``*f``
kullanarak kalıptaki alanların referans olmaktan çıkartılabileceğini sanabilirsiniz. Örneğin:

.. code-block:: rust

    fn foo(f: &Fruit) {
        match *f {
            Fruit::Apple =>println!("Apple"),
            Fruit::Banana(items) => println!("Banana: {}", *items),
            Fruit::Orange {origin: o, weight: w} => println!("Orange {}, {}", *o, *w)
        }
    }

Burada artık ``*f`` ifadesi ``&Fruit`` türünden değil, ``Fruit`` türündendir. Dolayısıyla ``items``, ``o`` ve ``w``
alanlarının artık referans olmaması gerekir. Ancak Rust'ta ``Copy`` türünden olmayan bir referans ile o referansın
gösterdiği yerdeki değere erişilip onun sahipliği devredilememektedir. (Eğer bu yapılabilseydi Rust'taki *sahiplik
(ownership)* ve *ödünç alma (borrow)* mekanizmalarının yürütülmesi mümkün olmazdı.) Tabii yukarıdaki ``enum`` türünün
bütün alanları ``Copy`` türünden olsaydı yukarıdaki işlem yapılabilirdi. Örneğin aşağıdaki durumda ``match`` deyimi
geçerli olacaktır:

.. code-block:: rust

    fn main() {
        let f = Fruit::Orange { origin: 10, weight: 3.56};

        foo(&f);
    }

    fn foo(f: &Fruit) {
        match *f {
            Fruit::Apple =>println!("Apple"),
            Fruit::Banana(items) => println!("Banana: {}", items),
            Fruit::Orange {origin: o, weight: w} => println!("Orange {}, {}", o, w)
        }
    }

    enum Fruit {
        Apple,
        Banana(i32),
        Orange { origin: i32, weight: f64},
    }

Burada artık ``Fruit`` türünün varyantlarına ilişkin tüm alanların ``Copy`` türünden olduğuna dikkat ediniz. Eğer bir
``enum`` türünün tüm varyantlarındaki alanlar ``Copy`` türündense ``enum`` türü de ``Copy`` türünden kabul edilmektedir.

enum Türlerinde Metotlar
========================

Rust'ta ``enum`` türleri tıpkı yapılarda olduğu gibi metotlara ve ilişkili fonksiyonlara (associated functions) sahip
olabilmektedir. Tabii biz kursumuzda henüz metotların ve ilişkili fonksiyonların nasıl oluşturulduğunu görmedik. Ancak
burada ``enum`` türlerinin yapılar gibi metotlara ve ilişkili fonksiyonlara sahip olabileceğini şimdiden belirtmek
istiyoruz. Bu nedenle ``enum`` türünden bir değişkenle o ``enum`` türünün bir metodunun çağrıldığını görürseniz
şaşırmamalısınız. Örneğin ``f`` değişkeni bir ``enum`` türünden olsun. Kodda da aşağıdaki gibi bir ifade bulunuyor 
olsun:

.. code-block:: rust

    f.foo();

Burada ``foo`` söz konusu ``enum`` türünün bir metodudur.

Bu konular ileride ayrı bir başlık altında ele alınacaktır. Aşağıdaki örnekte ``Fruit`` isimli ``enum`` türü için
``disp`` isimli bir metot yazılmıştır. Bu ``disp`` metodunda ``match`` deyimi kullanılarak ``enum`` değişkeninin
içerisinde hangi varyantın bulunduğu belirlenmiş ve onun bilgileri ekrana (stdout dosyasına) yazdırılmıştır:

.. code-block:: rust

    fn main() {
        let f = Fruit::Apple;
        let k = Fruit::Banana(100);
        let m = Fruit::Orange {amount: 10, weight: 6.32};

        f.disp();
        k.disp();
        m.disp();
    }

    enum Fruit {
        Apple,
        Banana(i32),
        Orange { amount: i32, weight: f64},
    }

    impl Fruit {
        fn disp(&self) {
            match self {
                Fruit::Apple => println!("Apple"),
                Fruit::Banana(amount) => println!("Banana({})", *amount),
                Fruit::Orange { amount, weight } => println!("Orange({}, {})", *amount, *weight)
            }
        }
    }

Metotların ``impl`` anahtar sözcüğüyle oluşturulmuş bir blok içerisinde yazıldığına dikkat ediniz. ``disp``
metodundaki ``self`` anahtar sözcüğü ``disp`` metodunun çağrıldığı ``enum`` değişkenini ya da değerini temsil etmektedir.
Parametredeki ``&self`` ifadesi ise bu değişkenin ya da değerin adresinin metoda aktarıldığını belirtmektedir. Yani
metot içerisinde ``self`` anahtar sözcüğü ``&Fruit`` türündendir ve metodun çağrıldığı ``enum`` değişkeninin ya da
değerinin adresini belirtmektedir.

``enum`` türleri trait'leri de destekleyebilmektedir. Trait'ler konusu ileride ayrı bir bölümde ele alınacaktır.

Örnek: unwrap ve expect Metotları
---------------------------------

Aşağıdaki örnekte daha önce yapmış olduğumuz ikinci derece denklemin köklerini temsil eden ``QuadraticResult`` isimli
``enum`` türüne ``unwrap`` ve ``expect`` isimli iki metot yerleştirdik. Bu ``enum`` türünü yeniden anımsatmak istiyoruz:

.. code-block:: rust

    enum QuadraticResult {
        Roots(f64, f64),
        None
    }

Bu ``enum`` türünün ``Roots`` varyantı ikinci derece denklemin köklerini, ``None`` varyantı ise köklerin olmayışını
belirtmektedir. Örneğimizdeki ``enum`` türünün ``unwrap`` metodu eğer kök varsa bu köklere ilişkin *(f64, f64) türünden
bir demete* geri dönmektedir. Metot eğer kök yoksa ``panic`` oluşturup programı sonlandırmaktadır. Yani bu metodu
çağırdığımızda kök varsa kökleri elde ederiz, kök yoksa program ``panic`` ile sonlandırılır. Metodun kullanımına dikkat
ediniz:

.. code-block:: rust

    let result = get_roots(-2., 0., -4.);
    let (x1, x2) = result.unwrap();

``expect`` metodunun ``unwrap`` metodundan tek farkı bir string sabitini de argüman olarak alabilmesidir. Bu metot kök
yoksa ``panic`` oluştururken bu ``panic`` mesajını ekrana (stderr dosyasına) bastırmaktadır. Metotlar aşağıdaki gibi
yazılmıştır:

.. code-block:: rust

    impl QuadraticResult {
        fn unwrap(&self) -> (f64, f64) {
            match self {
                Self::Roots(x1, x2) => (*x1, *x2),
                Self::None => panic!(),
            }
        }

        fn expect(&self, msg: &str) -> (f64, f64) {
            match self {
                Self::Roots(x1, x2) => (*x1, *x2),
                Self::None => panic!("{}", msg),
            }
        }
    }

Metotlardaki ``&self`` anahtar sözcüğü (``s``'nin küçük harf olduğuna dikkat ediniz) metodun çağrıldığı ``enum`` 
türünden değişkenin ya da değerin adresini, ``Self`` anahtar sözcüğü ise (``S``'nin büyük harf olduğuna dikkat ediniz)
``impl`` bloğundaki ``enum`` ismini temsil etmektedir. Ancak bir kez daha vurgulamak istiyoruz: Biz burada *metotların
yazımı konusunu ele almıyoruz*. Bu konu ileride ayrıca zaten ele alınacaktır. Burada yapmak istediğimiz şey ``enum``
türlerinin metotlara sahip olabildiği fikrini vermektir.

.. code-block:: rust

    fn main() {
        let result = get_roots(-2., 0., -4.);

        let (x1, x2) = result.expect("No root found!");
        println!("x1 = {}, x2 = {}", x1, x2);
    }

    fn get_roots(a: f64, b: f64, c: f64) -> QuadraticResult {
        let delta = b * b - 4. * a * c;

        if delta > 0. {
            let x1 = (-b + delta.sqrt()) / (2. * a);
            let x2 = (-b - delta.sqrt()) / (2. * a);
            QuadraticResult::Roots(x1, x2)
        }
        else {
            QuadraticResult::None
        }
    }

    enum QuadraticResult {
        Roots(f64, f64),
        None
    }

    impl QuadraticResult {
        fn unwrap(&self) -> (f64, f64) {
            match self {
                Self::Roots(x1, x2) => (*x1, *x2),
                Self::None => panic!(),
            }
        }

        fn expect(&self, msg: &str) -> (f64, f64) {
            match self {
                Self::Roots(x1, x2) => (*x1, *x2),
                Self::None => panic!("{}", msg),
            }
        }
    }

Generic enum Türleri
====================

Generic Tür Kavramı
-------------------

Rust'ta ``enum`` türleri de generic olabilmektedir. Kursumuzda generic türler ve fonksiyonlar ileride ayrı bir bölümde
ele alınacaktır. Fakat biz burada generic ``enum`` türleri hakkında bazı temel bilgiler vermek istiyoruz.

Bir türün generic olması demek o türün genel bir biçimde oluşturulması demektir. Generic türler pek çok programlama
diline zaman içerisinde sokulmuştur. Java'da ve C#'ta da bu özellik *generic* ismiyle, C++'ta ise *template* ismiyle
bulunmaktadır. C'de böyle bir özellik yoktur.

Bir ``enum`` generic olarak yazılırken *enum isminden* sonra açısal parantezler içerisinde tür parametreleri belirtilir.
Örneğin:

.. code-block:: rust

    enum MyEnum<T, K> {
        //...
    }

Burada ``T`` ve ``K`` türleri temsil eden tür parametreleridir. ``enum`` türü de bu tür parametrelerine dayalı olarak
oluşturulur. Tür parametreleri isimlendirme kuralına uygun herhangi biçimde isimlendirilebilmektedir. Ancak Rust
programcıları generic türlerin tür parametrelerini tek bir büyük harfle isimlendirmeyi tercih etmektedir. Örneğin:

.. code-block:: rust

    enum Fruit<T> {
        Apple,
        Banana(T, T),
        Orange(T)
    }

Burada tür parametresi olan ``T`` aslında herhangi bir türü temsil etmektedir. Örneğin eğer ``T`` tür parametresi
``i32`` olsa bu ``enum`` türü aşağıdaki hale gelecektir:

.. code-block:: rust

    enum Fruit {
        Apple,
        Banana(i32, i32),
        Orange(i32)
    }

Eğer ``T`` tür parametresi ``f64`` olsaydı bu durumda ``Fruit`` ``enum`` türü şu hale gelecekti:

.. code-block:: rust

    enum Fruit {
        Apple,
        Banana(f64, f64),
        Orange(f64)
    }

İşte generic türlerde derleyici bir şablon eşliğinde gerçek türleri belirleyerek kendisi oluşturmaktadır. Derleyicinin
generic bir türe bakıp generic parametreler yerine gerçek türleri yerleştirip o generic türden gerçek bir tür
oluşturma sürecine Rust'ta *monomorfizasyon (monomorphization)* denilmektedir. (C++'ta ve C#'ta bu sürece *örnek
oluşturma (instantiation)* adı verilmektedir.) Peki derleyici ne zaman ve nasıl monomorfizasyon yapmaktadır? İşte bu
konunun çeşitli ayrıntıları vardır. Biz zaten bu konuyu ayrı bir bölümde ayrıntılarıyla ele alacağız.

Generic enum Türünden Değişken Bildirmek
----------------------------------------

Bir generic ``enum`` türünden değişken bildirilirken yalnızca ``enum`` ismi belirtilemez, açısal parantezler içerisinde
``enum`` tür parametrelerinin de belirtilmesi gerekir. Örneğin:

.. code-block:: rust

    let f: Fruit<i32>;

Burada ``f`` değişkeni ``Fruit`` türünden değildir, ``Fruit<i32>`` türündedir. Örneğin:

.. code-block:: rust

    let k: Fruit<f64>;

Burada ``k`` değişkeni ``Fruit<f64>`` türündendir.

Rust derleyicisi generic bir türün kullanıldığını gördüğünde generic parametreleri yerlerine yerleştirerek gerçek
türlerle gerçek ``enum`` türünü oluşturur. Yukarıda da belirttiğimiz gibi bu sürece Rust'ta *monomorfizasyon*
denilmektedir.

Generic enum Varyantlarının Oluşturulması
-----------------------------------------

Peki generic bir ``enum`` türünden ``enum`` varyantı nasıl oluşturulmaktadır? İşte generic ``enum`` türlerine ilişkin 
varyantları yapıcılarla (constructors) iki biçimde oluşturulabilmektedir:

1) Generic tür parametrelerinin açıkça (explicit) belirtilmesi yoluyla
2) Derleyicinin içinde bulunulan bağlamdan hareketle generic parametrelerin türlerini kendisinin tespit etmesi
   (implicit) yoluyla

Varyant oluştururken generic parametreler açıkça aşağıdaki biçimde belirtilmektedir:

.. code-block:: text

    <generic_enum_ismi>::<<tür_parametre_listesi>>::<varyant_ismi>...

Örneğin:

.. code-block:: rust

    let f: Fruit<i32>;

    f = Fruit::<i32>::Banana(10, 20);

Burada varyantın belirtilme biçimi size tuhaf gelebilir. Sanki bunun aşağıdaki gibi olması gerektiğini
düşünebilirsiniz:

.. code-block:: rust

    f = Fruit<i32>::Banana(10, 20);

Özellikle C++, Java ve C# gibi dillerden geçen kişiler Rust'taki bu sentaksı yadırgamaktadır. Bu dillerden geçen
programcılar açıkça tür belirtirken ``Fruit<i32>`` sentaksı kullanıldığı halde varyant oluşturulurken
``Fruit::<i32>::Banana`` sentaksının kullanılmasını bir çelişki olarak yorumlamaktadır. Bu düşünce kısmen doğru olsa
da Rust kendine özgü bir programlama dilidir. Rust'ta varyant oluştururken yanlışlıkla ``Fruit<i32>::Banana`` gibi
bir sentaksı kullanırsanız burada açısal parantezler operatör gibi ele alınacak bu da error oluşturacaktır.

Ancak Rust'ta generic varyantlar oluşturulurken çok nadir açıkça tür belirtmeye gereksinim duyulmaktadır. Genellikle
tür çıkarımı *bağlama (context)* dayalı bir biçimde derleyici tarafından otomatik biçimde yapılabilmektedir. Örneğin
derleyici eğer hedef değişkenin türü belliyse ``enum`` parametrelerinin türünü de kendisi otomatik olarak
belirleyebilmektedir. Örneğin:

.. code-block:: rust

    let f: Fruit<i32>;

    f = Fruit::Banana(10, 20);

Burada Rust derleyicisi hedef tür ``Fruit<i32>`` olduğu için ``Banana`` varyantının da ``Fruit<i32>`` türünün bir 
``enum`` türünün varyantı olduğunu anlayabilmektedir. Tabii aşağıdaki varyant oluşturma işlemi error'le sonuçlanacaktır:

.. code-block:: rust

    let f: Fruit<f64>;

    f = Fruit::Banana(10, 20);          // error!

Çünkü derleyici burada hedef türün ``Fruit<f64>`` olduğunu gördüğü için ``Banana`` varyantının da ``Fruit<f64>``
türüne ilişkin olması gerektiğini düşünecektir. Ancak ``Fruit<f64>`` türünün ``Banana`` varyantının alanları ``f64``
türündendir. Oysa ``10`` ve ``20`` ``i32`` türündendir.

===========================================
Generic enum Türleri (Devam) ve Option Türü
===========================================

Generic enum Türleri (Devam)
============================

Tür Çıkarımı
------------

Yukarıda da belirttiğimiz gibi Rust derleyicisi eğer bağlamdan hareketle generic parametrelerin hangi gerçek türlere
karşı geldiğini çıkarsayabiliyorsa programcının tür parametrelerini açıkça belirtmesine gerek kalmamaktadır. Örneğin:

.. code-block:: rust

    fn main() {
        foo(Fruit::Banana(10, 20));
    }

    fn foo(f: Fruit<i32>) {
        match f {
            Fruit::Apple => println!("Apple"),
            Fruit::Banana(a, b) => println!("Banana ({}, {})", a, b),
            Fruit::Orange(orange) => println!("Orange ({})", orange),
        }
    }

    enum Fruit<T> {
        Apple,
        Banana(T, T),
        Orange(T)
    }

Burada ``foo`` fonksiyonunun parametre değişkeni ``Fruit<i32>`` türündendir. Fonksiyonu çağırırken ``enum`` varyantında
generic türün belirtilmesine gerek kalmaz. Çünkü derleyici bu hedef değişkene bakarak tür tespitini kendisi
yapabilecektir. Fonksiyonun çağrımına dikkat ediniz:

.. code-block:: rust

    foo(Fruit::Banana(10, 20));

Tabii biz generic türler için açıkça tür parametresini belirterek de fonksiyonu aşağıdaki gibi çağırabilirdik:

.. code-block:: rust

    foo(Fruit::<i32>::Banana(10, 20));

Şimdi de ``foo`` içerisindeki ``match`` ifadesine dikkat ediniz:

.. code-block:: rust

    fn foo(f: Fruit<i32>) {
        match f {
            Fruit::Apple => println!("Apple"),
            Fruit::Banana(a, b) => println!("Banana ({}, {})", a, b),
            Fruit::Orange(orange) => println!("Orange ({})", orange),
        }
    }

Burada ``match`` ifadesi ``Fruit<i32>`` türündendir. O halde ``match`` kollarının da bu türden olması gerekir. Bu
nedenle programcının ``match`` kollarında açıkça tür belirtmesine gerek yoktur. Tabii programcı açıkça tür belirtirse
de bir sorun oluşmayacaktır. Örneğin:

.. code-block:: rust

    fn foo(f: Fruit<i32>) {
        match f {
            Fruit::<i32>::Apple => println!("Apple"),
            Fruit::<i32>::Banana(a, b) => println!("Banana ({}, {})", a, b),
            Fruit::<i32>::Orange(orange) => println!("Orange ({})", orange),
        }
    }

Şimdi de aşağıdaki örneğe dikkat ediniz:

.. code-block:: rust

    let f = Fruit::Banana(10, 20);

Burada derleyici hedef türü bilmediğine göre tür çıkarımını yapabilir mi? İşte bu tür durumlarda derleyici ``enum``
varyantı için kullanılan alanların türlerinden hareketle generic parametrelerin türünü tespit edebilmektedir.
Örneğimizde ``Banana`` varyantı oluşturulurken yapıcı fonksiyona (constructor) girilen argümanlar ``i32`` türünden
olduğu için derleyici de buradaki türü ``Fruit<i32>`` biçiminde çıkarsayacaktır. Tabii böyle bir çıkarsamanın mümkün
olması için yapıcı fonksiyondaki argümanların da tutarlı olması gerekir. Örneğin:

.. code-block:: rust

    let f = Fruit::Banana(10, 3.54);        // error!

Burada ``Banana`` yapıcı fonksiyonunun argümanları farklı türlerdendir. Dolayısıyla tür çıkarımı yapılamayacaktır.

Biz yukarıda *eğer mümkünse derleyici generic parametrelerin türleri için tür çıkarımı yapabilmektedir* dedik. Tabii bu
sürecin biçimsel bir ifadesi vardır. Biz burada yalnızca sezgisel açıklamayla yetineceğiz. Biçimsel açıklamaları
*generic* konusunu ele aldığımız bölümde yapacağız.

Tabii bazen gerçekten (seyrek de olsa) programcının generic parametreleri açıkça (explicitly) belirtmesi gerekebilir.

.. code-block:: rust

    enum Fruit<T> {
        Apple,
        Banana(T, T),
        Orange(T)
    }

    let f = Fruit::Apple;      // error!

``Apple`` varyantının birimsel (unit) bir varyant olduğuna dikkat ediniz. Bu durumda Rust derleyicisi generic tür
çıkarımını yapamayacaktır. Dolayısıyla bağlama işlemi error ile sonuçlanacaktır. Şimdi siz *nasıl olsa Apple varyantı
T türünden bir bilgi barındırmıyor, bu durumda T türünün önemi olmadığına göre bağlamanın geçerli olması gerektiğini*
düşünebilirsiniz. Ancak programın başka bir yerinde bu ``f`` değişkenine bir şey atanabileceği (tabii değişkenin
``mut`` olması gerekir) için derleyicinin tür çıkarımını yapmış olması gerekir. İşte böylesi durumlarda ``enum`` türü
açıkça belirtilmek zorundadır. Örneğin:

.. code-block:: rust

    let f = Fruit::<i32>::Apple;

Generic enum Türlerinde Metotlar
--------------------------------

``enum`` türlerinin metotlara sahip olabileceğini belirtmiştik. İşte generic ``enum`` türleri de metotlara sahip
olabilmektedir. Generic türlere ilişkin metotlar yazılırken hem ``impl`` anahtar sözcüğünden sonra hem de ``enum``
isminden sonra generic parametrelerin açısal parantezler içerisinde belirtilmesi gerekir. Örneğin:

.. code-block:: rust

    enum Fruit<T> {
        Apple,
        Banana(T, T),
        Orange(T)
    }

    impl<T> Fruit<T> {
        fn disp(&self) {
            match &self {
                Fruit::Apple => println!("Apple"),
                Fruit::Banana(a, b) => println!("Banana"),
                Fruit::Orange(a) => println!("Orange"),
            }
        }
    }

Burada ``disp`` generic ``enum`` türünün bir metodudur. Bu metot içerisinde biz ``T`` türünü kullanabiliriz. Tabii
metotlar konusu, generic türler konusu aslında ayrı bir bölümde ayrıntılarıyla ele alınacaktır. Biz yalnızca burada
generic türlerin de metotlara sahip olabileceğine dikkatinizi çekmek istiyoruz.

.. code-block:: rust

    fn main() {
        let f = Fruit::Banana(10, 20);

        f.disp();
    }

    enum Fruit<T> {
        Apple,
        Banana(T, T),
        Orange(T)
    }

    impl<T> Fruit<T> {
        fn disp(&self) {
            match &self {
                Fruit::Apple => println!("Apple"),
                Fruit::Banana(a, b) => println!("Banana"),
                Fruit::Orange(a) => println!("Orange"),
            }
        }
    }

Option Türü
===========

Option Türünün Tanımı
---------------------

Rust'ın standart kütüphanesinde ``std::option`` modülünde bulunan ``Option`` isimli bir ``enum`` türü sıkça
kullanılmaktadır. Biz de bu bölümde bu ``Option`` ``enum`` türünü ele alacağız. ``Option`` ``enum`` türü generic
biçimdedir. Türün bir tane generic parametresi vardır. ``Option`` ``enum`` türü şöyle tanımlanmıştır:

.. code-block:: rust

    pub enum Option<T> {
        Some(T),
        None
    }

Görüldüğü gibi ``Option`` türünün ``Some`` ve ``None`` isimli iki varyantı vardır. ``Some`` varyantı demetsel bir
varyanttır ve ``T`` türünden bir alana (field) sahiptir. ``None`` varyantı ise birimsel bir varyanttır. Ek bilgi
tutmamaktadır. Her ne kadar ``Option`` türü ``std::option`` modülünde bulunuyor olsa da standart prelude içerisinde bu
tür için ``use`` işlemi yapılmıştır. Bu sayede ``Option`` tür ismi ve ``Option`` içerisindeki ``Some`` ve ``None``
varyantları ``::`` operatörü ile hiç niteliklendirme yapılmadan da kullanılabilir. Örneğin:

.. code-block:: rust

    let ops = std::option::Option::<i32>::Some(123);

Burada tam niteliklendirme yapılarak ``Option`` ``enum``'unun ``Some`` varyantına ilişkin bir değer oluşturulmuştur.
Generic parametre açıkça belirtilmeden de aynı işlem yapılabilirdi:

.. code-block:: rust

    let ops = std::option::Option::Some(123);

Tabii aslında prelude sayesinde ``std::option`` niteliklendirmesini kullanmayabiliriz. Örneğin:

.. code-block:: rust

    let ops = Option::Some(123);

Örneğin:

.. code-block:: rust

    let t = Option::<i32>::Some(10);

Yine prelude sayesinde ``Option`` ismiyle niteliklendirmeyi de kaldırabiliriz. Örneğin:

.. code-block:: rust

    let ops = Some(123);

Burada ``ops`` değişkeni ``Option<i32>`` türündendir.

Anımsanacağı gibi ``None`` varyantı kullanılıyor olsa bile yine generic parametrenin türü belirlenebilir olmalıdır.
Örneğin aşağıdaki bağlama error ile sonuçlanacaktır. Çünkü burada ``None`` varyantının hangi generic parametreye sahip
``Option`` türüne ilişkin olduğu bilinmemektedir.

.. code-block:: rust

    let ops = None;     // error!

Tabii daha önceden de belirttiğimiz gibi bağlamada kalıpta belirtilen hedef tür dikkate alınarak otomatik tür çıkarımı
yapılabilmektedir. Örneğin:

.. code-block:: rust

    let ops: Option<f64> = None;

Burada ``None`` varyantının ``Option<f64>`` türünün ``None`` varyantı olduğu anlaşılmaktadır.

Tabii ``Option`` ``enum`` türünün tür parametresi bir yapı türünden de olabilir. Örneğin:

.. code-block:: rust

    let ops: Option<String> = Some(String::from("this is a message"));

Örneğin:

.. code-block:: rust

    let ops: Option<&str> = Some("this is a message");

``Option`` ``enum``'undaki tür parametresi bir demet türünden de olabilir. Örneğin:

.. code-block:: rust

    let ops: Option<(f64, f64)> = Some((2.4, 6.5));

Burada ``Option`` tür parametresi ``(f64, f64)`` türündendir. Tabii aslında biz yukarıdaki bağlamayı daha az tuşa
basarak aşağıdaki gibi de yapabilirdik:

.. code-block:: rust

    let ops = Some((2.4, 6.5));

Burada özellikle Python programcıları için bir uyarıda bulunmak istiyoruz. Python'da ``None`` bir anahtar sözcüktür.
Rust'ta ``None`` sıradan bir isimdir. Aşağıdaki gibi bir kod gördüğünüzde ``None`` ismini bir anahtar sözcük
sanmayınız:

.. code-block:: rust

    let ops: Option<i32>;
    //...
    ops = None;

Option ile Başarı Durumunu İfade Etme
-------------------------------------

``Option`` türü önceki örneklerimizden de anlayacağınız gibi *hem bir değeri hem de başarısızlık durumunu* ifade etmek
için kullanılmaktadır. Aşağıdaki gibi bir fonksiyon olsun:

.. code-block:: rust

    fn foo() -> Option<i32> {
        //...
    }

Bu fonksiyon başarı durumunda ``Some(i32)`` varyantına ilişkin bir değer, başarısızlık durumunda ``None`` varyantına
ilişkin bir değer vermektedir. Yani ``Option`` türü bir birlik organizasyonu içerisinde *ya başarı durumundaki değeri
ya da başarısızlık bilgisini* ifade etmek için kullanılmaktadır. Örneğin C'de ``foo`` isimli bir fonksiyon başarı
durumunda herhangi bir ``int`` değeri bize verebiliyor olsun. Ancak fonksiyon başarısız da olabilsin. Biz bu
fonksiyonu şöyle tanımlayamayız:

.. code-block:: c

    int foo(void)
    {
        //...
    }

Burada fonksiyon başarısız olduğunda verebileceği bir ``int`` değer yoktur. Çünkü fonksiyon başarı durumunda herhangi
bir ``int`` değer verebilmektedir. Biz C'de bu tür durumlarda genellikle fonksiyonun geri dönüş değerini
*başarı-başarısızlık* için kullanırız. Fonksiyonun da geri dönüş değerini parametre yoluyla iletmesini sağlarız.
Örneğin:

.. code-block:: c

    bool foo(int *val)
    {
        //...
    }

Burada programcı önce fonksiyonun geri dönüş değerine bakar. Ancak geri dönüş değeri ``true`` ise parametre olarak
verdiği nesneye başvurur. İşte böylesi durumlarda Rust'ta ``Option`` ``enum`` türü kullanılmaktadır:

.. code-block:: rust

    fn foo() -> Option<i32> {
        //...
    }

Burada ``foo`` başarılıysa ``Some(i32)`` varyantına ilişkin bir değere, başarısızsa ``None`` varyantına ilişkin bir
değere geri dönecektir. Örneğin:

.. code-block:: rust

    fn mysqrt(x: f64) -> Option<f64> {
        if x < 0.0 {
            return None;
        }
        else {
            Some(x.sqrt())
        }
    }

Burada eğer ``mysqrt`` fonksiyonunun parametresi negatif ise fonksiyon başarısızlıkla yani ``Option::<f64>::None``
varyantı ile geri dönmektedir. Eğer parametre negatif değilse ``Option::<f64>::Some`` varyantıyla geri dönmektedir.

Option Değerinin İncelenmesi: match ve if let
---------------------------------------------

Peki elimizde ``Option`` türünden bir değişken ya da değer olsun. Biz bu değişken ya da değerin içerisindeki varyantın
ne olduğunu nasıl anlayabiliriz? İşte Rust'ta bunun için iki yol vardır:

1) ``match`` ya da ``if let`` deyimi ile
2) ``Option`` ``enum`` türünün metotları ile

Elimizde bir ``Option`` türünden değişken ya da değer varsa biz ``match`` deyimi ile hem başarısızlığı anlayabilir hem
de başarı durumunda ``Some`` varyantının alan değerini elde edebiliriz. Örneğin:

.. code-block:: rust

    val = getval();

    match mysqrt(val) {
        Some(x) => println!("{}", x),
        None => println!("failed to get sqrt of val")
    }

Burada ``mysqrt`` fonksiyonunun geri dönüş değeri ``Option<f64>`` türündendir. ``match`` deyimi ile varyant tespiti
yapılıp ``Some`` varyantının içerisindeki alan değeri elde edilmiştir. Tabii aynı işlem ``if let`` deyimiyle de
yapılabilirdi:

.. code-block:: rust

    val = getval();

    if let Some(x) = mysqrt(val) {
        println!("{}", x);
    }
    else {
        println!("failed to get sqrt of val")
    }

.. code-block:: rust

    fn main() {
        let val: f64;

        val = getval();

        match mysqrt(val) {
            Some(x) => println!("{}", x),
            None => println!("failed to get sqrt of val")
        }
    }

    fn mysqrt(x: f64) -> Option<f64> {
        if x < 0.0 {
            return None;
        }
        else {
            Some(x.sqrt())
        }
    }

    fn getval() -> f64 {
        let mut buf: String = String::new();

        std::io::stdin().read_line(&mut buf).expect("read line failed");
        buf.trim().parse().expect("parse into number")
    }

Örnek: get_roots Fonksiyonu
---------------------------

Şimdi daha önce yapmış olduğumuz ikinci derece denklemin köklerine geri dönen fonksiyonu ``Option`` ``enum``'unu
kullanarak yeniden yazalım:

.. code-block:: rust

    fn get_roots(a: f64, b: f64, c: f64) -> Option<(f64, f64)> {
        let delta: f64;

        delta = b * b - 4.0 * a * c;
        if delta < 0.0 {
            None
        }
        else {
            let x1 = (-b + delta.sqrt()) / (2. * a);
            let x2 =  (-b - delta.sqrt()) / (2. * a);
            Some((x1, x2))
        }
    }

Burada ``get_roots`` fonksiyonunun geri dönüş değeri ``Option<(f64, f64)>`` türündendir. ``Option`` türünün
``(f64, f64)`` monomorfizasyonu aşağıdaki gibidir:

.. code-block:: rust

    enum Option {
        Some((f64, f64)),
        None
    }

İşte biz yine fonksiyonu çağırıp geri dönüş değerini ``match`` deyimi içerisine sokarak varyantın ``Some`` olup
olmadığını tespit edebiliriz:

.. code-block:: rust

    result = get_roots(1., 0., -4.);

    match result {
        Some((x1, x2)) => println!("x1: {}, x2: {}", x1, x2),
        None => println!("No result"),
    }

Burada ilk ``match`` koluna dikkat ediniz. ``Some`` varyantının alanı da bir demet olduğu için açım (destructuring)
doğrudan yapılmıştır. Burada ``x1`` ve ``x2``'nin ``f64`` türünden olduğuna dikkat ediniz.

Tabii ``Some`` kalıbı aşağıdaki gibi de düzenlenebilirdi:

.. code-block:: rust

    match result {
        Some(t) => println!("x1: {}, x2: {}", t.0, t.1),
        None => println!("No result"),
    }

Burada artık ``t`` değişkeni ``(f64, f64)`` türünden bir demettir. Benzer biçimde bu işlemi ``if let`` deyimiyle de
yapabilirdik:

.. code-block:: rust

    if let Some(t) = result {
        println!("x1: {}, x2: {}", t.0, t.1)
    }
    else {
        println!("No results found!")
    }

.. code-block:: rust

    fn main() {
        let result;

        result = get_roots(1., 0., -4.);
        if let Some(t) = result {
           println!("x1: {}, x2: {}", t.0, t.1)
        }
        else {
           println!("No results found!")
        }
    }

    fn get_roots(a: f64, b: f64, c: f64) -> Option<(f64, f64)> {
        let delta: f64;

        delta = b * b - 4.0 * a * c;
        if delta < 0.0 {
            None
        }
        else {
            let x1 = (-b + delta.sqrt()) / (2. * a);
            let x2 =  (-b - delta.sqrt()) / (2. * a);
            Some((x1, x2))
        }
    }

Farklı Option Türlerinin Atanması
---------------------------------

``Option<T>`` türünden bir değişkene ``Option<K>`` türünden bir değer atanamaz. Örneğin:

.. code-block:: rust

    let x: Option<i32>;
    let y: Option<i64> = Some(20);;

    x = y;      // error!

Burada ``Option<i32>`` türüne ``Option<i64>`` atanmak istenmiştir. Bu iki ``Option`` türü farklı türlerdir.
Aşağıdaki atama da aynı gerekçeyle error oluşturacaktır:

.. code-block:: rust

    let x: Option<i32>;
    let y: Option<i64> = None;

    x = y;      // error!

Option Türünün Metotları
------------------------

``Option`` türünden bir değişken ya da değerin varyantları hakkında bilgi edinebilmek için ``Option`` türünün
metotlarından da faydalanılabilmektedir. ``Option`` türünün pek çok metodu vardır. Burada ``Option`` türünün önemli
metotları üzerinde duracağız.

is_some ve is_none
~~~~~~~~~~~~~~~~~~

``Option`` ``enum``'unun ``is_some`` ve ``is_none`` metotları parametre almaz. İlgili ``enum`` değişkeninin ya da 
değerinin içerisindeki varyantın ``Some`` ya da ``None`` olup olmadığını belirten ``bool`` türden bir değerle geri
dönmektedir. Metotların parametrik yapıları şöyledir:

.. code-block:: rust

    pub const fn is_some(&self) -> bool
    pub const fn is_none(&self) -> bool

Metotların ilk parametrelerinin ``&self`` ya da ``self`` biçiminde olduğunu belirtmiştik. Bu parametre metodun
çağrıldığı değişkeni ya da değeri belirtiyordu. Burada bir noktaya dikkat ediniz. ``is_some`` metodu bize ``Some``
varyantındaki alanın değerini vermemektedir. Yalnızca ``enum``'un ``Some`` varyantını içerip içermediği bilgisini
vermektedir. Örneğin:

.. code-block:: rust

    result = get_roots(1., 0., -4.);

    if result.is_some() {
        println!("roots found")
    }
    else {
        println!("root not found")
    }

Burada biz kökün olup olmadığını anlayabildik. Kök varsa bunun değerlerini elde edemedik. Tabii aynı şey aslında
``is_none`` metoduyla da yapılabilirdi:

.. code-block:: rust

    result = get_roots(1., 0., -4.);

    if result.is_none() {
        println!("root not found")
    }
    else {
        println!("roots found")
    }

unwrap ve expect
~~~~~~~~~~~~~~~~

``Option`` ``enum``'unun en çok kullanılan metotlarından ikisi ``unwrap`` ve ``expect`` metotlarıdır. ``unwrap`` metodu
eğer ``enum`` içerisindeki varyant ``Some`` ise bize o ``Some`` içerisindeki alan değerini verir, ``None`` ise ``panic``
oluşturur. ``expect`` metodu da çok benzerdir. ``unwrap`` metodundan farkı ``panic`` oluştururken ``panic``
mesajının belirlenmesine olanak sağlamasıdır. Metotların parametrik yapıları şöyledir:

.. code-block:: rust

    pub const fn unwrap(self) -> T
    pub const fn expect(self, msg: &str) -> T

``unwrap`` metodunun parametre almadığına ``expect`` metodunun ise string dilim referansı parametresi aldığına dikkat
ediniz. Her iki metot da eğer ``enum``'da ``Some`` varyantı varsa onun alan değerine geri dönmektedir. Örneğin:

.. code-block:: rust

    let roots: (f64, f64);

    roots = get_roots(1., 0., -4.).unwrap();
    println!("x1 = {}, x2 = {}", roots.0, roots.1);

Burada ``get_roots`` fonksiyonunun geri dönüş değerinin ``Option<(f64, f64)>`` türünden olduğuna dikkat ediniz. Bu
değer hemen ``unwrap`` edilmiştir. Zaten kök yoksa ``panic`` ile program sonlandırılacaktır. Programın devam etmesi
için kök olması gerekir. ``unwrap`` metodunun başarı durumunda bize ``(f64, f64)`` türünden bir demet verdiğine dikkat
ediniz. Tabii fonksiyonun geri dönüş değeri ``(f64, f64)`` türünden olduğuna göre biz ``let`` deyimi ile açım
(destructuring) da yapabiliriz:

.. code-block:: rust

    let (x1, x2) =  get_roots(1., 0., -4.).unwrap();
    println!("x1 = {}, x2 = {}", x1, x2);

``let`` deyiminde kalıbın *irrefutable* olması gerektiğini anımsayınız. Buradaki kalıp irrefutable biçimdedir.

Yukarıda da belirttiğimiz gibi ``expect`` metodunun ``unwrap`` metodundan farkı ``panic`` durumunda ``panic``
mesajının da belirlenmesine olanak sağlamasıdır. Örneğin:

.. code-block:: rust

    let roots: (f64, f64);

    roots = get_roots(1., 0., 4.).expect("root not found");

unwrap_or
~~~~~~~~~

``Option`` ``enum`` türünün ``unwrap_or`` metodu eğer ``enum`` içerisinde ``None`` değeri varsa ``panic`` yerine metodun
bizim belirlediğimiz değerle geri döndürülmesini sağlamaktadır. Metodun parametrik yapısı şöyledir:

.. code-block:: rust

    pub fn unwrap_or(self, default: T) -> T

Örneğin:

.. code-block:: rust

    let val: f64;
    let result: f64;

    val = getval();
    result = mysqrt(val).unwrap_or(-1.0);

Burada ``unwrap_or`` metodu *eğer enum'da Some varyantı varsa onun alan değerini, eğer None varyantı varsa -1.0
değerini* vermektedir. Yani hiçbir zaman ``panic`` oluşmayacaktır.

.. code-block:: rust

    fn main() {
        let val: f64;
        let result: f64;

        val = getval();

        result = mysqrt(val).unwrap_or(-1.);
        println!("{}", result);
    }

    fn mysqrt(x: f64) -> Option<f64> {
        if x < 0.0 {
            return None;
        }
        else {
            Some(x.sqrt())
        }
    }

    fn getval() -> f64 {
        let mut buf: String = String::new();

        std::io::stdin().read_line(&mut buf).expect("read line failed");
        buf.trim().parse().expect("parse into number")
    }

``get_roots`` fonksiyonunun geri dönüş değerinin ``Option<(f64, f64)>`` türünden olduğunu anımsayınız. Eğer bu enum
için ``unwrap_or`` kullanacaksanız metoda argüman olarak ``(f64, f64)`` türünden bir demet vermelisiniz. Örneğin:

.. code-block:: rust

    let result: (f64, f64);

    result = get_roots(1., 0., 4.).unwrap_or((0., 0.));
    println!("{}, {}", result.0, result.1);

.. code-block:: rust

    fn main() {
        let result: (f64, f64);

        result = get_roots(1., 0., 4.).unwrap_or((0., 0.));
        println!("{}, {}", result.0, result.1);
    }

    fn get_roots(a: f64, b: f64, c: f64) -> Option<(f64, f64)> {
        let delta: f64;

        delta = b * b - 4.0 * a * c;
        if delta < 0.0 {
            None
        }
        else {
            let x1 = (-b + delta.sqrt()) / (2. * a);
            let x2 =  (-b - delta.sqrt()) / (2. * a);
            Some((x1, x2))
        }
    }

unwrap_or_default
~~~~~~~~~~~~~~~~~

``Option`` ``enum`` türünün ``unwrap_or_default`` metodu ek bir argüman almaz. Enum'daki varyant ``Some`` ise ``Some``
içerisindeki alanın değeri ile ``None`` ise ``T`` türünün default değeri ile geri dönmektedir. Metodun parametrik
yapısı şöyledir:

.. code-block:: rust

    pub fn unwrap_or_default(self) -> T

Nümerik türlerin default değerleri ``0``'dır. ``bool`` türünün ise ``false`` biçimdedir. Örneğin:

.. code-block:: rust

    val = getval();
    result = mysqrt(val).unwrap_or_default();

Buradan biz ya ``val`` değerinin karekökünü ya da ``f64`` türünün default değeri olan ``0``'ı elde ederiz. Örneğin:

.. code-block:: rust

    let ops = Option::<bool>::None;
    let result: bool;

    result = ops.unwrap_or_default();
    println!("{}", result);             // false

Demetlerin default değerleri elemanların default değerlerinden oluşmaktadır. Örneğin ``(i32, i32)`` demet türünün
default değeri ``(0, 0)`` biçimindedir:

.. code-block:: rust

    let ops = Option::<(i32, i32)>::None;
    let result: (i32, i32);

    result = ops.unwrap_or_default();
    println!("{:?}", result);           // (0, 0)

Tabii her türün bir default değeri olmak zorunda değildir. Programcı kendi türleri için default değer oluşturmak
istiyorsa ``Default`` isimli trait'i desteklemesi gerekir. Bu konu ileride trait'lerin anlatıldığı bölümde ele
alınacaktır.

as_ref
~~~~~~

``Option`` ``enum`` türünün ``as_ref`` isimli metodu argümansız olarak kullanılmaktadır. Metodun parametrik yapısı
şöyledir:

.. code-block:: rust

    pub const fn as_ref(&self) -> Option<&T>

Bu metot ``Option<T>`` türünden bir değişken ya da değere uygulanırsa ``Option<&T>`` türünden bir değer elde
edilmektedir. Örneğin elimizde şöyle bir ``Option<i32>`` değişkeni olsun:

.. code-block:: rust

    let ops1 = Option::<i32>::Some(123);
    let ops2: Option<&i32>;

    let ops2 = ops1.as_ref();

Peki ``Option<T>`` değerinden ``Option<&T>`` değerinin oluşturulmasının ne anlamı olabilir? İşte bazen ``Option``
içerisinde tutulan nesnenin kendisini değil de onun adresini elde etmek isteyebiliriz. Böylece değeri taşımadan ödünç
almış oluruz. Örneğin:

.. code-block:: rust

    let opt = Some(String::from("ankara"));

    if let Some(s) = opt {
        println!("{}", s);
    }
    else {
        println!("None")
    }

    let s = opt.unwrap();       // error!

Burada biz ``opt`` içerisindeki değer kalıp uyuşumu ile elde ettik. Ancak bu değer ``String`` türünden olduğu için ve
``String`` türü de ``Copy`` türünden olmadığı için ``opt`` içerisindeki değer taşınmıştır. Dolayısıyla ``if let``
deyiminden sonra artık biz ``opt`` değişkenini kullanamayız. İşte hem ``opt`` içerisindeki değeri taşımadan elde
etmek için ``as_ref`` metodu kullanılmaktadır. Örneğin:

.. code-block:: rust

    let opt = Some(String::from("ankara"));

    if let Some(s) = opt.as_ref() {
        println!("{}", *s);
    }
    else {
        println!("None")
    }

    let s = opt.unwrap();       // geçerli

Burada biz ``if let`` ile artık ``opt`` içerisindeki değerin adresini elde etmiş oluyoruz. Çünkü ``as_ref`` metodu
bize ``Option<&String>`` değeri vermektedir.

Her ne kadar biz henüz görmemiş olsak da aynı işlem aslında ``as_ref`` metodu yerine *ref kalıbı* ile de
yapılabilmektedir. ``ref`` kalıbı ``enum`` alanının kendisini değil adresini elde etmekte kullanılmaktadır. Dolayısıyla
yukarıdaki işlemin eşdeğeri şöyle oluşturulabilmektedir:

.. code-block:: rust

    let opt = Some(String::from("ankara"));

    if let Some(ref s) = opt {
        println!("{}", *s);
    }
    else {
        println!("None")
    }

    let s = opt.unwrap();       // geçerli

Burada kalıp olarak ``Some(ref s)`` kullanıldığına dikkat ediniz. Bu kalıp zaten ``Some`` varyantının alanının
adresini elde etmekte kullanılmaktadır.

=============================================
Option Türünün Diğer Metotları ve Result Türü
=============================================

Option Türünün Diğer Metotları
==============================

insert
------

``Option`` ``enum`` türünün ``insert`` isimli metodu ``Option`` türünden ``mut`` bir değişken ya da geçici değerle
(temporary value) çağrılabilir. Bu metot ``Option`` içerisindeki ``Some`` varyantına ``T`` türünden yeni bir alan
değeri yerleştirir. Tabii eğer ``Option`` değişkeninin içerisinde ``None`` varyantı varsa artık bu işlemden sonra
``Some`` varyantı bulunacaktır. Metodun parametrik yapısı şöyledir:

.. code-block:: rust

    pub fn insert(&mut self, value: T) -> &mut T

Metot ``insert`` edilen değerin yerleştirildiği ``Some`` alanın ``mut`` referansıyla geri dönmektedir. Ancak metodun
geri dönüş değerine pek gereksinim duyulmamaktadır. Örneğin:

.. code-block:: rust

    fn main() {
        let mut opt = Some(123);
        let mut result: i32;

        result = opt.unwrap();
        println!("{}", result);     // 123

        opt.insert(500);

        result = opt.unwrap();
        println!("{}", result);     // 500
    }

get_or_insert
-------------

``Option`` ``enum`` türünün ``get_or_insert`` metodu ``insert`` metoduna benzerdir. Ancak bu metot eğer ``enum`` 
değişkeni ya da geçici değeri içerisinde ``Some`` varyantı varsa onun alanın ``mut`` referansıyla geri döner. Ancak 
``None`` varyantı varsa gerçekten ``insert`` işlemi yapar. Metodun parametrik yapısı şöyledir:

.. code-block:: rust

    pub fn get_or_insert(&mut self, value: T) -> &mut T

Örneğin:

.. code-block:: rust

    let mut opt = Option::<i32>::None;
    let mut result: i32;

    opt.get_or_insert(500);

    result = opt.unwrap();
    println!("{}", result);     // 500

    let rval: &mut i32 = opt.get_or_insert(1000);
    println!("{}", rval);           // 500

Burada ``get_or_insert`` metodunun ilk çağrılmasında ``opt`` içerisinde ``None`` varyantı olduğu için ``Some``
varyantı ``insert`` edilmiştir. Ancak ikinci çağrıda artık ``opt`` içerisinde ``Some`` varyantı vardır. Dolayısıyla
bir ``insert`` işlemi yapılamamıştır.

get_or_insert_default
---------------------

``Option`` ``enum`` türünün ``get_or_insert_default`` metodu da ``get_or_insert`` metodu gibidir. Bu metottan farkı 
eğer değişken ya da geçici değer içerisinde ``None`` varyantı varsa default alan değerine sahip ``Some`` varyantı
``insert`` edilmektedir. Metodun parametrik yapısı şöyledir:

.. code-block:: rust

    pub fn get_or_insert_default(&mut self) -> &mut T

take
----

``Option`` ``enum`` türünün ``take`` metodu ``mut`` bir referansla çağrılmak zorundadır. Bu metot ``enum`` içerisindeki
değeri bir ``Option`` değeri olarak taşıma yoluyla geri döndürür. Ancak metodun çağrıldığı değişkene ``None``
varyantını yerleştirir. Metodun parametrik yapısı şöyledir:

.. code-block:: rust

    pub const fn take(&mut self) -> Option<T>

Örneğin:

.. code-block:: rust

    let mut opt = Option::Some(123);
    let opt2: Option<i32>;

    opt2 = opt.take();

Burada artık ``opt`` içerisindeki değer ``Option`` olarak ``opt2``'ye taşınmıştır. Ancak ``opt`` değişkeninin
içerisinde artık ``None`` varyantı vardır. Bunu şöyle test edebilirsiniz:

.. code-block:: rust

    match opt {
        Option::Some(val) => { println!("{}", val); },
        Option::None => { println!("None"); }               // None
    }
    match opt2 {
        Option::Some(val) => { println!("{}", val); },      // 123
        Option::None => { println!("None"); }
    }

replace
-------

``Option`` ``enum`` türünün ``replace`` metodu ``enum`` içerisindeki eski değeri verip ona yeni bir ``Some`` varyantı
yerleştirmektedir. Metodun parametrik yapısı şöyledir:

.. code-block:: rust

    pub const fn replace(&mut self, value: T) -> Option<T>

Örneğin:

.. code-block:: rust

    let mut opt = Some(123);
    let old: Option<i32>;

    old = opt.replace(500);

    println!("{:?}", opt);      // Some(500)
    println!("{:?}", old);      // Some(123)

``Option`` ``enum`` türünün diğer metotları için *Rust Standard Library* dokümanlarına başvurabilirsiniz.

Option Döndüren Fonksiyon Örnekleri
===================================

Dizide Değer Arayan find Fonksiyonu
-----------------------------------

Aşağıda ``i32`` türünden bir dizinin içerisinde bir değer arayan ``find`` isimli fonksiyon örneği verilmiştir.
Fonksiyonun parametrik yapısı şöyledir:

.. code-block:: rust

    fn find(a: &[i32], val: i32) -> Option<usize>

Fonksiyonun birinci parametresi dilim referansını ikinci parametresi aranacak değeri belirtmektedir. Fonksiyon başarı
durumunda değerin bulunduğu dizi indeksine ilişkin ``Some`` varyantı ile başarısızlık durumunda ``None`` varyantı ile
geri dönmektedir. Fonksiyonun çağrımı şöyle yapılabilir:

.. code-block:: rust

    let a = [10, 4, 67, 34, 21];

    if let Some(index) = find(&a, 67) {
        println!("{}", index);      // 2
    }
    else {
        println!("cannot find!");
    }

Aslında dilimlerin de bu işi yapan metotları vardır.

.. code-block:: rust

    fn main() {
        let a = [10, 4, 67, 34, 21];

        if let Some(index) = find(&a, 67) {
            println!("{}", index);      // 2
        }
        else {
            println!("cannot find!");
        }
    }

    fn find(a: &[i32], val: i32) -> Option<usize> {
        for i in 0..a.len() {
            if a[i] == val {
                return Some(i);
            }
        }
        None
    }

str Türünün find ve rfind Metotları
-----------------------------------

String dilimlerinin (yani ``str`` türünün) de ``find`` ve ``rfind`` isimli metotları benzer biçimde
``Option<usize>`` değeri ile geri dönmektedir. Metotların parametrik yapıları şöyledir:

.. code-block:: rust

    pub fn find<P>(&self, pat: P) -> Option<usize>
    pub fn rfind<P>(&self, pat: P) -> Option<usize>

Metotlar yazı içerisinde bir kalıp aramaktadır. Kalıp bir karakter olabilir. Eğer kalıp bulunursa bulunduğu yerin
indeksine ilişkin ``Some`` varyantı ile bulunamazsa ``None`` varyantı ile geri dönmektedir. Örneğin:

.. code-block:: rust

    fn main() {
        let s: &str = "ankara";

        if let Some(index) = s.find('k') {
            println!("{}", &s[index..]);            // kara
        }
        else {
            println!("cannot find!..")
        }
    }

Burada ``index`` değişkeninin ``usize`` türünden olduğuna dikkat ediniz.

Result Türü
===========

Result Türünün Tanımı
---------------------

Rust'ta çok kullanılan standart kütüphanedeki diğer bir ``enum`` türü de ``Result`` isimli ``enum`` türüdür. ``Result``
``enum`` türü ``std::result`` modülünde aşağıdaki gibi tanımlanmıştır:

.. code-block:: rust

    pub enum Result<T, E> {
        Ok(T),
        Err(E),
    }

Görüldüğü gibi bu ``enum`` türü de generic biçimdedir. Ancak iki generic parametresi vardır. Bu ``enum`` türünde 
``Ok`` ve ``Err`` isimli iki demetsel varyant bulunmaktadır. ``Result`` ``enum`` türü başarı ya da başarısızlığı bilgi 
ile aktarmak amacıyla kullanılmaktadır. Yani başarı durumunda da başarısızlık durumunda da programcı bir bilgi elde
etmektedir. Tabii elde edeceği bilgi başarı durumunda başarılı işleme ilişkin bir değer başarısızlık durumunda ise
başarısızlık nedenine ilişkin bir değer olur. Başarısızlık durumunda ``Err`` varyantında tipik olarak bir hata kodu
ya da bir mesaj iletilmektedir. Burada bir noktaya dikkat ediniz. Başarı-başarısızlık ifadesi ``Option`` türüyle de
yapılabilmektedir. Ancak ``Result`` türünde başarısızlığın nedenine ilişkin de bir bilginin iletildiğine dikkat
ediniz. ``Option`` türünün kullanımı daha geneldir. ``Option`` türü yalnızca başarısızlığı belirtmek için değil
*yokluğu*, *hiçliği* belirtmek için de kullanılmaktadır. Oysa ``Result`` türü tamamen başarı-başarısızlık durumunun
iletiminde kullanılmaktadır. O halde bir işlemin başarısız olması durumunda bir bilginin iletilmediği durumlarda
``Option`` kullanımı daha uygundur. Çünkü ``Option`` kullanmak daha basittir. Ancak bir işlemin başarısızlığı
sonucunda başarısızlığın nedeni bir biçimde iletilecekse bu durumda ``Result`` tercih edilmelidir.

``Result`` ``enum`` türünün kendisi ve onun ``Ok`` ve ``Err`` varyantları tıpkı ``Option`` ``enum`` türünde olduğu gibi
prelude içerisinde ``use`` deyimiyle dışarıya aktarılmıştır. Yani biz ``std::result::Result`` ismi yerine doğrudan
``Result`` ismini ``Result::Ok`` ya da ``Result::Err`` yerine de doğrudan ``Ok`` ve ``Err`` isimlerini
kullanabiliriz. Yine ``Result`` türünde de generic parametrelerin türlerinin bir biçimde derleyici tarafından tespit
edilmesi gerekir. Bu açıkça yapılabileceği gibi bağlamdan hareketle de yapılabilmektedir. Örneğin:

.. code-block:: rust

    let r: std::result::Result<i32, &str>;      // geçerli

Burada ``Result`` ismi açıkça modül ismi belirtilerek niteliklendirme yoluyla kullanılmıştır. Ancak niteliklendirme
yapmaya gerek yoktur:

.. code-block:: rust

    let r: Result<i32, &str>;                   // geçerli

``Result`` ``enum`` türünde iki generic parametre vardır. Dolayısıyla derleyicinin bu iki parametreyi tespit etmesi
gerekir. Aşağıdaki bağlamada birinci generic parametrenin türü tespit edilebildiği halde ikinci generic parametrenin
türü tespit edilemediğinden dolayı bağlama error ile sonuçlanacaktır:

.. code-block:: rust

    let r = Result::Ok(123);                  // error!

Aşağıdaki bağlama geçerlidir. Çünkü her iki generic tür de açıkça belirtilmiştir:

.. code-block:: rust

    let r = Result::<i32, &str>::Ok(123);       // geçerli

Tabii bağlam izin veriyorsa generic parametrelerin açıkça belirtilmesine gerek kalmaz. Örneğin:

.. code-block:: rust

    let r: Result<i32, &str>;

    r = Ok(123);   // geçerli

Burada ``r`` değişkeninin türü zaten derleyici tarafından bilinmektedir. Dolayısıyla ``Ok`` varyantı için generic tür
belirtilmesine gerek yoktur. Benzer biçimde örneğin:

.. code-block:: rust

    let r: Result<i32, &str>;

    r = Err("file not found");      // geçerli

Burada da generic türlerin belirtilmesine gerek yoktur.

Option mı, Result mı?
---------------------

Örneğin bir soket için ``accept`` işlemi yapan bir fonksiyon yazmak isteyelim. Bu fonksiyon başarı durumunda
``Socket`` türünden bir yapı değerine, başarısızlık durumunda da hataya ilişkin bir hata yazısına geri dönecek olsun.
Burada ``Option`` yerine ``Result`` türünün kullanılması daha uygundur:

.. code-block:: rust

    fn accept_socket(s: Socket) -> Result<Socket, String>

Eğer bu fonksiyon ``Option`` ile geri dönüyor olsaydı fonksiyonun parametrik yapısı şöyle olurdu:

.. code-block:: rust

    fn accept_socket(s: Socket) -> Option<Socket>

Fakat bu durumda hatanın nedenini anlayamazdık.

Peki ikinci derece denklemin köklerini bulan ``get_roots`` fonksiyonunun geri dönüş değerinin ``Option`` türünden
olması mı yoksa ``Result`` türünden olması mı daha uygundur? İkinci derece denklemin köklerinin olmamasının tek bir
nedeni vardır. O da ``delta < 0`` durumudur. Bu fonksiyonun başarısızlık için ek bir bilgi iletmesine gerek yoktur.
Eğer bu fonksiyon başka nedenlerden dolayı da başarısız olabilseydi bu durumda ``Option`` yerine ``Result`` türünü
tercih ederdik.

Örnek: read_line Metodu
-----------------------

Aslında biz kursumuzun ilk kısımlarında farkında olmasak da ``Result`` ``enum`` türüyle karşılaştık. Klavyeden (stdin
dosyasından) bir yazı okurken ``std::io::stdin()`` fonksiyonu bize ``Stdin`` yapısı türünden bir değer veriyordu. Biz
de bu yapının ``read_line`` metodunu çağırıyorduk. ``Stdin`` yapısının ``read_line`` metodunun parametrik yapısı
şöyledir:

.. code-block:: rust

    pub fn read_line(&self, buf: &mut String) -> Result<usize>

Fonksiyon ``stdin`` türünden bir değişken ya da değerin adresini ve bir de ``mut`` bir ``String`` değişkeninin
adresini almaktadır. Fonksiyon klavyeden okunanları bir ``String`` değişkeninin içerisine yerleştirecektir. Ancak bu
fonksiyon başarısız da olabilmektedir. Fonksiyonun geri dönüş değerindeki ``Result`` ismi türü aslında yukarıda
açıkladığımız ``Result`` ismi değildir. Geri dönüş değerinde belirtilen ``Result`` ismi takma addır (Yani bunu C'deki
``typedef`` ismi gibi düşünebilirsiniz.) Bu takma ad ``std::io`` modülünde aşağıdaki gibi tanımlanmıştır:

.. code-block:: rust

    pub type Result<T> = Result<T, Error>;

Yani aslında geri dönüş değerindeki ``std::io::Result<usize>`` türü ``std::result::Result<usize, std::io::Error>``
anlamına gelmektedir. Dolayısıyla bu metot başarı durumunda ``Ok(usize)`` değeri ile başarısızlık durumunda ``Error``
isimli bir yapı değeri ile geri dönmektedir. Eğer bu metodun geri dönüş değeri kullanılacak olsaydı bu şöyle
yapılabilirdi:

.. code-block:: rust

    let mut s: String = String::new();
    let result: std::result::Result<usize, std::io::Error> = std::io::stdin().read_line(&mut s);

    match result {
        Ok(n) => {
            println!("{} bytes read...", n);
            println!("{}", s);
        },
        Err(e) => println!("Error: {}", e),
    }

Buradaki ``e`` değişkeninin ``std::io::Error`` isimli yapı türünden olduğuna dikkat ediniz. Biz burada bu yapı
üzerinde durmayacağız. Ancak bu yapı ``Display`` ve ``Debug`` trait'lerini desteklediği için doğrudan hatayı yazı
olarak ``println!`` makrosuyla yazdırabiliriz. Tabii klavyeden (yani stdin dosyasından) bir satır okurken genellikle
programcı bu işlemin başarısını kontrol etmez. Çünkü böyle bir işlem sırasında bir IO hatası olması normal bir durum
değildir. Buradan elde edilen byte sayısıyla da genellikle programcı ilgilenmez. Zaten okunan karakterler ``String``
türünden değişken içerisine yerleştirilmektedir.

Peki ``read_line`` metodunun geri dönüş değeri ``Result`` yerine ``Option`` türünden olamaz mıydı? İşte tasarımcı
hata durumunda bir hata bilgisini de dışarıya iletmek istemiştir. Bu nedenle ``Result`` türünü tercih etmiştir.

Farklı Result Türlerinin Atanması
---------------------------------

``Result<T, E>`` türünden bir değer ``Result<K, E>`` türüne atanamamaktadır. Çünkü bunlar farklı türlerdir.
Örneğin:

.. code-block:: rust

    let x: Result<i32, String>;
    let y: Result<i64, String> = Err(String::from("invalid value"));

    x = y;      // error!

Burada türler farklı olduğu için atama geçerli değildir. Benzer biçimde örneğin:

.. code-block:: rust

    let x: Result<i32, String> = Result<i64, String>::Err(String::from("invalid value"));        // error!

============================================
Result Türünün Metotları ve Yapılar (struct)
============================================

Result Türünün Metotları
========================

Nasıl ``Option`` ``enum`` türünün metotları varsa ``Result`` ``enum`` türünün de metotları vardır. Biz de burada bu
``enum`` türünün bazı önemli metotları üzerinde duracağız.

is_ok ve is_err
---------------

``Result`` ``enum`` türünün ``is_ok`` ve ``is_err`` isimli metotları ``enum`` içerisinde ``Ok`` varyantının mı ``Err``
varyantının mı bulunduğunu tespit etmekte kullanılmaktadır. Metotların parametrik yapıları şöyledir:

.. code-block:: rust

    pub const fn is_ok(&self) -> bool
    pub const fn is_err(&self) -> bool

Örneğin:

.. code-block:: rust

    fn main() {
        let result = foo();

        if result.is_ok() {
            println!("Ok")
        }
        else {
            println!("Err")
        }
    }

    fn foo() -> Result<i32, String> {
        //...
        Ok(123)
    }

ok ve err
---------

``Result`` ``enum`` türünün ``ok`` ve ``err`` metotları ``Result`` değerini ``Option`` haline getirip ``Option`` 
``enum`` değeriyle geri dönmektedir. Metotların parametrik yapıları şöyledir:

.. code-block:: rust

    pub fn ok(self) -> Option<T>
    pub fn err(self) -> Option<E>

``ok`` metodu eğer ``Result`` içerisinde ``Ok`` varyantı varsa bu ``Ok`` varyantının tuttuğu alan bilgisini ``Some``
varyantı içeren bir ``Option`` haline getirmektedir. ``Result`` içerisinde ``Err`` varyantı varsa geri döndürülen
``Option`` değeri ``None`` varyantı içerecektir. Örneğin:

.. code-block:: rust

    fn main() {
        let result: Result<i32, String> = foo();
        let opt: Option<i32> = result.ok();

        if let Some(val) = opt {
            println!("{}", val);                // 123
        }
        else {
            println!("There is no result");
        }
    }

    fn foo() -> Result<i32, String> {
        //...
        Ok(123)
    }

Burada ``foo`` fonksiyonu ``Err`` varyantıyla geri dönseydi ``is_ok`` metodunun verdiği ``Option`` değeri ``None``
varyantına sahip olurdu. ``err`` metodu da tersten benzer işlemi yapmaktadır. Yani ``Result`` içerisinde ``Err``
varyantı varsa metot ``Some`` varyantında bu ``Err`` alanındaki bilgi olan bir ``Option`` değeri, ``Ok`` varyantı
varsa ``None`` varyantı içeren bir ``Option`` değeri geri döndürecektir.

Aslında ``Option`` ``enum`` türünün de benzer biçimde ``Option`` değerini ``Result`` türüne dönüştüren ``or_ok`` isimli
metodu da vardır. ``Option`` ``enum`` türünün ``or_ok`` metodu eğer ``Option`` içerisinde ``Some`` değeri varsa bu 
değeri ``Ok`` değeri yaparak bir ``Result`` değeri geri döndürür. Bu metot eğer ``Option`` içerisinde ``None`` varyantı
varsa argüman olarak aldığı bilgiden ``Err`` varyantı içeren bir ``Result`` değeri oluşturup bu ``result`` değerini
geri döndürmektedir. Metodun parametrik yapısı şöyledir:

.. code-block:: rust

    pub fn ok_or_else<E, F>(self, err: F) -> Result<T, E>

Bu metot generic bir metottur. Metodun ``self`` dışındaki parametresi ``Err`` varyantının alanını oluşturmaktadır.
Örneğin:

.. code-block:: rust

    fn main() {
        let opt: Option<i32>;
        let result: Result<i32, &str>;

        opt = foo();
        result = opt.ok_or("Unknown error");

        match result {
            Ok(val) => println!("{}",val),
            Err(val) => println!("{}",val),         // Unknown error
        }
    }

    fn foo() -> Option<i32> {
        //...
        None
    }

Burada ``foo`` fonksiyonu ``None`` varyantıyla geri dönmüştür ``ok_or`` metodu *Unknown error* yazını barındıran
``Err`` varyantına sahip bir ``Result`` değeri verecektir.

as_ref ve as_mut
----------------

Tıpkı ``Option`` ``enum`` türünde olduğu gibi ``Result`` ``enum`` türünde de ``as_ref`` metodu vardır. Bu metot 
sayesinde biz ``Result`` içerisindeki varyantın sahipliğini almak yerine onu ödünç alabiliriz. ``as_ref`` metodunun 
parametrik yapısı şöyledir:

.. code-block:: rust

    pub const fn as_ref(&self) -> Result<&T, &E>

Örneğin:

.. code-block:: rust

    fn main() {
        let result = foo();

        match result.as_ref() {
            Ok(val) => println!("{}", *val),            // 123
            Err(e) => println!("{}", *e),
        }
    }

    fn foo() -> Result<i32, String> {
        //...
        Ok(123)
    }

Burada ``match`` ifadesinin ilk kolundaki ``val`` değişkeni ``i32`` türünden değil ``&i32`` türündendir. Benzer
biçimde ``match`` ifadesinin ikinci kolundaki ``e`` de ``String`` türünden değil ``&String`` türündendir. Aslında
daha önceden de belirttiğimiz gibi aynı etki ``ref`` kalıbıyla da sağlanabilmektedir. Örneğin:

.. code-block:: rust

    fn main() {
        let result = foo();

        match result {
            Ok(ref val) => println!("{}", *val),
            Err(ref e) => println!("{}", *e),
        }
    }

    fn foo() -> Result<i32, String> {
        //...
        Ok(123)
    }

Burada da yine ``val`` değişkeni ``&i32`` türünden ``e`` değişkeni de ``&String`` türündendir.

``as_mut`` metodu da ``as_ref`` ile aynı şeyi yapmaktadır. Ancak bu metot ``mut`` bir referans ile çalışır ve ``mut``
referanslardan oluşan bir ``Result`` nesnesi verir. Metodun parametrik yapısı şöyledir:

.. code-block:: rust

    pub const fn as_mut(&mut self) -> Result<&mut T, &mut E>

unwrap, expect, unwrap_or ve unwrap_or_default
----------------------------------------------

``Result`` ``enum`` türünün de ``unwrap`` ve ``expect`` metotları vardır. ``unwrap`` metodu eğer ``Result`` içerisinde
``Ok`` varyantı varsa onun alan değerine geri döner, ``Err`` varyantı varsa ``panic`` oluşturur. ``expect`` metodu da
aynı şeyi yapmaktadır ancak bu metotta ``panic`` mesajı belirtilebilmektedir. ``expect`` metoduna verilen değer
``panic`` mesajında aşağıdaki biçimde görüntülenmektedir:

.. code-block:: text

    mesaj: <Err varyantındaki değer>

Metotların parametrik yapıları şöyledir:

.. code-block:: rust

    pub fn expect(self, msg: &str) -> T
    pub fn unwrap(self) -> T

Örneğin:

.. code-block:: rust

    fn main() {
        let val: i32;

        val = foo().expect("fail");
        println!("{}", val);
    }

    fn foo() -> Result<i32, String> {
        //...
        Ok(123)
    }

Eğer hata oluşma olasılığı çok düşükse hatayı ``match`` ile ele almak yerine doğrudan ``unwrap`` ya da ``expect``
metotlarını kullanmayı tercih edebilirsiniz.

``unwrap`` metodunun da tıpkı ``Option`` ``enum`` türünde olduğu gibi ``unwrap_or`` ve ``unwrap_or_default`` isminde
benzerleri de vardır:

.. code-block:: rust

    pub fn unwrap_or(self, default: T) -> T
    pub fn unwrap_or_default(self) -> T

``unwrap_or`` metodu eğer ``Result`` içerisinde ``Ok`` varyantı varsa onun alan değeri ile ``Err`` varyantı varsa
metotta belirtilen değerle geri dönmektedir. ``unwrap_or_default`` metodu ise ``Err`` varyantı için ``T`` türünün
default değeriyle geri dönmektedir.

``Result`` ``enum`` türünün diğer metotları için *Rust Standard Library* dokümanlarına başvurabilirsiniz:

https://doc.rust-lang.org/std/result/enum.Result.html

Result<(), E> Kullanımı
-----------------------

Bazen başarı durumunda bir bilginin iletilmesi gerekmez ancak başarısızlık durumunda gerekebilir. Örneğin biz bir
dosyayı ``flush`` edecek olalım. Bu işlemi başardığımızda bize verilecek ek bir bilgi yoktur. Ancak bu işi
başaramadığımızda neden başaramadığımıza ilişkin bir bilgi elde etmek isteriz. İşte bu tür durumlarda genellikle Rust
programcıları ``Result`` ``enum`` türünün ``Ok`` varyantına ilişkin generic parametreyi ``()`` ile birim türü biçiminde
belirtmektedir. Örneğin:

.. code-block:: rust

    fn foo() -> Result<(), String>

Burada ``foo`` başarı durumunda bize bir bilgi vermemektedir. Ancak başarısızlık durumunda bir hata mesajı
vermektedir. Örneğin:

.. code-block:: rust

    use std::process::exit;

    fn main() {
        if let Err(e) = foo() {
            println!("{}", e);
            exit(1);
        }
        println!("everything is ok");
    }

    fn foo() -> Result<(), String> {
        //...
        Err(String::from("Unknown error"))
    }

Örneğin daha önce de biz ekrana ``'\n'`` karakteri ile sonlanmayan bir yazı basmak için ``Stdout`` türünden bir
değerle ``Stdout`` yapısının ``flush`` metodunu çağırmıştık. Bu ``flush`` metodunun parametrik yapısı şöyledir:

.. code-block:: rust

    fn flush(&mut self) -> Result<()>

Buradaki ``Result`` ismi ``std::result::Result`` ismi değil daha önceden de sözünü ettiğimiz ``std::io``
modülündeki takma isimdir:

.. code-block:: rust

    pub type Result<T> = Result<T, Error>;

Yani aslında buradaki ``std::io::Result<()>`` türü tamamen ``std::result::Result<(), std::io::Error>`` türü
anlamına gelmektedir. O halde ``result`` başarılıysa bize bir şey vermemekte, başarısızsa başarısızlığın nedenini
``Error`` türünden bir değerle vermektedir. Biz ``flush`` işlemini daha önce hep aşağıdaki gibi kullanmıştık:

.. code-block:: rust

    println!("Bir değer giriniz:");
    std::io::stdout().flush().expect("Can't flush stdout");

Mademki ``flush`` metodunun başarısız olma olasılığı çok düşüktür o halde ``unwrap`` ya da ``expect`` metotlarıyla bu
durumu geçiştirebiliriz.

Yapılar (struct)
================

Giriş
-----

Bu bölümde Rust'ta *yapı (structure)* denilen veri yapılarını ele alacağız. Yapılar Rust'ta en önemli veri
yapılarındandır. Rust'ın yapıları C'nin yapılarına benzemekle birlikte metotlara da sahip olabilmektedir ve
trait'leri de destekleyebilmektedir. Bu özelliklerinden dolayı Rust'ın yapıları C++, Java ve C# gibi dillerdeki
sınıfları (classes) çağrıştırsa da özellik bakımından sınıflardan farklıdır.

Yapı Tanımlama
--------------

Programlama dillerinde *elemanları farklı türlerden olabilen ancak bellekte ardışıl bir biçimde tutulan* veri
yapılarına *yapı (struct)* denilmektedir. Yapılar bileşik türlerdir ve elemanlardan oluşurlar. Rust'ta yapı
elemanlarına *alan (field)* denilmektedir. (Java ve C# gibi bazı dillerde de sınıfın veri elemanlarına da *alan
(field)* denildiğini anımsayınız.)

Rust'ta yapılar gramer olarak *Item* grubundadır. Yani yapılar global alanda ya da bir fonksiyonun yerel bloğunda
tanımlanabilir. Tanımlamanın sonunda ``;`` atomu bulundurulmaz.

Yapı tanımlamanın temel genel biçimi şöyledir:

.. code-block:: text

    struct <yapı_ismi> {
        <alan_ismi>: <tür>,
        <alan_ismi>: <tür>,
        <alan_ismi>: <tür>,
        ....
    }

Örneğin:

.. code-block:: rust

    struct Complex {
        real: f64,
        imag: f64,
    }

Burada ``real`` ve ``imag`` ``Complex`` yapısının alanlarıdır. Örneğin:

.. code-block:: rust

    struct Person {
        name: String,
        no: i32
    }

Burada ``name`` ve ``no`` ``Person`` yapısının alanlarıdır. Örneğin:

.. code-block:: rust

    struct Date {
        day: u32,
        month: u32,
        year: u32
    }

Burada ``day``, ``month`` ve ``year`` ``Date`` yapısının alanlarıdır.

Yapılara Neden Gereksinim Duyulmaktadır?
----------------------------------------

Peki programlama dillerinde yapı benzeri veri yapılarına neden gereksinim duyulmaktadır? Bunun birkaç temel nedeni
vardır:

1) Birbirleriyle ilişkili olan birtakım olguları tekil değişkenlerle ifade etmek yerine bir yapı biçiminde ifade
   etmek algısal karışıklığı azaltır ve kodun daha anlaşılır olmasını sağlar. Örneğin bir tarihi belirten gün, ay ve
   yıl değerlerini bağımsız değişkenlerde tutmak yerine ``Date`` isimli bir yapının alanları biçiminde tutmak kodun
   daha iyi algılanmasına ve kavranmasına yol açmaktadır.

2) Yapı alanları (elemanları) bellekte ardışıl tutulduğu için birbirleriyle ilişkili ancak farklı türlerden
   bilgilerin adres yoluyla tek hamlede fonksiyonlara aktarılması mümkün hale getirilmektedir.

Rust'ta yapılara ilişkin metotlar da yazılabilmektedir. Rust'ta yapılar trait'leri de destekleyebilmektedir. Böylece
nesne yönelimli programlama tekniğinin bazı temel prensipleri de Rust'ta yapılar yoluyla uygulanabilmektedir.

Yapı Türünden Değer Oluşturma
-----------------------------

Bir yapı türünden değişken yine ``let`` deyimiyle bildirilebilmektedir (bağlanabilmektedir). Örneğin:

.. code-block:: rust

    let date: Date;

Burada ``date`` değişkeni ``Date`` isimli yapı türündendir. Rust'ta bir yapı türünden değer aşağıdaki genel biçimle
oluşturulmaktadır:

.. code-block:: text

    <yapı_ismi> {
        <alan_ismi>: değer,
        <alan_ismi>: değer,
        <alan_ismi>: değer,
        ...
    }

Bu genel biçimde önce yapı isminin belirtildiğine sonra küme parantezleri içerisinde tek tek elemanlar için
değerlerin belirtildiğine dikkat ediniz. Örneğin:

.. code-block:: rust

    let date: Date;

    date = Date {
        day: 10,
        month: 12,
        year: 2020,
    };

Burada ``Date`` türünden bir yapı değeri oluşturulup ``date`` değişkenine atanmıştır. Tabii biz bu işlemi doğrudan
``let`` deyiminde de yapabilirdik:

.. code-block:: rust

    let date = Date {
        day: 10,
        month: 12,
        year: 2020,
    };

Burada ``date`` değişkeni ``Date`` türündendir. Bir yapı türünden bir değer oluştururken yapının her alanı için değer
belirtilmesi gerekir. Örneğin aşağıdaki bağlama error ile sonuçlanacaktır:

.. code-block:: rust

    let date = Date {
        day: 10,
        month: 12,
    };

Burada yapının ``year`` alanı için bir değer belirtilmemiştir. (C'de yapının ilk elemanlarına değer verilip diğer
elemanlarına değer verilmezse diğer elemanlara ``0`` (ya da ``NULL`` adres) değeri verilmiş gibi işlem yapıldığını
anımsayınız. Ancak Rust'ta böyle bir durum söz konusu değildir.)

Tabii yapı türünden değer oluştururken alanların yapı tanımlamasındaki sıraya uygun olması gerekmez. Örneğin:

.. code-block:: rust

    let date = Date {               // geçerli
        year: 2020,
        month: 12,
        day: 10,
    };

Görüldüğü gibi yapı tanımlamasında sıra ``day``, ``month``, ``year`` biçiminde olduğu halde değer oluştururken bu
sıra izlenmek zorunda değildir. Dolayısıyla yukarıdaki kod geçerlidir.

Alanlara Kısa Yolla (shorthand) Değer Verme
-------------------------------------------

Bir yapı türünden değer oluştururken yalnızca yapı alanının ismi belirtilirse bu alana faaliyet alanında olan aynı
isimli değişkenin değerinin atandığı kabul edilmektedir. Örneğin:

.. code-block:: rust

    struct Sample {
        a: i32,
        b: i32,
        c: i32
    }

Bu yapı türünden bir değeri aşağıdaki gibi yaratabiliriz:

.. code-block:: rust

    let a = 10;
    let c = 30;

    let s: Sample = Sample {
        a,
        b: 20,
        c
    };

Bir yapı türünden değer oluştururken yapı alanının yalnızca isminin belirtilmesi faaliyet alanında bulunan o
isimdeki değişkenin o alana yerleştirileceği anlamına gelmektedir. Yani örneğin yapı alanının ismi ``a`` olmak üzere
değer oluştururken bu ``a`` ismini ``:`` olmadan kullanırsak bu sentaks tamamen ``a: a`` anlamına gelmektedir.
Yukarıdaki ilkdeğer vermenin eşdeğeri aşağıdaki gibidir:

.. code-block:: rust

    let a = 10;
    let c = 30;

    let s: Sample = Sample {
        a: a,
        b: 20,
        c: c
    };

Buna Rust'ta *yapı alanlarına kısa yolla (shorthand) değer verme* denilmektedir. Bu sentaks özellikle fonksiyonlarda
sıkça kullanılmaktadır. Örneğin:

.. code-block:: rust

    fn create_sample(a: i32, b: i32, c: i32) -> Sample {
        Sample { a, b, c }
    }

Bu fonksiyon aldığı üç parametre ile yapı türünden değer oluşturup o değerle geri dönmektedir. Bu tanımlama
aşağıdakiyle eşdeğerdir:

.. code-block:: rust

    fn create_sample(a: i32, b: i32, c: i32) -> Sample {
        Sample { a: a, b: a, c: c }
    }

Bu fonksiyon sayesinde biz bu yapı türünden bir değeri daha kolay oluşturabiliriz:

.. code-block:: rust

    let s: Sample;

    s = create_sample(10, 20, 30);

.. code-block:: rust

    fn main() {
        let s: Sample;

        s = create_sample(10, 20, 30);
        //...
    }

    fn create_sample(a: i32, b: i32, c: i32) -> Sample {
        Sample { a: a, b: a, c: c }
    }

    struct Sample {
        a: i32,
        b: i32,
        c: i32
    }

``..`` Sentaksı
---------------

Rust'ta bir yapı türünden değer oluştururken kullanılan diğer bir kısa yol da ``..`` sentaksıdır. Bu sentaksta yapı
türünden değer oluştururken küme parantezlerinin sonuna ``..`` atomu ve aynı yapı türünden bir ifade (tipik olarak
bir değişken) getirilir. Bu sentaks *geri kalan alanlar bu yapı ifadesindeki (tipik olarak değişkenindeki) alanların
aynısı olacak* anlamına gelmektedir. Örneğin:

.. code-block:: rust

    let s = Sample { a: 10, b: 20, c: 30 };
    let k: Sample;

    k = Sample { b: 100, ..s };

Burada oluşturulan yapı değerinin ``b`` alanında ``100`` olacaktır. Geri kalan alanlar ``a`` ve ``c``'dir. Bunlar da
``s``'ten alınacaktır. Yani bu kodun eşdeğeri şöyledir:

.. code-block:: rust

    k = Sample { b: 100, a: s.a, b: s.b };

Tabii sonraki paragraflarda göreceğimiz gibi burada bir sahiplik devri de söz konusudur. Yani bu örnekte eğer
``Sample`` ``Copy`` türünden değilse ``s``'nin sahipliği de devredilmektedir.

Bu kısa yol sentaksında ``..`` atomu ile yapı ifadesi (tipik olarak yapı değişkeni) bitişik bulundurulmak zorunda
değildir. Ancak tipik yazım böyledir. Bu sentaks küme parantezlerinin sonunda ve yalnızca bir kez bulundurulmak
zorundadır. Aşağıdaki değer yaratımı da geçerlidir:

.. code-block:: rust

    let s = Sample { a: 10, b: 20, c: 30 };
    let k: Sample;

    k = Sample { ..s };

Tabii bu işlem zaten aşağıdakiyle eşdeğerdir:

.. code-block:: rust

    k = s;

Yapı Alanlarına Erişim
----------------------

Rust'ta yapı alanlarına diğer pek çok dilde olduğu gibi bir yapı türünden değişken ya da referansla nokta operatörü
kullanılarak erişilmektedir. Nokta operatörü iki operand'lı araek bir operatördür. Öncelik tablosunda yüksek
öncelikli bir biçimde bulunmaktadır (tablo yüksek öncelikten düşük önceliğe doğru sıralanmıştır):

==========================================  =====================================
Operatörler                                 Birleşme Yönü
==========================================  =====================================
``.``                                       Soldan Sağa
``()``                                      Soldan Sağa
``- !``                                     Sağdan Sola
``as``                                      Soldan Sağa
``* /  %``                                  Soldan Sağa
``+ -``                                     Soldan Sağa
``<<  >>``                                  Soldan Sağa
``&``                                       Soldan Sağa
``^``                                       Soldan Sağa
``|``                                       Soldan Sağa
``< > >= <= == !=``                         Parantezsiz Kombine Edilemez
``&&``                                      Soldan Sağa
``||``                                      Soldan Sağa
``.. ..=``                                  Parantezsiz Kombine Edilemez
``= += -= *= /= %= &= |= ^= <<= >>=``       Sağdan Sola
==========================================  =====================================

Nokta operatörünün sol tarafındaki operand bir yapı türünden değişken ya da değer, sağ tarafındaki operand ise o
yapının bir alanı (ya da bir metodu) olmalıdır. Örneğin:

.. code-block:: rust

    fn main() {
        let date = Date {
            day: 10,
            month: 12,
            year: 2020,
        };

        println!("{}/{}/{}", date.day, date.month, date.year);
    }

    struct Date {
        day: u32,
        month: u32,
        year: u32
    }

Yine Rust'ta da yapının bir alanı başka bir yapı türünden olabilir. Bu durumda iç alanlara birden fazla nokta
operatörüyle erişilmektedir. Örneğin:

.. code-block:: rust

    struct Person {
        name: String,
        bday: Date
    }

    struct Date {
        day: u32,
        month: u32,
        year: u32
    }

Burada ``Person`` yapısının ``bday`` alanı ``Date`` yapısı türündendir. ``Person`` türünden bir yapı değerini şöyle
oluşturabiliriz:

.. code-block:: rust

    let per = Person {
        name: String::from("Ali Güneş"),
        bday: Date {
            day: 12,
            month: 12,
            year: 1995
        }
    };
    println!("{}, {}/{}/{}", per.name, per.bday.day, per.bday.month, per.bday.year);

.. code-block:: rust

    fn main() {
        let per = Person {
            name: String::from("Ali Güneş"),
            bday: Date {
                day: 12,
                month: 12,
                year: 1995
            }
        };
        println!("{}, {}/{}/{}", per.name, per.bday.day, per.bday.month, per.bday.year);
    }

    struct Person {
        name: String,
        bday: Date
    }

    struct Date {
        day: u32,
        month: u32,
        year: u32
    }

Yapı Değişkenlerinin Atanması
-----------------------------

Rust'ta da tıpkı C'de olduğu gibi bir yapı değişkenine aynı türden bir yapı değişkeni ya da değeri atanabilir. Bu
durumda sonraki bölümde ele alınacağı gibi eğer yapı ``Copy`` türünden değilse hedef yapı değişkeni önce ``drop``
edilir. Sonra kaynak değişkenin değeri bit düzeyinde kopyalama yoluyla hedef değişkene taşınır. Bu işlem sırasında
padding byte'ları da taşınmaktadır. Eğer ilgili yapı ``Copy`` türünden değilse artık kaynak yapı değişkenini
kullanamayız. Rust'ta yapı türleri aksi belirtilmediği sürece (default durumda) ``Copy`` türüne ilişkin değildir,
yani taşınmaktadır. Örneğin:

.. code-block:: rust

    fn main() {
        let d1 = Date {
            day: 10,
            month: 12,
            year: 1995
        };
        let d2: Date;

        d2 = d1;            // geçerli
        println!("{}/{}/{}", d2.day, d2.month, d2.year);

        println!("{}/{}/{}", d1.day, d1.month, d1.year);            // error!
    }

    struct Date {
        day: u32,
        month: u32,
        year: u32
    }

Bu örnekte ``d1`` yapı değişkeni ``d2``'ye atanmıştır. Atama default durumda bit düzeyinde kopyalama yoluyla (bunun
tek hamlede ``memcpy`` gibi bir fonksiyonla yapıldığını varsayabilirsiniz) yoluyla yapılmaktadır. Ancak C'den farklı
olarak Rust'ta kaynak yapı değişkeni artık sahipliği kaybetmiştir. Bu nedenle örneğimizde bu atamadan sonra artık biz
``d1`` yapı değişkenini kullanamayız. Bu tür atamalarda Rust padding byte'larının da hedefe atanmasını garanti
etmektedir.

Yapı Değişkenleri ve mut
------------------------

Bir yapı değişkeni ``mut`` değilse onun herhangi bir alanının değerini değiştiremeyiz. Örneğin:

.. code-block:: rust

    struct Point {
        x: i32,
        y: i32,
    }
    //...

    let pt = Point { x: 10, y: 20 };

    pt.x = 100;     // error
    pt.y = 100;     // error

Eğer yapının alanları daha sonra değiştirilmek isteniyorsa bu durumda yapı değişkeni ``mut`` yapılmalıdır. Örneğin:

.. code-block:: rust

    let mut pt = Point { x: 10, y: 20 };

    pt.x = 100;     // geçerli
    pt.y = 200;     // geçerli

C'de yapı nesnesi ``const`` olmadığı halde onun belli elemanları ``const`` yapılabilmektedir. Ancak Rust'ta bir yapı
değişkeninin belli alanları ``mut`` yapılamaz. Yapı değişkeni ya bütünsel olarak ``mut`` olur ya da bütünsel olarak
``mut`` olmaz.

Alanlara Atama Yoluyla İlkdeğer Verilememesi
--------------------------------------------

Bir yapı türünden değişkenin alanlarına değer atayarak ona ilkdeğer veremeyiz. Örneğin:

.. code-block:: rust

    struct Point {
        x: i32,
        y: i32,
    }
    //...

    let pt: Point;

    pt.x = 10;      // error!
    pt.y = 20;      // error!

Yapı değişkenine ilkdeğer vermek ancak küme parantezleriyle sağlanmaktadır:

.. code-block:: rust

    let pt = Point { x: 10, y: 20 };

Demetsel Yapılar (tuple struct)
-------------------------------

Rust'ta yapı ile demet arasında kalan ismine *demetsel yapı (tuple struct)* denilen ilginç bir yapı türü de vardır.
Demetsel yapılar ``struct`` anahtar sözcüğü ile ancak yapı sentaksıyla değil demet sentaksıyla tanımlanmaktadır.
Demetsel yapı tanımlamanın genel biçimi şöyledir:

.. code-block:: text

    struct <yapı_ismi>(<tür>, <tür>, <tür>, ...);

Örneğin:

.. code-block:: rust

    struct Date(i32, i32, i32);
    struct Person(String, i32);
    struct Complex(f64, f64);

Görüldüğü gibi demetsel yapıların kendi isimleri vardır ancak alanlarının isimleri yoktur. Demetlerin birer tür
ismine sahip olmadığını, demet elemanlarının da isimlerinin olmadığını anımsayınız. İşte demetsel yapılar adeta bir
demet gibi davranmakla birlikte isme sahip oldukları için metotlara da sahip olabilmekte ve trait'leri de
desteklemektedir. Oysa Rust'ta demetler metotlara sahip olamamaktadır. (Ancak demetler de trait'leri
destekleyebilmektedir.) Demetsel yapılar tanımlanırken tanımlamanın sonunda ``;`` atomunun bulundurulduğuna da dikkat
ediniz.

Demetsel bir yapı türünden değer oluşturmanın genel biçimi şöyledir:

.. code-block:: text

    <demetsel_yapı_ismi>(değer, değer, ....)

Örneğin:

.. code-block:: rust

    struct Person(String, i32);
    //...

    let per: Person = Person(String::from("Ali Kale"), 123);

Demetsel yapı oluştururken alanlar için değerlerin küme parantezleriyle değil de demet sentaksında olduğu gibi normal
parantezlerle verildiğine dikkat ediniz.

Demetsel yapılardaki alanların isimleri olmadığı için onlara erişim isim yoluyla değil demet sentaksında olduğu gibi
``<değişken>.0``, ``<değişken>.1``, ``<değişken>.2``, ... biçiminde indeks belirtilerek yapılmaktadır. Örneğin:

.. code-block:: rust

    struct Person(String, i32);
    //...

    let per: Person = Person(String::from("Ali Kale"), 123);
    println!("{}, {}", per.0, per.1);
