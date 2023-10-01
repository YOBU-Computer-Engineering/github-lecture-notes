# Pointer 101
---

Pointer; C/C++ dillerinde önemli bir yere sahip ve uzmanlaşmamız için
biraz efor sarfetmemiz gereken bir konudur. Ama temel mantığını
anladığımız sürece bize oldukça kolaylıklar sağlayacaktır. Bu yazımızda,
birlikte pointer ile ilgili önemli işlevleri ve o işlevleri kavramamıza
yardımcı olacak örnekleri gözden geçireceğiz.

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
