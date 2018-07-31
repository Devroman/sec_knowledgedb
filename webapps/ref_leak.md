# Referer leak

**Referer** - заголовок **http**-запроса, содержащий **URL** источника запроса.  

Почти все используемые на данный момент веб-сервера ведут лог обращений. Например в **nginx** есть **access.log**, который выглядит примерно так:  

    192.168.222.159 - - [29/Jul/2018:22:43:55 +0300] "GET /d/mirP00imz/favicon.ico HTTP/1.1" 404 208 "-" "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 YaBrowser/18.6.1.770 Yowser/2.5 Safari/537.36"
    192.168.222.159 - - [29/Jul/2018:22:43:56 +0300] "GET /d/mirP00imz/favicon.ico HTTP/1.1" 404 208 "-" "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 YaBrowser/18.6.1.770 Yowser/2.5 Safari/537.36"
    192.168.222.159 - - [29/Jul/2018:22:43:56 +0300] "GET /d/mirP00imz/favicon.ico HTTP/1.1" 404 208 "-" "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 YaBrowser/18.6.1.770 Yowser/2.5 Safari/537.36"

Логирование можно настроить и включить запись **referer**-а.

Уязвимость **referer leak** возникает тогда, когда в **URL** страницы оказываются критичные данные, например какие-нибудь id, токены итд, а на самой странице есть внешний ресурс.  

## Пример

Страница восстановления пароля, содержащая токен аккаунта в **query string**:  
`http://example.com/user/restorePassword?secret=12390982349083423`  

На этой же странице есть картинка с сайта `leaks.org`:  
`<img src="http://leaks.org/images/myMemes.png">`

При каждом заходе на такую страницу на сайт `leaks.org` будет уходить **http** запрос, содержащий в заголовке **referer** секрет пользователя.  

[← Назад](../README.md)