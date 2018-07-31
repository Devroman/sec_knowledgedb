# Content Security Policy

**CSP** - браузерный механизм защиты от внедрения контента, представляющий из себя белый список **origin**-ов, с которых доступна загрузка.

## Механизм

**CSP** можно включить двумя способами:  

* **http** заголовком `Content-Security-Policy`

Пример:  
`Content-Security-Policy: default-src 'self'; script-src http://cdn.example.com;`

* тегом **meta** в **html** разметке страницы

Пример:  
`<meta http-equiv="Content-Security-Policy" content="default-src https://cdn.example.net; child-src 'none'; object-src 'none'">`

Далее браузер будет исходя из описанных правил подгружать контент, либо блокировать загрузку.  

### CSP директивы

Каждая из директив устанавливается в одно из следующих значений:  

* `'self'` - загрузка только с текущего **origin**
* `'none'` - запрещает загрузку указанного типа контента
* перечень разрешенных **origin** в формате **wildcard**
* `*` - по сути частный случай предыдущего, разрешающий загрузку откуда угодно
* у некоторых директив есть отдельно свои значения
* отдельно указанная схема/протокол, например: `'https:'`, `'data:'`

Список директив(неполный):  

* `default-src` - правила по-умолчанию для необъявленных типов контента,
* `script-src` - правила для скриптов,
* `object-src` - правила для плагинов, например flash,
* `style-src` - правила для файлов стилей
* `img-src` - правила для загрузки картинок,
* `media-src` - правила для видео, аудио,
* `frame-src` - правила для iframe,
* `font-src` - правила для файлов шрифтов,
* `connect-src` - правила для установки соединений через **XHR**, **websocket**,
* `form-action` - правила для отправки форм,
* `frame-ancestors` - правила, регулирующие установку текущего ресурса в **iframe**, то есть можно/нельзя/куда можно,
* `worker-src` - правила для **serivce-worker**.

## Особенности

Если директива не указана в правиле, то соответствующий контент можно загружать откуда угодно, то есть аналогично значению `*`. Чтобы изменить это поведение, нужно задать значение `default-src`.  

### script-src и style-src

Имеет два дополнительных значения:  

* `'unsafe-inline'` - разрешает использовать инлайн **JS** и **CSS**
* `'unsafe-eval'` - позволяет использовать `eval` в **JS** и разрешает передачу строк для выполнения в `setInterval` и `setTimeout`

### inline

По-уполчанию все инлайны запрещены, ибо они сводят иначе на нет CSP. Инлайны зло - не используйте инлайны, если это возможно.  

## CSP Report

Спецификация **CSP** предписывает браузерам сообщать о попытках нарушения заданных правил **CSP** по **URI**, заданому директивой `report-uri`.  

Пример:  
`Content-Security-Policy: default-src 'self'; report-uri /my-csp-logger;`

Репорт выглядит так:  

    {
        "csp-report": {
            "blocked-uri:" "http://ajax.googleapis.com",
            "document-uri:" "http://example.com/",
            "original-policy": "default-src 'self'; report-uri     http://example.com/my-csp-logger",
            "referrer:" "",
            "violated-directive": "default-src 'self'",
            "effective-directive": "script-src",
            "status-code": 200
        }
    }

### Content-Security-Policy-Report-Only

Можно использовать заголовок `Content-Security-Policy-Report-Only` вместо `Content-Security-Policy`, тогда блокировки ресурсов не произойдет, но нарушения правил будут регистрироваться.  
Можно использовать эти заголовки вместе, тестируя конфигурации через `Content-Security-Policy-Report-Only`.  

[← Назад](../README.md)