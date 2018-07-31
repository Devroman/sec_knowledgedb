# Cross-Origin Resource Sharing

Это технология для обхода [**Same Origin Policy**](./sop.md).  

Сервер, желающий открыть доступ к ресурсам на своем **origin** другому **origin** может добавить некоторые заголовки в ответ, после чего ограничения **SOP** снимаются частично или полностью.

> Любой кросс-доменный запрос содерит заголовок **Origin**

## Заголовки

### Запроса

> По-умолчанию кросс-доменные запросы не содержат **Cookie**

* `Origin` - сигнализирует серверу о поддержке **CORS**
* `Access-Control-Request-Method` - запрос разрешенных методов
* `Access-Control-Request-Headers` - запрос разрешенных заголовков

### Заголовки ответа некоторые

* `Access-Control-Allow-Origin` - **origin** для которых снимается ограничение **SOP**
* * `Access-Control-Allow-Credentials: true` - включает передачу **Cookie**
* `Access-Control-Allow-Methods` - разрешенные методы
* `Access-Control-Allow-Headers` - разрешенные заголовки

[← Назад](../README.md)