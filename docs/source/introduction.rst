=====
Giriş
=====

Bu bölümde Rust'a bir giriş yapacağız. Rust'ta kullanılan temel kavramları açıklayacağız ve bir Rust programının nasıl derlenip 
çalıştırıldığını göstereceğiz. 

Rust Nasıl Bir Dildir?
======================

Rust genel amaçlı, yapısal, prosedürel ve fonksiyonel programlama modellerini destekleyen çok modelli (multi-paradigm),
statik tür sistemine sahip modern bir programlama dilidir. Tasarımında özellikle eşzamanlı (concurrent) programlama
etkinliklerinin daha güvenli biçimde sağlanması ön planda tutulmuştur. Bunun sağlanabilmesi için de bellek erişim
güvenliğine (memory safety) özel bir önem verilmiştir.

Rust bir sistem programlama dili olarak kullanılabilecek biçimde tasarlanmıştır. Bu nedenle C Programlama Diline ve
kısmen de C++ Programlama Diline yenilikçi bir seçenek oluşturmaktadır. Rust C Programlama Dilinden biraz daha yüksek
seviyeli bir dildir. Birtakım modern özellikler performans kaybına yol açmayacak biçimde dile entegre edilmiştir.

Rust nesne yönelimli ya da nesne tabanlı bir programlama dili değildir. Ancak nesne yönelimli programlama tekniğindeki
bazı prensiplerin uygulanabileceği bir tasarım özelliğine sahiptir.

Rust çeşitli programlama dillerinden ilham alınarak tasarlanmıştır. Rust'ı etkileyen programlama dilleri şunlardır:

- C/C++
- ML / OCaml
- Erlang
- Scheme
- Haskell
- Go
- Swift
- Kotlin
- Python

Rust'ın temel bir kütüphanesi vardır. Ayrıca zamanla kendine özgü bir ekosistem de oluşmuştur. Bu ekosistemde Rust
programcıları kendi çalışmalarını başkalarıyla paylaşmaktadır. Dolayısıyla daha spesifik gereksinimler için *crates.io*
deposu içerisindeki üçüncü parti kütüphanelerden faydalanılmaktadır.

Rust'ın Tarihsel Gelişimi
=========================

Rust nispeten yeni bir programlama dilidir. Dolayısıyla kısa bir tarihi vardır.

Rust'ın tasarımına 2006 yılında Mozilla şirketi çalışanı olan *Graydon Hoare* (*Greydın Ho:r* biçiminde okunuyor)
tarafından başlanmıştır. Hoare bu çalışmaları 2009'a kadar tek başına ve ancak boş zamanlarında yürütmüştür. Hoare 2009
yılından itibaren yaptığı çalışmaları Mozilla'daki çalışma arkadaşlarıyla paylaşmaya başlamıştır. İlk Rust derleyicisi
de deneysel amaçla OCaml Dili kullanılarak yazılmıştır. Mozilla 2009 yılında Rust projesine sponsor olmaya başlamıştır.
Rust derleyicisi 2010 yılında LLVM kullanılarak yeniden yazılmıştır. Rust'ın ilk resmi sürümü (0.1 versiyonu) 2012
yılında herkesin kullanımına sunulmuştur. Bu yıllarda Rust projesine Mozilla dışında pek çok gönüllü de katılmaya
başlamıştır. Mozilla tam zamanlı çalışmak üzere çeşitli geliştiricileri de işe almıştır. Rust 2012 yılından sonra
sürekli iyileştirilmeye çalışılmıştır. Graydon Hoare 2013 yılında Rust'ın geliştirilme sürecinde kendini geri plana
çekmiştir. Dil üzerindeki pek çok değişiklikten sonra 2015 yılında Rust'ın 1.0 versiyonu kullanıma sunulmuştur. Bu
süreçte Rust topluluğu da gitgide büyümüştür.

Rust'ın ilk versiyonu çıktıktan sonra Mozilla dışında Facebook (Meta), Dropbox, Amazon gibi şirketler de Rust'a ilgi
göstermeye başlamıştır. University of Minho, NOVA University Lisbon ve University of Coimbra'nın yaptığı araştırmalara
göre Rust Java'dan iki kat daha az elektrik harcamaktadır. Ancak C ile kıyaslandığında C'den biraz daha fazla elektrik
harcamaktadır.

