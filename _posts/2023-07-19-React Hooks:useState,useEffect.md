# Kitabın Ortasından. React Hooks:useState(), useEffect()

Merhaba, uzun aradan sonra yeni blog yazımız ile yayındayız. Bugün React 16.8 ile hayatımıza giren React hooklarından useState ve useEffect ile çalışıyor olacağız.

## React Hooks Nedir ?

Hooks, React 16.8 ile kullanılmaya başlayan, sınıf yazmadan state ve diğer react bileşenlerini kullanmamıza, manupule etmemize olanak sağlayan yapılardır. useState,useEffect, useCOntext,useReducer gibi birçok hook tanımı yer almaktadır.

## useState
useState, sınıf tanımlamadan state değişken tanımlayıp, bunu manupule  edilmesini sağlayan bir hooktur.
Class componentlerde önceden bildiğiniz gibi aşağıda yer alan yapıda state tanımlar ve erişirdik.

``` js
import React, { Component } from 'react';
import './Product.css';

export default class Product extends Component {

  state = {
    cart: [],
    total: 0
  }

  render() {
    return(
      <div className="wrapper">
        <div>
          Shopping Cart: {this.state.cart.length} total items.
        </div>
        <div>Total {this.state.total}</div>

        <div className="product"><span role="img" aria-label="ice cream">🍦</span></div>
        <button>Add</button> <button>Remove</button>
      </div>
    )
  }
}
```

useState yapısı bun tanımlamalar ile uğraşmadan çok kolay şekilde state tanımlamamızı sağlar.
react sayfasın örneği : 

``` js
import React, { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
örnekte görüldüğü gibi, useState() tanımı bir array dönmüş, ilk parametresi state'in adı, ikinci parametresi ile bu state'i manupule edeceğimiz fonksiyonun adı. Kültür olarak manupule edecek fonksyion  ==> setStateadi şeklinde camel case tanımlanır.Yukarıdaki örnekte button tıklandığında , setCount() fonksiyonu , counter değerini 1 artırarak counter state'ini yeniden set edecektir.useState(0) kullanımı başlangıç değer belirlenmek için kullanılmıştır.

## useEffect
useEffect hook ile, bir componenet  render olduktan  ya da bir fonksyion çalıştıktan sonra  çalışacak fonksiyonu tanımlamamızı sağlayan yapıdır.

``` js
  useEffect(()=>{},[])  
```
şeklinde kullanılır. ilk parametre fonksiyon, 2. parametre ise dependecy belirtir. Yani hangi duruma ya da state değerine göre bir işlem tetiklenecekse o state belirtilir.

 ``` js
useEffect(()=>console.log("Merhaba"),[])
```
yukarıdaki örnek uygulama render edildiğinde 1 kez çalışmak suretiyle  console'a Merhaba basacaktır.

 ``` js
useEffect(()=>console.log("Merhaba"),[counter])
```
Bu örnek uygulama ise counter state'ine bağımlılık içermektedir. Yani counter değeri her değiştiğinde console'a merhaba basacaktır.

şimdi detaylı inceleme yapacağımız örnek ile hooklarımızı kullanalım.
senaryo : Bir api endpointen, id değerine göre ilgili nesneyi çeken, ,response içerisinde title değerine göre title state'ini set eden uygulama
ApiSample.js
ilgili componentleri import edelim :

``` js
import React, { useEffect, useState } from "react";
import ApiTitle from "./ApiTitle";


```
useState ile stateleri tanımlayalım. id ve title isimli 2 adet state var. Başlangıç değerleri 1 ve "". 

``` js
export default function ApiSample() {
  const [id, setId] = useState(1);
  const [title,setTitle]=useState("");
```
Biz uygulamamızda id değeri değiştiğinde , api endpointe ilgili id'deki nesne için istek atmasını isteyeceğız. Bu sebeple id state her değiştiğinde, useEffect ile api'ye istek atan, getApi() fonksiyonunu tetikleyeceğiz.
``` js
useEffect(() => fetchApi(), [id]);
```
şimdi api fetch edelim.

``` js
function fetchApi() {
    const url="https://jsonplaceholder.typicode.com/photos?id="+id;
    fetch(url)
    .then(res=>res.json())
    .then(result=>setTitle(result[0].title))
```
``` js
 return (
     <div >
     <h5>{title}</h5>
       <button onClick={()=>setId(id+1)}>{id} Id'ye ait datayı fetch </button>
       <ApiTitle  setTitle={setTitle} title={title}></ApiTitle>
     </div>
  );
```
bu componente tanımlı statelere diğer  componenetlerden erişmek isteyebilir ya da onları değiştirmek isteyebiliriz. Bu durumda props yardımıyla ilgili state ve fonksiyonları ilgili componentlere göndeririz
aşağıda görüldüğü üzere, ApiTitle Componentine setTitle ve title isimli props ile state ve fonksiyonu gönderdik.
``` js
 <ApiTitle  setTitle={setTitle} title={title}></ApiTitle>
```

ApiTitle.js
import React from 'react'
export default function ApiTitle(props) {
  return (
    <div>
        title is: {props.title}
       <button onClick={()=>
    {
        props.setTitle("bbb");
    }} >set title </button>
        </div>
  )
}

``` js

```


