Скрипт предназначен для решения проблемы работы атрибута media в тегах ```<video><source media=""></video>``` в браузере Chrome
После его установки атрибут media будет учитываться

Поместите скрипт в конце документа, где есть тег video с тегами ```<source media>```

```
<script>
// BuslikDrev полифил media
if ('matchMedia' in window && window.navigator.userAgent.toLowerCase().match(/(chrome)\/(\d+\.)/)) {
'use strict';
'use asm';
	(function() {
		var i, v = document.querySelectorAll('video source[media]');

		for (i = 0; i < v.length; ++i) {
			if (window.matchMedia(v[i].media).matches) {
				if (v[i].getAttribute('data-src')) {
					v[i].setAttribute('src', v[i].getAttribute('data-src'));
					v[i].removeAttribute('data-src');
				}
			} else {
				if (v[i].getAttribute('src')) {
					v[i].setAttribute('data-src', v[i].getAttribute('src'));
					v[i].removeAttribute('src');
				}
			}
		}
	})();
}
</script>
```

Или в документе в тегах source переименуйте атрибут "src" на "data-src", и тогда скрипт можно поместить в отделный файл и загружать в удобный для вас момент.
