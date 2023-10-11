# Pointer 101
---

Pointer; C/C++ dillerinde önemli bir yere sahip ve uzmanlaşmamız için
biraz efor sarfetmemiz gereken bir konudur. Ama temel mantığını
anladığımız sürece bize oldukça kolaylıklar sağlayacaktır. Bu yazımızda,
birlikte pointer ile ilgili önemli işlevleri ve o işlevleri kavramamıza
yardımcı olacak örnekleri gözden geçireceğiz.

Konu Başlıkları:

### [Pointer'ın Temel İşlevi](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/blob/main/Pointer%20101.md#pointer%C4%B1n-temel-i%CC%87%C5%9Flevi)
[Pointer ile Call by Reference (Referans ile Çağrı)](#pointer-ile-call-by-reference(referans-ile-çağrı))
### Pointer, Dizi ve Pointer Aritmetiği
### Pointer ile Dinamik Bellek Tahsisi (Dynamic Memory Allocation)
### Fonksiyon İşaretçisi (Function Pointer)
### Double Pointer
### Pointer Dönüşlü Fonksiyonlar
### Pointer ile Struct ve Enum
### File Pointer

## Pointer'ın Temel İşlevi
  
Bilgisayar ortamındaki her bir veri değeri, hafızada belli bir yer
kaplar: float ise 4 byte, char ise 1 byte, double ise 8 byte... Bir de
değerlerin kapladığı alanı temsil eden adresler vardır. Adresin temsil
ettiği bit değeri, mimariye göre değişiklik gösterir. Günümüzdeki
bilgisayarların çoğunda ise her bir adres, 1 byte'lık (8 bit'lik) bir
alanı temsil eder. Adresler onaltılık (hexadecimal) sisteme göre
numaralandırılır.

Nasıl ki temelde gördüğümüz her bir değişken bir değeri ifade eder,
pointer da o değişkenlerin adreslerini ifade eder. Bunun daha iyi
anlaşılması için aşağıdaki basit kodu ve tabloyu inceleyelim.

![1](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/43602725/e7db5d80-f872-48eb-9aff-fd4c3df41690)


Burada 42 sayısını atadığımız number değişkeninin adresine işaret edecek
bir pointer tanımlandığını görüyoruz. Tanımlama için öncelikle
pointer’ın veri tipi ile işaret edeceği adresteki değişkenin tipi aynı
olmalı. Bu, ileriki aşamalarda bahsedeceğimiz pointer aritmetiği ile
ilgili işlemlerde de bellek hatalarının olmaması için önemlidir. Yıldız
işareti (\*), işlemciye bir pointer atanacağını ifade eder. &number,
number değişkenin adres değeridir. Ampersand (&), isminin başına geldiği
değişkenin adresini belirtir. nPtr isimli pointer’a, number’ın adresini
atadığımıza göre printf ile ekrana çıktı gönderebiliriz:

![2](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/43602725/6ff80561-ff65-4b66-8b18-fc7391b538e4)

Gördüğümüz üzere nPtr pointer’ının bulunduğu adres, işaret ettiği adres
ve o adresin içeriğindeki değeri ekrana bastırdık. Evet, pointer da
aslında adresi olan bir değişkendir: Diğer bildiğimiz değişkenlerle
arasındaki fark ise değerinin bir adresi ifade etmesidir. Bunu daha iyi
kavramak için yukarıdaki kodda yaptığımızı basit bir tabloya aktarmak
mümkün:

![3](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/43602725/ed2e0e34-6362-4216-aa90-9ddf88031459)


Tablo, kodda 61fe1c adresindeki pointer’a, number’ın bulunduğu 61fe10
adresinin atandığını gösteriyor. Yani pointer, adres depolayan bir
değişken görevi görüyor.

## Pointer ile Call by Reference (Referans ile Çağrı)

Call by value (değer ile çağrı) işleminde fonksiyonlar, main’den aldığı
değişkenlerin değerini değiştiremez. Değişkenleri klonlayıp kopyaları
üzerinde işlemler yapar. Ama orijinal değişkenler üzerinde hiçbir etkisi
olmaz. Örnek olarak bir sayının faktöriyelini hesaplayan koda
bakalım.