Mozilla 2020 yılında Covid salgınının da etkisiyle Rust projesinden çekilme kararı almıştır. Böylece sponsorluk
bağlamında işe aldığı kişileri de işten çıkarmıştır. 2021 yılında Amazon Web Services, Google, Huawei, Microsoft,
Mozilla şirketlerinin desteğiyle *Rust Foundation* isimli bir kurum oluşturulmuştur. 2021'de Google Android'de Rust'ın
da kullanılacağını belirtmiştir. 2024'te Beyaz Saray Rust'ın kullanılmasını teşvik eden 19 sayfalık bir doküman
yayınlamıştır. Her ne kadar Beyaz Saray'ın bu konuyla doğrudan bir ilgisi yoksa da bu rapor Rust'ın geleceği konusunda
iyimser bir hava estirmiştir.

.. figure:: _static/rust-history-timeline.png
   :align: center
   :alt: Rust'ın tarihsel gelişimi
   :width: 65%

Rust'ın geliştirilmesine halen *Rust Foundation* (https://rustfoundation.org) kurumu tarafından devam edilmektedir. 

Rust'ın versiyonlarının tarihsel gelişimi şöyledir:
 
- 2006: Rust'ın geliştirilmesine başlandı.
- 2009: Mozilla, Rust projesini desteklemeye başladı.
- 2010: İlk bootstrap (ön-yükleme) derleyicisi yayımlandı.
- 2012: Rust'ın kendini derleyen (self-hosted) derleyicisinin alfa sürümleri (örneğin, 0.3 alfa) yayımlandı.
- Mayıs 2015: Rust 1.0 stabil sürümü yayımlandı; dil, artık üretim ortamında kullanılmaya hazır hale geldi.
- Temmuz 2016: 1.10 sürümü yayımlandı.
- Ağustos 2017: 1.20 sürümü yayımlandı.
- Ekim 2018: 1.30 sürümü yayımlandı.
- Aralık 2019: 1.40 sürümü yayımlandı.
- Şubat 2021: 1.50 sürümü yayımlandı.
- Nisan 2022: 1.60 sürümü yayımlandı.
- Haziran 2023: 1.70 sürümü yayımlandı.
- Temmuz 2024: 1.80 sürümü yayımlandı.
- Eylül 2025: 1.90 sürümü yayımlandı.
- Ekim 2026: 1.99 sürümü yayımlandı.

Rust'ın Resmi Dokümanları
=========================

*Rust Foundation* Rust Programlama Dilinin geliştirilmesinden ve sürdürümünden sorumludur. Resmi sitesine aşağıdaki
bağlantıdan erişebilirsiniz:

https://rustfoundation.org

Rust'ın üç önemli resmi (formal) dokümanı vardır.

