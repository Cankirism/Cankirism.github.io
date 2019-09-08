---
layout: post
title: C# Event(Olay) ve Delegate(Temsilci) Kullanımına Genel Bakış
---  

Bu yazımızda C# dili ile olay ve temsilci yönetimini ve kullanımını irdeleyeceğiz.  
## Temsilciler(Delegates) ##  
Adından da anlaşıldığı üzere temsil eden, işaret eden anlamına gelmektedir.Methodların adresini tutan, onları işaretleyen yapılardır.  
Temsilciler referans tiplidir. Dolayısıyla onlardan da nesne oluşturulabiliriz. Fazla zaman kaybetmeden örnek uygulama ile kullanımına geçelim.  
Öncelikle bir tesmcilsi yapısı nasıl olur ona bakalım:    
``` C#
private delegate int DelegateHandler(); 
``` 
 inceleyecek olursa erişim belirleyicimiz -delegate sözcüğü(keyword) - method dönüş tip, temsici adımız.  
 Dikkat edilecek önemli hususlardan birisi; temsilcimiz bir methodu işaret ettiği için, o methoda imzası benzemelidir. Yani dönüş tipi , aldığı parametreler aynı olmalıdır.  Temsilci adının sonuna - Handler- konması gerekmekz ama yazılım kültüründe böyle kullanılması tercih edilmektedir.    
 ```C#
 public delegate void DelegateHandler(); // geriye değer döndürmeyen ve parametre almayan method için kullanılır
 public delegate int DelegateHandler(int a, int b); // Geriye int tipinde değer döndüren ve 2 adet parametre alan method için kullanılır.
 
 ```
 
  ### Örnek Uygulama ###
  Bu örnek uygulama, toplama işlemi yapan bir method tanımlayıp, onun bir temsilci ile nasıl kullandığını göstermektedir. 
  
 ```C#
 class Program
    {
      
        private  delegate int  ToplamHandler(int a,int b); // 2 int parametre alan ve geriye int döndüren temsilci tanımladık.
        static void Main(string[] args)
        {
            ToplamHandler handler = new ToplamHandler(Topla); // Temsilsi nesnesi tanımladık.
            Console.WriteLine( handler.Invoke(3, 4)); // delegemizin invoke methodu ile işaret edilen methodu çalıştırdık.
            Console.ReadLine();
        }

        static int Topla(int x , int y)
        { return x + y; }

    }

```  
Örnek uygulama kaynak kodlarında da görüldüğü üzere, delegemizle aynı parametre alan ve aynı dönüş tipi alan methodumuzu bağlıyoruz. Farklı dönüş tipi olan bir methodu bağlayamayacaktık. Yukarıdaki kodda şu düzenlemeyi yapsak;  
``` c#
  static void Main(string[] args)
        {
            ToplamHandler handler = new ToplamHandler(Carp); // Tip dönüş hatası verecektir.
            Console.WriteLine( handler.Invoke(3, 4));
            Console.ReadLine();
        }
static string Carp(int x, int y)
        {
            return "text";
        }
```  
kodumıza string dönüş tipinde mothod ekleyip onu delegemize bağlamaya çalışırsak "Ger. dönüş Tipi" hatası alacağız. Çünkü delegemiz int dönüş tipi method beklerken biz string dönüş tipi olan method bağlamaya çalıştık.   
Aşağıdaki bir durumda  bir durumda nasıl aksiyon alırdık ? 
``` c#
 static void Main(string[] args)
        {
            ToplamHandler handler = new ToplamHandler(Topla);
            Console.WriteLine( handler.Invoke(3, 4));
            handler += Carp;
            Console.WriteLine(handler.Invoke(3, 4));
            Console.ReadLine();
        }

        static int Topla(int x , int y)
        { return x + y; }
        static int Carp(int x, int y)
        {
            return x * y;
        }

```  
Tahmin edeceğiniz gibi, uygulama ilk olarak ekrana 7 , ardından ise 12 çıktısını basacaktır. Çünkü ilk olarak topla methodunu bağlayıp, sonra ise += Carp ifadesi ile artık delegemizi Carp methoduna bağlamış olduk. -= Carp ifadesi ile de  delegemizin bu methoda bağlanmasını  iptal edebiliriz.  
## Event (Olay) ## 
Sıklıkla gui programlamada karşılaştığımız yapılardır olaylar. fareye sağ tıkladık, fareye çift tıkladık vb. gibi olay örnekleri verebiliriz.Oysaki sadece gui tarafında kullanmıyoruz olayları. Yani her şey bir butona tıklanması ya da fareye sağ tık yapılması gibi olmayabilir. Yeri gelir olayları kendimiz tanımlar ve fırlatırız.  
Aklımıza ilk gelen örnekler kontrol durumları olacaktır. Mesela stoktaki bir ürünün stok durumu kontrolü, bir personelin izin durum kontrolü vb. gibi. Bir olayın tanımlanması için iki mühim husus vardır :     
1- Bir temsilci ile eşleştirilmesi ,   
2- Ortama fırlatılması (Koşul tanımı)  