![4](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/43602725/6715d7ad-df88-4452-9dd1-c9818ac6e3d1)

number değişkeninin bir kopyası fonksiyonda parametre olarak kullanıldı.
Görüldüğü üzere fonksiyon içindeki komutlar, verilen parametrenin
değerini değiştiriyor. Ama number değişkeni üzerinde hiçbir etkisi
olmuyor. Kodumuzu çalıştırıp sonucun nasıl olacağını birlikte görelim:

![5](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/43602725/873422ef-7d90-47c1-80d9-82fb1b7d34ee)

Evet, number değişkeni üzerinde hiçbir değişiklik yok. Peki ya
değişkenin adresini kopyalayıp parametre olarak kullansaydık? İşte bu,
doğrudan adresin içindeki bağımsız değişkene erişmemize olanak tanır.
Aynı kod üzerinde oynamalar yapalım:

![6](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/43602725/78316d34-ae63-4174-8578-a2aac478b383)

Gördüğünüz gibi buradaki fark, number’ın adres değerinin kopyasının
fonksiyondaki pointer’a atanmasıdır.

![7](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/43602725/6e43e432-8b52-43d9-8983-3ca719598a13)

Kodumuzu çalıştırdığımızda number değerinin değiştiğini fark ediyoruz.
Call by value ile call by reference arasındaki bu farkın nedeni, birden
fazla değişkenin “aynı” içeriğe sahip olabilmesi ama her birinin “farklı
ve tek” bir adreste bulunmasında gizli.

## Pointer, Dizi ve Pointer Aritmetiği

Diziler, birden fazla değeri temsil eden değişkenlerdir. Belirtilen
değer sayısı kadar statik bellek tahsisi yapılır. Örneğin double tipinde
dört değer tutan dizinin bellekte kapladığı alan 4xsizeof(double) = 32
byte olacaktır. Pointer ve dizinin belli başlı ortak yönleri vardır.
Beraber inceleyelim.

![8](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/43602725/303e4b7d-e401-4673-8492-53fbf7a56079)

Diziyi tanımladıktan sonra dizinin ismi olan arr2, bir adresi ifade
eder. Yukarıdaki a değişkenine atadığımız basit mantıksal işleme bakacak
olursak sonuç, 1 olacaktır. Çünkü arr2, birinci elemanın adresine işaret
eden bir pointer görevi görür. Bu yüzden birinci elemanın adresinin
içeriğini ifade eden \*arr2 ile arr2\[0\], aynı değeri ifade eder. Son
olarak arr2’nin tuttuğu adres değeri hexadecimal biçimde ekrana
yazdırılır. Ekran çıktısı:

![33](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/43602725/b62fceb6-18c3-4c71-bef9-562cf1e22660)


Bir de pointer aritmetiğine göz atalım. Hatırlarsanız, pointer’ın veri
tipinin bu hususta önemli olduğunu belirtmiştim. Evet, başlarda da
belirttiğimiz gibi günümüzdeki çoğu bilgisayarda adreslerin her biri 1
byte’lık bir alanı temsil eder. İnt tipinde tek bir değişken
tanımlayacak olursak 4 byte’lık bir alan kaplanacağı için değişken dört
adrese depolanır. Eğer int tipinde 6 elemanlık bir dizi tanımlayacak
olursak dizinin kapladığı alan 24 byte olacağı için bu bellek bloğunun
adres sayısı da 24 olacaktır. Her bir elemanı ise 4 adreslik alan
kaplayacaktır ve başlangıç adresleri içerikteki değeri temsil edecektir.
Pointer aritmetiği adresler arasında geçişler yapmamızı mümkün kılar.
Mesela 6 elemanlık bu dizide birinci elemanın adres değeri a ise, ikinci
elemanın adres değeri a+sizeof(int) olur. Bunun nedeni, birinci elemanın
adresine işaret eden pointer, int cinsinde tanımlı. Bu da birim artış
değerini int boyutuna çeviriyor. Aşağıdaki kod ve tabloyla daha iyi
anlaşılacak:

