# README-es

> #### You Don't Need jQuery

### Tú no necesitas jQuery

El desarrollo Frontend evoluciona día a día, y los navegadores modernos ya han implementado nativamente APIs para trabajar con DOM/BOM, las cuales son muy buenas, por lo que definitivamente no es necesario aprender jQuery desde cero para manipular el DOM. En la actualidad, gracias al surgimiento de librerías frontend como React, Angular y Vue, manipular el DOM es contrario a los patrones establecidos, y jQuery se ha vuelto menos importante. Este proyecto resume la mayoría de métodos alternativos a jQuery, pero de forma nativa con soporte IE 10+.

### Tabla de Contenidos

1. [Query Selector](readme-es.md#query-selector)
2. [CSS & Estilo](readme-es.md#css--estilo)
3. [Manipulación DOM](readme-es.md#manipulación-dom)
4. [Ajax](readme-es.md#ajax)
5. [Eventos](readme-es.md#eventos)
6. [Utilidades](readme-es.md#utilidades)
7. [Traducción](readme-es.md#traducción)
8. [Soporte de Navegadores](readme-es.md#soporte-de-navegadores)

### Query Selector

En lugar de los selectores comunes como clase, id o atributos podemos usar `document.querySelector` o `document.querySelectorAll` como alternativas. Las diferencias radican en:

* `document.querySelector` devuelve el primer elemento que cumpla con la condición
* `document.querySelectorAll` devuelve todos los elementos que cumplen con la condición en forma de NodeList. Puede ser convertido a Array usando `[].slice.call(document.querySelectorAll(selector) || []);`
* Si ningún elemento cumple con la condición, jQuery retornaría `[]` mientras la API DOM retornaría `null`. Nótese el NullPointerException. Se puede usar `||` para establecer el valor por defecto al no encontrar elementos, como en `document.querySelectorAll(selector) || []`

> Notice: `document.querySelector` and `document.querySelectorAll` are quite **SLOW**, try to use `getElementById`, `document.getElementsByClassName` o `document.getElementsByTagName` if you want to Obtener a performance bonus.

* [1.0](readme-es.md#1.0)  Buscar por selector

  ```javascript
  // jQuery
  $('selector');

  // Nativo
  document.querySelectorAll('selector');
  ```

* [1.1](readme-es.md#1.1)  Buscar por Clase

  ```javascript
  // jQuery
  $('.class');

  // Nativo
  document.querySelectorAll('.class');

  // Forma alternativa
  document.getElementsByClassName('class');
  ```

* [1.2](readme-es.md#1.2)  Buscar por id

  ```javascript
  // jQuery
  $('#id');

  // Nativo
  document.querySelector('#id');

  // Forma alternativa
  document.getElementById('id');
  ```

* [1.3](readme-es.md#1.3)  Buscar por atributo

  ```javascript
  // jQuery
  $('a[target=_blank]');

  // Nativo
  document.querySelectorAll('a[target=_blank]');
  ```

* [1.4](readme-es.md#1.4)  Buscar
  * Buscar nodos

    ```javascript
    // jQuery
    $el.find('li');

    // Nativo
    el.querySelectorAll('li');
    ```

  * Buscar "body"

    ```javascript
    // jQuery
    $('body');

    // Nativo
    document.body;
    ```

  * Buscar Atributo

    ```javascript
    // jQuery
    $el.attr('foo');

    // Nativo
    e.getAttribute('foo');
    ```

  * Buscar atributo "data"

    ```javascript
    // jQuery
    $el.data('foo');

    // Nativo
    // Usando getAttribute
    el.getAttribute('data-foo');
    // También puedes utilizar `dataset` desde IE 11+
    el.dataset['foo'];
    ```
* [1.5](readme-es.md#1.5)  Elementos Hermanos/Previos/Siguientes
  * Elementos hermanos

    ```javascript
    // jQuery
    $el.siblings();

    // Nativo
    [].filter.call(el.parentNode.children, function(child) {
      return child !== el;
    });
    ```

  * Elementos previos

    ```javascript
    // jQuery
    $el.prev();

    // Nativo
    el.previousElementSibling;
    ```

  * Elementos siguientes

    ```javascript
    // jQuery
    $el.next();

    // Nativo
    el.nextElementSibling;
    ```
* [1.6](readme-es.md#1.6)  Closest

  Retorna el elemento más cercano que coincida con la condición, partiendo desde el nodo actual hasta document.

  ```javascript
  // jQuery
  $el.closest(queryString);

  // Nativo
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

* [1.7](readme-es.md#1.7)  Parents Until

  Obtiene los ancestros de cada elemento en el set actual de elementos que cumplan con la condición, sin incluir el actual

  ```javascript
  // jQuery
  $el.parentsUntil(selector, filter);

  // Nativo
  function parentsUntil(el, selector, filter) {
    const result = [];
    const matchesSelector = el.matches || el.webkitMatchesSelector || el.mozMatchesSelector || el.msMatchesSelector;

    // Partir desde el elemento padre
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

* [1.8](readme-es.md#1.8)  Formularios
  * Input/Textarea

    ```javascript
    // jQuery
    $('#my-input').val();

    // Nativo
    document.querySelector('#my-input').value;
    ```

  * Obtener el índice de e.currentTarget en `.radio`

    ```javascript
    // jQuery
    $(e.currentTarget).index('.radio');

    // Nativo
    [].indexOf.call(document.querySelectAll('.radio'), e.currentTarget);
    ```
* [1.9](readme-es.md#1.9)  Contenidos de Iframe

  `$('iframe').contents()` devuelve `contentDocument` para este iframe específico

  * Contenidos de Iframe

    ```javascript
    // jQuery
    $iframe.contents();

    // Nativo
    iframe.contentDocument;
    ```

  * Buscar dentro de un Iframe

    ```javascript
    // jQuery
    $iframe.contents().find('.css');

    // Nativo
    iframe.contentDocument.querySelectorAll('.css');
    ```

[**⬆ volver al inicio**](readme-es.md#tabla-de-contenidos)

### CSS & Estilo

* [2.1](readme-es.md#2.1)  CSS
  * Obtener Estilo

    ```javascript
    // jQuery
    $el.css("color");

    // Nativo
    // NOTA: Bug conocido, retornará 'auto' si el valor de estilo es 'auto'
    const win = el.ownerDocument.defaultView;
    // null significa que no tiene pseudo estilos
    win.getComputedStyle(el, null).color;
    ```

  * Establecer style

    ```javascript
    // jQuery
    $el.css({ color: "#ff0011" });

    // Nativo
    el.style.color = '#ff0011';
    ```

  * Obtener/Establecer Estilos

    Nótese que si se desea establecer múltiples estilos a la vez, se puede utilizar el método [setStyles](https://github.com/oneuijs/oui-dom-utils/blob/master/src/index.js#L194) en el paquete oui-dom-utils.

  * Agregar clase

    ```javascript
    // jQuery
    $el.addClass(className);

    // Nativo
    el.classList.add(className);
    ```

  * Quitar Clase

    ```javascript
    // jQuery
    $el.removeClass(className);

    // Nativo
    el.classList.remove(className);
    ```

  * Consultar si tiene clase

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
* [2.2](readme-es.md#2.2)  Width & Height

  Ancho y Alto son teóricamente idénticos. Usaremos el Alto como ejemplo:

  * Alto de Ventana

    ```javascript
    // alto de ventana
    $(window).height();
    // Sin scrollbar, se comporta como jQuery
    window.document.documentElement.clientHeight;
    // Con scrollbar
    window.innerHeight;
    ```

  * Alto de Documento

    ```javascript
    // jQuery
    $(document).height();

    // Nativo
    document.documentElement.scrollHeight;
    ```

  * Alto de Elemento

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
    // Precisión de integer（when `border-box`, it's `height`; when `content-box`, it's `height + padding + border`）
    el.clientHeight;
    // Precisión de decimal（when `border-box`, it's `height`; when `content-box`, it's `height + padding + border`）
    el.getBoundingClientRect().height;
    ```

* [2.3](readme-es.md#2.3)  Posición & Offset
  * Posición

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
* [2.4](readme-es.md#2.4)  Posición del Scroll Vertical

  ```javascript
  // jQuery
  $(window).scrollTop();

  // Nativo
  (document.documentElement && document.documentElement.scrollTop) || document.body.scrollTop;
  ```

[**⬆ volver al inicio**](readme-es.md#tabla-de-contenidos)

### Manipulación DOM

* [3.1](readme-es.md#3.1)  Remove

  ```javascript
  // jQuery
  $el.remove();

  // Nativo
  el.parentNode.removeChild(el);
  ```

* [3.2](readme-es.md#3.2)  Text
  * Obtener Texto

    ```javascript
    // jQuery
    $el.text();

    // Nativo
    el.textContent;
    ```

  * Establecer Texto

    ```javascript
    // jQuery
    $el.text(string);

    // Nativo
    el.textContent = string;
    ```
* [3.3](readme-es.md#3.3)  HTML
  * Obtener HTML

    ```javascript
    // jQuery
    $el.html();

    // Nativo
    el.innerHTML;
    ```

  * Establecer HTML

    ```javascript
    // jQuery
    $el.html(htmlString);

    // Nativo
    el.innerHTML = htmlString;
    ```
* [3.4](readme-es.md#3.4)  Append

  Añadir elemento hijo después del último hijo del elemento padre

  ```javascript
  // jQuery
  $el.append("<div id='container'>hello</div>");

  // Nativo
  el.insertAdjacentHTML("beforeend","<div id='container'>hello</div>");
  ```

* [3.5](readme-es.md#3.5)  Prepend

  Añadir elemento hijo después del último hijo del elemento padre

  ```javascript
  // jQuery
  $el.prepend("<div id='container'>hello</div>");

  // Nativo
  el.insertAdjacentHTML("afterbegin","<div id='container'>hello</div>");
  ```

* [3.6](readme-es.md#3.6)  insertBefore

  Insertar un nuevo nodo antes del primero de los elementos seleccionados

  ```javascript
  // jQuery
  $newEl.insertBefore(queryString);

  // Nativo
  const target = document.querySelector(queryString);
  target.parentNode.insertBefore(newEl, target);
  ```

* [3.7](readme-es.md#3.7)  insertAfter

  Insertar un nuevo nodo después de los elementos seleccionados

  ```javascript
  // jQuery
  $newEl.insertAfter(queryString);

  // Nativo
  const target = document.querySelector(queryString);
  target.parentNode.insertBefore(newEl, target.nextSibling);
  ```

[**⬆ volver al inicio**](readme-es.md#tabla-de-contenidos)

### Ajax

Reemplazar con [fetch](https://github.com/camsong/fetch-ie8) y [fetch-jsonp](https://github.com/camsong/fetch-jsonp) +[Fetch API](https://fetch.spec.whatwg.org/) es el nuevo estándar quue reemplaza a XMLHttpRequest para efectuar peticiones AJAX. Funciona en Chrome y Firefox, como también es posible usar un polyfill en otros navegadores. + +Es una buena alternativa utilizar [github/fetch](http://github.com/github/fetch) en IE9+ o [fetch-ie8](https://github.com/camsong/fetch-ie8/) en IE8+, [fetch-jsonp](https://github.com/camsong/fetch-jsonp) para efectuar peticiones JSONP. [**⬆ volver al inicio**](readme-es.md#tabla-de-contenidos)

### Eventos

Para un reemplazo completo con namespace y delegación, utilizar [https://github.com/oneuijs/oui-dom-events](https://github.com/oneuijs/oui-dom-events)

* [5.1](readme-es.md#5.1)  Asignar un evento con "on"

  ```javascript
  // jQuery
  $el.on(eventName, eventHandler);

  // Nativo
  el.addEventListener(eventName, eventHandler);
  ```

* [5.2](readme-es.md#5.2)  Desasignar un evento con "off"

  ```javascript
  // jQuery
  $el.off(eventName, eventHandler);

  // Nativo
  el.removeEventListener(eventName, eventHandler);
  ```

* [5.3](readme-es.md#5.3)  Trigger

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

[**⬆ volver al inicio**](readme-es.md#tabla-de-contenidos)

### Utilidades

* [6.1](readme-es.md#6.1)  isArray

  ```javascript
  // jQuery
  $.isArray(range);

  // Nativo
  Array.isArray(range);
  ```

* [6.2](readme-es.md#6.2)  Trim

  ```javascript
  // jQuery
  $.trim(string);

  // Nativo
  string.trim();
  ```

* [6.3](readme-es.md#6.3)  Object Assign

  Utilizar polyfill para object.assign [https://github.com/ljharb/object.assign](https://github.com/ljharb/object.assign)

  ```javascript
  // jQuery
  $.extend({}, defaultOpts, opts);

  // Nativo
  Object.assign({}, defaultOpts, opts);
  ```

* [6.4](readme-es.md#6.4)  Contains

  ```javascript
  // jQuery
  $.contains(el, child);

  // Nativo
  el !== child && el.contains(child);
  ```

[**⬆ volver al inicio**](readme-es.md#tabla-de-contenidos)

### Traducción

* [한국어](readme.ko-kr.md)
* [简体中文](readme.zh-cn.md)
* [Bahasa Melayu](readme-my.md)
* [Bahasa Indonesia](readme-id.md)
* [Português\(PT-BR\)](readme.pt-br.md)
* [Tiếng Việt Nam](readme-vi.md)
* [Español](readme-es.md)
* [Русский](readme-ru.md)
* [Türkçe](readme-tr.md)

### Soporte de Navegadores

| ![Chrome](https://raw.github.com/alrra/browser-logos/master/chrome/chrome_48x48.png) | ![Firefox](https://raw.github.com/alrra/browser-logos/master/firefox/firefox_48x48.png) | ![IE](https://raw.github.com/alrra/browser-logos/master/internet-explorer/internet-explorer_48x48.png) | ![Opera](https://raw.github.com/alrra/browser-logos/master/opera/opera_48x48.png) | ![Safari](https://raw.github.com/alrra/browser-logos/master/safari/safari_48x48.png) |
| :--- | :--- | :--- | :--- | :--- |
| Última ✔ | Última ✔ | 10+ ✔ | Última ✔ | 6.1+ ✔ |

## Licencia

MIT

