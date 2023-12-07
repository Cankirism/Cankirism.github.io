---
layout: post
title: Fetch Api işlemlerinde Hata Yönetimi  
---
## Fetch Api Nedir ? 
Merhaba, bildiğiniz üzere FetchApi, sunucu tarafındaki kaynakların(resources) getirilmesini sağlayan arayüzdür.Ajax'tan farkı, promise tabanlı olması ve asenkron veri alışverişini desteklemesidir.
## Promise 
Peki nedir bu promise? Javascript'te asenkron programlama dediğimizde aklımıza 2 yöntem gelir.Biri Callback diğer ise promise.  
Callback, bir işlem bittiğinde çağrılır ve işlem sonucu bu fonksiyona aktarılır.Örneğin dosya okuma işlemi tamamlandığında tanımlı callback fonksiyonu çağrılır ve sonuç buna atanır.  
Callback ile birlikte fonksiyonların karmaşık hale gelmesi noktasında promise devreye girmiştir.  
Promise JS'de asenkron işlemler için kullanılan bir objecttir.Bir işlem tamamlandığında ya da başarısız olduğunda kullanılabilirler. İşlemlerin sıralı yürütülmesini kolaylaştırmaktadır.
Promise'de 3 farklı durum vardır.  
1- Resolved: İşlemin tamamlandığını belirtir. İstek tamamlandıysa resolved olmuştur.  
2- Rejected: İşlem reddedilmiştir. Hata oluşmuştur(Network error, Cors error vb.)  
3- Pending: İşlem henüz tamamlanmamıştır.  


## Hata Yönetimi 
Fetch APi işlemlerinde sunucu tarafına istek atıldıktan sonra sunucudan gelen response yönetmemiz ve kullanıcıyı buna göre bilgilendirmemiz gerekmektedir. Peki hata / istisna oluştuğunda bu durumu nasıl ele alacağız?  
``` js
try {
  const response = await fetch("https://jsonplaceholder.typicode.com/todoss");
  console.log("İşlem başarılı");
} catch {
  console.error("hata alındı");
}

```

Yukarıdaki  kod örneğinde  nasıl bir çıktı oluşur? Sunucu http 404 döndüğünde bunu hata olarak mı yakalar?  Yanıt tabiki "hayır". Sunucu  http 404 dönecek ama konsolda "işlem başarılı" yazacaktır. Bu istenmeyen bir senaryo olabilir.

## Fetch Api Kullanımında Hatayı Nasıl Yönetmeliyiz ? 
Fetch api  kullanırken birden farklı hata ile karşılaşabiliriz. Bunlar sunucu taraflı hatalar (5xx), not found(404), network hataları ya da cors hataları olabilir.

## Try/Catch bloğu
Fetch api promise tabanlı olduğu için, resolved,rejected,pending durumlarını yönetirken then,catch,finally keywordlerini tam anlamıyla kullanmamızı sağlar. Try/catch bloğu bildiğiniz gibi  hata yönetiminde standart haline geldi desek abartmış olmayız.

```js
try {
    const response = await  fetch('https://google.com/api');
  } catch {
    console.error('Hata oluştu');
  }
}

```
Yukarıdaki kod örneği, promise rejected olduğunda hata verecek şekilde tasarlanmıştır. Bu istek CORS hatası döneceği için konsolda "hata oluştu " yazacaktır. Bu kod bloğuna göre İşlem resolved olduğu durumda hata alınmaz. Yani 404,500 gibi status hatalarında catch() bloğu bunları yakalamayacaktır.

## Hata yönetiminde Http Status kontrolü 
Promise'in resolved olduğu, yani isteğimizin başarılı olduğu ancak, http status değerinin Ok olmadığı durumları ele alacağız.

```js
const response = await fetch("https://jsonplaceholder.typicode.com/todoss");

  if(response.ok){
    console.log("status Ok")
  }else{
    
    console.error("hata, statusCode:"+response.status );
  }

```
Yukarıdaki kod bloğu, http status kodu 2xx olduğunda "status ok" yazacaktır. Onun dışındaki durum kodlarında ise konsolda hata basacaktır. Bu kod örneğinde sunucu 404 döndüğü için konsolda "hata, statusCode:404" yazacaktır.

Peki Http Status 2xx dışındakileri de catch() bloğumuz yakalasın istersek nasıl bir senaryo oluşturmalıyız.
## Benim tek doğrum var, ondan gayrisi hatadır kardeşim
... şeklinde serzenişte bulunanlar için şöyle bir senaryo var: Response.Ok olduğu durumların true, diğer tüm durumların( http status kodları ve cors,network errorlar dahil) hata olarak catch bloğunda yakalanacağı bir kod bloğu oluşturacağız.
```js
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

```js
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
Yukarıdaki kod bloğunda ise sunucu 404 döndüğünden, throw new Error kodu ile hata fırlatılmış ve catch() bloğunda yakalanmıştır.Çıktısı "Error: 404" olacaktır. 

```js
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
Yukarıdaki kod örneğinde ise CORS error oluştuğu için catch() bloğu devreye girmiş ve hata yakalanmıştır. Çıktısı : TypeError: NetworkError when attempting to fetch resource  

Bu yazımız da bu kadar. Sonraki yazımızda görüşmek üzere ...   
<a href="https://github.com/Cankirism/Cankirism.github.io/blob/master/_posts/2023-12-07-FetchApi-hata-yonetimi.md">Yazının raw hali için tıklayınız</a>




