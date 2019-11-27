# README-it

### Non hai bisogno di jQuery

Il mondo del Frontend si evolve rapidamente oggigiorno, i browsers moderni hanno gia' implementato un'ampia gamma di DOM/BOM API soddisfacenti. Non dobbiamo imparare jQuery dalle fondamenta per la manipolazione del DOM o di eventi. Nel frattempo, grazie al prevalicare di librerie per il frontend come React, Angular a Vue, manipolare il DOM direttamente diventa un anti-pattern, di consequenza jQuery non e' mai stato meno importante. Questo progetto sommarizza la maggior parte dei metodi e implementazioni alternative a jQuery, con il supporto di IE 10+.

### Tabella contenuti

1. [Query Selector](readme-it.md#query-selector)
2. [CSS & Style](readme-it.md#css--style)
3. [Manipolazione DOM](readme-it.md#manipolazione-dom)
4. [Ajax](readme-it.md#ajax)
5. [Eventi](readme-it.md#eventi)
6. [Utilities](readme-it.md#utilities)
7. [Alternative](readme-it.md#alternative)
8. [Traduzioni](readme-it.md#traduzioni)
9. [Supporto Browsers](readme-it.md#supporto-browsers)

### Query Selector

Al posto di comuni selettori come class, id o attributi possiamo usare `document.querySelector` o `document.querySelectorAll` per sostituzioni. La differenza risiede in:

* `document.querySelector` restituisce il primo elemento combiaciante
* `document.querySelectorAll` restituisce tutti gli elementi combiacianti della NodeList. Puo' essere convertito in Array usando `[].slice.call(document.querySelectorAll(selector) || []);`
* Se nessun elemento combiacia, jQuery restituitirebbe `[]` li' dove il DOM API ritornera' `null`. Prestate attenzione al Null Pointer Exception. Potete anche usare `||` per settare valori di default se non trovato, come `document.querySelectorAll(selector) || []`

> Notare: `document.querySelector` e `document.querySelectorAll` sono abbastanza **SLOW**, provate ad usare `getElementById`, `document.getElementsByClassName` o `document.getElementsByTagName` se volete avere un bonus in termini di performance.

* [1.0](readme-it.md#1.0)  Query da selettore

  ```javascript
  // jQuery
  $('selector');

  // Nativo
  document.querySelectorAll('selector');
  ```

* [1.1](readme-it.md#1.1)  Query da classe

  ```javascript
  // jQuery
  $('.class');

  // Nativo
  document.querySelectorAll('.class');

  // or
  document.getElementsByClassName('class');
  ```

* [1.2](readme-it.md#1.2)  Query da id

  ```javascript
  // jQuery
  $('#id');

  // Nativo
  document.querySelector('#id');

  // o
  document.getElementById('id');
  ```

* [1.3](readme-it.md#1.3)  Query da attributo

  ```javascript
  // jQuery
  $('a[target=_blank]');

  // Nativo
  document.querySelectorAll('a[target=_blank]');
  ```

* [1.4](readme-it.md#1.4)  Trovare qualcosa.
  * Trovare nodes

    ```javascript
    // jQuery
    $el.find('li');

    // Nativo
    el.querySelectorAll('li');
    ```

  * Trovare body

    ```javascript
    // jQuery
    $('body');

    // Nativo
    document.body;
    ```

  * Trovare Attributi

    ```javascript
    // jQuery
    $el.attr('foo');

    // Nativo
    e.getAttribute('foo');
    ```

  * Trovare attributo data

    ```javascript
    // jQuery
    $el.data('foo');

    // Nativo
    // using getAttribute
    el.getAttribute('data-foo');
    // potete usare `dataset` solo se supportate IE 11+
    el.dataset['foo'];
    ```
* [1.5](readme-it.md#1.5)  Fratelli/Precedento/Successivo Elemento
  * Elementi fratelli

    ```javascript
    // jQuery
    $el.siblings();

    // Nativo
    [].filter.call(el.parentNode.children, function(child) {
      return child !== el;
    });
    ```

  * Elementi precedenti

    ```javascript
    // jQuery
    $el.prev();

    // Nativo
    el.previousElementSibling;
    ```

  * Elementi successivi

    ```javascript
    // jQuery
    $el.next();

    // Nativo
    el.nextElementSibling;
    ```
* [1.6](readme-it.md#1.6)  Il piu' vicino

  Restituisce il primo elementi combiaciante il selettore fornito, attraversando dall'elemento corrente fino al document .

  ```javascript
  // jQuery
  $el.closest(queryString);

  // Nativo - Solo ultimo, NO IE
  el.closest(selector);

  // Nativo - IE10+ 
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

* [1.7](readme-it.md#1.7)  Fino a parenti

  Ottiene il parente di ogni elemento nel set corrente di elementi combiacianti, fino a ma non incluso, l'elemento combiaciante il selettorer, DOM node, o jQuery object.

  ```javascript
  // jQuery
  $el.parentsUntil(selector, filter);

  // Nativo
  function parentsUntil(el, selector, filter) {
    const result = [];
    const matchesSelector = el.matches || el.webkitMatchesSelector || el.mozMatchesSelector || el.msMatchesSelector;

    // il match parte dal parente
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

* [1.8](readme-it.md#1.8)  Form
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

    // Nativo
    [].indexOf.call(document.querySelectAll('.radio'), e.currentTarget);
    ```
* [1.9](readme-it.md#1.9)  Iframe Contents

  `$('iframe').contents()` restituisce `contentDocument` per questo specifico iframe

  * Iframe contenuti

    ```javascript
    // jQuery
    $iframe.contents();

    // Nativo
    iframe.contentDocument;
    ```

  * Iframe Query

    ```javascript
    // jQuery
    $iframe.contents().find('.css');

    // Nativo
    iframe.contentDocument.querySelectorAll('.css');
    ```

[**⬆ back to top**](readme-it.md#table-of-contents)

### CSS & Style

* [2.1](readme-it.md#2.1)  CSS
  * Ottenere style

    ```javascript
    // jQuery
    $el.css("color");

    // Nativo
    // NOTA: Bug conosciuto, restituira' 'auto' se il valore di style e' 'auto'
    const win = el.ownerDocument.defaultView;
    // null significa che non restituira' lo psuedo style
    win.getComputedStyle(el, null).color;
    ```

  * Settare style

    ```javascript
    // jQuery
    $el.css({ color: "#ff0011" });

    // Nativo
    el.style.color = '#ff0011';
    ```

  * Ottenere/Settare Styles

    Nota che se volete settare styles multipli in una sola volta, potete riferire [setStyles](https://github.com/oneuijs/oui-dom-utils/blob/master/src/index.js#L194) metodo in oui-dom-utils package.
* Aggiungere classe

  ```javascript
  // jQuery
  $el.addClass(className);

  // Nativo
  el.classList.add(className);
  ```

* Rimouvere class

  ```javascript
  // jQuery
  $el.removeClass(className);

  // Nativo
  el.classList.remove(className);
  ```

* has class

  ```javascript
  // jQuery
  $el.hasClass(className);

  // Nativo
  el.classList.contains(className);
  ```

* Toggle class

  ```javascript
  // jQuery
  $el.toggleClass(className);

  // Nativo
  el.classList.toggle(className);
  ```

* [2.2](readme-it.md#2.2)  Width & Height

  Width e Height sono teoricamente identici, prendendo Height come esempio:

  * Window height

    ```javascript
    // window height
    $(window).height();
    // senza scrollbar, si comporta comporta jQuery
    window.document.documentElement.clientHeight;
    // con scrollbar
    window.innerHeight;
    ```

  * Document height

    ```javascript
    // jQuery
    $(document).height();

    // Nativo
    document.documentElement.scrollHeight;
    ```

  * Element height

    ```javascript
    // jQuery
    $el.height();

    // Nativo
    function getHeight(el) {
      const styles = this.getComputedStyles(el);
      const height = el.offsetHeight;
      const borderTopWidth = parseFloat(styles.borderTopWidth);
      const borderBottomWidth = parseFloat(styles.borderBottomWidth);
      const paddingTop = parseFloat(styles.paddingTop);
      const paddingBottom = parseFloat(styles.paddingBottom);
      return height - borderBottomWidth - borderTopWidth - paddingTop - paddingBottom;
    }
    // preciso a intero（quando `border-box`, e' `height`; quando `content-box`, e' `height + padding + border`）
    el.clientHeight;
    // preciso a decimale（quando `border-box`, e' `height`; quando `content-box`, e' `height + padding + border`）
    el.getBoundingClientRect().height;
    ```

* [2.3](readme-it.md#2.3)  Position & Offset
  * Position

    ```javascript
    // jQuery
    $el.position();

    // Nativo
    { left: el.offsetLeft, top: el.offsetTop }
    ```

  * Offset

    ```javascript
    // jQuery
    $el.offset();

    // Nativo
    function getOffset (el) {
      const box = el.getBoundingClientRect();

      return {
        top: box.top + window.pageYOffset - document.documentElement.clientTop,
        left: box.left + window.pageXOffset - document.documentElement.clientLeft
      }
    }
    ```
* [2.4](readme-it.md#2.4)  Scroll Top

  ```javascript
  // jQuery
  $(window).scrollTop();

  // Nativo
  (document.documentElement && document.documentElement.scrollTop) || document.body.scrollTop;
  ```

[**⬆ back to top**](readme-it.md#table-of-contents)

### Manipolazione DOM

* [3.1](readme-it.md#3.1)  Remove

  ```javascript
  // jQuery
  $el.remove();

  // Nativo
  el.parentNode.removeChild(el);
  ```

* [3.2](readme-it.md#3.2)  Text
  * Get text

    ```javascript
    // jQuery
    $el.text();

    // Nativo
    el.textContent;
    ```

  * Set text

    ```javascript
    // jQuery
    $el.text(string);

    // Nativo
    el.textContent = string;
    ```
* [3.3](readme-it.md#3.3)  HTML
  * Ottenere HTML

    ```javascript
    // jQuery
    $el.html();

    // Nativo
    el.innerHTML;
    ```

  * Settare HTML

    ```javascript
    // jQuery
    $el.html(htmlString);

    // Nativo
    el.innerHTML = htmlString;
    ```
* [3.4](readme-it.md#3.4)  Append

  appendere elemento figlio dopo l'ultimo elemento figlio del genitore

  ```javascript
  // jQuery
  $el.append("<div id='container'>hello</div>");

  // Nativo
  el.insertAdjacentHTML("beforeend","<div id='container'>hello</div>");
  ```

* [3.5](readme-it.md#3.5)  Prepend

  ```javascript
  // jQuery
  $el.prepend("<div id='container'>hello</div>");

  // Nativo
  el.insertAdjacentHTML("afterbegin","<div id='container'>hello</div>");
  ```

* [3.6](readme-it.md#3.6)  insertBefore

  Inserire un nuovo node dopo l'elmento selezionato

  ```javascript
  // jQuery
  $newEl.insertBefore(queryString);

  // Nativo
  const target = document.querySelector(queryString);
  target.parentNode.insertBefore(newEl, target);
  ```

* [3.7](readme-it.md#3.7)  insertAfter

  Insert a new node after the selected elements

  ```javascript
  // jQuery
  $newEl.insertAfter(queryString);

  // Nativo
  const target = document.querySelector(queryString);
  target.parentNode.insertBefore(newEl, target.nextSibling);
  ```

* [3.8](readme-it.md#3.8)  is

  Restituisce `true` se combacia con l'elemento selezionato

  ```javascript
  // jQuery - Notare `is` funziona anche con `function` o `elements` non di importanza qui
  $el.is(selector);

  // Nativo
  el.matches(selector);
  ```

[**⬆ back to top**](readme-it.md#table-of-contents)

### Ajax

Sostituire con [fetch](https://github.com/camsong/fetch-ie8) and [fetch-jsonp](https://github.com/camsong/fetch-jsonp)

[**⬆ back to top**](readme-it.md#table-of-contents)

### Eventi

Per una completa sostituzione con namespace e delegation, riferire a [https://github.com/oneuijs/oui-dom-events](https://github.com/oneuijs/oui-dom-events)

* [5.1](readme-it.md#5.1)  Bind un evento con on

  ```javascript
  // jQuery
  $el.on(eventName, eventHandler);

  // Nativo
  el.addEventListener(eventName, eventHandler);
  ```

* [5.2](readme-it.md#5.2)  Unbind an event with off

  ```javascript
  // jQuery
  $el.off(eventName, eventHandler);

  // Nativo
  el.removeEventListener(eventName, eventHandler);
  ```

* [5.3](readme-it.md#5.3)  Trigger

  ```javascript
  // jQuery
  $(el).trigger('custom-event', {key1: 'data'});

  // Nativo
  if (window.CustomEvent) {
    const event = new CustomEvent('custom-event', {detail: {key1: 'data'}});
  } else {
    const event = document.createEvent('CustomEvent');
    event.initCustomEvent('custom-event', true, true, {key1: 'data'});
  }

  el.dispatchEvent(event);
  ```

[**⬆ back to top**](readme-it.md#table-of-contents)

### Utilities

* [6.1](readme-it.md#6.1)  isArray

  ```javascript
  // jQuery
  $.isArray(range);

  // Nativo
  Array.isArray(range);
  ```

* [6.2](readme-it.md#6.2)  Trim

  ```javascript
  // jQuery
  $.trim(string);

  // Nativo
  string.trim();
  ```

* [6.3](readme-it.md#6.3)  Object Assign

  Extend, usa object.assign polyfill [https://github.com/ljharb/object.assign](https://github.com/ljharb/object.assign)

  ```javascript
  // jQuery
  $.extend({}, defaultOpts, opts);

  // Nativo
  Object.assign({}, defaultOpts, opts);
  ```

* [6.4](readme-it.md#6.4)  Contains

  ```javascript
  // jQuery
  $.contains(el, child);

  // Nativo
  el !== child && el.contains(child);
  ```

[**⬆ back to top**](readme-it.md#table-of-contents)

### Alternative

* [Forse non hai bisogno di jQuery](http://youmightnotneedjquery.com/) - Esempi di come creare eventi comuni, elementi, ajax etc usando puramente javascript.
* [npm-dom](http://github.com/npm-dom) e [webmodules](http://github.com/webmodules) - Organizzazione dove puoi trovare moduli per il DOM individuale su NPM

### Traduzioni

* [한국어](readme.ko-kr.md)
* [简体中文](readme.zh-cn.md)
* [Bahasa Melayu](readme-my.md)
* [Bahasa Indonesia](readme-id.md)
* [Português\(PT-BR\)](readme.pt-br.md)
* [Tiếng Việt Nam](readme-vi.md)
* [Español](readme-es.md)
* [Italiano](readme-it.md)
* [Türkçe](readme-tr.md)

### Supporto Browsers

| ![Chrome](https://raw.github.com/alrra/browser-logos/master/chrome/chrome_48x48.png) | ![Firefox](https://raw.github.com/alrra/browser-logos/master/firefox/firefox_48x48.png) | ![IE](https://raw.github.com/alrra/browser-logos/master/internet-explorer/internet-explorer_48x48.png) | ![Opera](https://raw.github.com/alrra/browser-logos/master/opera/opera_48x48.png) | ![Safari](https://raw.github.com/alrra/browser-logos/master/safari/safari_48x48.png) |
| :--- | :--- | :--- | :--- | :--- |
| Ultimo ✔ | Ultimo ✔ | 10+ ✔ | Ultimo ✔ | 6.1+ ✔ |

## Licenza

MIT