Yani bir olay tanımlayacağız ve bu olayı ortama fırlatacağız. Olayı tetikleyip, bu tetikleme sonucu çalışacak methodu ise temsilcimiz ile işaret edeceğiz. Bunu örnek uygulama ile daha anlaşılır yapalım.  
``` C#

 public class Personel
    {

        public delegate void IzinKontrolHandler();
        public event IzinKontrolHandler IzinEvent;

        private long _tcNo;
        private string _sicilNo;
        private byte _izinSayisi;
        public string Adi { get; set; }
        public long TcNo
        {
            get { return _tcNo; }
            set { _tcNo = value; }
        }
        public string SicilNo
        {
            get { return _sicilNo; }
            set { _sicilNo = value; }
        }

        public byte IzinSayisi
        {
            get
            {
                return _izinSayisi;
            }

            set
            {
                _izinSayisi = value;
                if (value < 3 && IzinEvent != null)
                { IzinEvent(); }
            }
        }

        public Personel(string adi, long tcNo, string sicilNo, byte izinSayisi)
        {
            Adi = adi;
            TcNo = tcNo;
            SicilNo = sicilNo;
            IzinSayisi = izinSayisi;
        }
    }
```
Uygulamamızın amacı personel kaydı yapıp, personel izin kullandığında, kalan izin gün sayısını kontrol edip, kalan izin 3'ün altında ise ortama olay fıtlatmak. Şimdi kodumuzda ne yaptık bakalım :   
1- Öncelikle PersonelLibrary isimli kütüphanemizi tanımlaıdk ve içerisinde Personel isminde sınıf tanımlaması gerçekleştirdik.  
2- IzinKontrolHandler adında delegemizi tanımladık.   
3- IzinEvent adında olayımızı tanımladık. Bir olay fırlatıldığında, br methodun tetiklenmesi için o olayın o methodu işaret etmesi gerekir. İşaret etme işini ise delegemiz yapacaktır. 
```C#
  public event IzinKontrolHandler IzinEvent;
```
Böylece olayımızı tanımlayıp, methodları işaret edecek temsilcimizi belirtmiş olduk. Şimdi sıra geldi ilgili methodu nasıl tetikleyeceğiz ? Aşaüıda görüldüğü üzere, bu tetikleme için IzinSayisi isimli Property'nin(özellik) set bloğu kullanılmıştır.
izin sayısı 3'ün altındaysa olayı fırlat. IzinEvent olayının null olmaması , += oparatörü ile yüklendiği anlamına gelir.
```C#
 set
            {
                _izinSayisi = value;
                if (value < 3 && IzinEvent != null)
                { IzinEvent(); }
            }
```
Şimdi gelelim bir methodu bağlamaya. Yani += parametresi kullanılarak olayımız fırlatıldığında tetiklenecek methodu yüklememiz gerekecek. Bunun için yeni bir uygulama oluşturup, PersonelLibrary dll'imizi referans olarak ekledik.  

```C#

using System;
using PersonelLibrary;

namespace EvetAndDelegates
{
    class Program
    
        static void Main(string[] args)
        {
            Personel personel = new Personel("Ali ÇAM", 11111111111, "MM123789", 20);
            personel.IzinEvent += new IzinKontrolHandler(IzinUyariVer);
            
            for (int a = 0; a < 5; a++)
            {
                Console.WriteLine("{0} Personeli kaç gün izin kullanacak ? ", personel.Adi);
                byte izin = Convert.ToByte(Console.ReadLine());
                personel.IzinSayisi -= izin;
                Console.WriteLine("Kalan İzin Sayısı:{0}", personel.IzinSayisi);
            }
            
            Console.ReadLine();    
        }

        static void IzinUyariVer()
        {
            Console.WriteLine("izniniz azaldı");
        }
    }
}
```
Personel sınıfına ait bir nesne oluşturulduktan sonra; IzinEvent adlı olayımız += operatörü ile yüklenmiştir. operatörden sonra ise olay methodumuz bağlanmıştır. Görüldüğü üzere tetiklenecek methodumuz IzinUyariVer isimli methodumuzdur.  
Uygulamayı çalıştıralım:   
![cikti](/images/cikti.png)  
  
  görüldüğü gibi izin sayısı 3',n altına düştüğünde Event fırlatılmış ve tetiklediğimiz method çalışarak "İzniniz Azaldı" yazdırmıştır.
  
  Görüşmek üzere , Sağlıcakla 




    




 

