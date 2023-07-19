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
örnekte görüldüğü gibi, useState() tanımı bir array dönmüş, ilk parametresi state'in adı, ikinci parametresi ile bu state'i manupule edeceğimiz fonksiyonun adı. Kültür olarak manupule edecek fonksyion  ==> setStateadi şeklinde camel case tanımlanır.Yukarıdaki örnekte button tıklandığında , setCount() fonksiyonu , counter değerini 1 artırarak counter state'ini yeniden set edecektir.





