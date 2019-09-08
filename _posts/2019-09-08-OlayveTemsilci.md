---
layout: post
title: C# Event(Olay) ve Delegate(Temsilci) Kullanımına Genel Bakış
---  

Bu yazımızda C# dili ile olay ve temsilci yönetimini ve kullanımını irdeleyeceğiz.  
## Temsilciler(Delegates) ##  
Adından da anlaşıldığı üzere temsil eden, işaret eden anlamına gelmektedir.Methodların adresini tutan, onları işaretleyen yapılardır.  
Temsilciler referans tiplidir. Dolayısıyla onlardan da nesne oluşturulabiliriz. Fazla zaman kaybetmeden örnek uygulama ile kullanımına geçelim.  
Öncelikle bir tesmcilsi yapısı nasıl olur ona bakalım:    
``` Delegates
private delegate int DelegateHandler(); 
``` 
 inceleyecek olursa erişim belirleyicimiz -delegate sözcüğü(keyword) - method dönüş tip, temsici adımız.  
 Dikkat edilecek önemli hususlardan birisi; temsilcimiz bir methodu işaret ettiği için, o methoda imzası benzemelidir. Yani dönüş tipi , aldığı parametreler aynı olmalıdır.  Temsilci adının sonuna - Handler- konması gerekmekz ama yazılım kültüründe böyle kullanılması tercih edilmektedir.    
 ```Ornek
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
```
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
```
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


    




 

