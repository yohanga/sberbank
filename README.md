[![Latest Version on Packagist][ico-version]][link-packagist]
[![Total Downloads][ico-downloads]][link-downloads]

# Laravel sberbank bank acquiring library.
Библиотека для приема платежей через интернет для Сбербанк.

### Возможности

 * Генерация URL для оплаты товаров
 * Просмотр статуса платжа

### Установка

С помощью [composer](https://getcomposer.org/):

```bash
composer require Xnf4o/Sberbank
```

Подключение в контроллере:

```php
use Xnf4o\Sberbank;
```

## Примеры использования
### 1. Инициализация, если у нас есть токен

```php
$access_token  = 'sberbank_secret_token';

$sberbank = new Sberbank(true, ['access_token' => $access_token]);
```

### 1.1 Инициализация, если у нас логин и пароль

```php
$login = 'sberbank_login';
$password  = 'sberbank_password';

$sberbank = new Sberbank(false, ['login' => $login, 'password' => $password]);
```

### 2. Получить URL для оплаты
```php
//Подготовка массива с данными об оплате
$payment = [
    'orderNumber'   => '1234567',                           //Номер заказа
    'amount'        => 100,                                 //Сумма заказа в рублях
    'language'      => 'ru',                                //Локализация
    'description'   => 'New payment',                       //Описание заказа
    'returnUrl'     => 'http://my.site/successful-payment', //URL сайта в случае успешной оплаты
    'failUrl'       => 'http://my.site/fail-payment',       //URL сайта в случае НЕуспешной оплаты
];

//Получение url для оплаты
$result = $sberbank->paymentURL($payment);

//Контроль ошибок
if(!$result['success']){
  echo($result['error']);
} else{
  $payment_id = $result['payment_id'];
  return redirect($result['payment_url']);
}
```

### 3. Получить статус платежа
```php
//$payment_id Идентификатор платежа банка (полученый в пункте "2 Получить URL для оплаты")

$result = $sberbank->getState($payment_id)

//Контроль ошибок
if(!$result['success']){
  echo($result['error']);
} else{
  echo($result['payment_status']);
}
```
