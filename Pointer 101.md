# Pointer 101
---

Pointer; C/C++ dillerinde önemli bir yere sahip ve uzmanlaşmamız için efor sarfetmemiz gereken bir konudur. Ama temel mantığını
anladığımız sürece bize oldukça kolaylıklar sağlayacaktır. Bu yazımızda,
birlikte pointer ile ilgili önemli işlevleri ve o işlevleri kavramamıza
yardımcı olacak örnekleri gözden geçireceğiz.

Yazı içindeki dilediğiniz konulara buradaki başlıkların üstüne tıklayarak ulaşabilirsiniz:

### [-Pointer'ın Temel İşlevi](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/blob/main/Pointer%20101.md#pointer%C4%B1n-temel-i%CC%87%C5%9Flevi)
### [-Pointer ile Call by Reference (Referans ile Çağrı)](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/blob/main/Pointer%20101.md#pointer-ile-call-by-reference-referans-ile-%C3%A7a%C4%9Fr%C4%B1)
### [-Pointer, Dizi ve Pointer Aritmetiği](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/blob/main/Pointer%20101.md#pointer-dizi-ve-pointer-aritmeti%C4%9Fi)
### [-Pointer ile Dinamik Bellek Tahsisi (Dynamic Memory Allocation)](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/blob/main/Pointer%20101.md#pointer-ile-dinamik-bellek-tahsisi-dynamic-memory-allocation)
### [-Fonksiyon İşaretçisi (Function Pointer)](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/blob/main/Pointer%20101.md#fonksiyon-i%CC%87%C5%9Faret%C3%A7isi-function-pointer)
### [-Double Pointer](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/blob/main/Pointer%20101.md#double-pointer)
### [-Pointer Dönüşlü Fonksiyonlar](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/blob/main/Pointer%20101.md#pointer-d%C3%B6n%C3%BC%C5%9Fl%C3%BC-fonksiyonlar)
### [-Pointer ile Struct ve Enum](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/blob/main/Pointer%20101.md#pointer-ile-struct-ve-enum)
### [-File Pointer](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/blob/main/Pointer%20101.md#file-pointer)

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

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
    int number=42;
    int* nPtr;
    nPtr=&number;
    printf("nPtr's address: %x \n",&nPtr);
    printf("address pointed to by nPtr: %x \ncontent of address: %d \n",nPtr,*nPtr);
    return 0;
}
```

Burada 42 sayısını atadığımız number değişkeninin adresine işaret edecek
bir pointer tanımlandığını görüyoruz. Tanımlama için öncelikle
pointer’ın veri tipi ile işaret edeceği adresteki değişkenin tipi aynı
olmalı. Bu, ileriki aşamalarda bahsedeceğimiz pointer aritmetiği ile
ilgili işlemlerde de bellek hatalarının olmaması için önemlidir. Yıldız
işareti (\*), işlemciye bir pointer atanacağını ifade eder. &number,
number değişkenin adres değeridir. Ampersand (&), isminin başına geldiği
değişkenin adresini belirtir. nPtr isimli pointer’a, number’ın adresini
atadığımıza göre printf ile ekrana çıktı gönderebiliriz:

```console
nPtr's address: 61fe10
address pointed to by nPtr: 61fe1c
content of address: 42
```

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

```c
#include <stdio.h>
#include <stdlib.h>

int fact(int);

int main()
{
    int number=4;
    printf("number value before function runs: %d\n",number);
    int result=fact(number);
    printf("%d \n", result);
    printf("number value after function runs: %d\n", number);
    return 0;
}

int fact(int a){
    int p=a;
    for(int i=1;i<p;i++){
        a*=i;
    }
    return a;
}
```

number değişkeninin bir kopyası fonksiyonda parametre olarak kullanıldı.
Görüldüğü üzere fonksiyon içindeki komutlar, verilen parametrenin
değerini değiştiriyor. Ama number değişkeni üzerinde hiçbir etkisi
olmuyor. Kodumuzu çalıştırıp sonucun nasıl olacağını birlikte görelim:

```console
number value before function runs: 4
24
number value after function runs: 4
```


Evet, number değişkeni üzerinde hiçbir değişiklik yok. Peki ya
değişkenin adresini kopyalayıp parametre olarak kullansaydık? İşte bu,
doğrudan adresin içindeki bağımsız değişkene erişmemize olanak tanır.
Aynı kod üzerinde oynamalar yapalım:

```c
#include <stdio.h>
#include <stdlib.h>

int fact(int*);

int main()
{
    int number=4;
    printf("number value before function runs: %d\n",number);
    int result=fact(&number);
    printf("result: %d\n",result);
    printf("number value after function runs: %d\n",number);
    return 0;
}

int fact(int* a) {
    int p= *a;
    for(int i=1;i<p;i++){
        *a *=i;
    }
    return *a;
}
```

Gördüğünüz gibi buradaki fark, number’ın adres değerinin kopyasının
fonksiyondaki pointer’a atanmasıdır.

```console
number value before function runs: 4
result: 24
number value after function runs: 24
```

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

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
    int arr2[6]={1,2,3,4,5,6};
    int a=(*arr2==arr2[0]);
    printf("%d\n",a);
    printf("%x",arr2);
    return 0;
}
```