![555](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/43602725/3b4d50ba-b115-44d8-9cbf-ccd184c45309)


Dizinin ilk elemanının adresini belirten arr2’yi, aPtr isimli pointer’a
atadık (Bu atamayı yapmadan da dizinin ilk elemanın pointerı ile
geçişler yapabileceğinizi unutmayın. Ben daha anlaşılır olması açısından
bir pointer kullandım. Ama dizi ismi olan arr2, bir constant pointer
olduğu için sonradan adres değerinin değiştirilemeyeceğini ve bu yüzden
“arr2++” değil de sadece “arr2+(sayı)” şeklinde geçişler için
kullanabileceğimizi de hatırlatmakta fayda var) Burada her bir dizi
elemanının adres ve değerini sırasıyla ekrana yazdıracağız. Kodumuzu
çalıştıralım:

![9](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/43602725/1eba6fb9-ba13-4e87-944a-79d13a954948)

Tablomuz yardımıyla bellekteki işlemleri gözden geçirelim:

![66](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/43602725/ac905540-9030-475d-8fe1-8fa38e110032)

Evet, aPtr int türünde olduğu için “aPtr + (sayı)” ifadesi, “aPtr +
sizeof(int) \* (sayı)” ifadesine eş değer oluyor.

## Pointer ile Dinamik Bellek Tahsisi (Dynamic Memory Allocation) 

 

Önceden de belirtiğim gibi, bir dizi tanımladığınızda eğer dizinin uzunluğu belliyse o dizi için statik bellek
tahsisi yapılır. Mesela “int dizi[20];” ifadesini girdiğimizde bellekte 20*sizeof(int)=80 byte’lık bir bellek bloğu
tahsis edilir. Peki, önceden tanımlanmış dizi uzunluğundan daha az ya da daha fazla alana gerek duyarsak? Statik
bellek tahsisiyle program çalışırken önceden tanımlı olan uzunluğu değiştiremezsiniz. Ya herhangi bir değer
girmeden yalnızca kullanıcıdan istesek? Bu da mümkün değil. Çünkü program, dizinin uzunluğu belirtilmediği sürece
çalışmayacaktır. Ama eğer pointer yardımıyla dinamik bellek tahsisi prosedüründen yararlanırsak, yukarıdaki bu
sorunlar ortadan kalkacaktır. Burada yapacağımız uygulamalar pointer’ın işlevini anlamak amacını gütmekte. O yüzden
dinamik bellek tahsisi gibi uzun bir konunun detaylarına inmemiz uygun değil. Farklı kaynaklardan bu konunun
detaylarını araştırabilirsiniz. Öncelikle statik bellek tahsisi ile ilgili bahsettiğim iki durumu inceleyelim. 

![1 1](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/3fad935f-9112-4d39-854d-437ad8108367)

Gördüğünüz gibi bu durumda programın çalışması için öncelikle arr dizisinin uzunluğu belirtilmeli. 

 ![1](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/4a839eab-b54f-446b-b6cf-b51ac08d2231)


Bu sefer arr dizisinin uzunluğu kod çalıştırılmadan önce belirtildi. Bu durumda ise size’a verilen değer 5’ten
büyükse bellek ihlali meydana gelecek. Genellikle bu tür bellek ihlallerini C derleyicileri algılamaz. O yüzden
tahmin edilemeyecek türden sonuçlar meydana gelebilir. Programı çalıştıracak olursak: 

 ![2](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/ec42925e-0ba4-4df8-a7c2-2e70b63e9d58)


Görüldüğü gibi tanımladığımız dizinin uzunluğu 5 olmasına rağmen bir eleman fazla ekrana yazdırılmış. Farklı
uzunlukta tanımlı dizilerde bu fazlalık, farklı sonuçlar verecektir. Şimdi de dinamik bellek ayırma ile ilgili
uygulamalarımızı yapalım. 

 ![3](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/e0d644b8-397a-4604-8601-1e3a94fea0ab)


