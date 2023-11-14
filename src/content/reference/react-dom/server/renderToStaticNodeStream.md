---
title: renderToStaticNodeStream
---

<Intro>

`renderToStaticNodeStream` interaktif olmayan bir React ağacını [Okunabilir Node.js Akışına](https://nodejs.org/api/stream.html#readable-streams) render etmenize olanak tanır.

```js
const stream = renderToStaticNodeStream(reactNode)
```

</Intro>

<InlineToc />

---

## Başvuru dokümanı {/*reference*/}

### `renderToStaticNodeStream(reactNode)` {/*rendertostaticnodestream*/}

Sunucuda, [Okunabilir Node.js Akışı](https://nodejs.org/api/stream.html#readable-streams) elde etmek için `renderToStaticNodeStream` fonksiyonunu çağırınız.

```js
import { renderToStaticNodeStream } from 'react-dom/server';

const stream = renderToStaticNodeStream(<Page />);
stream.pipe(response);
```

[Buradan daha fazla örnek görebilirsiniz.](#usage)

Akış, React bileşenlerinizin etkileşimli olmayan HTML çıktılarını üretecektir.

#### Parametreler {/*parameters*/}

* `reactNode`: HTML'e render etmek istediğiniz bir React düğümü. Örneğin, `<Sayfa />` gibi bir JSX elemanı.

#### Dönüş Değeri {/*returns*/}

HTML string'i döndüren bir [Okunabilir Node.js Akışı](https://nodejs.org/api/stream.html#readable-streams) döndürür. Ortaya çıkan HTML istemcide sulanamaz.

#### Uyarılar {/*caveats*/}

* `renderToStaticNodeStream` çıktısı sulanamaz.

* Bu yöntem, herhangi bir çıktı döndürmeden önce tüm [Suspense sınırlarının](/reference/react/Suspense) tamamlanmasını bekler.

* React 18'den itibaren, bu yöntem tüm çıktısını ara belleğe alır, aslında bu nedenle herhangi bir akış avantajı sağlamaz.

* Döndürülen akış utf-8 olarak kodlanmış bir byte akışıdır. Başka bir kodlamada akışa ihtiyacınız varsa, metni dönüştürmek için dönüştürme akışları sağlayan [iconv-lite](https://www.npmjs.com/package/iconv-lite) gibi bir projeye göz atabilirsiniz.

---

## Kullanım {/*usage*/}

### Bir React ağacını statik HTML olarak Okunabilir Node.js Akışına dönüştürme {/*rendering-a-react-tree-as-static-html-to-a-nodejs-readable-stream*/}

Sunucu yanıtınıza aktarabileceğiniz bir [Okunabilir Node.js Akışı](https://nodejs.org/api/stream.html#readable-streams) elde etmek için `renderToStaticNodeStream` fonksiyonunu çağırabilirsiniz:

```js {5-6}
import { renderToStaticNodeStream } from 'react-dom/server';

// Route handler syntax backend çatınıza bağlıdır
app.use('/', (request, response) => {
  const stream = renderToStaticNodeStream(<Page />);
  stream.pipe(response);
});
```

Akış, React bileşenlerinizin etkileşimli olmayan ilk HTML çıktısını üretecektir.

<Pitfall>

Bu yöntem, **sulanamayan ve interaktif olmayan HTML oluşturur.** Bu yöntem, React'i basit bir statik sayfa oluşturucu olarak kullanmak istiyorsanız veya e-postalar gibi tamamen statik içerik oluşturuyorsanız kullanışlıdır.

İnteraktif uygulamalar [`renderToPipeableStream`](/reference/react-dom/server/renderToPipeableStream)'i sunucu tarafında kullanmalıdır. [`hydrateRoot`](/reference/react-dom/client/hydrateRoot)'u ise kullanıcı tarafında kullanmalıdır.

</Pitfall>