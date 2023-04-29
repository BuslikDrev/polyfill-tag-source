Скрипт предназначен для решения проблемы работы атрибута media в тегах ```<video><source media=""></video>``` в браузере Chrome
После его установки атрибут media будет учитываться

Поместите скрипт в конце документа, где есть тег video с тегами ```<source media>```

```
<script>
// BuslikDrev полифил media
'use strict';
'use asm';
	// https://developer.mozilla.org/ru/docs/Web/API/HTMLMediaElement
	if (!('BusEngine' in window)) {
		window.BusEngine = {};
	}
	window.BusEngine.polyfillTagSource = function(ex) {
		if (typeof ex == 'undefined') {
			ex = [];
		}
		var i, l, v = document.querySelectorAll('video source[media]:not([data-error])');
		l = v.length;

		for (i = 0; i < l; ++i) {
			if (window.matchMedia(v[i].media).matches) {
				if (v[i].getAttribute('data-src') && ex.indexOf(v[i].getAttribute('data-src')) == -1) {
					v[i].setAttribute('src', v[i].getAttribute('data-src'));
					v[i].removeAttribute('data-src');
					v[i].parentNode.addEventListener('error', function(e) {
						e.target.setAttribute('data-error', e.target.src);
						ex.push(e.target.src);
						BusEngine.polyfillTagSource(ex);
					});
					v[i].parentNode.src = v[i].getAttribute('src');
					break;
				}
			} else {
				if (v[i].getAttribute('src')) {
					v[i].setAttribute('data-src', v[i].getAttribute('src'));
					v[i].removeAttribute('src');
				}
			}
		}
	};

	if ('matchMedia' in window && window.navigator.userAgent.toLowerCase().match(/(chrome|busengine)\/(\d+\.)/)) {
		//window.addEventListener('load', function() {
			BusEngine.polyfillTagSource();
		//});
	}
</script>
```

Или в документе в тегах source переименуйте атрибут "src" на "data-src", и тогда скрипт можно поместить в отделный файл и загружать в удобный для вас момент.