Bu programda ben dinamik bellek tahsisi için kullanılan dört fonksiyondan ikisi olan malloc() ve free()’yi
kullandım. Malloc() (memory allocation), bellek bloğunun (bu bellek bloğu bir array, structure vb. için ayrılmış
olabilir) boyutunun değerini ifade eden bir parametreye sahip olan ve dönüş değeri olarak bu ayrılan bellek
bloğunun başlangıç adresini döndüren bir fonksiyondur. Alternatifi olan calloc()’a (contiguous allocation) göre hız
açısından daha avantajlıdır. Ama calloc()’un aksine, ayrılan bloklara otomatik olarak herhangi bir başlangıç değeri
atanmadığı için atama yapılmadan okuma yapılırsa ekrana rastgele çöp değerleri dönecektir. O yüzden değerler
okunmadan önce atama yapılması gerekir. Free() fonksiyonu ise program çalıştırıldıktan sonra bellekteki ayrılan
bloğu serbest bırakır. Dinamik bellek tahsisi manuel olarak kontrol edildiği için bu konseptte bellekteki ayrılan
alanı serbest bırakma işleminin programcı tarafından yapılması gerekir. Aksi taktirde programda istem dışı sonuçlar
meydana gelebilir, bellek sızıntıları oluşabilir. Bu bilgiler ışığında programımızı incelersek ilk başta
istediğimiz “size” değeri ile int’in boyut değerinin çarpımı uzunluğunda bir bellek bloğu ayrılıyor. Malloc,
(void*) tipi olduğu için ptarr’e atanırken eşitliğin sağında başta (int*) tipine dönüştürüldü. Bu tipteki dönüşüm
bloğun int boyutuna göre içeriklere ayrılmasını sağlıyor. Böylelikle bellek bloğumuz, int tipinde bir diziyi temsil
etmiş oluyor. Bu dizinin her bir elemanını, diğer bir deyişle bellek bloğunun her bir içeriğini, birinci for
döngüsündeki komutlarla tanımlayabiliyoruz. İkinci for döngüsüyle de girilen değerleri ekrana yazdırıyoruz. Son
olarak free(ptarr) fonksiyonu ile ptarr için ayrılan bellek bloğunu serbest bırakıyoruz. Programımızı çalıştıralım: 

 ![4](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/cdc225f8-16c0-42ad-8c45-1d2b4ee367ce)


İşte dinamik bellek ayırma işlemiyle farklı boyutta verileri işlememiz mümkün. Ayrıca ilerleyen bölümlerde struct
kullanacağımız pointer uygulamalarında da dinamik bellek ayırmanın önemine bir kez daha değinmiş olacağız. 

 

## Fonksiyon İşaretçisi (Function Pointer) 

 

Fonksiyon işaretçisi, adı üstünde fonksiyona işaret eder. Kendisine fonksiyonun adresi atanan bir pointer, o
fonksiyondaki talimatlara erişim sağlar. 

 ![5](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/9cb43cb3-84a2-4790-a6be-3fd3df8c7e05)


Görmüş olduğumuz örnekte iki int tipinde parametreye sahip ve void tipinde extra fonksiyonu, kendisiyle özdeş bir
fonksiyon pointer’ına atandı. Bu atama sayesinde pointer’ı kullanarak istediğimiz parametre değerleriyle extra
fonksiyonundaki talimatlara erişebiliriz. 

 ![6](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/4c92deb7-3ec4-433a-a915-b8db7ae26d5f)


Kodu çalıştırdığımızda fonksiyon isminin bir adres değeri döndürdüğünü görüyoruz. Bu değer, fonksiyonun başlangıç
adresidir. Nasıl ki bir dizinin ismi, ilk elemanının adres değerini ifade ediyordu. Fonksiyon ismi de içindeki
kodların başlangıç adresini ifade ediyor. 

 

