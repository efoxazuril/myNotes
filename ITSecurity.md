IT Security на основе курса https://free.codebashing.com/courses/php/



## **SQL Инъекции**

**vulnerable application** - уязвимое (беззащитное) приложение

Это отправление частей запросов прямо в БД приложения.

Обычно отправляют синтакчически верную строку, чтобы SQL вернул результат успеха.

Варианты защиты: 

Подготовленные выражения (параметризованные запросы)

Те основная причина SQL атак - никак не валидированные параметры запроса, подставленные в чистый запрос.

## **XXE-атаки**

XXE атаки совершаются с помощью XML. Атака основанана XML eXternal Entities, которые позволяют загрузить сторонний ресурс через XML документ.

С помощью file:// атакующий может обмануть SAX парсер и заставить исполнить файл на стороне сервера.

Пример: 

прочитаем /etc/passwd из атакуемой системы будет выведен в

```xml
<foo>&bar;</foo>
```

```xml
<!DOCTYPE foo [<!ELEMENT foo ANY >
<!ENTITY bar SYSTEM "file:///etc/passwd" >]>
<trades>
    <metadata>
        <name>Apple Inc</name>
        <stock>APPL</stock>
        <trader>
            <foo>&bar;</foo>
            <name>C.K Frode</name>
        </trader>
        <units>1500</units>
        <price>106</price>
        <name>Microsoft Corp</name>
        <stock>MSFT</stock>
        <trader>
            <name>C.K Frode</name>
        </trader>
        <units>5000</units>
        <price>45</price>
        <name>Amazon Inc</name>
        <stock>AMZN</stock>
        <trader>
            <name>C.K Frode</name>
        </trader>
        <units>4500</units>
        <price>195</price>
    </metadata>
</trades>
```

Как защитится(PHP): 

Отключаем чтение **DOCTYPE** в xml файле

```php
libxml_disable_entity_loader(true)
```



## **XSS**-атаки

Это способ вставить вредоносный js в input формы. Обычная защита от этого htmlspecialchars()

## SESSION FIXATION

1. Ensure that only server-generated session values are accepted by the application. 

2. Upon a successful login, invalidate the original session token, and re-issue a new session token. 

3. Prevent the application from accepting session tokens via GET or POST requests and instead store session values within HTTP cookies only. 

## Use Of Insufficiently Random Values

Finally, although the proposed fix is sufficient to remediate our vulnerable example, the overall logic and security design can be significantly improved by not implementing your own session management system, and instead leveraging PHP's native session generation routines e.g. `session_start()` , `session_regenerate_id()`

Further improvements can be made by amending `php.ini`, such as ensuring `session.hash_function` uses SHA hashing instead of MD5, configuring a `session.entropy_file` , and specifying the number of entropy bytes with `session.entropy_length`

## Reflected Xss

The main **Reflected XSS remediation strategy** is to treat all the user input as a text, not as a code. This can be achieved by the following actions: 

\1. **Escape user input** using language-specific or framework-specific instruments, like templates or contextual escaping. Usually, these mechanisms are enabled by default, so make sure not to disable them. 

\2. Know all the locations where user input is used, and try to **avoid returning unsanitized user input** to potentially dangerous locations like HTML body and attributes, javascript, GET parameters, URLs, links, CSS. 

\3. **Use additional security controls** that help to prevent XSS in case escaping controls fail or are missing. 

- **HTTPOnly cookie flag**. This flag prevents Javascript from accessing the cookie content, thus protecting it from being stolen if **Reflected XSS** is present. 
- **Content Security Policy HTTP Header**. This header restricts sources of all the page's content, including javascript code, to the **whitelist of sources**, thus making XSS exploitation more hard to perform.
- **X-XSS-Protection HTTP Header**. This header prevents browsers from loading a page if they detect **Reflected XSS** exploitation.



Let's see how these techniques can be applied to our vulnerable example to remediate the Reflected XSS vulnerability.****

## Stored (Persistent) Xss

To defend against **Stored Cross-Site Scripting** attacks, it is important to ensure that user-supplied data output is encoded before being served by the application to other users. 

**Output encoding** effectively works by escaping user-supplied data immediately before it is served to users of the application. 

When the **data is correctly escaped** before being served to the user for display in their browser, the browser does not interpret it as code and instead interprets it as data, thus ensuring it does not get executed. 

For example, the string **<script>** is converted to **<script>** when properly escaped and is simply rendered as text in the user's browser window rather than being interpreted as code. 

Let's see how these techniques can be applied to our vulnerable example to remediate the **Stored XSS**vulnerability.

## DOM Cross Site Scripting

To defend against **Cross-Site Scripting** attacks in a **Document Object Model (DOM)** environment, a defense-in-depth approach is required, combining several security best practices. 