Diziyi tanımladıktan sonra dizinin ismi olan arr2, bir adresi ifade
eder. Yukarıdaki a değişkenine atadığımız basit mantıksal işleme bakacak
olursak sonuç, 1 olacaktır. Çünkü arr2, birinci elemanın adresine işaret
eden bir pointer görevi görür. Bu yüzden birinci elemanın adresinin
içeriğini ifade eden \*arr2 ile arr2\[0\], aynı değeri ifade eder. Son
olarak arr2’nin tuttuğu adres değeri hexadecimal biçimde ekrana
yazdırılır. Ekran çıktısı:

```console
1
61fe00
```

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

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
    int arr2[6]={1,2,3,4,5,6};
    int* aPtr=arr2;
    for(int i=0;i<6 ;i++){
            printf("address of arr2[%d]: %x\n",i,aPtr+i);
            printf("value of arr2[%d]: %d\n\n",i,*(aPtr+i));
    }
    return 0;
}
```

Dizinin ilk elemanının adresini belirten arr2’yi, aPtr isimli pointer’a
atadık (Bu atamayı yapmadan da dizinin ilk elemanın pointerı ile
geçişler yapabileceğinizi unutmayın. Ben daha anlaşılır olması açısından
bir pointer kullandım. Ama dizi ismi olan arr2, bir constant pointer
olduğu için sonradan adres değerinin değiştirilemeyeceğini ve bu yüzden
“arr2++” değil de sadece “arr2+(sayı)” şeklinde geçişler için
kullanabileceğimizi de hatırlatmakta fayda var) Burada her bir dizi
elemanının adres ve değerini sırasıyla ekrana yazdıracağız. Kodumuzu
çalıştıralım:

```console
address of arr2[0]: 61fdf0
value of arr2[0]: 1

address of arr2[1]: 61fdf4
value of arr2[1]: 2

address of arr2[2]: 61fdf8
value of arr2[2]: 3

address of arr2[3]: 61fdfc
value of arr2[3]: 4

address of arr2[4]: 61fe00
value of arr2[4]: 5

address of arr2[5]: 61fe04
value of arr2[5]: 6
```

Tablomuz yardımıyla bellekteki işlemleri gözden geçirelim:

![66](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/43602725/ac905540-9030-475d-8fe1-8fa38e110032)

Evet, aPtr int türünde olduğu için ```aPtr + (sayı)``` ifadesi, ```aPtr +
sizeof(int) \* (sayı)``` ifadesine eş değer oluyor.

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

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
    int size;
    int arr[]; //error: array size missing in 'arr'
    
    printf("Dizimiz kac elemanli olsun? -> ");
    scanf("%d",&size);
    for(int i=0;i<size-1;i++){
        arr[i]=i+1;
    }
    for(int i=0;i<size;i++){
        printf("arr[%d]=%d\n",i,arr[i]);
    }
    return 0;
}
```

Gördüğünüz gibi bu durumda programın çalışması için öncelikle arr dizisinin uzunluğu belirtilmeli. 

 ```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
    int size;
    int arr[5];
    
    printf("Dizimiz kac elemanli olsun? -> ");
    scanf("%d",&size);
    for(int i=0;i<size-1;i++){
        arr[i]=i+1;
    }
    for(int i=0;i<size;i++){
        printf("arr[%d]=%d\n",i,arr[i]);
    }
    return 0;
}
```


Bu sefer arr dizisinin uzunluğu kod çalıştırılmadan önce belirtildi. Bu durumda ise size’a verilen değer 5’ten
büyükse bellek ihlali meydana gelecek. Genellikle bu tür bellek ihlallerini C derleyicileri algılamaz. O yüzden
tahmin edilemeyecek türden sonuçlar meydana gelebilir. Programı çalıştıracak olursak: 

```console
Dizimiz kac elemanli olsun? -> 7
arr[0]=1
arr[1]=2
arr[2]=3
arr[3]=4
arr[4]=5
arr[5]=6
```

Görüldüğü gibi tanımladığımız dizinin uzunluğu 5 olmasına rağmen bir eleman fazla ekrana yazdırılmış. Farklı
uzunlukta tanımlı dizilerde bu fazlalık, farklı sonuçlar verecektir. Şimdi de dinamik bellek ayırma ile ilgili
uygulamalarımızı yapalım.

```c
 #include <stdio.h>
#include <stdlib.h>

int main()
{
    int size;
    printf("Dizinin eleman sayisini giriniz: ");
    scanf("%d",&size);
    int* ptarr=(int*)malloc(size*sizeof(int));

    if(ptarr==NULL)
        printf("Bellek tahsisi basarisiz.");

    for(int i=0;i<size;i++){
        ptarr[i]=2*i;
    }

    for(int i=0;i<size;i++){
        printf("ptarr[%d] = %d\n",i,ptarr[i]);
    }

    free(ptarr);

    return 0;
}
```


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

```console
Dizinin eleman sayisini giriniz: 7
ptarr[0] = 0
ptarr[1] = 2
ptarr[2] = 4
ptarr[3] = 6
ptarr[4] = 8
ptarr[5] = 10
ptarr[6] = 12
```

İşte dinamik bellek ayırma işlemiyle farklı boyutta verileri işlememiz mümkün. Ayrıca ilerleyen bölümlerde struct
kullanacağımız pointer uygulamalarında da dinamik bellek ayırmanın önemine bir kez daha değinmiş olacağız. 

 

## Fonksiyon İşaretçisi (Function Pointer) 

 

Fonksiyon işaretçisi, adı üstünde fonksiyona işaret eder. Kendisine fonksiyonun adresi atanan bir pointer, o
fonksiyondaki talimatlara erişim sağlar. 