Fonksiyon pointer’ını tanımlarken dikkat edilmesi gereken noktalardan biri, parantez kullanılarak tanımlanmasıdır.
int *ptr(int,int) ifadesinde parantez olmadığı için derleyici bunu int tipinde pointer dönüşlü ve iki int tipinde
parametreye sahip bir fonksiyon olarak algılar. Eğer int (*ptr)(int,int) olarak kullanılırsa bir fonksiyon
işaretçisi tanımlanmış olur. Pointer dönüşlü fonksiyonları da ilerleyen bölümlerde inceleyeceğiz. 

 

Dizilere değinmişken, bir fonksiyon işaretçisini birden fazla fonksiyona sahip bir dizi şeklinde kullanabiliriz.
Bir örnek üzerinden inceleyelim. 

![9](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/192f309e-82d8-4136-9c18-dacc37fcf5c0)

Bu programda iki sayıyı karşılaştıran, ortalama değerlerini ve birinci sayının ikinci sayıya bölümünden kalanını
veren ve bu bilgileri ekrana yazdıran üç void tipi fonksiyonu, void tipi bir fonksiyon pointer’ına görülen sırayla
atadık. Bu sayede kullanıcıdan yapmak istediği işlemi ve kullanmak istediği iki sayıyı alıp değeri (*ptr[c])(a,b)
ifadesiyle ekrana yazdırabiliyoruz. 

 ![10](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/38704592-a917-40e9-aac6-b3c82b44cb50)
![12](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/a200fe71-19aa-423a-880b-774b4022d831)
![11](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/8363e85e-5109-4c7a-a4ff-acb20ff31b7c) 

Yukarıdaki görseller, üç işlevin ayrı ayrı çıktılarını ifade ediyor. 

Bir de fonksiyon işaretçisi sayesinde herhangi bir fonksiyonu farklı bir fonksiyonda parametre olarak
kullanabiliyor ve sonuç olarak döndürebiliyoruz. 
 
![13](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/b47154f9-91b1-425a-8fd1-68e573b689a7)
![14](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/1a78b9c4-5e20-45c1-8221-d80a5607a394)

Bu örnekte kullanıcının girmiş olduğu rakamın rastgele rakam barındıran b’nin değeriyle eşit olup olmadığını
belirliyoruz. “esitlikdurumu” fonksiyonu, doğru cevap verildiğinde “esit”, yanlış cevap verildiğinde “esitdegil”
fonksiyonunu parametre olarak alır. Bu işlem fonksiyon pointer’ı sayesinde mümkün olur. 

 

## Double Pointer 

 

Pointer’lar ötelenebilme özelliğine sahiptirler. Bir a değişkeninin pointer’ı a’ya işaret ettiği gibi a değişkenin
pointer’ının pointer’ı da a değişkeninin pointer’ına işaret eder. 

 ![15](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/06ab06b4-99ec-4044-9c18-008bf91d940a)

C dilinde pointer boyutunda belli bir sınır yoktur yalnız ne kadar fazla boyutta pointer kullanırsak bir o kadar da
programımız karmaşık ve hataya müsait olur. Biz burada örnek ve yaygın olması açısından double pointer’ları ele
alacağız. 

Double pointer’ın önemli uygulamalarından biri bellek ayırarak matris (iki boyutlu dizi) oluşturmaktır. Bir örnek
üzerinden inceleyelim. 

![16](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/1c1181cb-5a96-468d-ae7c-82d886134900)

Burada size_1 dizimizin satır sayısını, size_2 ise sütun sayısını ifade ediyor. malloc() fonksiyonunu kullanarak
öncelikle satır sayısı kadar her bir sütundaki elemanlara işaret edecek pointer’ı, dptr ismindeki double pointer
için ayırdık. Sonra bu ayrılan satır sayısı kadar pointer’a sütun sayısı kadar integer boyutunda bellek bloğu
ayırdık. Sonra da random algoritmamızın başlangıç değerini ifade eden srand() fonksiyonunu kullanıp programın her
başlatıldığı anda farklı bir değerin ekrana yazdırılması için mevcut POSIX zamanını (UNIX olarak da bilinir,
başlangıç tarihi 1 Ocak 1970’tir) saniye cinsinde tutan time(NULL) ifadesini seed olarak kullandık. Sonra iç içe
for döngüsü yardımıyla her bir matris (iki boyutlu dizi) elemanına sırasıyla random değerler atadık. Son olarak da
dinamik bellek ayırma sürecinde asla unutmamamız gerek fonksiyon olan free() ile dptr için ayrılan bellek
bloklarını serbest bıraktık. Programı çalıştırırsak: 

