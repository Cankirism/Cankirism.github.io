---
layout: post
title: Derinlemesine Yup Kullanımı 
---

## Yup Nedir ?   
Yup, react uygulamalarında form validasyon yapmamızı sağlayan kütüphanedir.  

npm install yup komutu ile kolayca orpjenize ekleyebilirsiniz. Yup kurulumu tamamlanınca şema tanımlama işlemine başlayabilirsiniz. Yup'ta form validasyonları şema olarak adlandırılan  object'ler ile yapılır.   

örnek olarak kullanıcı adı ve parola alanı için bir validasyon şeması tanımlayalım.  
``` js

import schema from './validationScheme';
export default function YupTest() {
  const formData = {
    username:"user",
    password:"pass123"
  };
  const errors = schema.validate(formData);
  console.error(errors)
}

```
validationSchema.js
``` js
import * as YUP from 'yup'

const schema=YUP.object().shape({
    username:YUP.string().required(),
    password:YUP.string().required().min(8)
})
export default schema;
```

uygulamayı çalıştırdığımızda konsolda aşağıdaki gibi hata alacağız
```js
ValidationError: password must be at least 8 characters
```
Dikkat edilmesi gereken hususlar, formdaki alanların, schmema içerisindeki field'lar ile aynı olmasıdır.
Yup, farklı veri türleri için validasyon şema nesnesi barındırır. Örnein number(),positive(),email() vb.   
``` js
import schema from './validationScheme';
export default function YupTest() {
  const formData = {
    username:"user",
    password:"pass123",
    email:"aaa.gmail.com"
  };
  const errors = schema.validate(formData);
  console.error(errors)
}
```
validationSchema.js  
``` js
import * as YUP from 'yup'

const schema=YUP.object().shape({
    username:YUP.string().required(),
    password:YUP.string().required().min(8),
    email:YUP.string().email("eposta formatına uygun olmalıdır ").required("E posta alanı gereklidir")
})

export default schema;
```
uygulamayı çalıştırdığımızda aşağdaki hatayı alacağız 
```js
ValidationError: eposta formatına uygun olmalıdır 
```
aşağıdaki gibi bir formData validate etmeye çalışırsak, "Epost alanı gereklidir" hatası alıyor olacağız.

```js
import schema from './validationScheme';
export default function YupTest() {
  const formData = {
    username:"user",
    password:"pass123",
    email:""
  };
  const errors = schema.validate(formData);
  console.error(errors)
}


```
Yup ile regex patter yazıp, formdaki alanın bu kalıba uyup uymadığını da test edebiliriz.

Telefon numarası için  tanımlanmış bir yup şeması 
```js
import * as YUP from 'yup'

const schema=YUP.object().shape({
    phone:YUP.string()
             .required()
             .matches("^5[0-9]{2}[0-9]{7}","Hatalı telefon numarası") //5 ile  başlayan 10 haneli telefon numarası ile match olacak 
})

export default schema;
```
validationSchema.js  

```js

import schema from './validationScheme';
export default function YupTest() {
  const formData = {
   phone:"1441234567"
  };
  const errors = schema.validate(formData);
  console.error(errors)
}

// çıktı :ValidationError: Hatalı telefon numarası 



```
yup ile url validasyonu yapabilirsiniz   
```js
import schema from './validationScheme';
export default function YupTest() {
  const formData = {
   url:"http://aa.com"
  };
  const errors = schema.validate(formData);
  console.error(errors)
}


```
validationScheme.js
```js
import * as YUP from 'yup'

const schema=YUP.object().shape({
   url:YUP.string().url("tanımsız url")
})

export default schema;


```
Tarih(date) alanları için validasyon 

```js
import * as YUP from 'yup'

const schema=YUP.object().shape({
   tarih:YUP.date().default(()=>new Date())
})

export default schema;
```
Numerik alanlar için validasyon  şeması
pozitif tam sayı şeması tanımlanmıştır.

``` js

const schema=YUP.object().shape({
   sayi:YUP
         .number("Nümerik olmalı") 
         .positive("Sıfırdan büyük olmalı")
         .integer("tam sayı olmalı")
})

export default schema;
```
```js
import schema from './validationScheme';
export default function YupTest() {
  const formData = {
   sayi:1.2
  };
  const errors = schema.validate(formData);
  console.error(errors)
}

```
çalıştırdığımızda "ValidationError: tam sayı olmalı" hatasını alacağız














