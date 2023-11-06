# Htaccess редиректы

- [Синтаксис](#синтаксис)
  - [Основные спецсимволы](#основные-спецсимволы)
  - [Основные переменные](#основные-переменные)
  - [Общие правила](#общие-правила)
- [Примеры](#Примеры)
  - [На без www](#на-без-www)
  - [Http на https](#http-на-https)
  - [Замена части урла](#замена-части-урла)
  - [С одного домена на другой](#с-одного-домена-на-другой)
  - [С одной страницы на другую](#с-одной-страницы-на-другую)
  - [Множество слэшей](#множество-слэшей)
  - [Убираем слэши в конце URL для статических файлов](#убираем-слэши-в-конце-url-для-статических-файлов)
  - [Добавляем слэш](#добавляем-слэш)
  - [С index.php index.html](#с-indexphp-indexhtml)
  - [Добавляем или убираем html/php в конце](#добавляем-или-убираем-htmlphp-в-конце)
  - [Ограничение доступа к сайту по IP](#ограничение-доступа-к-сайту-по-ip)
  - [Ограничение доступа к сайту ботам](#ограничение-доступа-к-сайту-ботам)
  - [Переопределение главной страницы сайта](#переопределение-главной-страницы-сайта)
  - [Кеширование](#кеширование)
  - [Управление настройками php](#управление-настройками-php)
  - [Предупреждаем о недоступности сайта](#предупреждаем-о-недоступности-сайта)
  - [Подмена файла](#подмена-файла)
  - [Своя страница для ошибки 500](#своя-страница-для-ошибки-500)

## Синтаксис

Каждая директива (команда) начинается с новой строки, после знака # можно добавлять комментарии, которые не будут учитываться сервером. Изменения на сайте вступают в силу сразу, перезагрузка сервера не требуется. Можно почитать тут https://htaccess.net.ru/.

### Основные спецсимволы:
| Символ | Описание                                                                                                                             |
|--------|--------------------------------------------------------------------------------------------------------------------------------------|
| ^      | начало строки                                                                                                                        |
| $      | конец строки                                                                                                                        |
| .      | (точка) любой символ                                                                                                                 |
| ()     | используются для выделения групп символов. В дальнейшем к ним можно обращаться по номеру                                             |
| *      | ставится после символа (группы), который может отсутствовать или присутствовать неограниченное число раз подряд                      |
| ?      | ставится после символа (группы), который может как присутствовать, так и отсутствовать                                               |
| +      | действует аналогично символу * с той лишь разницей, что предшествующий ему символ обязательно должен присутствовать хотя бы один раз |
| [0-9]  | последовательность символов, например, от 0 до 9, A-z                                                                                |
| [^]    | используются для перечисления недоступных символов                                                                                   |
| \|     | символ «или», выбирается или одна группа, или другая. Например, выражения "A\|B" означают "A или B"                                  |
| \      | ставится перед спецсимволами, если они нужны в своем первозданном виде                                                                                                                         |
| #      | все, что расположено после символа '#', считается комментарием                                                                                                                         |

### Основные переменные:
%{NAME_OF_VARIABLE} — где NAME_OF_VARIABLE может быть одной из ниже приведенных переменных:

| Переменная | Описание |
|------------|----------|
| HTTP_USER_AGENT     | содержит информацию о типе и версии браузера и операционной системы посетителя      |
| HTTP_REFERER      | приводится адрес страницы, с которой посетитель пришел на данную страницу      |
| HTTP_COOKIE      | список COOKIE, передаваемых браузером      |
| HTTP_FORWARDED     | страница непосредственно, с которой перешел пользователь      |
| HTTP_HOST     | адрес сервера, например, site.ru      |
| HTTP_ACCEPT      | описываются предпочтения клиента относительно типа документа      |
| REMOTE_ADDR      | IP-адрес посетителя      |
| REMOTE_HOST      | адрес посетителя в нормальной форме — например, 23.beeline.ru      |
| REMOTE_IDENT      | имя удаленного пользователя      |
| REMOTE_USER      | тоже, что и REMOTE_IDENT, но содержит только имя |
| REQUEST_METHOD     | позволяет определить тип запроса (GET или POST). Должен обязательно анализироваться, т.к. определяет дальнейший способ обработки информации      |
| SCRIPT_FILENAME     | полный путь к веб-странице на сервере      |
| PATH_INFO      | содержит в себе все, что передавалось в скрипт      |
| QUERY_STRING      | содержит строку, переданную в качестве запроса при вызове CGI скрипта      |
| AUTH_TYPE      | используется для идентификации пользователя      |
| DOCUMENT_ROOT      | содержит путь к корневой директории сервера      |
| SERVER_ADMIN      | почтовый адрес владельца сервера, указанный при установке      |
| SERVER_NAME      | адрес сервера, типа idea.site.ru      |
| SERVER_ADDR      | IP-адрес вашего сайта      |
| SERVER_PORT      | порт, на котором работает Apache      |
| SERVER_PROTOCOL      | версия HTTP протокола      |
| SERVER_SOFTWARE      | название сервера, например, Apache/1.3.2 (Unix)      |
| TIME_YEAR TIME_MON TIME_DAY TIME_HOUR TIME_MIN TIME_SEC TIME_WDAY TIME     | переменные предназначены для работы со временем в разных форматах      |
| API_VERSION      | это версия API модуля Apache (внутренний интерфейс между сервером и модулем) в текущей сборке сервера, что определено в include/ap_mmn.h      |
| THE_REQUEST      | полная строка HTTP запроса отправленная браузером серверу (т.е., «GET /index.html HTTP/1.1»). Она не включает какие-либо дополнительные заголовки отправляемые браузером      |
| REQUEST_URI      | ресурс, запрошенный в строке HTTP запроса      |
| REQUEST_FILENAME      | полный путь в файловой системе сервера к файлу или скрипту соответствующим этому запросу      |
| IS_SUBREQ      | будет содержать текст «true» если запрос выполняется в текущий момент как подзапрос, «false» в другом случае. Подзапросы могут быть сгенерированы модулями которым нужно иметь дело с дополнительными файлами или URI для того чтобы выполнить собственные задачи      |

### Общие правила:

| Правила                                                                                                                                                                               |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Всегда делайте резервную копию файла перед внесением изменений, чтобы оперативно «откатить» их.|
| Вносите изменения пошагово, добавляйте по одному правилу и оценивайте, как оно сработало. |
| Если размещаете несколько файлов .htaccess в разных каталогах, прописывайте в дочерних только новые директивы, которые актуальны для конкретного каталога, остальные наследуются от родительского каталога или файла в корневой папке.                                                                                                                                                                               |
| Очищайте кэш браузера: Ctrl + F5, в Safari: Ctrl + R, в Mac OS: Cmd + R.                                                                                                                                                                               |
| Если возникает ошибка 500, проверьте синтаксис правила (нет ли опечатки). Можно воспользоваться сервисами проверки файла .htaccess онлайн, например http://www.htaccesscheck.com/. Если ошибок не найдено, значит в главном конфигурационном файле такой тип директивы запрещен.                                                                                                                                                                               |
| В директивах .htaccess кириллические символы не допускаются. Если необходимо указать адрес кириллического домена (мойсайт.рф), воспользуйтесь любым whois-сервисом, чтобы узнать его написание по методу punycode. Например, адрес «сайт.рф» будет выглядеть как «xn--80aswg.xn--p1ai/$1».                                                                                                                                                                               |
| Слишком большое количество директив в .htaccess может снизить работоспособность сайта. Старайтесь использовать файл только в том случае, если другим путем задачу решить нельзя.|

## Примеры

### Первая строчка всегда

```sh 
# Директива включает редиректы
RewriteEngine On

# Без директивы (.*)=/$1 будет /var/www/site/web/$1 с директивой =/$1
RewriteBase / 
   
# Разрешает переход по символическим ссылкам.
Options +FollowSymLinks   
```

### На без www
```sh 
# Вариант 1
RewriteCond %{HTTP_HOST} ^www.site.ru$ [NC]
RewriteRule ^(.*)$ https://site.ru/$1 [R=301,L]

# Вариант 2
RewriteCond %{HTTP_HOST} ^www\.(.*)$
RewriteRule ^(.*)$ https://%1/$1 [L,R=301] 
```

### Http на https
```sh 
# Вариант 1
RewriteCond %{HTTP:HTTPS} !YES [OR]
RewriteCond %{THE_REQUEST} // [OR]
RewriteCond %{HTTP_HOST} !^site.ru [NC]
RewriteRule (.*) https://site.ru%{REQUEST_URI} [R=301,L]

# Вариант 2
RewriteCond %{HTTPS} off
RewriteCond %{HTTP:X-Forwarded-Proto} !https
RewriteCond %{REQUEST_URI} !(.*)/$
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI}/ [L,R=301]

# Вариант 3
RewriteCond %{SERVER_PORT} 80
RewriteRule ^(.*)$ https://site.ru/$1 [R,L]

# Вариант 4
RewriteCond %{HTTPS} !=on
RewriteRule ^.*$ https://%{SERVER_NAME}%{REQUEST_URI} [R,L]
```

### Замена части урла
В случае если нужно сделать редирект со страниц \
https://site.ru/catalog/smartfony/ на https://site.ru/catalog/telefony/ \
https://site.ru/catalog/smartfony/iphone/ на https://site.ru/catalog/telefony/iphone/
```sh 
RewriteRule ^catalog/smartfony/(.*)$ /catalog/telefony/$1 [R=301,L]
```

### С одного домена на другой
```sh 
# Вариант 1
RewriteRule ^(.*)$ http://www.site.ru/$1 [R=301,L]

# Вариант 2
RewriteCond %{HTTP_HOST} ^www\.site1\.ru$ [NC]
RewriteRule ^(.*)$ http://www.site2.ru/$1 [R=301,L]
```

### С одной страницы на другую
Удобный генератор https://www.301-redirect.online/htaccess-rewrite-generator
```sh 
# Вариант 1. Без Get параметров
# Будет работать только для страницы без get параметров
# https://site.ru/blog/, для https://site.ru/blog/?param1=1&param2=2 редиректа не будет.

RewriteCond %{QUERY_STRING} ^$
RewriteRule ^blog/$ https://blog.site.ru/? [R=301,L]

RewriteCond %{QUERY_STRING} ^$
RewriteRule ^product\-category/kompjutery/mac\-mini/$ https://site.ru/mac/? [R=301,L]

# Вариант 2. С Get параметрами
# Будет работать и для страницы https://site.ru/saint-petersburg/ и для https://site.ru/saint-petersburg/?param1=1&param2=2. 

RewriteCond %{QUERY_STRING} ^(.+) [NC]
RewriteRule ^saint\-petersburg/$ https://site.ru/spb/? [R=301,L]

```

### Множество слэшей
Если нужно убрать повторы слэшей. \
https://site.ru//blog/ \
https://site.ru/blog// \
https://site.ru//blog//article/ 

```sh 
# Вариант 1
# Проверить, повторяется ли слэш (//) более двух раз.
RewriteCond %{THE_REQUEST} // 
# Исключить все лишние слэши.
RewriteRule .* /$0 [R=301,L] 

# Вариант 2
# Проверить, повторяется ли слэш (//) более двух раз.
RewriteCond %{REQUEST_URI} ^(.*)/{2,}(.*)$
# Исключить все лишние слэши.
RewriteRule . %1/%2 [R=301,L]

# Вариант 3
RewriteCond %{REQUEST_URI} ^(.*?)\/{2,}(.*?)$
RewriteRule . %1/%2 [L,R=301]
RewriteCond %{THE_REQUEST} //
RewriteRule .* /$0 [R=301,L]
```

### Убираем слэши в конце URL для статических файлов
```sh 
# Если файл содержит точку.
RewriteCond %{REQUEST_URI} \..+$  
# И это не директория. 
RewriteCond %{REQUEST_FILENAME} !-d   
# Является файлом.   
RewriteCond %{REQUEST_FILENAME} -f
# И в конце URL есть слэш.
RewriteCond %{REQUEST_URI} ^(.+)/$      
# Исключить слэш  
RewriteRule ^(.+)/$ /$1 [R=301,L]
```

### Добавляем слэш 
Если его нет, и это не файл.
```sh 
# Если слэша в конце нет.
RewriteCond %{REQUEST_URI} !(.*)/$
# Не является файлом.
RewriteCond %{REQUEST_FILENAME} !-f
# В URL нет точки (файл). 
RewriteCond %{REQUEST_URI} !\..+$
# Добавляем слэш в конце.   
RewriteRule ^(.*)$ $1/ [L,R=301]
```

### С index.php index.html
```sh 
# Вариант 1
RewriteRule ^(.*)index\.php$ $1 [R=301,L]

# Вариант 2
RewriteRule ^index\.php$ / [R=301,L]
RewriteRule ^(.*)/index\.php$ /$1/ [R=301,L]

# Вариант 3
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ /index.php?/$1 [L]

# Вариант 4
RewriteCond %{THE_REQUEST} ^[A-Z]{3,9}\ /index\.html\ HTTP/
RewriteRule ^index\.html$ https://site.ru/ [R=301,L]

RewriteCond %{THE_REQUEST} ^[A-Z]{3,9}\ /index\.php\ HTTP/
RewriteRule ^index\.php$ https://site.ru/ [R=301,L]

# Вариант 5. Исключая админку
RewriteCond %{REQUEST_URI} !/bitrix/.* [NC]
RewriteRule ^(.*)?index\.php$ https://site.ru/$1 [R=301,NE,L]
```

### Добавляем или убираем html/php в конце
```sh 
# Вариант 1. Убрать .html/.php
RewriteRule ^([^.?]+)$ %{REQUEST_URI}.php [L]
RewriteCond %{THE_REQUEST} "^[^ ]* .*?\.php[? ].*$"
RewriteRule .* - [L,R=404]

# Вариант 2. Добавить .html/.php
RewriteCond %{REQUEST_URI} (.*/[^/.]+)($|\?)
RewriteRule .* %1.html [R=301,L]
RewriteRule ^(.*)/$ /$1.html [R=301,L]

# Вариант 3. Убрать .html/.php
RewriteRule (.*)\.html$ $1 [R=301,L]
```

### Ограничение доступа к сайту по IP
```sh 
# Запретить доступ к сайту с IP-адресов 123.4.5.6 и 123.5.4.3
Order Allow,Deny
Allow from all
Deny from 123.4.5.6 123.5.4.3

# Разрешить для всех адресов кроме 123.4.5.6 и 123.5.4.3
Order Deny,Allow
Deny from all
Allow from 123.4.5.6 123.5.4.3

# Запретить доступ к сайту с IP-адресов по маске (198.69.132.24, 198.69.136.89, 198.69.1.8)
Order Allow,Deny
Allow from all
Deny from 198.69.
```

### Ограничение доступа к сайту ботам
Боты AhrefsBot, DotBot и т.д.
```sh 
RewriteCond %{HTTP_USER_AGENT} ^.*(SemrushBot|MJ12bot|AhrefsBot|bingbot|DotBot|LinkpadBot|SputnikBot|statdom.ru|MegaIndex.ru|WebDataStats|Jooblebot|Baiduspider|BackupLand|NetcraftSurveyAgent|openstat.ru|TinyTestBot|DataForSeoBot|Bytespider|ZoominfoBot|WOW64|bingbot|Applebot|SEMrushBot).*$ [NC]
RewriteRule .* - [F,L]
```

### Переопределение главной страницы сайта
Индексного файла каталога.
```sh 
DirectoryIndex menu.html
```

### Кеширование
```sh 
<IfModule mod_expires.c>
  ExpiresActive on
  ExpiresByType image/jpg "access 1 year"
  ExpiresByType image/jpeg "access 1 year"
  ExpiresByType image/gif "access 1 year"
  ExpiresByType image/png "access 1 year"
  ExpiresByType text/css "access 1 year"
  ExpiresByType text/html "access 1 year"
  ExpiresByType image/webp "access 1 year"
  ExpiresByType application/pdf "access 1 year"
  ExpiresByType text/x-javascript "access 1 year"
  ExpiresByType application/javascript "access 1 year"
  ExpiresByType application/x-javascript "access 1 year"
  ExpiresByType application/x-shockwave-flash "access 1 year"
  ExpiresByType image/x-icon "access 1 year"
  ExpiresByType application/x-font-woff "access 1 year"
  ExpiresByType application/x-font-woff2 "access 1 year"
  ExpiresByType image/svg+xml "access 1 year"
  ExpiresDefault "access 1 year"
</IfModule>

<FilesMatch "\.(ttf|otf|eot|woff|font.css)$">
  <IfModule mod_headers.c>
    Header set Access-Control-Allow-Origin "*"
  </IfModule>
</FilesMatch>

AddType application/vnd.ms-fontobject eot
AddType font/truetype ttf
AddType font/opentype otf
AddType application/x-font-woff woff

<filesMatch ".(jpg|jpeg|png|gif|ico|svg|webp|ttf|eot|woff|woff2)$">
  Header set Cache-Control "max-age=31536000, public"
</filesMatch>

<IfModule mod_deflate.c>
  <FilesMatch "\.(ttf|otf|eot|svg)$" >
    SetOutputFilter DEFLATE
  </FilesMatch>
</IfModule>
```

### Управление настройками php
```sh 
<ifModule mod_php.c>
  php_value upload_max_filesize 32M
  php_value post_max_size 10M
  php_value max_execution_time 240
  php_value max_input_time 200
  php_flag display_errors on
</ifModule>
```

### Предупреждаем о недоступности сайта
IP-адрес в примере (12\.345\.678\.90) замените на свой, в последней строке укажите адрес страницы вашего ресурса с информацией о характере и сроках завершения работ.

```sh 
RewriteCond %{REQUEST_URI} !/info.html$
RewriteCond %{REMOTE_HOST} !^12\.345\.678\.90
RewriteRule $ https://mysite.ru/info.html [R=302,L]
```

### Подмена файла
Есть файл robots.txt и нужно чтобы по урлу https://site.ru/robots.txt показывалось не то что напрямую написано в этом файле, а например то что генерируется в файле https://site.ru/robots.php
!Только для серверов apache, nginx отдает статику и не допускает этот запрос до apache.
```sh 
RewriteRule ^robots.txt$ /robots.php [L]
```

### Своя страница для ошибки 500
```sh 
ErrorDocument 500 /500.php
ErrorDocument 501 /501.php
ErrorDocument 502 /502.php

# Полный список возможных ошибок
#  400 - Bad Request
#  401 - Unauthorized
#  402 - Payment Required
#  403 - Forbidden
#  404 - Not Found
#  405 - Method Not Allowed
#  406 - Not Acceptable
#  407 - Proxy Authentication Required
#  408 - Request Time-out
#  409 - Conflict
#  410 - Gone
#  411 - Length Required
#  412 - Precondition Failed
#  413 - Request Entity Too Large
#  414 - Request-URI Too Large
#  415 - Unsupported Media Type
#  500 - Internal Server Error
#  501 - Not Implemented
#  502 - Bad Gateway
#  503 - Service Unavailable
#  504 - Gateway Time-out
#  505 - HTTP Version not supported
```