![17](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/0ffdaa51-db95-4b20-a66c-9f8d7e07faae)

Bu bellek ayırma işlemini char tipi bir iki boyutlu dizi için de yapabilir ve istediğimiz uzunlukta cümleler
oluşturabiliriz. 

Bir diğer önemli uygulamalarından biri de call by reference’dır. Call by reference, önceden de tanımladığımız gibi,
fonksiyon aracılığıyla bir değişkenin içeriğini değiştirmek için adresini parametre olarak kullanmaktır. 

![18](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/cd763e01-d28a-4e4f-aef6-35e0f18acd69)
 ![19](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/60b227d8-adeb-4ed9-81fb-67ab6cf5d9a2)

Bu örnekte c’nin adres değerini tutan cPtr ile d’nin adres değerini tutan dPtr’nin tuttukları adres değerlerini
değiştirdik. Bunu yapmak için degisim ismindeki void tipi ve iki double pointer parametreli fonksiyonu kullandık.



## Pointer Dönüşlü Fonksiyonlar 



Pointer dönüşlü fonksiyonlarda döndürülen değer pointer tipindedir. Söz dizimi, önceden de söz ettiğimiz gibi 
fonksiyon pointer’ından biraz farklı olarak şöyledir: type* ptr(type var1, type var2). 

 ![20](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/3535ce46-ebd3-49de-8b6a-c2f571712ced)


Burada int tipi bir dizinin eleman sayısı ve değerleri kullanıcıdan alınıyor. Eleman sayısı alındıktan sonra 
intmalloc fonksiyonunda int tipine uygun uzunlukta bir bellek bloğu ayrılıyor ve döndürülen ptr, 
ptarr’a atanarak ptarr için gerekli bellek tahsisi yapılmış oluyor. 

 
![21](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/9547f9ec-b9b2-4b8e-9346-11d638d97382)

 

## Pointer ile Struct ve Enum 

 

Structure’lar (kısacası struct) birbirinden farklı tipteki değişkenleri bir arada tutabilirler. Enumeration (kısacası enum) ise kullanıcı tarafından oluşturulan ve isimli tam sayıları tutan bir veri türüdür. 
Enum içindeki constant değişkenler, sabit ya da kullanıcı tarafından verilmiş değerlere sahip olabilir. Structure ve enumeration konularıyla ilgili eksiklik hissediyorsanız bu iki konuya göz attıktan sonra 
devam etmeniz daha iyi olabilir. Yine de yapacağım örnekler pointer’ın buradaki işlevinin anlaşılması açısından temel seviyede olacaktır ve bu konularla ilgili bazı noktalar özetlenecektir. 

 

Struct’ı kullanarak aşağıda görüldüğü gibi veri girişleri yapmak mümkün: 

 ![22](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/c3a6c26c-99bd-4f50-a441-8d87bd69a9a2)


Typedef, veri türünü isimlendirmemizi mümkün kılan bir işlevdir. “filmturu” ismindeki enum’ımızı “ft” ile, “film” 
ismindeki struct’ımızı da “f” ile adlandırdık. Struct’ımızı kullanmak için main’de film1 adlı değişkenimizi 
oluşturduk. 

 ![23](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/c4fc3c85-a245-4c30-8dae-1f05368a5b63)