```c
#include <stdio.h>
#include <stdlib.h>

void extra(int,int);

int main()
{
    void (*funcptr)(int,int);
    funcptr=extra;
    funcptr(5,4);
    printf("address: %x\n",extra);
    return 0;
}

void extra(int a,int b){
    int c=a-b;
    printf("result: %d\n",c);
}
```

Görmüş olduğumuz örnekte iki int tipinde parametreye sahip ve void tipinde extra fonksiyonu, kendisiyle özdeş bir
fonksiyon pointer’ına atandı. Bu atama sayesinde pointer’ı kullanarak istediğimiz parametre değerleriyle extra
fonksiyonundaki talimatlara erişebiliriz. 

```console
result: 1
address: 401596
```

Kodu çalıştırdığımızda fonksiyon isminin bir adres değeri döndürdüğünü görüyoruz. Bu değer, fonksiyonun başlangıç
adresidir. Nasıl ki bir dizinin ismi, ilk elemanının adres değerini ifade ediyordu. Fonksiyon ismi de içindeki
kodların başlangıç adresini ifade ediyor. 

 

Fonksiyon pointer’ını tanımlarken dikkat edilmesi gereken noktalardan biri, parantez kullanılarak tanımlanmasıdır.
int *ptr(int,int) ifadesinde parantez olmadığı için derleyici bunu int tipinde pointer dönüşlü ve iki int tipinde
parametreye sahip bir fonksiyon olarak algılar. Eğer int (*ptr)(int,int) olarak kullanılırsa bir fonksiyon
işaretçisi tanımlanmış olur. Pointer dönüşlü fonksiyonları da ilerleyen bölümlerde inceleyeceğiz. 

 

Dizilere değinmişken, bir fonksiyon işaretçisini birden fazla fonksiyona sahip bir dizi şeklinde kullanabiliriz.
Bir örnek üzerinden inceleyelim.

```c
#include <stdio.h>
#include <stdlib.h>

void remainder(int,int);
void compare(int,int);
void average(int,int);

int main()
{
    void (*ptr[])(int,int)={remainder,compare,average};
    int a,b,c;
    printf("enter your action (0:remainder, 1:compare, 2:average): ");
    scanf("%d",&c);
    printf("enter first number: ");
    scanf("%d",&a);
    printf("enter second number: ");
    scanf("%d",&b);
    (*ptr[c])(a,b);
    return 0;
}

void remainder(int n1,int n2){
    int r=n1%n2;
    printf("reminder: %d",r);
}
void compare(int n1,int n2){
    if(n1>n2)
        printf("%d is greater than %d",n1,n2);
    else if(n1<n2)
        printf("%d is greater than %d",n2,n1);
    else
        printf("both numbers are equal.");
}
void average(int n1,int n2){
    float r=(float)(n1+n2)/2;
    printf("average value: %f",r);
}
```

Bu programda iki sayıyı karşılaştıran, ortalama değerlerini ve birinci sayının ikinci sayıya bölümünden kalanını
veren ve bu bilgileri ekrana yazdıran üç void tipi fonksiyonu, void tipi bir fonksiyon pointer’ına görülen sırayla
atadık. Bu sayede kullanıcıdan yapmak istediği işlemi ve kullanmak istediği iki sayıyı alıp değeri (*ptr[c])(a,b)
ifadesiyle ekrana yazdırabiliyoruz. 

```console
enter your action (0:remainder, 1:compare, 2:average): 0
enter first number: 8
enter second number: 5
reminder: 3
```
```console
enter your action (0:remainder, 1:compare, 2:average): 1
enter first number: 1
enter second number: 1
both numbers are equal.
```
```console
enter your action (0:remainder, 1:compare, 2:average): 2
enter first number: 33
enter second number: 56
average value: 44.500000
```

Yukarıdaki görseller, üç işlevin ayrı ayrı çıktılarını ifade ediyor. 

Bir de fonksiyon işaretçisi sayesinde herhangi bir fonksiyonu farklı bir fonksiyonda parametre olarak
kullanabiliyor ve sonuç olarak döndürebiliyoruz. 

```c
#include <stdio.h>
#include <stdlib.h>
void esit();
void esitdegil();
void esitlikdurumu(void (*funptr)());
int main()
{
    int a;
    printf("bir rakam giriniz: ");
    scanf("%d",&a);
    srand(time(NULL));
    int b=rand()%10;
    printf("istenilen rakam: %d\n", b);
    if(a==b)
        esitlikdurumu(esit);
    else
        esitlikdurumu(esitdegil);

    return 0;
}
void esit(){
    printf("Tebrikler, dogru rakamý girdiniz.\n");
}
void esitdegil(){
    printf("Yanlis rakam, tekrar deneyiz.\n");
}
void esitlikdurumu(void (*funptr)()){
    funptr();
}
```
```console
bir rakam giriniz: 4
istenilen rakam: 8
Yanlis rakam, tekrar deneyiz.
```
Bu örnekte kullanıcının girmiş olduğu rakamın rastgele rakam barındıran b’nin değeriyle eşit olup olmadığını
belirliyoruz. “esitlikdurumu” fonksiyonu, doğru cevap verildiğinde “esit”, yanlış cevap verildiğinde “esitdegil”
fonksiyonunu parametre olarak alır. Bu işlem fonksiyon pointer’ı sayesinde mümkün olur. 

 

## Double Pointer 

 

