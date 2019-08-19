---
layout: post
title: Bir e-posta ile Aktif Dizin Servislerininin takibi
---  
   Genellikle tüm sistem yöneticilerinin ortak problemidir yönettiği sistemlerin takibi. Bazı IT ekipleri kendi yazdıkları araçlar ile, bazı IT ekipleri ise kullandıkları sistemlerin kendilerine sundukları arayüzler ve araçlar ile sistemlerin durumlarını takip etmektedirler.   Bu yazımız , aktif dizin yöneticilerinin, aktif dizin servis sağlığını, ilave bir işlem yapmadan, e-posta adreslerine  gelen bir mail ile takip etmelerini amaçlamaktadır.   
   
Yazımızın ilham kaynağı, Sukhija Vikas'ın yayınlamış olduğu PowerShell betiğidir. Bu yazımızda, betik üzerinde ufak düzenlemeler yapıp, sunucu üzerinde otomatik görev oluşturup, düzenli olarak bildirim gelmesini sağlayacağız.  
## Ön Hazırlık ##

Öncelikle betiğimizi [Şu](https://gallery.technet.microsoft.com/scriptcenter/Active-Directory-Health-709336cd) likten indirelim.Klasör içerisinde 1 adet PS betik dosyası ve 1 adet bat dosyası yer almaktadır. AD Sunucumuzda C:\ dizini altında "ADHealth" adlı klasör oluşturup içerisine bu dosyaları kopyalayalım.  

Şimdi sıra geldi görev(task) oluşturmaya. Biz her sabah saat 09:15'de sonucun bize mail olarak gelmesi yönünde planlama yaptık.Buna göre de görevimizi oluşturduk.  

Öncelikle sunucumuz üzerinde görev Zamanlayıcı (Task Scheduler) açalım ve temel görev tanımlayalım ve sonraki aşamaya geçelim.  

![gorev](/images/CreateTask.png)  


  
Şimdi görevimize isim verelim.  

![isim](/images/Name.png)  


Bir sonraki aşamada görev planlaması yapalım.Görev her gün saat 09:15'de çalışsın.  

![zaman](/images/1Days.png)  

Bir sonraki aşamada görevimizin fonksiyonunu belirtelim. Görevimiz bir programı tetikleyecektir.  

![start](/images/StartProgram.png)  

Sıra geldi program bileşenlerini belirtmeye. Görevimiz PowerShell.exe'yi tetikleyecek ve C:\ADHealth dizini altındaki ps1 dosyasımızı çalıştıracaktır.  
![betik](/images/betik.png)  
ve görevimiz hazır.   

## Betik Düzenleme ## 
Betiği istediğimiz gibi düzenleyebiliriz. Örneğin biz, tanımladığımız bir gmail adresinden bize mail gelmesi için düzenleme yapacağız.  

Betik içerisindeki 17-20. satırları;  
```PowerShell
$smtphost = "smtp.labtest.com" 
$from = "DoNotReply@labtest.com" 
$email1 = "Sukhija@labtest.com"
$timeout = "60"
``` 
 Aşağıdaki gibi düzenledik.
 
 ```PowerShell 
$smtphost = "smtp.gmail.com" 
$from = "gonderenEposta@gmail.com" 
$email1 = "alicininEpostasi"
$timeout = "60"
```    
Ayrıca SSL ve Kimlik Bilgileri(Credentials) için 343. satırdan itiabren şu düzenlemeleri yaptık :  

```PowerShell
$smtp= New-Object System.Net.Mail.SmtpClient $smtphost 
$smtp.Credentials = New-Object System.Net.NetworkCredential("gonderenEposta@gmail.com","parola")
$smtp.EnableSsl = $true
$msg = New-Object System.Net.Mail.MailMessage 
$msg.To.Add($email1)
$msg.from = $from
$msg.subject = $subject
$msg.body = $body 
$msg.isBodyhtml = $true 

$smtp.send($msg) 
```  
Eğer mail olarak bildirim alamıyorsanız e-posta sağlayıcı güvenlik ayarlarını ve sunucunuz önünde konumlandırılan güvenlik duvarını kontrol etmenizi tavsiye ederiz. Ayrıca betiğin koştuğu platformun dilinin İngilizce olması gerekmektedir.  

## Çıktı ##  

![mail](/images/mail.png)  

Hepsi bu kadar. Kıymetli vaktinizi ayırdığınız için teşekkür ederiz.  
<a href="https://github.com/Cankirism/Cankirism.github.io/blob/master/_posts/2019-08-19-ADServisTakip.md">Yazının github linki</a> 










```

```



