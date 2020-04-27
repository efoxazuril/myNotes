## HTTP

Это протокол передачи данных.

В общении участвуют сервер и клиент (обычно браузер)

Полезные ссылки:

1. [Простым языком об HTTP](https://habr.com/ru/post/215117/)

2. [Формат HTTP-запросов](http://citforum.ru/internet/cgi_tut/rqst.shtml)

3. [Формат HTTP-ответов](http://citforum.ru/internet/cgi_tut/spns.shtml)

4. [Стандарт HTTP 1.1](https://www.ietf.org/rfc/rfc2616.txt)

5. [Википедия / Постоянное HTTP-соединение](https://ru.wikipedia.org/wiki/Постоянное_HTTP-соединение)

6. [Википедия / Тело сообщения](https://ru.wikipedia.org/wiki/HTTP#.D0.A2.D0.B5.D0.BB.D0.BE_.D1.81.D0.BE.D0.BE.D0.B1.D1.89.D0.B5.D0.BD.D0.B8.D1.8F)

7. [Что такое HTML-форма? / Википедия](http://bit.ly/1J8hnrJ)

8. [Sending form data](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Forms/Sending_and_retrieving_form_data)

9. [Ссылка на изображение, передаваемое с помощью chunks (пример из видео)](http://www.httpwatch.com/httpgallery/chunked/chunkedimage.aspx)

10. [Chunked transfer encoding / Википедия](https://ru.wikipedia.org/wiki/Chunked_transfer_encoding)

11. [Query String / Wikipedia](https://en.wikipedia.org/wiki/Query_string)

12. [RFC 3986 (соответствующий стандарт)](https://tools.ietf.org/html/rfc3986)

13. [HTTP 301 / Wikipedia](https://en.wikipedia.org/wiki/HTTP_301)

14. [Basic access authentication / Wikipedia](https://en.wikipedia.org/wiki/Basic_access_authentication)

    


Некоторые вопросы: 

1. Что (обычно) означает сообщение "Connection closed by foreign host", отправленное веб-сервером?
   
- превышен интервал ожидания, соединение закрыто
  
2. HTTP-запрос невозможен без
	
	- стартовой строки
	
3. Какой метод запроса (request method) возвращает только заголовок, и этот заголовок идентичен тому заголовку, который возвращается вместе с телом при использовании GET-запроса?
	
	   - HEAD
	
4. Как при HTTP-запросе обозначается конец запроса?
	
	   - два перевода строки
	
5. Это является особенностью HTTP версии 1.1
	
	   - Возможность повторно использовать TCP-соединение вместо создания нового соединения при каждом запросе
	
6. Какое значение имеет поле "content length" в ответе (response) при использовании метода HEAD?

	- такое же, какое было бы у тела сообщения, возвращаемого при использовании GET
	
7. Content-length может присутствовать

	- и в запросе, и в ответе
	
8. В какой части HTTP-запроса находятся данные формы при ее отправке?
	
	- В теле запроса
	
9. При отправке формы с помощью, например, GET-запроса, разные части (поля) формы можно разделять этим символом.

	- `&`

10. Может ли ответ на HTTP-запрос (то есть HTTP response) содержать тело с какой-то информацией?

	- Да

11. С чего начинается каждый chunk?
	
	- число, указывающее на его размер

12. Чем заканчивается каждый chunk?

	- переводом строки
	
13. Каким образом обозначается последний chunk?

	- он нулевой длины, после него — двойной перевод строки

14. В случае с передачей ответа с помощью chunks, каким образом тело ответа отделяется от заголовков?

	-  два перевода строки

15. Какой из типов запросов должен использоваться только для получения данных и не должен приводить к изменениям данных?
	
	- GET
	
16. Какой из типов запросов лучше всего поддается кэшированию (если запросы используются правильно)?

	- GET

17. Возможно ли в одном запросе передавать данные и в body, и в request line с помощью query string?

	- Да

18. Запрос публикует новый комментарий в блоге. Является ли такой запрос идемпотентным?

	- Нет
	
19. Возможно в ответ 301 включить тело?

	- Да

20. При ответе "301" новый адрес указан в поле "Location". Чем является это поле?

	- Заголовком

21. Возможно ли делать редирект на страницу, которая в свою очередь содержит новый редирект на другую страницу?

	- Да

### Выжные понятия

`Идемпотентность` -
	
	

```bash
#пример запроса
bash-4.4$ telnet localhost 8080
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
GET /about HTTP/1.0
HOST: localhost
	
#ответ от сервера
HTTP/1.1 200 OK
X-Powered-By: Express
Connection: close
Content-Type: text/html; charset=utf-8
Content-Length: 12
ETag: W/"c-r0WEeVxJ7IpMIG20rN7HX9ndB4c"
Date: Thu, 24 Oct 2019 19:42:37 GMT
	
You've done!Connection closed by foreign host.
```


При отправке POST запроса, необходимы дополнительные заголовки : Сontent-type и Content-length

```bash
#тело запроса отделяется одним переводом строки
telnet localhost 8080
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
POST /upload HTTP/1.1
HOST: hexlet.local
Content-type: text/plain
Content-length: 15

my request body
```

Пример отправки формы

```bash
$ telnet localhost 8080
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
POST /session/new HTTP/1.1 
Host: hexlet.local
Content-type: application/x-www-form-urlencoded
Content-length: 30

username=admin&password=secret
```

Формат сообщений 

Для отделения записей размеров блоков (частей) от их содержания используется разделитель `<CRLF>` (как строка: «\r\n»; как байты в формате HEX: 0x0D, 0x0A). Длина блока — это размер содержания блока, разделители `<CRLF>` не учитываются.

Схематическое представление: `<длина блока в HEX><содержание блока>`

Последний блок строится по той же схеме, потому имеет следующий вид по причине отсутствия содержания: 0`<CRLF>``<CRLF>`

Посылаем chuncked запрос

```bash
$ telnet localhost 8080
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
GET /stream HTTP/1.1
Host: hexlet.local
Transfet-Encoding: chunked
```

Передача Query String

```bash
$ telnet localhost 8080
Trying 127.0.0.1...
Connected to localhost.
GET /?key=value&another_key=another_value HTTP/1.1
HOST: hexlet.local
```

