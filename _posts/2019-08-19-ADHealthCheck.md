---
layout: post
title: Bir e-posta ile Active Dizin Servislerininin takibi
---  
   Genellikle tüm sistem yöneticilerinin ortak problemidir yönettiği sistemlerin takibi. Bazı IT ekipleri kendi yazdıkları araçlar ile, bazı IT ekipleri ise kullandıkları sistemlerin kendilerine sundukları arayüzler ve araçlar ile sistemlerin durumlarını takip etmektedirler.   Bu yazımız , aktif dizin yöneticilerinin, aktif dizin servis sağlığını, ilave bir işlem yapmadan, e-posta adreslerine  gelen bir mail ile takip etmelerini amaçlamaktadır.   
   
Yazımızın ilham kaynağı, Sukhija Vikas'ın yayınlamış olduğu PowerShell betiğidir. Bu yazımızda, betik üzerinde ufak düzenlemeler yapıp, sunucu üzerinde otomatik görev oluşturup, düzenli olarak bildirim gelmesini sağlayacağız.  

Öncelikle betiğimizi [Şu](https://gallery.technet.microsoft.com/scriptcenter/Active-Directory-Health-709336cd) likten indirelim.Klasör içerisinde 1 adet PS betik dosyası ve 1 adet bat dosyası yer almaktadır. AD Sunucumuzda C:\ dizini altında "ADHealth" adlı klasör oluşturup içerisine bu dosyaları kopyalayalım.  

Şimdi sıra geldi görev(task) oluşturmaya. Biz her sabah saat 09:15'de sonucun bize mail olarak gelmesi yönünde planlama yaptık.Buna göre de görevimizi oluşturduk.  

Öncelikle sunucumuz üzerinde görev Zamanlayıcı (Task Scheduler) açalım ve temel görev tanımlayalım ve sonraki aşamaya geçelim.  

![gorev](/images/CreateTask.png)  


  
Şimdi görevimize isim verelim.  

![isim](/images/Name.png)  


Bir sonraki aşamada görev planlaması yapalım.Görev hergün saat 09:15'de çalışsın.  

![zaman](/images/1Days.png)  



```

```



