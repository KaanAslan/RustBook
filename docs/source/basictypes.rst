=============
Temel Türler
=============

Bir programlama dili öğrenilirken ilk öğrenilecek şeylerden biri o programlama dilinin temel türleridir. Biz de bu bölümde
Rust'ın temel türlerini açıklayacağız.

Rust'taki Tür Kategorileri
==========================

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

Biz burada önce temel türleri (primitive types) sonra ilerleyen bölümlerde de demet (tuple), dizi (array) ve dilim
(slice) gibi dizilim türlerini (sequence types) ve kullanıcı tanımlı türleri göreceğiz.

Tamsayı Türleri
===============

Rust'ta nümerik türler kendi aralarında tamsayı türleri ve gerçek sayı türleri biçiminde ikiye ayrılmaktadır. Tamsayı
türleri de kendi aralarında *işaretli tamsayı türleri*, *işaretsiz tamsayı türleri* ve *makine bağımlı tamsayı türleri*
olmak üzere üçe ayrılmaktadır. Rust'ta işaretli tamsayılar 2'ye tümleyen aritmetiği ile tutulmaktadır. (Halbuki örneğin
C ve C++ standartlarında her ne kadar tüm derleyiciler 2'ye tümleyen aritmetiğini kullanıyor olsa da işaretli
tamsayıların 2'ye tümleyen aritmetiği ile tutulacağı konusunda bir garanti verilmemiştir.) Rust'ta tamsayı türlerinin
isimleri oldukça kısa bir biçimde oluşturulmuştur. N bit sayısını belirtmek üzere işaretli tamsayı türlerinin isimleri
iN biçiminde, işaretsiz tamsayı türlerinin isimleri de uN biçimindedir. Rust'ın tüm tamsayı türlerinin isimleri aşağıda
verilmiştir:

========================== =========================== ==============================
İşaretli Tamsayı Türleri   İşaretsiz Tamsayı Türleri   Makine Bağımlı Tamsayı Türleri
========================== =========================== ==============================
``i8``                     ``u8``                      ``isize``
``i16``                    ``u16``                     ``usize``
``i32``                    ``u32``
``i64``                    ``u64``
``i128``                   ``u128``
========================== =========================== ==============================

Anımsanacağı gibi 2'ye tümleyen aritmetiğinde N bitlik bir işaretli tamsayının sınır değerleri [-2^N, +2^N - 1]
aralığındadır. Rust'ta 128 bitlik tamsayı türünün de olduğuna dikkat ediniz. Henüz 128 bitlik işlemciler kullanılmıyor
olsa da bazı durumlarda (özellikle bit işlemlerinde) 128 bitlik tamsayılar daha verimli kod yazımını sağlayabilmektedir.

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

