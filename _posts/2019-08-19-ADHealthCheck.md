
---
layout: post
title: Bir e-posta ile Active Dizin Servislerininin takibi .
---  

Genellikle tüm sistem yöneticilerinin ortak problemidir yönettiği sistemlerin takibi. Bazı IT ekipleri kendi yazdıkları araçlar ile, bazı IT ekipleri ise kullandıkları sistemlerin kendilerine sundukları arayüzler ve araçlar ile sistemlerin durumlarını takip etmektedirler. Bu yazımız , aktif dizin yöneticilerinin, aktif dizin servis sağlığını, ilave bir işlem yapmadan, e-posta adreslerine  gelen bir mail ile takip etmelerini amaçlamaktadır.  
Yazımızın ilham kaynağı, Sukhija Vikas'ın yayınlamış olduğu PowerShell betiğidir. Bu yazımızda, betik üzerinde ufak düzenlemeler yapıp, sunucu üzerinde otomatik görev oluşturup, düzenli olarak bildirim gelmesini sağlayacağız.
Öncelikle betiğimizi ![Şu](https://gallery.technet.microsoft.com/scriptcenter/Active-Directory-Health-709336cd) likten indirelim. 
Klasör içerisinde 1 adet PS betik dosyası ve 1 adet bat dosyası yer almaktadır. AD Sunucumuzda C:\ dizini altında Script adlı klasör oluşturup içerisine bu dosyaları kopyalayalım. 


