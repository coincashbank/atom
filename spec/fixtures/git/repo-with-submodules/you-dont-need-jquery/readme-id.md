# README-id

### Anda tidak memerlukan jQuery

Dewasa ini perkembangan environment frontend sangatlah pesat, dimana banyak browser sudah mengimplementasikan DOM/BOM APIs dengan baik. Kita tidak perlu lagi belajar jQuery dari nol untuk keperluan manipulasi DOM atau events. Disaat yang sama; dengan berterimakasih kepada library frontend terkini seperti React, Angular dan Vue; Memanipulasi DOM secara langsung telah menjadi anti-pattern alias sesuatu yang tidak perlu dilakukan. Dengan kata lain, jQuery sekarang menjadi semakin tidak diperlukan. Projek ini memberikan informasi mengenai metode alternatif dari jQuery untuk implementasi Native dengan support untuk browser IE 10+.

### Daftar Isi

1. [Query Selector](readme-id.md#query-selector)
2. [CSS & Style](readme-id.md#css-style)
3. [DOM Manipulation](readme-id.md#dom-manipulation)
4. [Ajax](readme-id.md#ajax)
5. [Events](readme-id.md#events)
6. [Utilities](readme-id.md#utilities)
7. [Translation](readme-id.md#translation)
8. [Browser Support](readme-id.md#browser-yang-di-support)

### Query Selector

Untuk selector-selector umum seperti class, id atau attribute, kita dapat menggunakan `document.querySelector` atau `document.querySelectorAll` sebagai pengganti. Perbedaan diantaranya adalah:

* `document.querySelector` mengembalikan elemen pertama yang cocok
* `document.querySelectorAll` mengembalikan semua elemen yang cocok sebagai NodeList. Hasilnya bisa dikonversikan menjadi Array `[].slice.call(document.querySelectorAll(selector) || []);`
* Bila tidak ada hasil pengembalian elemen yang cocok, jQuery akan mengembalikan `[]` sedangkan DOM API akan mengembalikan `null`. Mohon diperhatikan mengenai Null Pointer Exception. Anda juga bisa menggunakan operator `||` untuk set nilai awal jika hasil pencarian tidak ditemukan : `document.querySelectorAll(selector) || []`

> Perhatian: `document.querySelector` dan `document.querySelectorAll` sedikit **LAMBAT**. Silahkan menggunakan `getElementById`, `document.getElementsByClassName` atau `document.getElementsByTagName` jika anda menginginkan tambahan performa.

* [1.0](readme-id.md#1.0)  Query by selector

  ```javascript
  // jQuery
  $('selector');

  // Native
  document.querySelectorAll('selector');
  ```

* [1.1](readme-id.md#1.1)  Query by class

  ```javascript
  // jQuery
  $('.class');

  // Native
  document.querySelectorAll('.class');

  // or
  document.getElementsByClassName('class');
  ```

* [1.2](readme-id.md#1.2)  Query by id

  ```javascript
  // jQuery
  $('#id');

  // Native
  document.querySelector('#id');

  // or
  document.getElementById('id');
  ```

* [1.3](readme-id.md#1.3)  Query menggunakan attribute

  ```javascript
  // jQuery
  $('a[target=_blank]');

  // Native
  document.querySelectorAll('a[target=_blank]');
  ```

* [1.4](readme-id.md#1.4)  Pencarian.
  * Mencari nodes

    ```javascript
    // jQuery
    $el.find('li');

    // Native
    el.querySelectorAll('li');
    ```

  * Mencari body

    ```javascript
    // jQuery
    $('body');

    // Native
    document.body;
    ```

  * Mencari Attribute

    ```javascript
    // jQuery
    $el.attr('foo');

    // Native
    e.getAttribute('foo');
    ```

  * Mencari data attribute

    ```javascript
    // jQuery
    $el.data('foo');

    // Native
    // gunakan getAttribute
    el.getAttribute('data-foo');
    // anda juga bisa menggunakan `dataset` bila anda perlu support IE 11+
    el.dataset['foo'];
    ```
* [1.5](readme-id.md#1.5)  Elemen-elemen Sibling/Previous/Next
  * Elemen Sibling

    ```javascript
    // jQuery
    $el.siblings();

    // Native
    [].filter.call(el.parentNode.children, function(child) {
      return child !== el;
    });
    ```

  * Elemen Previous

    ```javascript
    // jQuery
    $el.prev();

    // Native
    el.previousElementSibling;
    ```

  * Elemen Next

    ```javascript
    // next
    $el.next();
    el.nextElementSibling;
    ```
* [1.6](readme-id.md#1.6)  Closest

  Mengembalikan elemen pertama yang cocok dari selector yang digunakan, dengan cara mencari mulai dari elemen-sekarang sampai ke document.

  ```javascript
  // jQuery
  $el.closest(queryString);

  // Native
  function closest(el, selector) {
    const matchesSelector = el.matches || el.webkitMatchesSelector || el.mozMatchesSelector || el.msMatchesSelector;

    while (el) {
      if (matchesSelector.call(el, selector)) {
        return el;
      } else {
        el = el.parentElement;
      }
    }
    return null;
  }
  ```

* [1.7](readme-id.md#1.7)  Parents Until

  Digunakan untuk mendapatkan "ancestor" dari setiap elemen yang ditemukan. Namun tidak termasuk elemen-sekarang yang didapat dari pencarian oleh selector, DOM node, atau object jQuery.

  ```javascript
  // jQuery
  $el.parentsUntil(selector, filter);

  // Native
  function parentsUntil(el, selector, filter) {
    const result = [];
    const matchesSelector = el.matches || el.webkitMatchesSelector || el.mozMatchesSelector || el.msMatchesSelector;

    // match start from parent
    el = el.parentElement;
    while (el && !matchesSelector.call(el, selector)) {
      if (!filter) {
        result.push(el);
      } else {
        if (matchesSelector.call(el, filter)) {
          result.push(el);
        }
      }
      el = el.parentElement;
    }
    return result;
  }
  ```

* [1.8](readme-id.md#1.8)  Form
  * Input/Textarea

    ```javascript
    // jQuery
    $('#my-input').val();

    // Native
    document.querySelector('#my-input').value;
    ```

  * Get index of e.currentTarget between `.radio`

    ```javascript
    // jQuery
    $(e.currentTarget).index('.radio');

    // Native
    [].indexOf.call(document.querySelectAll('.radio'), e.currentTarget);
    ```
* [1.9](readme-id.md#1.9)  Iframe Contents

  `$('iframe').contents()` mengembalikan `contentDocument`

  * Iframe contents

    ```javascript
    // jQuery
    $iframe.contents();

    // Native
    iframe.contentDocument;
    ```

  * Iframe Query

    ```javascript
    // jQuery
    $iframe.contents().find('.css');

    // Native
    iframe.contentDocument.querySelectorAll('.css');
    ```

[**⬆ back to top**](readme-id.md#daftar-isi)

### CSS Style

* [2.1](readme-id.md#2.1)  CSS
  * Get style

    ```javascript
    // jQuery
    $el.css("color");

    // Native
    // PERHATIAN: ada bug disini, dimana fungsi ini akan mengembalikan nilai 'auto' bila nilai dari atribut style adalah 'auto'
    const win = el.ownerDocument.defaultView;
    // null artinya tidak mengembalikan pseudo styles
    win.getComputedStyle(el, null).color;
    ```

  * Set style

    ```javascript
    // jQuery
    $el.css({ color: "#ff0011" });

    // Native
    el.style.color = '#ff0011';
    ```

  * Get/Set Styles

    Mohon dicatat jika anda ingin men-set banyak style bersamaan, anda dapat menemukan referensi di metode [setStyles](https://github.com/oneuijs/oui-dom-utils/blob/master/src/index.js#L194) pada package oui-dom-utils

  * Add class

    ```javascript
    // jQuery
    $el.addClass(className);

    // Native
    el.classList.add(className);
    ```

  * Remove class

    ```javascript
    // jQuery
    $el.removeClass(className);

    // Native
    el.classList.remove(className);
    ```

  * has class

    ```javascript
    // jQuery
    $el.hasClass(className);

    // Native
    el.classList.contains(className);
    ```

  * Toggle class

    ```javascript
    // jQuery
    $el.toggleClass(className);

    // Native
    el.classList.toggle(className);
    ```
* [2.2](readme-id.md#2.2)  Width & Height

  Secara teori, width dan height identik, contohnya Height:

  * Window height

    ```javascript
    // window height
    $(window).height();
    // without scrollbar, behaves like jQuery
    window.document.documentElement.clientHeight;
    // with scrollbar
    window.innerHeight;
    ```

  * Document height

    ```javascript
    // jQuery
    $(document).height();

    // Native
    document.documentElement.scrollHeight;
    ```

  * Element height

    ```javascript
    // jQuery
    $el.height();

    // Native
    function getHeight(el) {
      const styles = this.getComputedStyles(el);
      const height = el.offsetHeight;
      const borderTopWidth = parseFloat(styles.borderTopWidth);
      const borderBottomWidth = parseFloat(styles.borderBottomWidth);
      const paddingTop = parseFloat(styles.paddingTop);
      const paddingBottom = parseFloat(styles.paddingBottom);
      return height - borderBottomWidth - borderTopWidth - paddingTop - paddingBottom;
    }
    // accurate to integer（when `border-box`, it's `height`; when `content-box`, it's `height + padding + border`）
    el.clientHeight;
    // accurate to decimal（when `border-box`, it's `height`; when `content-box`, it's `height + padding + border`）
    el.getBoundingClientRect().height;
    ```

* [2.3](readme-id.md#2.3)  Position & Offset
  * Position

    ```javascript
    // jQuery
    $el.position();

    // Native
    { left: el.offsetLeft, top: el.offsetTop }
    ```

  * Offset

    ```javascript
    // jQuery
    $el.offset();

    // Native
    function getOffset (el) {
      const box = el.getBoundingClientRect();

      return {
        top: box.top + window.pageYOffset - document.documentElement.clientTop,
        left: box.left + window.pageXOffset - document.documentElement.clientLeft
      }
    }
    ```
* [2.4](readme-id.md#2.4)  Scroll Top

  ```javascript
  // jQuery
  $(window).scrollTop();

  // Native
  (document.documentElement && document.documentElement.scrollTop) || document.body.scrollTop;
  ```

[**⬆ back to top**](readme-id.md#daftar-isi)

### DOM Manipulation

* [3.1](readme-id.md#3.1)  Remove

  ```javascript
  // jQuery
  $el.remove();

  // Native
  el.parentNode.removeChild(el);
  ```

* [3.2](readme-id.md#3.2)  Text
  * Get text

    ```javascript
    // jQuery
    $el.text();

    // Native
    el.textContent;
    ```

  * Set text

    ```javascript
    // jQuery
    $el.text(string);

    // Native
    el.textContent = string;
    ```
* [3.3](readme-id.md#3.3)  HTML
  * Get HTML

    ```javascript
    // jQuery
    $el.html();

    // Native
    el.innerHTML;
    ```

  * Set HTML

    ```javascript
    // jQuery
    $el.html(htmlString);

    // Native
    el.innerHTML = htmlString;
    ```
* [3.4](readme-id.md#3.4)  Append

  Menambahkan elemen-anak setelah anak terakhir dari elemen-parent

  ```javascript
  // jQuery
  $el.append("<div id='container'>hello</div>");

  // Native
  let newEl = document.createElement('div');
  newEl.setAttribute('id', 'container');
  newEl.innerHTML = 'hello';
  el.appendChild(newEl);
  ```

* [3.5](readme-id.md#3.5)  Prepend

  ```javascript
  // jQuery
  $el.prepend("<div id='container'>hello</div>");

  // Native
  let newEl = document.createElement('div');
  newEl.setAttribute('id', 'container');
  newEl.innerHTML = 'hello';
  el.insertBefore(newEl, el.firstChild);
  ```

* [3.6](readme-id.md#3.6)  insertBefore

  Memasukkan node baru sebelum elemen yang dipilih.

  ```javascript
  // jQuery
  $newEl.insertBefore(queryString);

  // Native
  const target = document.querySelector(queryString);
  target.parentNode.insertBefore(newEl, target);
  ```

* [3.7](readme-id.md#3.7)  insertAfter

  Memasukkan node baru sesudah elemen yang dipilih.

  ```javascript
  // jQuery
  $newEl.insertAfter(queryString);

  // Native
  const target = document.querySelector(queryString);
  target.parentNode.insertBefore(newEl, target.nextSibling);
  ```

[**⬆ back to top**](readme-id.md#daftar-isi)

### Ajax

Gantikan dengan [fetch](https://github.com/camsong/fetch-ie8) dan [fetch-jsonp](https://github.com/camsong/fetch-jsonp)

[**⬆ back to top**](readme-id.md#daftar-isi)

### Events

Untuk penggantian secara menyeluruh dengan namespace dan delegation, rujuk ke [https://github.com/oneuijs/oui-dom-events](https://github.com/oneuijs/oui-dom-events)

* [5.1](readme-id.md#5.1)  Bind event dengan menggunakan on

  ```javascript
  // jQuery
  $el.on(eventName, eventHandler);

  // Native
  el.addEventListener(eventName, eventHandler);
  ```

* [5.2](readme-id.md#5.2)  Unbind event dengan menggunakan off

  ```javascript
  // jQuery
  $el.off(eventName, eventHandler);

  // Native
  el.removeEventListener(eventName, eventHandler);
  ```

* [5.3](readme-id.md#5.3)  Trigger

  ```javascript
  // jQuery
  $(el).trigger('custom-event', {key1: 'data'});

  // Native
  if (window.CustomEvent) {
    const event = new CustomEvent('custom-event', {detail: {key1: 'data'}});
  } else {
    const event = document.createEvent('CustomEvent');
    event.initCustomEvent('custom-event', true, true, {key1: 'data'});
  }

  el.dispatchEvent(event);
  ```

[**⬆ back to top**](readme-id.md#daftar-isi)

### Utilities

* [6.1](readme-id.md#6.1)  isArray

  ```javascript
  // jQuery
  $.isArray(range);

  // Native
  Array.isArray(range);
  ```

* [6.2](readme-id.md#6.2)  Trim

  ```javascript
  // jQuery
  $.trim(string);

  // Native
  string.trim();
  ```

* [6.3](readme-id.md#6.3)  Object Assign

  Extend, use object.assign polyfill [https://github.com/ljharb/object.assign](https://github.com/ljharb/object.assign)

  ```javascript
  // jQuery
  $.extend({}, defaultOpts, opts);

  // Native
  Object.assign({}, defaultOpts, opts);
  ```

* [6.4](readme-id.md#6.4)  Contains

  ```javascript
  // jQuery
  $.contains(el, child);

  // Native
  el !== child && el.contains(child);
  ```

[**⬆ back to top**](readme-id.md#daftar-isi)

### Terjemahan

* [한국어](readme.ko-kr.md)
* [简体中文](readme.zh-cn.md)
* [Bahasa Melayu](readme-my.md)
* [Bahasa Indonesia](readme-id.md)
* [Português\(PT-BR\)](readme.pt-br.md)
* [Tiếng Việt Nam](readme-vi.md)
* [Русский](readme-ru.md)
* [Türkçe](readme-tr.md)

### Browser yang di Support

| ![Chrome](https://raw.github.com/alrra/browser-logos/master/chrome/chrome_48x48.png) | ![Firefox](https://raw.github.com/alrra/browser-logos/master/firefox/firefox_48x48.png) | ![IE](https://raw.github.com/alrra/browser-logos/master/internet-explorer/internet-explorer_48x48.png) | ![Opera](https://raw.github.com/alrra/browser-logos/master/opera/opera_48x48.png) | ![Safari](https://raw.github.com/alrra/browser-logos/master/safari/safari_48x48.png) |
| :--- | :--- | :--- | :--- | :--- |
| Latest ✔ | Latest ✔ | 10+ ✔ | Latest ✔ | 6.1+ ✔ |

## License

MIT