Pointer’lar ötelenebilme özelliğine sahiptirler. Bir a değişkeninin pointer’ı a’ya işaret ettiği gibi a değişkenin
pointer’ının pointer’ı da a değişkeninin pointer’ına işaret eder. 

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
    int a=12;
    int* aPtr=&a;
    int** aPtr_ptr=&aPtr;
    int*** aPtr_ptr_ptr=&aPtr_ptr;

    printf("a'nin degeri: %d\n",a);
    printf("pointer araciligiyla a'nin degeri: %d\n",*aPtr);
    printf("double pointer araciligiyla a'nin degeri: %d\n",**aPtr_ptr);
    printf("triple pointer araciligiyla a'nin degeri: %d\n",***aPtr_ptr_ptr);

    return 0;
}
```
```console
a'nin degeri: 12
pointer araciligiyla a'nin degeri: 12
double pointer araciligiyla a'nin degeri: 12
triple pointer araciligiyla a'nin degeri: 12
```

C dilinde pointer boyutunda belli bir sınır yoktur yalnız ne kadar fazla boyutta pointer kullanırsak bir o kadar da
programımız karmaşık ve hataya müsait olur. Biz burada örnek ve yaygın olması açısından double pointer’ları ele
alacağız. 

Double pointer’ın önemli uygulamalarından biri bellek ayırarak matris (iki boyutlu dizi) oluşturmaktır. Bir örnek
üzerinden inceleyelim.

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main()
{
    int** dptr,size_1,size_2;
    printf("Satir sayisini giriniz: ");
    scanf("%d",&size_1);
    printf("Sutun sayisini giriniz: ");
    scanf("%d",&size_2);

    dptr=(int**)malloc(sizeof(int*)*size_1);

    for(int i=0;i<size_1;i++){
        dptr[i]=(int*)malloc(sizeof(int)*size_2);
    }

    srand(time(NULL));

    for(int i=0;i<size_1;i++){
        for(int j=0;j<size_2;j++){
            dptr[i][j]=rand();
            printf("%d\t",dptr[i][j]);
        }
        printf("\n");
    }

    free(dptr);

    return 0;
}
```

Burada size_1 dizimizin satır sayısını, size_2 ise sütun sayısını ifade ediyor. malloc() fonksiyonunu kullanarak
öncelikle satır sayısı kadar her bir sütundaki elemanlara işaret edecek pointer’ı, dptr ismindeki double pointer
için ayırdık. Sonra bu ayrılan satır sayısı kadar pointer’a sütun sayısı kadar integer boyutunda bellek bloğu
ayırdık. Sonra da random algoritmamızın başlangıç değerini ifade eden srand() fonksiyonunu kullanıp programın her
başlatıldığı anda farklı bir değerin ekrana yazdırılması için mevcut POSIX zamanını (UNIX olarak da bilinir,
başlangıç tarihi 1 Ocak 1970’tir) saniye cinsinde tutan time(NULL) ifadesini seed olarak kullandık. Sonra iç içe
for döngüsü yardımıyla her bir matris (iki boyutlu dizi) elemanına sırasıyla random değerler atadık. Son olarak da
dinamik bellek ayırma sürecinde asla unutmamamız gerek fonksiyon olan free() ile dptr için ayrılan bellek
bloklarını serbest bıraktık. Programı çalıştırırsak: 

```console
Satir sayisini giriniz: 4
Sutun sayisini giriniz: 4
17079   10313   1861    23535
26371   20669   16417   17388
31631   8396    19733   13036
17732   30416   19430   25895
```

Bu bellek ayırma işlemini char tipi bir iki boyutlu dizi için de yapabilir ve istediğimiz uzunlukta cümleler
oluşturabiliriz. 

Bir diğer önemli uygulamalarından biri de call by reference’dır. Call by reference, önceden de tanımladığımız gibi,
fonksiyon aracılığıyla bir değişkenin içeriğini değiştirmek için adresini parametre olarak kullanmaktır. 

```c
#include <stdio.h>
#include <stdlib.h>

void degisim(int**,int**);

int main() {
    int c=3;
    int d=7;
    int *cPtr=&c;
    int *dPtr=&d;

    printf("cPtr degiskenin\nilk tuttugu adres: %x\nadresin icerigi: %d\n\n",cPtr,*cPtr);
    printf("dPtr degiskenin\nilk tuttugu adres: %x\nadresin icerigi: %d\n\n",dPtr,*dPtr);

    degisim(&cPtr,&dPtr);

    printf("cPtr degiskenin\nson tuttugu adres: %x\nadresin icerigi: %d\n\n",cPtr,*cPtr);
    printf("dPtr degiskenin\nson tuttugu adres: %x\nadresin icerigi: %d\n\n",dPtr,*dPtr);

    return 0;
}

void degisim(int**ptr1,int**ptr2){
    int* gecici;
    gecici=*ptr1;
    *ptr1=*ptr2;
    *ptr2=gecici;
}
```

```console
cPtr degiskenin
ilk tuttugu adres: 61fe1c
adresin icerigi: 3

dPtr degiskenin
ilk tuttugu adres: 61fe18
adresin icerigi: 7

cPtr degiskenin
son tuttugu adres: 61fe18
adresin icerigi: 7

dPtr degiskenin
son tuttugu adres: 61fe1c
adresin icerigi: 3
```

Bu örnekte c’nin adres değerini tutan cPtr ile d’nin adres değerini tutan dPtr’nin tuttukları adres değerlerini
değiştirdik. Bunu yapmak için degisim ismindeki void tipi ve iki double pointer parametreli fonksiyonu kullandık.



