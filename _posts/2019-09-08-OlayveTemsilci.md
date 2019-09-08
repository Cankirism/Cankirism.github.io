---
layout: post
title: C# Event(Olay) ve Delegate(Temsilci) Kullanımına Genel Bakış
---  

Bu yazımızda C# dili ile olay ve temsilci yönetimini ve kullanımını irdeleyeceğiz.  
## Temsilciler(Delegates) ##  
Adından da anlaşıldığı üzere temsil eden, işaret eden anlamına gelmektedir.Methodların adresini tutan, onları işaretleyen yapılardır.  
Temsilciler referans tiplidir. Dolayısıyla onlardan da nesne oluşturulabiliriz. Fazla zaman kaybetmeden örnek uygulama ile kullanımına geçelim.  
Öncelikle bir tesmcilsi yapısı nasıl olur ona bakalım:    
``` private delegate int DelegateHandler(); 
``` 
 inceleyecek olursa erişim belirleyicimiz -delegate sözcüğü(keyword) - method dönüş tip, temsici adımız.  
 Dikkat edilecek önemli hususlardan birisi; temsilcimiz bir methodu işaret ettiği için, o methoda imzası benzemelidir. Yani dönüş tipi , aldığı parametreler aynı olmalıdır.  Temsilci adının sonuna - Handler- konması gerekmekz ama yazılım kültüründe böyle kullanılması tercih edilmektedir.  
 
 