1) The Rust Reference (https://doc.rust-lang.org/reference/index.html)
2) The Rust Standard Library (https://doc.rust-lang.org/std/index.html)
3) The Rustonomicon (https://doc.rust-lang.org/nomicon/index.html)

*The Rust Reference* dokümanı programlama dilinin resmi (formal) belirlemelerini içermektedir. Yani bu doküman adeta
Rust Programlama Dilinin standart dokümanı gibi düşünülebilir. Ancak bu tür referans dokümanlarını oluşturmak kolay
değildir. Dolayısıyla kitabın yazıldığı tarihlerde bu dokümanda da eksiklikler de vardır. *The Rust Standard Library*
dokümanları Rust'ın standart kütüphanesini açıklamaktadır. *The Rustonomicon* dokümanları ise Rust'ta unsafe
özellikler ve unsafe kodların oluşturulmasına ilişkin bilgileri içermektedir. Ayrıca Rust'ın resmi sitesinde
*tutorial* biçiminde *The Rust Programming Language* isimli bir kitap da bulunmaktadır. Bu kitap aynı zamanda kağıda
basılı biçimde de satılmaktadır.

Kurulum
=======

Şüphesiz Rust'ı öğrenirken ilk yapılması gereken şey Rust derleyicisinin ve gerekli birtakım araçların bilgisayara
yüklenmesidir. Bu işlem *rustup* isimli program tarafından yapılmaktadır. Bu rustup programı güncelleme amacıyla da
kullanılmaktadır. rustup programını aşağıdaki bağlantıdan indirebilirsiniz:

https://rust-lang.org/tools/install/

Windows sistemlerinde *rustup* gerçekten *çalıştırılabilir (executable)* bir programdır. Unix/Linux ve macOS
sistemlerinde kurulum işlemi *sh.rustup.rs* isimli bir *kabuk betiği (shell script)* tarafından yapılmaktadır. Bu
sistemlerde tek yapılacak şey komut satırında aşağıdaki kabuk komutunu çalıştırmaktır:

.. code-block:: bash

   $ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

Bu komut çalıştırıldığında *rustup* programı kurulmuş olacaktır.

*rustup* programı ile Rust derleyicisini ve diğer araçları şöyle kurabilirsiniz:

.. code-block:: bash

   rustup default stable

*rustup* bir konsol programı gibi çalışmaktadır. Program çalıştırıldığında bir seçenek menüsü çıkartır. Kurulumu
kendisi yapar. Bu kurulum yapıldığında ``PATH`` çevre değişkeni otomatik olarak güncellenmektedir. Dolayısıyla bir
terminal açıldığında Rust derleyicisi ve *cargo* gibi araçlar çalıştırılabilmektedir. 

Merhaba Dünya Programının Derlenerek Çalıştırılması
===================================================

Şimdi de *Merhaba dünya (hello world)* programının nasıl derlenerek çalıştırılacağını görelim. Rust'ın derleyicisi
diğer derleyicilerde olduğu gibi bir konsol uygulaması biçiminde yazılmıştır. Derleyici programın ismi *rustc*
biçimindedir. Diğer dillerde olduğu gibi önce herhangi bir editörde *Merhaba dünya* programı yazılır ve dosya
uzantısı *.rs* olacak biçimde save edilir. Rust kaynak dosyalarının doğal uzantıları *.rs* biçimindedir. Aşağıdaki
programı herhangi bir text editörde yazarak *sample.rs* biçiminde save edelim:

.. code-block:: rust

   fn main() {
       println!("Merhaba dünya");
   }

Rust kaynak dosyalarının Unicode UTF-8 kodlamasıyla oluşturulmuş olması gerekmektedir. Yani Rust derleyicisi kaynak
kodun bu kodlama biçimine sahip olduğunu varsaymaktadır.

Derleme oldukça basittir. Derlenecek program dosyası *rustc* derleyicisine komut satırı argümanı olarak verilir:

.. code-block:: bash

   rustc sample.rs

Bu işlemle önce kaynak dosya derlenecek sonra bağlanarak (*link* edilerek) çalıştırılabilen dosya oluşturulacaktır. 
Windows sistemlerinde komut satırında doğrudan dosya ismini yazarak programı çalıştırabilirsiniz. UNIX/Linux ve macOS
sistemlerinde çalıştırma aşağıdaki gibi yapılmalıdır:

.. code-block:: bash

   $ ./sample

Rust'ı Windows'a yüklediğimizde *rustc* default durumda linbağlama (*link*) işlemi için Microsoft'un *link.exe* programını
kullanmaktadır. Dolayısıyla Windows sistemlerinde MSVC araçlarının yüklü olması gerekir. Kurulum sırasında *rustup*
programı bu kontrolü yapmakta ve eğer MSVC aracı yoksa bunu kullanıcıya sorarak yüklemektedir. Ancak Windows
sistemlerinde *GNU Araç Zinciriyle (GNU Toolchain)* çalışılabilmektedir.

Tıpkı **gcc** ve *clang* derleyicilerinde olduğu gibi *rustc* derleyicisi de bağlama işleminden sonra
*amaç dosyayı (object file)* silmektedir. Dolayısıyla *rustc* programını çalıştırdıktan sonra bir amaç dosya
göremezseniz şaşırmayınız.

Link işlemini yapmadan yalnızca amaç dosya oluşturmak için *--emit=obj* seçeneği kullanılmaktadır. Örneğin:

.. code-block:: bash

   rustc --emit=obj sample.rs

Bu işlemle *sample.o* isminde bir amaç dosya oluşturulacaktır. Ancak bağlama işlemi yapılmayacaktır.

Tıpkı *gcc* ve *clang* derleyicilerinde olduğu gibi *"-o <hedef_dosya_ismi>"* seçeneği ile hedef dosyaya istenilen bir
isim verilebilmektedir. Örneğin:

.. code-block:: bash

   rustc -o project.exe sample.rs

Burada artık Windows'ta çalıştırılabilen dosya *sample.exe* isminde değil *project.exe* isminde olacaktır. Tabii
UNIX/Linux ve macOS sistemlerinde çalıştırılabilen dosyaların belli bir uzantısı yoktur. Aynı işlem bu sistemlerde
şöyle yapılabilir:

.. code-block:: bash

   rustc -o project sample.rs

Sandık ve Paket Kavramları
==========================

Rust dünyasında derleyicinin tek parça olarak ele aldığı derleme birimine *sandık (crate)* denilmektedir.
Bir sandık (crate) derlendiğinde ya çalıştırılabilir bir dosya ya da bir kütüphane dosyası elde edilir. Birinci
durumda *ikili sandık (binary crate)*, ikinci durumda *kütüphane sandığı (library crate)* terimleri
kullanılmaktadır. Sandık tek bir kaynak dosyadan oluşabileceği gibi birden fazla kaynak dosyadan da oluşabilir.
Ancak derleyiciye yalnızca *sandığın kökü (crate root)* denilen tek bir kaynak dosya verilir (tipik olarak
*main.rs* ya da *lib.rs*). Diğer kaynak dosyalar bu dosyadan başlayarak *mod* bildirimleriyle sandığın modül
ağacına dahil edilir. Crate kavramı diğer programlama ortamlarındaki *proje* kavramına benzemektedir. Örneğin biz
C++'ta çalışırken bir proje yaratıp o projenin içerisine birden fazla kaynak dosya yerleştirip projeyi build
edebiliriz. İşte aynı biçimde Rust dünyasında da bir sandığın içerisinde birden fazla kaynak dosya
bulunabilmektedir. Yalnız C++'ta her kaynak dosya ayrı ayrı derlenip sonra bağlanırken (link edilirken), Rust'ta
sandığın (crate) tamamı tek bir derleme birimidir. *Paket (package)* ise Rust dilinin değil Cargo'nun bir
kavramıdır. Paket bir *Cargo.toml* dosyası ile tanımlanan ve sandıkları tutan mantıksal bir kap gibidir. Bir pakette
birden fazla sandık olabilir. Ancak bir pakette en fazla bir tane kütüphane sandığı bulunabilir, ikili sandıkların
sayısında ise bir sınır yoktur. Örneğin *Visual Studio* IDE'sinde projeleri tutan mantıksal kaba
*çözüm (solution)* denilmektedir. Rust'taki paket Visual Studio'daki çözüm kavramına kabaca benzetilebilir. Fakat
çözüm kavramının daha yakın karşılığı birden fazla paketi bir araya getiren *çalışma alanı (workspace)* kavramıdır.
Paket sözcüğü aslında diğer başka programlama dillerinde de benzer anlamda kullanılabilmektedir. Örneğin Python
dünyasında paket denildiğinde bir grup modül anlaşılmaktadır.

cargo Aracı
===========

Aslında Rust programcıları kaynak dosyaları komut satırından doğrudan *rustc* derleyicisi ile derlemek yerine
genellikle *cargo* isimli bir araçla derleme işlemini yaparlar. *cargo* hem bir paket yöneticisi hem de bir build
otomasyon aracıdır. Yani *cargo* programı hem depolardan çeşitli kütüphaneleri (*crates*) indirip projeye dahil
edebilmekte hem de birden fazla kaynak dosyayı derleyerek bağlama işlemini yapabilmektedir. Rust paket olarak
yüklendiğinde zaten *cargo* aracı da bilgisayarınıza yüklenmiş olmaktadır. Biz kitabımızda *cargo* programının ayrıntılı
kullanımını ileride ele alacağız. Ancak burada bu programın bir proje (paket) oluşturup onu derlemek ve çalıştırmak
için yalın bir biçimde nasıl kullanılacağını göreceğiz.

cargo ile Paket Oluşturma, Derleme ve Çalıştırma
------------------------------------------------

*cargo* ile yeni bir paket (proje de diyebiliriz) oluşturmak için *cargo new* komutu kullanılmaktadır. Komutun genel
biçimi şöyledir:

.. code-block:: shell

   cargo new <paket_ismi>

Bu işlemden sonra ismi "paket_ismi" olan bir dizin yaratılır. Yaratılan dizinin içeriği şöyle olacaktır:

.. code-block:: text

   23.11.2022  13:50    <DIR>          .
   23.11.2022  13:50    <DIR>          ..
   23.11.2022  13:50                 8 .gitignore
   23.11.2022  13:50               175 Cargo.toml
   23.11.2022  13:50    <DIR>          src
               2 File(s)            183 bytes
               3 Dir(s)  679.902.699.520 bytes free

Buradaki ``Cargo.toml`` dosyası paketin ana yönetim dosyasıdır. Pakete ilişkin öğelerle ilgili bilgiler bu dosyada
tutulmaktadır. Yeni yaratılmış olan minimal bir projede bu dosyanın içeriği aşağıdaki gibi çok küçüktür:

.. code-block:: toml

   [package]
   name = "myproject"
   version = "0.1.0"
   edition = "2021"

   [dependencies]

İkili bir sandık söz konusu ise ``src`` dizininin altında ``main.rs`` isminde aşağıdaki gibi bir *Hello World* programı 
oluşturulmaktadır:

.. code-block:: rust

   fn main() {
       println!("Hello, world!");
   }

Burada paketin ana sandığı ``main.rs`` dosyasından oluşan ikili bir sandıktır. Aslında paket yaratımı sırasında
*--bin* ya da *--lib* seçenekleri de kullanılabilmektedir. Bu durumda paketin ana sandığı ikili (*binary*) ya da
kütüphane (*library*) olmaktadır. Default durum eskiden *--lib* biçimindeydi ancak sonraları sürümlerde *--bin* 
haline getirildi.

Projeyi build etmek için proje dizinine geçilip "*cargo build*" komutu uygulanır. Örneğin:

.. code-block:: shell

   cargo build

Build işleminden sonra ``target`` isimli bir dizin ve onun içerisinde de ``debug`` isimli bir dizin oluşturulacaktır.
*cargo build* komutu proje dizininde ya da onun alt dizinlerinde uygulanabilir. Ancak proje dizini dışından
uygulanamaz. Build işlemi sonucunda çalıştırılabilen dosya ya da kütüphane dosyası varsayılan durumda ``target/debug``
dizininde paket ismiyle oluşturulacaktır. 

*cargo* varsaylan durumda *debug derlemesi* yapmaktadır. *release* derlemesi için *--release* seçeneğinin kullanılaması gerekir:

.. code-block:: shell

   cargo build --release

Bu durumda çalıştırılabilir dosya ya da kütüphane dosyası ``target/release`` dizininde oluşturulacaktır.

Build işleminden sonra oluşturulan çalıştırılabilir dosyayı çalıştırmak için doğrudan *cargo run* komutu
kullanılabilir. Örneğin:

.. code-block:: shell

   cargo run

Bu komut da proje dizininde ya da projenin bir alt dizininde uygulanabilmektedir. Tabii pek çok benzer build aracında
olduğu gibi sonraki aşama önceki aşamaların yapılmasına da yol açmaktadır. Yani biz *cargo build* komutundan önce
*cargo run* komutunu kullanırsak zaten önce build işlemi yapılıp sonra oluşturulan çalıştırılabilir dosya
çalıştırılacaktır.

*cargo build* komutuyla yapılan işlemlerin ortadan kaldırılması için *cargo clean* komutu kullanılmaktadır. Örneğin:

.. code-block:: shell

   cargo clean

VSCode Editöründe Rust ile Çalışmak
===================================

VSCode editöründe Rust ile çalışmak için pek çok plugin (*extension*) bulunmaktadır. Önemli Rust plugin'leri şunlardır:

* Rust Analyzer
* crates
* Better ToML
* CodeLLDB
* cargo

*Rust Extension Pack* isimli plugin'ler zaten yukarıdaki plugin'lerin çoğunu yüklemektedir.

VSCode editöründe program çalıştırmak için bazı JSON dosyaları üzerinde değişiklikler yapmak gerekebilir. Ancak pratik
bir biçimde program çalıştırmak için de plugin'ler vardır. *Code Runner* isimli plugin bu amaçla yaygın biçimde
kullanılmaktadır. Bu plugin tek bir tuşla (Ctrl+Alt+N) pek çok programlama dilinde yazılmış programı derleyerek
çalıştırmaktadır. Biz kursumuzda VSCode'da bu *Code Runner* plugin'ini kullanacağız.

Code Runner plugin'i kurulduğunda Rust için derleme default biçimde *rustc* derleyicisi ile yapılmaktadır. Yani *cargo*
ile yapılmamaktadır. Eğer derleme ve çalıştırmada cargo'yu kullanmak istiyorsanız bunun için ``settings.json`` dosyasını
uygun biçimde güncellemeniz gerekir. VSCode'ta üç önemli JSON dosyası vardır:

* Default Settings JSON dosyası
* User Settings JSON dosyası
* Workspace Settings JSON dosyası

Belirlemeyi hangi JSON dosyasında yaparsanız yukarıdakileri geçersiz hale getirecektir. Burada *User Settings JSON*
dosyasını kullanabilirsiniz. Bu dosyayı edit ederek dosyada *code-runner.executorMap* kısmına gelip *rust* satırında şu
değişikliği yapabilirsiniz:

.. code-block:: text

   "code-runner.executorMap": {
       ...
       "rust": "cd $dir && cargo run"
       ...
   }

Code Runner plugin'inin default *rust* satırı aşağıdaki gibidir:

.. code-block:: text

   "rust": "cd $dir && rustc $fileName && $dir$fileNameWithoutExt"

Tabii eski duruma dönmek için bu satırı değiştirmek yerine orijinal ismi de değiştirebilirsiniz. İsmini nasıl
değiştirdiğinizin bir önemi yoktur. Örneğin:

.. code-block:: text

   "code-runner.executorMap": {
       ...
       "rust_old": "cd $dir && rustc $fileName && $dir$fileNameWithoutExt",
       "rust": "cd $dir && cargo run"
       ...
   }

Şimdi artık *Ctrl+Alt+N* tuşlarına bastığınızda "*cargo run*" komutu çalıştırılacak dolayısıyla derleme ve çalıştırma
cargo tarafından yapılacaktır. Tabii cargo bir paket (proje de diyebiliriz) dizinini kullandığı için sizin daha önce
"*cargo new*" ile yeni bir paket dizini oluşturmuş olmanız gerekir. Bunu oluşturduktan sonra editöre ``src`` dizini
içerisindeki ``main.rs`` dosyasını çekip *Ctrl+Alt+N* tuşlarına basarsanız programınız cargo ile program sorunsuz
derlenip çalıştırılacaktır. Aslında VSCode bir klasörü de *File/Open Folder* menüsü ile açabilmektedir. Dolayısıyla
editöre tek bir dosyayı çekmek yerine dizini açıp dosyayı oradan seçmek daha kolay bir kullanım sunacaktır.

VSCode'da *Code Runner* ile çalışırken edit ettiğiniz kaynak dosyanın *Ctrl+Alt+N* tuşlarıyla çalıştırma sırasında save
edilmiş olduğuna emin olunuz. Bu tuşlara basmadan önce kaynak dosyanın otomatik save edilmesi *Code Runner* plugin'inin
kendi ayar menüsünden *Save File Before Run* ile yapılmaktadır.

