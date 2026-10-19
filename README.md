# Practice-2
# Практична робота № 2

**Дисципліна:** Основи побудови інформаційних систем та мереж (ОК-13)

**Тема:** Структура повідомлень прикладного протоколу HTTP. Формування запиту вручну

| Поле | Значення |
|---|---|
| Студент (прізвище, ім'я, по батькові) | Столярчук Станіслав Михайлович |
| Група | F5 2.01 |
| Номер варіанта | 20 |
| Індивідуальний домен | w3.org |
| «Чужий» домен для завдання A.3.1 | postgresql.org |
| Середовище виконання | Windows 11 |
| Дата виконання | 12.10.2026 |

> Бланк заповнюють, не змінюючи структури розділів. Порожні заготовки блоків коду замінюють власними виводами. Позначки-підказки в кутових дужках вилучають.

---

## Частина A. Збір експериментальних даних

### Завдання A.1. Формування запиту вручну

**Команда:**

```
nc -C w3.org 80
```

**Набраний запит:**

```
GET / HTTP/1.1
Host: archlinux.org
Connection: close

```

**Відповідь:**

```
HTTP/1.1 301 Moved Permanently
Date: Mon, 19 Oct 2026 19:11:19 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: close
Location: https://www.w3.org/
set-cookie: __cf_bm=rd6Gn85Fidz.Tq9H_QPuwjwIB5imha5HKB2qxtLBnZ8-1792437079.3275611-1.0.1.1-o5_KcrGP9kV16QmOSL79pjWDmm7WHDH62D0260TrbHDn9BpMUAqZUkG1THVfo9wFcZOsnKgw8G5gZzY.MGuXFcngrMRvhY49UNPsthYNesgEiehVE1Mb630SjM_QpbHy; HttpOnly; Path=/; Domain=w3.org; Expires=Mon, 19 Oct 2026 19:41:19 GMT
X-Content-Type-Options: nosniff
Server: cloudflare
CF-RAY: a4d21681ca4b2486-KBP
alt-svc: h3=":443"; ma=86400

a7
<html>
<head><title>301 Moved Permanently</title></head>
<body>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>cloudflare</center>
</body>
</html>
```

---

### Завдання A.2. Запит без поля `Host` у версії 1.1

**Команда:**

```
printf 'GET / HTTP/1.1\r\nConnection: close\r\n\r\n' | nc w3.org 80
```

**Вивід:**

```
HTTP/1.1 400 Bad Request
Server: cloudflare
Date: Mon, 19 Oct 2026 19:13:33 GMT
Content-Type: text/html
Content-Length: 155
Connection: close
CF-RAY: -

<html>
<head><title>400 Bad Request</title></head>
<body>
<center><h1>400 Bad Request</h1></center>
<hr><center>cloudflare</center>
</body>
</html>
```

---

### Завдання A.3. Вплив поля `Host` на відповідь сервера

#### A.3.1. Чуже доменне ім'я в полі `Host`

**Команда:**

```
printf 'GET / HTTP/1.1\r\nHost: postgresql.org\r\nConnection: close\r\n\r\n' | nc w3.org 80
```

**Вивід:**

```
HTTP/1.1 409 Conflict
Date: Mon, 19 Oct 2026 19:15:33 GMT
Content-Type: text/plain; charset=UTF-8
Content-Length: 17
Connection: close
Cache-Control: private, max-age=0, no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Expires: Thu, 01 Jan 1970 00:00:01 GMT
Referrer-Policy: same-origin
X-Frame-Options: SAMEORIGIN
Server: cloudflare
CF-RAY: a4d21cb6ca15ca30-KBP
```

#### A.3.2. Неіснуюче ім'я в полі `Host`

**Команда:**

```
printf 'GET / HTTP/1.1\r\nHost: opism-pr02.invalid\r\nConnection: close\r\n\r\n' | nc w3.org 80
```

**Вивід:**
```
HTTP/1.1 409 Conflict
Date: Mon, 19 Oct 2026 19:16:58 GMT
Content-Type: text/plain; charset=UTF-8
Content-Length: 17
Connection: close
Cache-Control: private, max-age=0, no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Expires: Thu, 01 Jan 1970 00:00:01 GMT
Referrer-Policy: same-origin
X-Frame-Options: SAMEORIGIN
Server: cloudflare
CF-RAY: a4d21ec92e68ca30-KBP
```

#### A.3.3. Запит без поля `Host` у версії 1.0

**Команда:**

```
printf 'GET / HTTP/1.0\r\n\r\n' | nc w3.org 80
```

**Вивід:**

```
HTTP/1.1 403 Forbidden
Date: Mon, 19 Oct 2026 19:17:38 GMT
Content-Length: 57
Connection: close
Cache-Control: private, max-age=0, no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Referrer-Policy: same-origin
Expires: Thu, 01 Jan 1970 00:00:01 GMT
CF-RAY: a4d21fc64d1cca4e-KBP
```

Зведення результатів наведено в **Додатку Д**.

---

### Завдання A.4. Два запити в одному з'єднанні

**Команда:**

```
printf 'GET /opism-pr02-12345 HTTP/1.1\r\nHost: w3.org\r\n\r\nGET / HTTP/1.1\r\nHost: w3.org\r\nConnection: close\r\n\r\n' | nc -C archlinux.org 80
```

**Вивід:**

```
HTTP/1.1 301 Moved Permanently
Date: Mon, 19 Oct 2026 19:19:59 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Location: https://www.w3.org/opism-pr02-12345
set-cookie: __cf_bm=cOC86Rd8maYuW5tcUsMw4Z0ckV4S3TX_UK5CyC1wUEg-1792437599.1981454-1.0.1.1-gwJd9wefqDRL8NFvmOfhqWyVolrsbZR.fCJWSjVqNhxQlG5W8.UAeKzDGtbAz5MooRRiX6mPpO0WAJYOMfPmL2rTvWaMvBzLRh86IAQTQ3RsrGOeTNtgKDa0yzaJ8m45; HttpOnly; Path=/; Domain=w3.org; Expires=Mon, 19 Oct 2026 19:49:59 GMT
X-Content-Type-Options: nosniff
Server: cloudflare
CF-RAY: a4d22332f8b977bf-KBP
alt-svc: h3=":443"; ma=86400

a7
<html>
<head><title>301 Moved Permanently</title></head>
<body>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>cloudflare</center>
</body>
</html>

0
```

**Кількість отриманих відповідей: 2** 

**Коди стану отриманих відповідей: 301 Moved Permanently, 301 Moved Permanently**

---

### Завдання A.5. Запит за допомогою клієнтської програми

**Команда:**

```
curl -v --http1.1 http://w3.org/ -o /dev/null
```

**Вивід:**

```
* Established connection to w3.org (104.18.23.19 port 80) from 172.20.10.7 port 51947
  % Total    % Received % Xferd  Average Speed  Time    Time    Time   Current
                                 Dload  Upload  Total   Spent   Left   Speed
  0      0   0      0   0      0      0      0                              0* using HTTP/1.x
> GET / HTTP/1.1
> Host: w3.org
> User-Agent: curl/8.21.0
> Accept: */*
>
* Request completely sent off
< HTTP/1.1 301 Moved Permanently
< Date: Mon, 19 Oct 2026 19:22:04 GMT
< Content-Type: text/html; charset=UTF-8
< Transfer-Encoding: chunked
< Connection: keep-alive
< Location: https://www.w3.org/
< set-cookie: __cf_bm=aqGfMuDQ68DipVSh2w6fFgU_53R3Y0_GDcpcvYLjzy0-1792437724.8786356-1.0.1.1-m9ra6Voyq17XbdvM7PDOvsKKwupTsnb5Q21f8h6LV31j.8UBJHtzd.txgQ.SaR6culiRTgKzC.ZFQa8OA6xvHdYXUX6SrdPRCdRpskDVsrSTc_7jtrg0ROkJ96oQCuIK; HttpOnly; Path=/; Domain=w3.org; Expires=Mon, 19 Oct 2026 19:52:04 GMT
< X-Content-Type-Options: nosniff
< Server: cloudflare
< CF-RAY: a4d226447b5424c5-KBP
< alt-svc: h3=":443"; ma=86400
<
{ [178 bytes data]
100    167   0    167   0      0    556      0                              0
* Connection #0 to host w3.org:80 left intact
```

---

### Завдання A.6. Запит через захищене з'єднання

**Ресурс, на якому виконано завдання:** w3.org / `iana.org`>

**Підстава для використання резервного ресурсу не використовувався, оскільки у A.1 отримано код класу 3xx і з'єднання з портом 443 встановлено.:**

**Команда:**

```
openssl s_client -connect w3.org:443 -servername w3.org -crlf -quiet
```

**Набраний запит:**

```
GET / HTTP/1.1
Host: w3.org
Connection: close
```

**Вивід:**

```
depth=3 C = US, O = Internet Security Research Group, CN = ISRG Root X2
verify return:1
depth=2 C = US, O = ISRG, CN = Root YE
verify return:1
depth=1 C = US, O = Let's Encrypt, CN = YE1
verify return:1
depth=0 CN = w3.org
verify return:1

HTTP/1.1 301 Moved Permanently
Server: nginx
Date: Tue, 29 Sep 2026 09:13:18 GMT
Content-Type: text/html
Content-Length: 162
Connection: close
Location: https://w3.org/

<html>
<head><title>301 Moved Permanently</title></head>
<body>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>nginx</center>
</body>
</html>
```

---

Частина B. Аналіз HTTP-заголовків


```


У таблицю потрібно внести значення з фактичної відповіді, отриманої в A.1. Видаліть рядки для заголовків, яких немає у відповіді.
Назва заголовка | 	 Значення з відповіді    |	Призначення
Date	          |Заповнити за результатом A.1|	Дата та час формування відповіді
Server          |Заповнити за результатом A.1|	Програмне забезпечення сервера, якщо вказане
Location        |	Заповнити, якщо присутній  |	Адреса перенаправлення
Connection	    |Заповнити за результатом A.1|	Параметри з'єднання
Content-Type	  |Заповнити за результатом A.1|	Тип вмісту відповіді
Content-Length	|Заповнити, якщо присутній   |	Довжина тіла відповіді в байтах


```

Частина D. Висновки
Під час практичної роботи досліджено принципи взаємодії клієнта із сервером за протоколом HTTP. Для виконання завдань використовується домен w3.org відповідно до варіанта 20.


```


У роботі розглядається структура HTTP-запиту, призначення заголовка Host та значення статус-кодів HTTP. Окремо перевіряється поведінка сервера у відповідь на запити з різними значеннями Host, запити без цього заголовка та запити версії HTTP/1.0.

За результатами виконання команд необхідно зафіксувати фактичні статус-коди та заголовки відповідей. Код 301 Moved Permanently означає постійне перенаправлення, а заголовок Location містить адресу, на яку сервер пропонує перейти. Інші статус-коди потрібно пояснювати відповідно до отриманих результатів.

Також використовується curl.exe для перегляду HTTP-запитів і відповідей. Цей інструмент дає змогу досліджувати заголовки, статус-коди та діагностувати взаємодію з вебсервером. Остаточні висновки слід узгодити з реальними результатами, отриманими під час виконання команд.

```

Контрольні запитання 

```
1. Для чого використовується заголовок Host?
Заголовок Host указує доменне ім'я сервера, до якого звертається клієнт. Він дозволяє одному серверу обслуговувати декілька доменів.
2. Чим можуть відрізнятися відповіді для різних значень Host?
Відповідь залежить від конфігурації сервера та віртуальних хостів. Для різних значень Host сервер може повертати різні сторінки, перенаправлення або помилки. У звіті слід навести фактичні статус-коди з A.3.1 та A.3.2.
3. Що означає статус-код 301 Moved Permanently?
Код 301 Moved Permanently означає, що ресурс постійно перенесено на іншу адресу. Нова адреса зазвичай указана в заголовку Location.
4. Для чого використовується curl?
curl — це утиліта для надсилання запитів до серверів і отримання відповідей. Вона дозволяє переглядати заголовки, статус-коди та інші дані відповіді.
5. Для чого потрібен заголовок Connection?
Заголовок Connection визначає параметри керування з'єднанням. Значення close вказує, що з'єднання слід закрити після завершення обміну даними.
6. Що містить HTTP-відповідь сервера?
HTTP-відповідь зазвичай містить рядок статусу, заголовки та, за наявності, тіло відповіді. Заголовки можуть містити тип і довжину вмісту, дату, інформацію про сервер та адресу перенаправлення.
Використання ШІ
Поле              | 	Значення
Система           | 	ChatGPT
Модель	          |   GPT-6
Мета використання |	  Допомога в оформленні звіту та поясненні HTTP-запитів
Примітка          |   Результати команд потрібно внести з фактичного виводу під час виконання практичної роботи
