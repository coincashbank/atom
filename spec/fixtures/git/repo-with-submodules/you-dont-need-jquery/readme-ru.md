# README-ru

### Вам не нужен jQuery

В наше время среда фронт энд разработки быстро развивается, современные браузеры уже реализовали значимую часть DOM/BOM APIs и это хорошо. Вам не нужно изучать jQuery с нуля для манипуляцией DOM'ом или обектами событий. В то же время, благодаря лидирующим фронт энд библиотекам, таким как React, Angular и Vue, манипуляция DOM'ом напрямую становится противо шаблонной, jQuery никогда не был менее важен. Этот проект суммирует большинство альтернатив методов jQuery в нативном исполнении с поддержкой IE 10+.

### Содержание

1. [Query Selector](readme-ru.md#query-selector)
2. [CSS & Style](readme-ru.md#css--style)
3. [Манипуляция DOM](readme-ru.md#Манипуляции-dom)
4. [Ajax](readme-ru.md#ajax)
5. [События](readme-ru.md#События)
6. [Утилиты](readme-ru.md#Утилиты)
7. [Альтернативы](readme-ru.md#Альтернативы)
8. [Переводы](readme-ru.md#Переводы)
9. [Поддержка браузеров](readme-ru.md#Поддержка-браузеров)

### Query Selector

Для часто используемых селекторов, таких как class, id или attribute мы можем использовать `document.querySelector` или `document.querySelectorAll` для замены. Разница такова:

* `document.querySelector` возвращает первый совпавший элемент
* `document.querySelectorAll` возвращает все совспавшие элементы как  коллекцию узлов\(NodeList\). Его можно конвертировать в массив используя `[].slice.call(document.querySelectorAll(selector) || []);`
* Если никакие элементы не совпадут, jQuery вернет `[]` где DOM API вернет `null`. Обратите внимание на указатель исключения  Null \(Null Pointer Exception\). Вы так же можете использовать `||` для установки значения по умолчанию если не было найдемо совпадений `document.querySelectorAll(selector) || []`

> Заметка: `document.querySelector` и `document.querySelectorAll` достаточно **МЕДЛЕННЫ**, старайтесь использовать `getElementById`, `document.getElementsByClassName` или `document.getElementsByTagName` если хотите улучшить производительность.

* [1.0](readme-ru.md#1.0)  Query by selector

  ```javascript
  // jQuery
  $('selector');

  // Нативно
  document.querySelectorAll('selector');
  ```

* [1.1](readme-ru.md#1.1)  Запрос по классу

  ```javascript
  // jQuery
  $('.class');

  // Нативно
  document.querySelectorAll('.class');

  // или
  document.getElementsByClassName('class');
  ```

* [1.2](readme-ru.md#1.2)  Запрос по ID

  ```javascript
  // jQuery
  $('#id');

  // Нативно
  document.querySelector('#id');

  // или
  document.getElementById('id');
  ```

* [1.3](readme-ru.md#1.3)  Запрос по атрибуту

  ```javascript
  // jQuery
  $('a[target=_blank]');

  // Нативно
  document.querySelectorAll('a[target=_blank]');
  ```

* [1.4](readme-ru.md#1.4)  Найти среди потомков
  * Найти nodes

    ```javascript
    // jQuery
    $el.find('li');

    // Нативно
    el.querySelectorAll('li');
    ```

  * Найти body

    ```javascript
    // jQuery
    $('body');

    // Нативно
    document.body;
    ```

  * Найти атрибуты

    ```javascript
    // jQuery
    $el.attr('foo');

    // Нативно
    e.getAttribute('foo');
    ```

  * Найти data attribute

    ```javascript
    // jQuery
    $el.data('foo');

    // Нативно
    // используя getAttribute
    el.getAttribute('data-foo');
    // также можно использовать `dataset`, если не требуется поддержка ниже IE 11.
    el.dataset['foo'];
    ```
* [1.5](readme-ru.md#1.5)  Родственные/Предыдущие/Следующие Элементы
  * Родственные элементы

    ```javascript
    // jQuery
    $el.siblings();

    // Нативно
    [].filter.call(el.parentNode.children, function(child) {
      return child !== el;
    });
    ```

  * Предыдущие элементы

    ```javascript
    // jQuery
    $el.prev();

    // Нативно
    el.previousElementSibling;
    ```

  * Следующие элементы

    ```javascript
    // jQuery
    $el.next();

    // Нативно
    el.nextElementSibling;
    ```
* [1.6](readme-ru.md#1.6)  Closest

  Возвращает первый совпавший элемент по предоставленному селектору, обоходя от текущего элементы до документа.

  ```javascript
  // jQuery
  $el.closest(queryString);

  // Нативно - Only latest, NO IE
  el.closest(selector);

  // Нативно - IE10+ 
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

* [1.7](readme-ru.md#1.7)  Родители до

  Получить родителей кажого элемента в текущем сете совпавших элементов, но не включая элемент совпавший с селектором, узел DOM'а, или объект jQuery.

  ```javascript
  // jQuery
  $el.parentsUntil(selector, filter);

  // Нативно
  function parentsUntil(el, selector, filter) {
    const result = [];
    const matchesSelector = el.matches || el.webkitMatchesSelector || el.mozMatchesSelector || el.msMatchesSelector;

    // Совпадать начиная от родителя
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

* [1.8](readme-ru.md#1.8)  От
  * Input/Textarea

    ```javascript
    // jQuery
    $('#my-input').val();

    // Нативно
    document.querySelector('#my-input').value;
    ```

  * получить индекс e.currentTarget между `.radio`

    ```javascript
    // jQuery
    $(e.currentTarget).index('.radio');

    // Нативно
    [].indexOf.call(document.querySelectAll('.radio'), e.currentTarget);
    ```
* [1.9](readme-ru.md#1.9)  Контент Iframe

  `$('iframe').contents()` возвращает `contentDocument` для именно этого iframe

  * Контент Iframe

    ```javascript
    // jQuery
    $iframe.contents();

    // Нативно
    iframe.contentDocument;
    ```

  * Iframe Query

    ```javascript
    // jQuery
    $iframe.contents().find('.css');

    // Нативно
    iframe.contentDocument.querySelectorAll('.css');
    ```

[**⬆ Наверх**](readme-ru.md#Содержание)

### CSS & Style

* [2.1](readme-ru.md#2.1)  CSS
  * Получить стиль

    ```javascript
    // jQuery
    $el.css("color");

    // Нативно
    // ЗАМЕТКА: Известная ошика, возвращает 'auto' если значение стиля 'auto'
    const win = el.ownerDocument.defaultView;
    // null означает не возвращать псевдостили
    win.getComputedStyle(el, null).color;
    ```

  * Присвоение style

    ```javascript
    // jQuery
    $el.css({ color: "#ff0011" });

    // Нативно
    el.style.color = '#ff0011';
    ```

  * Получение/Присвоение стилей

    Заметьте что если вы хотите присвоить несколько стилей за раз, вы можете сослаться на [setStyles](https://github.com/oneuijs/oui-dom-utils/blob/master/src/index.js#L194) метод в oui-dom-utils package.
* Добавить класс

  ```javascript
  // jQuery
  $el.addClass(className);

  // Нативно
  el.classList.add(className);
  ```

* Удалить class

  ```javascript
  // jQuery
  $el.removeClass(className);

  // Нативно
  el.classList.remove(className);
  ```

* Имеет класс

  ```javascript
  // jQuery
  $el.hasClass(className);

  // Нативно
  el.classList.contains(className);
  ```

* Переключать класс

  ```javascript
  // jQuery
  $el.toggleClass(className);

  // Нативно
  el.classList.toggle(className);
  ```

* [2.2](readme-ru.md#2.2)  Ширина и Высота

  Ширина и высота теоритечески идентичны, например возьмем высоту:

  * высота окна

    ```javascript
    // Высота окна
    $(window).height();
    // без скроллбара, ведет себя как jQuery
    window.document.documentElement.clientHeight;
    // вместе с скроллбаром
    window.innerHeight;
    ```

  * высота документа

    ```javascript
    // jQuery
    $(document).height();

    // Нативно
    document.documentElement.scrollHeight;
    ```

  * Высота элемента

    ```javascript
    // jQuery
    $el.height();

    // Нативно
    function getHeight(el) {
      const styles = this.getComputedStyles(el);
      const height = el.offsetHeight;
      const borderTopWidth = parseFloat(styles.borderTopWidth);
      const borderBottomWidth = parseFloat(styles.borderBottomWidth);
      const paddingTop = parseFloat(styles.paddingTop);
      const paddingBottom = parseFloat(styles.paddingBottom);
      return height - borderBottomWidth - borderTopWidth - paddingTop - paddingBottom;
    }
    // С точностью до целого числа（когда `border-box`, это `height`; когда `content-box`, это `height + padding + border`）
    el.clientHeight;
    // с точностью до десятых（когда `border-box`, это `height`; когда `content-box`, это `height + padding + border`）
    el.getBoundingClientRect().height;
    ```

* [2.3](readme-ru.md#2.3)  Позиция и смещение
  * Позиция

    ```javascript
    // jQuery
    $el.position();

    // Нативно
    { left: el.offsetLeft, top: el.offsetTop }
    ```

  * Смещение

    ```javascript
    // jQuery
    $el.offset();

    // Нативно
    function getOffset (el) {
      const box = el.getBoundingClientRect();

      return {
        top: box.top + window.pageYOffset - document.documentElement.clientTop,
        left: box.left + window.pageXOffset - document.documentElement.clientLeft
      }
    }
    ```
* [2.4](readme-ru.md#2.4)  Прокрутка вверх

  ```javascript
  // jQuery
  $(window).scrollTop();

  // Нативно
  (document.documentElement && document.documentElement.scrollTop) || document.body.scrollTop;
  ```

[**⬆ Наверх**](readme-ru.md#Содержание)

### Манипуляции DOM

* [3.1](readme-ru.md#3.1)  Remove

  ```javascript
  // jQuery
  $el.remove();

  // Нативно
  el.parentNode.removeChild(el);
  ```

* [3.2](readme-ru.md#3.2)  Текст
  * Получить текст

    ```javascript
    // jQuery
    $el.text();

    // Нативно
    el.textContent;
    ```

  * Присвоить текст

    ```javascript
    // jQuery
    $el.text(string);

    // Нативно
    el.textContent = string;
    ```
* [3.3](readme-ru.md#3.3)  HTML
  * Получить HTML

    ```javascript
    // jQuery
    $el.html();

    // Нативно
    el.innerHTML;
    ```

  * Присвоить HTML

    ```javascript
    // jQuery
    $el.html(htmlString);

    // Нативно
    el.innerHTML = htmlString;
    ```
* [3.4](readme-ru.md#3.4)  Append

  Добавление элемента ребенка после последнего ребенка элемента родителя

  ```javascript
  // jQuery
  $el.append("<div id='container'>hello</div>");

  // Нативно
  el.insertAdjacentHTML("beforeend","<div id='container'>hello</div>");
  ```

* [3.5](readme-ru.md#3.5)  Prepend

  ```javascript
  // jQuery
  $el.prepend("<div id='container'>hello</div>");

  // Нативно
  el.insertAdjacentHTML("afterbegin","<div id='container'>hello</div>");
  ```

* [3.6](readme-ru.md#3.6)  insertBefore

  Вставка нового элемента перед выбранным элементом

  ```javascript
  // jQuery
  $newEl.insertBefore(queryString);

  // Нативно
  const target = document.querySelector(queryString);
  target.parentNode.insertBefore(newEl, target);
  ```

* [3.7](readme-ru.md#3.7)  insertAfter

  Вставка новго элемента после выбранного элемента

  ```javascript
  // jQuery
  $newEl.insertAfter(queryString);

  // Нативно
  const target = document.querySelector(queryString);
  target.parentNode.insertBefore(newEl, target.nextSibling);
  ```

* [3.8](readme-ru.md#3.8)  is

  Возвращает `true` если совпадает с селектором запроса

  ```javascript
  // jQuery - заметьте что `is` так же работает с `function` или `elements` которые не имют к этому отношения
  $el.is(selector);

  // Нативно
  el.matches(selector);
  ```

[**⬆ Наверх**](readme-ru.md#Содержание)

### Ajax

Заменить с [fetch](https://github.com/camsong/fetch-ie8) и [fetch-jsonp](https://github.com/camsong/fetch-jsonp)

[**⬆ Наверх**](readme-ru.md#Содержание)

### События

Для полной замены с пространством имен и делегация, сослаться на [oui-dom-events](https://github.com/oneuijs/oui-dom-events)

* [5.1](readme-ru.md#5.1)  Связать событие используя on

  ```javascript
  // jQuery
  $el.on(eventName, eventHandler);

  // Нативно
  el.addEventListener(eventName, eventHandler);
  ```

* [5.2](readme-ru.md#5.2)  Отвязать событие используя off

  ```javascript
  // jQuery
  $el.off(eventName, eventHandler);

  // Нативно
  el.removeEventListener(eventName, eventHandler);
  ```

* [5.3](readme-ru.md#5.3)  Trigger

  ```javascript
  // jQuery
  $(el).trigger('custom-event', {key1: 'data'});

  // Нативно
  if (window.CustomEvent) {
    const event = new CustomEvent('custom-event', {detail: {key1: 'data'}});
  } else {
    const event = document.createEvent('CustomEvent');
    event.initCustomEvent('custom-event', true, true, {key1: 'data'});
  }

  el.dispatchEvent(event);
  ```

[**⬆ Наверх**](readme-ru.md#Содержание)

### Утилиты

* [6.1](readme-ru.md#6.1)  isArray

  ```javascript
  // jQuery
  $.isArray(range);

  // Нативно
  Array.isArray(range);
  ```

* [6.2](readme-ru.md#6.2)  Trim

  ```javascript
  // jQuery
  $.trim(string);

  // Нативно
  string.trim();
  ```

* [6.3](readme-ru.md#6.3)  Назначение объекта

  Дополнительно, используйте полифил object.assign [https://github.com/ljharb/object.assign](https://github.com/ljharb/object.assign)

  ```javascript
  // jQuery
  $.extend({}, defaultOpts, opts);

  // Нативно
  Object.assign({}, defaultOpts, opts);
  ```

* [6.4](readme-ru.md#6.4)  Contains

  ```javascript
  // jQuery
  $.contains(el, child);

  // Нативно
  el !== child && el.contains(child);
  ```

[**⬆ Наверх**](readme-ru.md#Содержание)

### Альтернативы

* [You Might Not Need jQuery](http://youmightnotneedjquery.com/) - Примеры как исполняются частые события, элементы, ajax и тд с ванильным javascript.
* [npm-dom](http://github.com/npm-dom) и [webmodules](http://github.com/webmodules) - Отдельные DOM модули можно найти на NPM

### Переводы

* [한국어](readme.ko-kr.md)
* [简体中文](readme.zh-cn.md)
* [Bahasa Melayu](readme-my.md)
* [Bahasa Indonesia](readme-id.md)
* [Português\(PT-BR\)](readme.pt-br.md)
* [Tiếng Việt Nam](readme-vi.md)
* [Español](readme-es.md)
* [Русский](readme-ru.md)
* [Türkçe](readme-tr.md)

### Поддержка браузеров

| ![Chrome](https://raw.github.com/alrra/browser-logos/master/chrome/chrome_48x48.png) | ![Firefox](https://raw.github.com/alrra/browser-logos/master/firefox/firefox_48x48.png) | ![IE](https://raw.github.com/alrra/browser-logos/master/internet-explorer/internet-explorer_48x48.png) | ![Opera](https://raw.github.com/alrra/browser-logos/master/opera/opera_48x48.png) | ![Safari](https://raw.github.com/alrra/browser-logos/master/safari/safari_48x48.png) |
| :--- | :--- | :--- | :--- | :--- |
| Latest ✔ | Latest ✔ | 10+ ✔ | Latest ✔ | 6.1+ ✔ |

## License

MIT

