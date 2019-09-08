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
  
 ```ornek 
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


 