Burada film1 değişkenindeki üyelere erişmek için “.” operatörünü kullandık. Öncelikle filmin adını atadık, bunu 
yapmak için string.h kütüphanesindeki strcpy() fonksiyonundan yararlandık. strcpy(), ikinci 
string değişkenini birinci string değişkenine kopyalamak için kullanılır. Bilindiği üzere C dilinde string 
ifadeler, char tipi dizilerdir. O yüzden direkt film1.filmadi= “The Dark Knight” gibi bir komutla 
atama yapmamız C dilinde mümkün değildir. Filmin türünü atarken de kısaca “ft” diye adlandırdığımız “filmturu” 
enum’ımızdaki Aksiyon ögesini kullandık. Burada “Aksiyon”, bir constant int değerini temsil eder 
ve değeri de “1”dir. Yani printf(“%s”,film1.tur) komutu doğru bir sonuç vermeyecektir. Bu yüzden enum ögelerinin
sahip oldukları ardışık değerleri switch-case'te kullanarak türü ekrana yazdırıyoruz. Bunu printtur fonksiyonunda 
gerçekleştiriyoruz. Filmin İMDb puanını tanımlarken float değişkeninden yararlandık. Yapım yılı için de int 
değişkeninden. Peki bunları bir pointer ile yapsak? 

 ![24](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/da914675-3eec-452d-ac5d-b418b1d42290)

Pointer ile struct içindeki değişkenlere değer atayacağımızda “->” operatörünü kullanıyoruz. Bu, pointer’ın işaret 
ettiği struct değişkenin içeriğini değiştirebilmemizi sağlar. Buradan yola çıkarsak “ptr->filmadi” ile 
“(*ptr).filmadi” ifadelerinin eşdeğerde olduğu sonucuna varabiliriz. 

 

Pointer, özellikle struct’ımızı kullanıp birden fazla değişken oluşturmak istediğimizde işimize yarayacaktır. 
Öncelikle statik bellek tahsisi ile bu uygulamayı yapalım. 

 ![25](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/8338635e-c29e-4e99-9849-e9c2ed00e464) 
![26](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/73d52dc1-4806-49f4-8ee1-28692e5eb95f)

Burada “size” değeri uzunluğunda f tipi bir dizi kullanarak statik bellek tahsisi yapıldı. Böylelikle dizinin 
başlangıç adresini ifade eden “film” değişkenini f tipi bir pointer’a atayıp pointer 
aritmetiğinden yararlandık. İlk döngünün sonunda getchar fonksiyonunu kullanmamızın nedeni, kullanıcının film yapım 
tarihini yazdıktan sonra enter’a tıkladığında fgets’in bunu bir veri olarak algılaması ve 
fonksiyondaki talimatları sonlandırmasıdır. Bu da birincisi hariç diğer filmlerin isimlerini giremememize neden 
olur. Enter’ı kullandığımızda “\n” verisi getchar tarafından algılanacak ve böylelikle fgets 
ile kullanıcıdan film ismi alınabilecek. İkinci döngüden önce birinci döngüde değerini arttırdığımız için ptr’ye 
dizinin başlangıç adresini tekrar atadık. 

 ![27](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/290b20d8-6d66-43e9-ba63-affd280f2ece)

Fark ettiyseniz alt satıra geçmek için kullandığımız “\n” karakterini 38. satırda kullanmamamıza rağmen her bir 
film adı ekrana yazdırıldıktan sonra bir satır altta yazdırma işlemi devam etmiş. Bunun nedeni,
yukarıda bahsedildiği üzere, fgets fonksiyonunun enter’ı veri olarak saymasıdır. Kullanıcı film isimlerini girip 
enter’a bastığında fonksiyon bunu sonlandırıcı karakter olarak algılar ve dizenin son
karakteri “\n” olur. 
 
Şimdi de dinamik bellek tahsisiyle kaç filmin bilgisi girileceğini kullanıcıya soralım. 
 
![28](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/4ecf83ee-57c4-49eb-81aa-f71f805152f4)

Önceki dinamik bellek tahsisi uygulamalarımızdan hatırlarsanız öncelikle kullanıcıdan kaç bellek bloğu ayırmak 
istediğini öğreniyoruz. (Girilen değer)x(struct’ımızın boyutu) kadar bloğu ptr için ayırıyoruz.
Ayrıca kullanıcı değeri girip enter’a tıkladığında fgets bunu algılayacağı için araya getchar işlevini 
yerleştiriyoruz. 