## Pointer Dönüşlü Fonksiyonlar 



Pointer dönüşlü fonksiyonlarda döndürülen değer pointer tipindedir. Söz dizimi, önceden de söz ettiğimiz gibi 
fonksiyon pointer’ından biraz farklı olarak şöyledir: type* ptr(type var1, type var2). 

```c
#include <stdio.h>
#include <stdlib.h>

int* intmalloc(int);

int main()
{
    int* ptarr, number;
    printf("Dizinizin eleman sayisini giriniz: ");
    scanf("%d",&number);
    ptarr=intmalloc(number);

    for(int i=0;i<number;i++){
        printf("%d. elemanin degerini giriniz: ",i+1);
        scanf("%d",ptarr+i);
    }
    printf("\n");
    for(int i=0;i<number;i++){
        printf("%d. elemanin degeri: %d\n",i+1,ptarr[i]);
    }
    free(ptarr);

    return 0;
}

int* intmalloc(int size){
    int* ptr=(int*)malloc(sizeof(int)*size);
    return ptr;
}
```

Burada int tipi bir dizinin eleman sayısı ve değerleri kullanıcıdan alınıyor. Eleman sayısı alındıktan sonra 
intmalloc fonksiyonunda int tipine uygun uzunlukta bir bellek bloğu ayrılıyor ve döndürülen ptr, 
ptarr’a atanarak ptarr için gerekli bellek tahsisi yapılmış oluyor. 

 
```console
Dizinizin eleman sayisini giriniz: 5
1. elemanin degerini giriniz: 3245
2. elemanin degerini giriniz: 234
3. elemanin degerini giriniz: 23
4. elemanin degerini giriniz: 7
5. elemanin degerini giriniz: 645

1. elemanin degeri: 3245
2. elemanin degeri: 234
3. elemanin degeri: 23
4. elemanin degeri: 7
5. elemanin degeri: 645
```


## Pointer ile Struct ve Enum 

 

Structure’lar (kısacası struct) birbirinden farklı tipteki değişkenleri bir arada tutabilirler. Enumeration (kısacası enum) ise kullanıcı tarafından oluşturulan ve isimli tam sayıları tutan bir veri türüdür. 
Enum içindeki constant değişkenler, sabit ya da kullanıcı tarafından verilmiş değerlere sahip olabilir. Structure ve enumeration konularıyla ilgili eksiklik hissediyorsanız bu iki konuya göz attıktan sonra 
devam etmeniz daha iyi olabilir. Yine de yapacağım örnekler pointer’ın buradaki işlevinin anlaşılması açısından temel seviyede olacaktır ve bu konularla ilgili bazı noktalar özetlenecektir. 

 

Struct’ı kullanarak aşağıda görüldüğü gibi veri girişleri yapmak mümkün: 

 ```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef enum filmturu{Aksiyon=1, BilimKurgu, Gerilim, Dram, Korku, Komedi}ft;

typedef struct film{
    char filmadi[30];
    ft tur;
    float imdb;
    int yapimyili;
}f;
void printtur(ft);
int main()
{
    f film1;
    return 0;
}
```


Typedef, veri türünü isimlendirmemizi mümkün kılan bir işlevdir. “filmturu” ismindeki enum’ımızı “ft” ile, “film” 
ismindeki struct’ımızı da “f” ile adlandırdık. Struct’ımızı kullanmak için main’de film1 adlı değişkenimizi 
oluşturduk. 

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef enum filmturu{Aksiyon=1, BilimKurgu, Gerilim, Dram, Korku, Komedi}ft;

typedef struct film{
    char filmadi[30];
    ft tur;
    float imdb;
    int yapimyili;
}f;
void printtur(ft);
int main()
{
    f film1;

    strcpy(film1.filmadi,"The Dark Knight");
    film1.tur=Aksiyon;
    film1.imdb=9.0f;
    film1.yapimyili=2008;

    printf("%s\n%d\n%.1f\n",film1.filmadi,film1.yapimyili,film1.imdb);
    printtur(film1.tur);

    return 0;
}

