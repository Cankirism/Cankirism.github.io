---
layout: post
title:Javascript Api isteklerinde Hata Yönetimi 
--- 

## Fetch Api Nedir ? 
Merhaba, bildiğiniz üzere sunucu tarafındaki kaynakların(resources) getirilmesini sağlayan arayüzdür.Ajax'tan farkı olarak promise tabanlı olması ve asenkron veri alışverişini desteklemesidir.
## Promise 
Peki nedir bu promise ? Javascript'te asenkron programlama dediğimizde aklımıza 2  yöntem gelir. Biri Callback diğer ise promise. Callback , bir işlem bittiğinde çağrılır ve işlem sonucu bu foknsiyona aktarılır. Örneğin dosya okuma işlemi tamamlandığında tanımlı callback fonksiyonu çağrılır ve sonuc buna atanır. Callback ile birlikte fonksiyonların karmaşık hale gelmesi noktasında promise devreye girmiştir. 
Promise JS'de asenkron işlemler için kullanılan bir objecttir.Bir işlem tamamlandığında ya da başarısız olduğunda kullanılabilirler. İşlemlerin sıralı yürütülmesini kolaylaştırmaktadır.
Promise'de 3 farklı durum vardır
1- Resolved: İŞlem tamamlandığını belirtir. İstek tamamlandıysa resolved olmuştur.
2- Rejected: İşlem reddedilmiştir. Hata oluşmuştur(Network error, Cors error vb.)
3- Pending: İşlem henüz tamamlanmamıştır.

## Hata Yönetimi 
Fetch APi işlemlerinde sunucu tarafına istek atıldıktan sonra sunucudan gelen response yönetmemiz ve kullanıcıyı buna göre bilgilendirmemiz gerekmektedir. Peki hata / istisna oluştuğunda bu durumu nasıl ele alacağız?  
``` JS
try {
  const response = await fetch("https://jsonplaceholder.typicode.com/todoss");
  console.log("İşlem başarılı");
} catch {
  console.error("hata alındı");
}

```

Yukarıdaki  kod örneğinde  nasıl bir çıktı oluşur? Sunucu http 404 döndüğünde bunu hata olarak mı yakalar?  Yanıt tabiki "hayır". Sunucu  http 404 dönecek ama konsolda "işlem başarılı" yazacaktır. Bu istenmeyen bir senaryodur.

## Fetch Api Kullanımında Hata/İstisnayı Nasıl Yönetmeliyiz ? 
Fetch api  kullanırken birden farklı hata ile karşılaşabiliriz. Bunlar sunucu taraflı hatalar (5xx), not found(404), network hataları ya da cors hataları olabilir.

## Try/Catch bloğu
Fetch api promise tabanlı olduğu için, resolved,rejected,pending durumlarını yönetirken then,catch,finally keywordlerini tam anlamıyla kullanmamızı sağlar. Try/catch bloğu bildiğiniz gibi  hata yönetiminde standart haline geldi desek abartmış olmayız.

```JS
try {
    const response = await  fetch('https://google.com/api');
  } catch {
    console.error('Hata oluştu');
  }
}

```
Yukarıdaki kod örneği promise rejected olduğunda hata verecek şekilde tasarlanmıştır. Bu istek CORS hatası döneceği için konsolda "hata oluştu " yazacaktır. Bu kod bloğuna göre İşlem resolved olduğu durumda hata alınmaz. Yani 404,500 gibi status hatalarında catch() bloğu bunları yakalamaycaktır. 