## File Pointer 


 
Herhangi bir veriyi dosyaya kaydetmek, dosyadaki verileri okumak gibi dosya giriş-çıkış işlemlerinde de pointer’a 
ihtiyaç duyarız. File pointer; dosyanın ismi, konumu, modu (“w”, “w+”, “a” gibi) ve dosyadaki
anlık konum gibi bilgilerini depolar. Derleyicideki söz dizimi şu şekildedir: 

FILE* ptr; 

Burada FILE, dosya işaretçisi için önceden tanımlı olan yapının typedef ile belirlenen adıdır. Öncelikle structure 
ve enumeration konularında olduğu gibi bu konunun da detaylarına inmemiz uygun değil.
İsteyenler, birçok mod ve işlevi barındıran bu konuyu ayrıca araştırabilir. Biz burada pointer’a bakan kısmını 
kısaca inceleyeceğiz. 

File pointer’ın işlevlerinden biri olan dosyaya veri aktarma uygulamasının basit bir örneğini inceleyelim: 
 
![29](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/9b766d9e-b7a1-4c7c-9c2b-b455ea13b0c8)

Örnekte tanımlanan fPtr file pointer’ımız, dosya üzerinde farklı modlarla işlem yapabilmek için kullandığımız fopen 
işlevinde belirtilen moda bağlı davranışlarda bulunur. Örneğin buradaki “w” (write) modu;
file pointer’ın eğer dosya mevcut değilse dosyayı oluşturmasına, mevcutsa dosyanın başlangıç konumundan 
başlamasına, eğer mevcut dosyada içerik de mevcutsa yazma işleminde içeriğin üzerine yazmasına, bu
işlem her devam ettiğinde dosyadaki konumunu ilerletmesine ve okuma işlemini yapamamasına neden olur. 
 
![30](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/c01b2808-e712-44c0-8c1f-b622814022b3)

Gördüğünüz gibi burada otomatik olarak projemizin bulunduğu klasörde dosyamız oluştu. İstersek dosyanın konumunu 
farklı yapabiliriz. Bunun için hedef klasörü dosyamızdan önce belirtmemiz yeterli olacaktır. 

Pointer’ın dosya işlemlerindeki işlevini göz attığımıza göre şimdi de bir önceki konumuzda bulunan struct ve enum 
ile ilgili dinamik bellek tahsisi uygulamamıza file pointer’ı ekleyerek girilen verileri bir
dosyaya yazdıralım. 

 ![31](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/22ba7d56-7874-449f-8598-bfce7b60c2d1)
![32](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/4db7b251-34f6-4879-8c50-62c929744f7a)


Burada yeni bir dosyaya verileri yazdırmak için “w” modunu kullandık. Yazdırma işlemi için de fprintf fonksiyonunu 
kullandık. Program devam ettiği sürece hazır dosya işlemleri arabellekte tutulur. Program
sonlanınca da işlemler diskte uygulanır. Yalnız burada free() fonksiyonu fclose()’dan önce geldiği için ayrılan 
bellek blokları serbest bırakılır ve işlemdeki veriler kaybolur. Bu yüzden de açılan dosya boş
olur. Tabii ki bu problem, free() fonksiyonunu fclose()’dan sonra yazmakla çözülebilir. Ama ben burada dosyanın her 
bir film bilgisinin sırasıyla işlemleri tamamlanınca dosyaya yazdırılmasını istediğim için for döngüsünün sonunda 
ffllush() fonksiyonunu kullandım. Böylelikle her bir for döngüsünün sonunda ara bellekteki işlemler diskteki 
dosyaya uygulanıyor. 

Konsola girilen değerler: 

 ![33](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/bd8c9833-ea09-41f2-acd9-3a70ac7ba96b)

Dosya içeriği: 

 ![34](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/2c61ea93-d35f-4298-bd42-767bb1b495f9)

Pointer ile ilgili anlatacaklarım bu kadardı. Buraya kadar konuyla ilgili herhangi bir soru, eleştri veya katkınız
olursa bana asbalandi@gmail.com adresinden ulaşabilirsiniz. 