void printtur(ft tur){
    switch(tur){
        case 1:printf("Aksiyon");break;
        case 2:printf("Bilim Kurgu");break;
        case 3:printf("Gerilim");break;
        case 4:printf("Dram");break;
        case 5:printf("Korku");break;
        case 6:printf("Komedi");break;
    }
    printf("\n");
```

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

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef enum filmturu{Aksiyon=1, BilimKurgu, Gerilim, Dram, Korku, Komedi}ft;

typedef struct film{
    char filmadi[30];
    ft tur;
    float imdb;
    int yapimyili;
}f;

void printtur(ft);

int main()
{
    f film1;
    f *ptr=&film1;

    strcpy(ptr->filmadi,"The Dark Knight");
    ptr->tur=Aksiyon;
    ptr->imdb=9.0;
    ptr->yapimyili=2008;

    printf("Filmin adi: %s\nYapim yili: %d\nIMDb puani: %.1f\nTuru: ",ptr->filmadi,ptr->yapimyili,ptr->imdb);
    printtur(ptr->tur);


    return 0;
}
void printtur(ft tur){
    switch(tur){
        case 1:printf("Aksiyon");break;
        case 2:printf("Bilim Kurgu");break;
        case 3:printf("Gerilim");break;
        case 4:printf("Dram");break;
        case 5:printf("Korku");break;
        case 6:printf("Komedi");break;
        default:printf("Tanimsiz"); break;
    }
    printf("\n");
}
```

```console
Filmin adi: The Dark Knight
Yapim yili: 2008
IMDb puani: 9.0
Turu: Aksiyon
```

Pointer ile struct içindeki değişkenlere değer atayacağımızda “->” operatörünü kullanıyoruz. Bu, pointer’ın işaret 
ettiği struct değişkenin içeriğini değiştirebilmemizi sağlar. Buradan yola çıkarsak “ptr->filmadi” ile 
“(*ptr).filmadi” ifadelerinin eşdeğerde olduğu sonucuna varabiliriz. 

Pointer, özellikle struct’ımızı kullanıp birden fazla değişken oluşturmak istediğimizde işimize yarayacaktır. 
Öncelikle statik bellek tahsisi ile bu uygulamayı yapalım. 

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef enum filmturu{Aksiyon=1, BilimKurgu, Gerilim, Dram, Korku, Komedi}ft;

typedef struct film{
    char filmadi[30];
    ft tur;
    float imdb;
    int yapimyili;
}f;

void printtur(ft);

int main()
{
    int size=5;
    f film[size];
    f* ptr=film;

    for(int i=0;i<size;i++,ptr++){
        printf("----- %d. Film -----\n",i+1);
        printf("Filmin adi: ");
        fgets(ptr->filmadi,sizeof(ptr->filmadi),stdin);
        printf("Filmin Turu(1: Aksiyon, 2: BilimKurgu, 3: Gerilim, 4: Dram, 5: Korku, 6: Komedi): ");
        scanf("%d",&ptr->tur);
        printf("Filmin imdb puani: ");
        scanf("%f",&ptr->imdb);
        printf("Filmin yapim yili: ");
        scanf("%d",&ptr->yapimyili);
        printf("\n");
        getchar();
    }
    ptr=film;
    for(int i=0;i<size;i++,ptr++){
        printf("----- %d. Film -----\n",i+1);
        printf("Filmin adi: %s",ptr->filmadi);
        printf("Filmin Turu:");
        printtur(ptr->tur);
        printf("Filmin imdb puani: %.1f\n",ptr->imdb);
        printf("Filmin yapim yili: %d\n\n",ptr->yapimyili);
    }

    return 0;
}

void printtur(ft tur){
    switch(tur){
        case 1:printf("Aksiyon");break;
        case 2:printf("Bilim Kurgu");break;
        case 3:printf("Gerilim");break;
        case 4:printf("Dram");break;
        case 5:printf("Korku");break;
        case 6:printf("Komedi");break;
        default:printf("Tanimsiz"); break;
    }
    printf("\n");
}
```

Burada “size” değeri uzunluğunda f tipi bir dizi kullanarak statik bellek tahsisi yapıldı. Böylelikle dizinin 
başlangıç adresini ifade eden “film” değişkenini f tipi bir pointer’a atayıp pointer 
aritmetiğinden yararlandık. İlk döngünün sonunda getchar fonksiyonunu kullanmamızın nedeni, kullanıcının film
yapım tarihini yazdıktan sonra enter’a tıkladığında fgets’in bunu bir veri olarak algılaması ve 
fonksiyondaki talimatları sonlandırmasıdır. Bu da birincisi hariç diğer filmlerin isimlerini giremememize neden 
olur. Enter’ı kullandığımızda “\n” verisi getchar tarafından algılanacak ve böylelikle fgets 
ile kullanıcıdan film ismi alınabilecek. İkinci döngüden önce birinci döngüde değerini arttırdığımız için ptr’ye 
dizinin başlangıç adresini tekrar atadık. 

```console
----- 1. Film -----
Filmin adi: The Dark Knight
Filmin Turu(1: Aksiyon, 2: BilimKurgu, 3: Gerilim, 4: Dram, 5: Korku, 6: Komedi): 1
Filmin imdb puani: 9
Filmin yapim yili: 2008

----- 2. Film -----
Filmin adi: IT
Filmin Turu(1: Aksiyon, 2: BilimKurgu, 3: Gerilim, 4: Dram, 5: Korku, 6: Komedi): 5
Filmin imdb puani: 7.3
Filmin yapim yili: 2017

----- 3. Film -----
Filmin adi: Prometheus
Filmin Turu(1: Aksiyon, 2: BilimKurgu, 3: Gerilim, 4: Dram, 5: Korku, 6: Komedi): 2
Filmin imdb puani: 7
Filmin yapim yili: 2012

----- 4. Film -----
Filmin adi: Joker
Filmin Turu(1: Aksiyon, 2: BilimKurgu, 3: Gerilim, 4: Dram, 5: Korku, 6: Komedi): 4
Filmin imdb puani: 8.4
Filmin yapim yili: 2019

----- 5. Film -----
Filmin adi:Yes Man
Filmin Turu(1: Aksiyon, 2: BilimKurgu, 3: Gerilim, 4: Dram, 5: Korku, 6: Komedi): 6
Filmin imdb puani: 6.8
Filmin yapim yili: 2008

----- 1. Film -----
Filmin adi: The Dark Knight
Filmin Turu:Aksiyon
Filmin imdb puani: 9.0
Filmin yapim yili: 2008

----- 2. Film -----
Filmin adi: IT
Filmin Turu:Korku
Filmin imdb puani: 7.3
Filmin yapim yili: 2017

----- 3. Film -----
Filmin adi: Prometheus
Filmin Turu:Bilim Kurgu
Filmin imdb puani: 7
Filmin yapim yili: 2012

----- 4. Film -----
Filmin adi: Joker
Filmin Turu:Dram
Filmin imdb puani: 8.4
Filmin yapim yili: 2019

----- 5. Film -----
Filmin adi:Yes Man
Filmin Turu:Komedi
Filmin imdb puani: 6.8
Filmin yapim yili: 2008
```

Fark ettiyseniz alt satıra geçmek için kullandığımız “\n” karakterini 38. satırda kullanmamamıza rağmen her bir 
film adı ekrana yazdırıldıktan sonra bir satır altta yazdırma işlemi devam etmiş. Bunun nedeni,
yukarıda bahsedildiği üzere, fgets fonksiyonunun enter’ı veri olarak saymasıdır. Kullanıcı film isimlerini girip 
enter’a bastığında fonksiyon bunu sonlandırıcı karakter olarak algılar ve dizenin son
karakteri “\n” olur. 
 
Şimdi de dinamik bellek tahsisiyle kaç filmin bilgisi girileceğini kullanıcıya soralım. 
 
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef enum filmturu{Aksiyon=1, BilimKurgu, Gerilim, Dram, Korku, Komedi}ft;

typedef struct film{
    char filmadi[30];
    ft tur;
    float imdb;
    int yapimyili;
}f;

void printtur(ft);

int main()
{
    int size;
    printf("Girilecek film sayisi: ");
    scanf("%d",&size);
    getchar();

    f* ptr=(f*)malloc(sizeof(f)*size);

    for(int i=0;i<size;i++,ptr++){
        printf("----- %d. Film -----\n",i+1);
        printf("Filmin adi: ");
        fgets(ptr->filmadi,sizeof(ptr->filmadi),stdin);
        printf("Filmin Turu(1: Aksiyon, 2: BilimKurgu, 3: Gerilim, 4: Dram, 5: Korku, 6: Komedi): ");
        scanf("%d",&ptr->tur);
        printf("Filmin imdb puani: ");
        scanf("%f",&ptr->imdb);
        printf("Filmin yapim yili: ");
        scanf("%d",&ptr->yapimyili);
        printf("\n");
        getchar();
    }
    ptr-=size;
    for(int i=0;i<size;i++,ptr++){
        printf("----- %d. Film -----\n",i+1);
        printf("Filmin adi: %s",ptr->filmadi);
        printf("Filmin Turu:");
        printtur(ptr->tur);
        printf("Filmin imdb puani: %.1f\n",ptr->imdb);
        printf("Filmin yapim yili: %d\n\n",ptr->yapimyili);
    }

    return 0;
}

void printtur(ft tur){
    switch(tur){
        case 1:printf("Aksiyon");break;
        case 2:printf("Bilim Kurgu");break;
        case 3:printf("Gerilim");break;
        case 4:printf("Dram");break;
        case 5:printf("Korku");break;
        case 6:printf("Komedi");break;
        default:printf("Tanimsiz"); break;
    }
    printf("\n");
}
```

Önceki dinamik bellek tahsisi uygulamalarımızdan hatırlarsanız öncelikle kullanıcıdan kaç bellek bloğu ayırmak 
istediğini öğreniyoruz. (Girilen değer)x(struct’ımızın boyutu) kadar bloğu ptr için ayırıyoruz.
Ayrıca kullanıcı değeri girip enter’a tıkladığında fgets bunu algılayacağı için araya getchar işlevini 
yerleştiriyoruz. 



## File Pointer 


 
Herhangi bir veriyi dosyaya kaydetmek, dosyadaki verileri okumak gibi dosya giriş-çıkış işlemlerinde de pointer’a 
ihtiyaç duyarız. File pointer; dosyanın ismi, konumu, modu (“w”, “w+”, “a” gibi) ve dosyadaki
anlık konum gibi bilgilerini depolar. Derleyicideki söz dizimi şu şekildedir: 

`FILE* ptr;`

Burada FILE, dosya işaretçisi için önceden tanımlı olan yapının typedef ile belirlenen adıdır. Öncelikle structure 
ve enumeration konularında olduğu gibi bu konunun da detaylarına inmemiz uygun değil.
İsteyenler, birçok mod ve işlevi barındıran bu konuyu ayrıca araştırabilir. Biz burada pointer’a bakan kısmını 
kısaca inceleyeceğiz. 

File pointer’ın işlevlerinden biri olan dosyaya veri aktarma uygulamasının basit bir örneğini inceleyelim: 
```c 
#include <stdio.h>
#include <stdlib.h>

int main()
{
    FILE* fPtr;
    fPtr=fopen("merhaba.txt","w");
    fputs("Merhaba Mars!", fPtr);
    fclose(fPtr);
    return 0;
}
```

Örnekte tanımlanan fPtr file pointer’ımız, dosya üzerinde farklı modlarla işlem yapabilmek için kullandığımız 
fopen işlevinde belirtilen moda bağlı davranışlarda bulunur. Örneğin buradaki “w” (write) modu;
file pointer’ın eğer dosya mevcut değilse dosyayı oluşturmasına, mevcutsa dosyanın başlangıç konumundan 
başlamasına, eğer mevcut dosyada içerik de mevcutsa yazma işleminde içeriğin üzerine yazmasına, bu
işlem her devam ettiğinde dosyadaki konumunu ilerletmesine ve okuma işlemini yapamamasına neden olur. 
 
![30](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/c01b2808-e712-44c0-8c1f-b622814022b3)

Gördüğünüz gibi burada otomatik olarak projemizin bulunduğu klasörde dosyamız oluştu. İstersek dosyanın konumunu 
farklı yapabiliriz. Bunun için hedef klasörü dosyamızdan önce belirtmemiz yeterli olacaktır. 

Pointer’ın dosya işlemlerindeki işlevini göz attığımıza göre şimdi de bir önceki konumuzda bulunan struct ve enum 
ile ilgili dinamik bellek tahsisi uygulamamıza file pointer’ı ekleyerek girilen verileri bir
dosyaya yazdıralım.

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef enum filmturu{Aksiyon=1, BilimKurgu, Gerilim, Dram, Korku, Komedi}ft;

typedef struct film{
    char filmadi[30];
    ft tur;
    float imdb;
    int yapimyili;
}f;

void fprinttur(ft,FILE*);

int main()
{
    FILE* fPtr=fopen("filmler1.txt","w");
    int size;
    printf("Girilecek film sayisi: ");
    scanf("%d",&size);
    getchar();

    f* ptr=(f*)malloc(sizeof(f)*size);
    if (ptr == NULL) {
        printf("Bellek tahsisi yapýlamadý.\n");
        return 1;
    }

    for(int i=0;i<size;i++,ptr++){
        printf("----- %d. Film -----\n",i+1);
        printf("Filmin adi: ");
        fgets(ptr->filmadi,sizeof(ptr->filmadi),stdin);
        printf("Filmin Turu(1: Aksiyon, 2: BilimKurgu, 3: Gerilim, 4: Dram, 5: Korku, 6: Komedi): ");
        scanf("%d",&ptr->tur);
        printf("Filmin imdb puani: ");
        scanf("%f",&ptr->imdb);
        printf("Filmin yapim yili: ");
        scanf("%d",&ptr->yapimyili);
        printf("\n");
        getchar();
        fprintf(fPtr,"----- %d. Film -----\n",i+1);
        fprintf(fPtr,"Filmin adi: %s",ptr->filmadi);
        fprintf(fPtr,"Filmin Turu:");
        fprinttur(ptr->tur,fPtr);
        fprintf(fPtr,"Filmin imdb puani: %.1f\n",ptr->imdb);
        fprintf(fPtr,"Filmin yapim yili: %d\n\n",ptr->yapimyili);
        fflush(fPtr);
    }

    free(ptr);
    fclose(fPtr);


    return 0;
}

void fprinttur(ft tur,FILE* fileptr){
    switch(tur){
        case 1:fprintf(fileptr,"Aksiyon");break;
        case 2:fprintf(fileptr,"Bilim Kurgu");break;
        case 3:fprintf(fileptr,"Gerilim");break;
        case 4:fprintf(fileptr,"Dram");break;
        case 5:fprintf(fileptr,"Korku");break;
        case 6:fprintf(fileptr,"Komedi");break;
        default:fprintf(fileptr,"Tanimsiz"); break;
    }
    fprintf(fileptr,"\n");
}
```

Burada yeni bir dosyaya verileri yazdırmak için “w” modunu kullandık. Yazdırma işlemi için de fprintf fonksiyonunu 
kullandık. Program devam ettiği sürece hazır dosya işlemleri arabellekte tutulur. Program
sonlanınca da işlemler diskte uygulanır. Yalnız burada free() fonksiyonu fclose()’dan önce geldiği için ayrılan 
bellek blokları serbest bırakılır ve işlemdeki veriler kaybolur. Bu yüzden de açılan dosya boş
olur. Tabii ki bu problem, free() fonksiyonunu fclose()’dan sonra yazmakla çözülebilir. Ama ben burada dosyanın
her bir film bilgisinin sırasıyla işlemleri tamamlanınca dosyaya yazdırılmasını istediğim için for döngüsünün 
sonunda ffllush() fonksiyonunu kullandım. Böylelikle her bir for döngüsünün sonunda ara bellekteki işlemler 
diskteki dosyaya uygulanıyor. 

Konsola girilen değerler: 

 ```c
----- 1. Film -----
Filmin adi: Kingsman: The Secret Service
Filmin Turu(1: Aksiyon, 2: BilimKurgu, 3: Gerilim, 4: Dram, 5: Korku, 6: Komedi):1
Filmin imdb puani: 7.7
Filmin yapim yili: 2014

----- 2. Film -----
Filmin adi: Oppenheimer
Filmin Turu(1: Aksiyon, 2: BilimKurgu, 3: Gerilim, 4: Dram, 5: Korku, 6: Komedi):4
Filmin imdb puani: 8.6
Filmin yapim yili: 2023

----- 3. Film -----
Filmin adi: Arrival
Filmin Turu(1: Aksiyon, 2: BilimKurgu, 3: Gerilim, 4: Dram, 5: Korku, 6: Komedi):2
Filmin imdb puani: 7.9
Filmin yapim yili: 2016

----- 4. Film -----
Filmin adi: Blade Runner 2049
Filmin Turu(1: Aksiyon, 2: BilimKurgu, 3: Gerilim, 4: Dram, 5: Korku, 6: Komedi):1
Filmin imdb puani: 8
Filmin yapim yili: 2017
```

Dosya içeriği: 

 ![34](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/assets/146577506/2c61ea93-d35f-4298-bd42-767bb1b495f9)

Pointer ile ilgili anlatacaklarım bu kadardı. Buraya kadar konuyla ilgili herhangi bir soru, eleştri veya katkınız
olursa bana <asbalandi@gmail.com> adresinden ulaşabilirsiniz.