You should recall that **Stored XSS** and **Reflected XSS**injections take place server-side rather than client-side. With **DOM XSS**, the attack is injected into the browser’s DOM thus adding complexity by making it very difficult to prevent and highly context specific (because an attacker can inject HTML, HTML Attributes, or CSS as well as URLs). 

As a general set of principles, the application should first **HTML-encode** and then **JavaScript-encode** any user-supplied data that is returned to the client. 

Due to the very broad attack surface, developers are strongly encouraged to review areas of code that are potentially susceptible to **DOM XSS**, including but not limited to:

```
window.name` `document.referrer``document.URL` `document.documentURI``location` `location.href``location.search` `location.hash` `eval``setTimeout` `setInterval``document.write` `document.writeIn``innerHTML` `outerHTML
```



Let us apply a suitable Regex pattern to remediate this **DOM XSS** vulnerability.

## Directory (Path) Traversal

## Privileged Interface Exposure

## Authentication Credentials In URL

Applications should be configured to only accept login parameters (e.g. A username and password pair, or other authentication material) via a`form` submitted within a `POST` request. This rule applies to any sensitive user-supplied data. 

Transmitting login credentials via `GET` requests is never a good idea because URLs are inevitably stored in multiple places that an application developer has no control over. When the Login Credentials are present within the URL, and that URL is stored, it increases the likelihood of inadvertent login credential exposure. For instance, URLs are commonly stored in: 
\1. Browser history 
\2. Browser bookmarks 
\3. Referrer headers when resources are linked 
\4. Upstream proxy logs 
\5. Web application logs 

Note also that to further protect sensitive user-supplied data, the application should only accept communication from a logged-in user over HTTPS, and never over HTTP.

## Session Exposure within URL

Session ID shouldn't be sent in the URL as the URL may be disclosed in multiple locations. Also, it shouldn't be sent in POST parameter with every request because it may be stolen via XSS. The secure way to send a session ID is a cookie with HttpOnly and Secure flags.

## Horizontal Privilege Escalation

## Vertical Privilege Escalation

## Cross Site Request Forgery (POST)

Many web development frameworks provide easy ways to integrate mechanisms for implementing the protections described in this lesson. 

Rather than re-inventing the wheel, it is recommended to use native features within the existing development framework. 

References are provided below: 

The Laravel Framework 
http://laravel.com/docs/master/routing#csrf-protection

Symfony Framework 
http://symfony.com/doc/current/cookbook/security/csrf_in_login_form.html

## Clickjacking
```bash
# Prevent Clickjacking using X-Frame-Options header
Header set X-Frame-Options "deny"
# Prevent Clickjacking using Content Security Policy (Not supported by all major browsers yet)
Header set Content-Security-Policy: frame-options 'deny'; # Chrome / Firefox
Header set X-Content-Security-Policy: frame-options 'deny'; # Internet Explorer
```

To combat **Clickjacking attacks** in modern browsers, a **Content-Security-Policy** header can be set to prevent framing, using **frame-ancestors** directive that specifies pages that may embed the current page. It has the following values: 

- **none** is the same as DENY
- **self** is the same as SAMEORIGIN
- **Source URI** - enables the server administrator to specify scheme, host, and port of allowed parent pages. This directive permits usage of wildcards, e.g. https://*.codebashing.com.



Additionally, for the majority of browsers, the **X-Frame-Options** response header can be set. This header indicates to the browser which other domains are allowed to frame the web page in the response: 

- **DENY** prevents any sort of framing for the current page
- **SAMEORIGIN** allows only the current domain to frame pages from its own domain
- **ALLOW-FROM** allows a single domain URI which may frame the page. 
  Unfortunately, this means that for multiple domains, the **ALLOW-FROM** header will require an implementation that would correctly set the header by the request's referrer header.



In PHP, this can be configured by specifying the following code block within your application's global configuration include directive: 



```
  <%= "" %> 
```



However, a more scalable approach would be to deploy HTTP headers globally, at the web server layer. Let's see how we can configure Apache to set the **X-Frame-Options** header across all pages.

### Insecure URL Redirect

If an application relies on a user-supplied parameter to determine the target of a redirect, it is important to ensure this parameter is properly validated. Otherwise the application’s redirection logic can be abused in order to have legitimate users of the application redirected to malicious website(s) without their knowledge (which is the first stage of a successful phishing attack). 

Correct input validation in this context means ensuring that the user supplied redirect-target is both relevant to the application, and authorized for the requesting user. 

A more elegant and secure solution would be to use an index / look-up value, whereby the application is responsible for looking-up the correct URL Redirect target based on an index value supplied by the user – in essence preventing any user supplied URLs. 

Alternatively, do not use redirects within the application. 

Let's see how the above recommendations can be applied to our vulnerable example to remediate the URL redirect vulnerability.

### Insecure TLS Validation

## Insecure Object Deserialization

