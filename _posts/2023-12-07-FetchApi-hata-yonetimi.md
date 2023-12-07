---
layout: post
title: Merhaba Dünya !
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
Yukarıdaki kod örneği promise rejected olduğunda hata verecek şekilde tasarlanmıştır. Bu istek CORS hatası döneceği için konsolda "hata oluştu " yazacaktır. Bu kod bloğuna göre İşlem resolved olduğu durumda hata alınmaz. Yani 404,500 gibi status hatalarında catch() bloğu bunları yakalamayacaktır.

## Hata yönetiminde Http Status kontrolü 
Promise'in resolved olduğu, yani isteğimizin başarılı olduğu ancak, http status değerinin Ok olmadığı durumları ele alacağız.

```JS
const response = await fetch("https://jsonplaceholder.typicode.com/todoss");

  if(response.ok){
    console.log("status Ok")
  }else{
    
    console.error("hata, statusCode:"+response.status );
  }

```
Yukarıdai kod bloğu, http status kodu 2xx olduğunda "status ok" yazacaktır. Onunda dışındaki durum kodlarında ise konsolda hata basacaktır. Bu kod örneğinde sunucu 404 döndüğü için konsolda "hata, statusCode:404" yazacaktır.

Peki Http Status 2xx dışındakileri de catch() bloğumuz yakalasın istersek nasıl bir senaryo oluşturmalıyız.
## Benim tek doğrum var, ondan gayrisi hatadır kardeşim
Diye sersenişte bulunanlar için şöyle bir senaryo var: Response.Ok olduğu durumların true, diğer tüm durumların( http status kodları ve cors,network errorlar dahil) hata olarak catch bloğunda yakalanacağı bir kod bloğu oluşturacağız.
```JS
 try {
    const response = await fetch("https://jsonplaceholder.typicode.com/todos");
    if (response.ok) {
      console.log("İşlem başarılı(resolved),Http statusCode:200");
    } else {
      throw new Error(response.status);
    }
  } catch (error) {
    console.error(error)
  }

```
Yukarıdaki kod bloğunda , sunucu 200 OK döndüğünden çıktımız "İşlem başarılı(resolved),Http statusCode:200"  olacaktır.

```JS
try {
    const response = await fetch("https://jsonplaceholder.typicode.com/todoss");
    if (response.ok) {
      console.log("İşlem başarılı(resolved),Http statusCode:200");
    } else {
      throw new Error(response.status);
    }
  } catch (error) {
    console.error(error)
  }

```
Yukarıdaki kod bloğunda ise sunucu 404 döndüğünden, throw new Error kodu ile hata fırlatılmış ve catch() bloğunda yakalanmıştır.Çıktısı "Error: 404" oalcaktır. 

```JS
 try {
    const response = await fetch("https://google.com/api");
    if (response.ok) {
      console.log("İşlem başarılı(resolved),Http statusCode:200");
    } else {
      throw new Error(response.status);
    }
  } catch (error) {
    console.error(error)
  }

```
Yukarıdaki kod örneğinde ise CORS error oluştuğu için catch() blopu devreye girmiş ve hata yakalanmıştır. çıktısı : TypeError: NetworkError when attempting to fetch resource
Bu yazımız da bu kadar. Sonraki yazımızda görüşmek üzere ... 




