# 2.Hafta Konuları 

**1.    Repository nedir, ne işe yarar?**

**2.     Kendi profilimde ilk repositorymi nasıl oluştururum?** 

**3.     Commit nedir, push nedir?** 

**4.     Kendi kodumu Githuba nasıl eklerim ve nasıl çalışırım?**

**5.     UYGULAMA: Kişisel Github profili üzerinde repository oluşturma ve commit. **

## 2. Hafta Giriş
Arkadaşlar merhabalar. Bundan sonraki örneklerde kavram yanılgılarını en aza indirgemek için Git yerine Github kullanılacaktır. Kavram yanılgılarından korkulmamalıdır. Pratik yaptıkça kavramlar oturacaktır. İkinci hafta derslerimizi anlattıktan sonra tüm kavramları anlatacağımız bir video çekeceğiz ve Github öğrenmek isteyen herkesin Github üzerinde uygulama yaptığından emin olacağız.
Yeni katılan arkadaşların 1. Hafta anlatımı okumaları tavsiye edilir. Bunun için link: 

Yeni katılan arkadaşların 1. Hafta anlatımı okumaları tavsiye edilir. Bunun için link: 

[1. Hafta](https://github.com/YOBU-Computer-Engineering/github-lecture-notes/blob/main/week1.md)

**Github Desktop**
Github Desktop uygulaması ile tüm git işlemleri kolay bir şekilde yapılabilir ve anlaşılabilir olmaktadır. Pratik olarak Github Desktop uygulaması ile Git kullanımını anladıktan sonra teorik kısmını anlaşılması oldukça kolaylaşacaktır.
Github Desktop uygulamasını kurmak için aşağıdaki linke tıklayabilirsiniz.
https://desktop.github.com/
Bu haftaki 3 temel konu Github Desktop uygulaması üzerinden anlatılacaktır: 
1.Repository
2.Commit
3.Push 

**Repository nedir, ne işe yarar?**

Repository(repo) ya da türkçesi ile depo, geliştirme yapılan proje klasörünü izlenebilir kılar. Proje klasörü içerisindeki tüm dosyalarda, klasörlerde yapılan değişiklikleri izlenebilir kılar.

Resimde de görüldüğü üzere Local repo ve Remote repo adında iki farklı alan vardır. Local kavramı geliştiricinin yani önünüzdeki bilgisayar olarak düşünülebilir(Local = Senin Bilgisayarın = geliştiricinin bilgisayarı). Github Desktop uygulaması kurulan bilgisayar local repository`dir. Yani geliştiricinin bilgisayarına kurulmuş Repository'dir.

![r1](https://user-images.githubusercontent.com/58172827/173430720-1e632880-3fa6-402a-a223-d99c04f85cf7.png)

**Uygulama**: OzgecmisRepo adında izlenebilir bir klasör yani repository oluşturalım.

 

“OzgecmisRepo” adında bir proje klasörü ve içerisinde Ali.txt adındaki dosya oluşturalım. 

OzgecmisRepo klasörünün repository olabilmesi için Git tarafından takip ediliyor olabilmesi gerekmektedir. Github Desktop uygulaması üzerinden OzgecmisRepo adında repository oluşturalım. 

1.   Github desktop uygulaması açılır. 
                               

2.   File>New Repository yolunu takip edilir. 

![r2](https://user-images.githubusercontent.com/58172827/173431164-b3c8870c-ebcb-4433-bcc9-8e2e05059592.png)

3. “*New Repository*”`e tıklandıktan sonra gelen ekranda Name alanına OzgecmisRepo yazıldıktan sonra *“Create Repository”*  (Repository Oluştur) butonuna basılır. *“Create Repository”* butona bastıktan sonra bir repository oluşturulur. 	

![r3](https://user-images.githubusercontent.com/58172827/173431402-231e5934-48e8-486b-bf4d-9658c34eb33b.png)

4. Bu işlem sonucunda şekildeki ekran gözükür. Bu ekran, geliştirme aşamasında en sık görülecek ekrandır. Remote repository'e kod göndermek ve Remote repository'deki son değişikliklerin kontrol edildiği ekrandır. Anasayfa niteliğindedir. 

![r4](https://user-images.githubusercontent.com/58172827/173431619-5b0c808a-48ec-44e3-92df-b4fafe55567b.png)

Repository neydi? Git olayları takip edilen bir klasör. Oluşturduğu tek şey aslında budur. Bu klasörü görebilmek için yukarıdaki resimde işaretlenen KISIM 2'nin altında “Show in Explorer” butonuna tıklanır veya kısayolu olan ctrl + shift + f yapılır. Create Repository butonuna tıklandıktan sonra şekildeki gibi .git klasörü Dökümanlar>Github>OzgecmisRepo içerisinde otomatik oluşturulduğu görülür. .

 

NOT: .git klasörü, komut satırına **git init** yazılarak veya github desktop uygulamasından ”Create Repository diyilerek oluşturulan, Git sisteminin klasörü repository olarak algılamasını sağlayan bir klasördür

1.Haftada anlatılan Ali.txt dosyası örneğinde bahsedilen 4 satır komutun ilki olan git init komutu yazıldığında, ana proje klasörü içerisinde .git klasörü oluşur. 

![r5](https://user-images.githubusercontent.com/58172827/173432503-24bf9fad-5f94-4ae4-aa75-252f9a5053cb.png)

Sonuç: Create Repository butonuna tıklandığında, bir local repository(bir adet klasör ve bu klasör içerisinde .git klasörü) oluşturulur. 1. haftada anlatılan ***git init*** komutu, Github Desktop uygulaması vasıtasıyla otomatik yürütülmüş olunur. Bu klasör Git'in çalışması için gerekli her şeyi tutar. 

Bu aşamada repository sadece local bilgisayarda oluşturulmuş olunur. Remote Repository üzerine yapılan değişiklikleri göndermek, orada bir repo oluşturmak ve local repodaki değişiklikleri oraya göndermek çok kolaydır. Kısım 1 olarak gösterilen yerde **"publish repository"** butonuna tıklandığında, Github desktop uygulaması, Github uygulamanıza bağlı olduğundan, remote repository üzerinde bir proje oluşturur. 

Not: Remote ve Local repository üzerindeki senkronizasyon, Kısım 1 olarak işaretlenen bölgedeki buton vasıtasıyla yapılır. **"Publish Repository"**  butonuna tıklayalım.

![r6](https://user-images.githubusercontent.com/58172827/173433190-8c2a715d-0784-4281-b854-03ee71f013d7.png)

Resimdeki ekranda repository üzerinde çeşitli ayarlamalar yapılır ve **”Publish Repository”** butonuna basılır. 

![r7](https://user-images.githubusercontent.com/58172827/173433415-6b4e825c-c005-4e45-9f7b-35efe31de180.png)

**Bu işlemi yaptıktan sonra resimdeki ekran gözükür**

![r8](https://user-images.githubusercontent.com/58172827/173433582-5ee92ed7-788d-4ea6-81b1-862cfd5c7620.png)

Bu aşamadan sonra KISIM 1 olarak işaretlenilen alanda *“Publish Repository”* butonu yerine *“Open in Visual Studio Code”* butonu belirir.  Artık Remote ve Local repository`nin senkron(eşlenmiş repositoryler) olduğu anlamına gelir. 

Bu butona tıklanması durumunda, Visual Studio Code programı çalışacaktır. Eğer VS Code uygulamasına sahip değilseniz bu linkten indirebilirsiniz: https://code.visualstudio.com/download

Not: Eğer geliştirme aşamasında kısım 1 de mavi bir buton görülmüyorsa, muhtemelen remote repository ile local repository senkron(birebir aynı) olmuştur. 

**“Open in Visual Studio Code” ** butonuna tıklandığında, VS Code programı içerisinde, Repository klasörü açılır. Açılan bu VS Code ekranında, Windows işletim sisteminden görüntülenen repository klasörünü aynen görülür

![r9](https://user-images.githubusercontent.com/58172827/173433930-481432fc-c20e-4469-9ba5-fca73cacabe7.png)

**VS Code nedir?**

Windows pencerelerinden(Windows yerine Linux'ta olabilir. Genel kullanım Windows olduğu varsayıyoruz) yapılabilen her türlü dosya veya klasör oluşturma, düzenleme işlemlerini daha hızlı bir şekilde yapılmasını sağlayan, çeşitli eklentiler ile kod geliştirmek için hazırlanmış, yazılım geliştirme aşamasındaki problemleri en aza indiren bir düzenleme aracıdır. 

**Uygulama:**

Local repository üzerinde Ali.txt dosyası oluşturup remote repository`e gönderelim.

Sol taraftaki panele dosya oluşturma simgesine tıkladıktan sonra, gösterilen alana Ali.txt yazalım. 

![r10](https://user-images.githubusercontent.com/58172827/173434203-1505d4ed-b758-44ec-b1fb-93d0a55685bb.png)

Dosyayı oluşturduktan sonra içerisine 5 satırlık bir metin girelim ve ctrl + s yaparak kaydedelim. Veya File>Save yapalım. 

![r11](https://user-images.githubusercontent.com/58172827/173434327-8d0e755f-8fd7-4efa-af41-aea92663d97f.png)

![r12](https://user-images.githubusercontent.com/58172827/173434386-94e24a95-69c1-4505-a135-59d34b5f4a63.png)

Bu aşamadan sonra Github Desktop uygulamasını açalım. Resimde görüldüğü üzere Github desktop uygulaması değişiklikleri algıladı.

![r13](https://user-images.githubusercontent.com/58172827/173434504-75097aee-ba36-41f0-868c-b525256746b8.png)

Resimde yapılan değişiklikleri Remote Repository'e atmak için yani Ali.txt dosyasının Remote repository(Github) üzerinde görünmesini sağlamak için yapılan değişiklikleri Commit ve ardından Push etmemiz gerekmektedir. Bu iki kavramın öğrenilmesi elzemdir.

Commit Nedir? Commit o anki yapılan değişikliklerin kaydını tutar. Örnekte yer alan 1 adet dosya değişikliği olan Ali.txt dosyası commit edildikten sonra bir kayıt noktası oluşturur. 1. Hafta anlatılan A ve B noktaları commit demektir. Yukarıdaki resimde yorum yazdıktan sonra **“Commit to main”**  butonuna bastığımızda, yapılan her değişikliği bir kayıt noktası olarak kaydedecektir. Bu kayıt noktaları projenin her bir sayfa kodunun o anki zaman dilimindeki kaydını tutar. Yani geçmişteki bir zamandaki, tam o andaki tüm kodlara erişilebilinir.

Push nedir? Push o anki commit edilen her bir kayıt noktasını, local repository'den remote repository'e gönderim işlemini sağlar.

Not: Commit, kayıt noktası aynı anlamda kullanılmıştır ve A ve B noktaları örnek kayıt noktalarıdır

*Uygulama - Commit ve Push*:

Ali.txt dosyasını düzenleyip değişiklikerin Github tarafından görülmesini sağlamıştık. Tam bu anda yapılan değişiklikleri kayıt noktası olarak kaydetmek istenilidiğinde, bir commit mesajı yazdıktan sonra **“Commit to Main”** butonuna tıklandığında Kayıt noktası işlemi gerçekleşir. 

![r14](https://user-images.githubusercontent.com/58172827/173435190-a3e3a6d1-554e-411b-9f45-598a627f4fbf.png)
![r15](https://user-images.githubusercontent.com/58172827/173435202-731955be-7297-4a0d-b9ad-6718169f8dd4.png)

Bu aşamadan sonra resimde görüldüğü gibi, KISIM 1 bölgesinde, **"Push Origin”** butonu belirmiştir. **”Push Origin"** butonu, Remote repository üzerine değişiklikleri atacaktır. Butona tıklayalım.

![r15](https://user-images.githubusercontent.com/58172827/173435491-24e3aff0-c9f8-4b37-b2a5-0f083e719348.png)
![r16](https://user-images.githubusercontent.com/58172827/173435507-f4f90226-7c04-49ae-934e-e0b5eeb6da88.png)

 

Yapılan değişiklikleri Github üzerinde görmek için Kısım 2 olarak işaretlenen bölgenin altındaki bölgede **“View on Github”**  butonuna tıklayalım. 

![r18](https://user-images.githubusercontent.com/58172827/173435692-d13763ca-9c1b-4f1b-b97c-4dee8497cbf4.png)

Github'da görüleceği üzere Ali.txt dosyası oluşmuştur. Commitlerimizi görmek için 2.resimde görülen icona tıklayalım

![r20](https://user-images.githubusercontent.com/58172827/173435856-cf1d708d-aaca-4f71-b35a-082bc0ab917d.png)



![r21](https://user-images.githubusercontent.com/58172827/173435996-b48cce85-bc0a-49cd-8937-5b5489e374fe.png)

İkona tıklandığında aşağıdaki commit detaylarını gösteren bir sayfa açılır

![r22](https://user-images.githubusercontent.com/58172827/173436153-1c4bf05f-0a5f-482a-a127-41d345bf153f.png)

Commits iconuna tıklanınca resimdeki gibi commits ekranı açılır. Bu noktada “Ali.txt dosyası oluşturuldu” notuyla yapılan commit işlemini görebilmekteyiz. 

![r23](https://user-images.githubusercontent.com/58172827/173436269-cac01f37-05c3-4288-8ea3-42705a266499.png)

Bu commit mesajının içeriği üzerine tıklandığında yapılan değişiklikler görülebilir. 

![r24](https://user-images.githubusercontent.com/58172827/173436408-8cd7126b-0f04-453c-8e6a-e28f3a5133ee.png)



