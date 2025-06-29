# IPC

ASF включает в себя собственный, уникальный интерфейс IPC который может использоваться для дальнейшего взаимодействия с процессом. IPC означает **[межпроцессное взаимодействие (inter-process communication)](https://en.wikipedia.org/wiki/Inter-process_communication)**, и, в самом простом определении, это "веб-интерфейс ASF", основанный на **[HTTP-сервере Kestrel](https://learn.microsoft.com/aspnet/core/fundamentals/servers/kestrel)**. Он может использоваться для дальнейшей интеграции с процессом как в качестве фронтенда для конечного пользователя (ASF-ui), так и в качестве бэкенда для сторонних интеграций (ASF API).

IPC может использоваться для множества различных вещей, в зависимости от ваших нужд и способностей. Например, вы можете использовать его для получения статуса ASF и всех ботов, отправки команд ASF, получения и изменения конфигурационных файлов, добавления новых ботов, удаления существующих ботов, добавления ключей в **[BGR](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer-ru-RU)** или для доступа к журналу ASF. Все эти действия доступны через наше API, а значит вы можете создавать собственные утилиты и скрипты, которые смогут взаимодействовать с ASF и влиять на него в процессе работы. В добавок к этому, часть действий (таких как отправка команд) также реализованы в нашем ASF-ui, который позволяет вам легко получить к ним доступ через дружественный веб-интерфейс.

---

# Использование

Если вы вручную не отключили IPC с помощью `IPC` **[глобального файла конфигурации](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-ru-RU#user-content-Файл-глобальной-конфигурации)**, то он включен по умолчанию. ASF сообщит о запуске IPC в своём журнале, который вы можете проверить чтобы узнать что IPC интерфейс удачно запущен:

```text
INFO|ASF|Start() Запуск IPC сервера...
INFO|ASF|Start() IPC сервер готов!
```

Http сервер ASF теперь ожидает входящих запросов на нескольких конечных точках. If you didn't provide a custom configuration file for IPC, it'll be `localhost`, both IPv4-based **[127.0.0.1](http://127.0.0.1:1242)** and IPv6-based **[[::1]](http://[::1]:1242)** on default `1242` port. You can access our IPC interface through above links, but only from the same machine as the one running ASF process.

Есть три способа доступа к интерфейсу IPC ASF, вы можете выбрать один из них в зависимости от планируемого использования.

На самом нижнем уровне нахоится **[ASF API](#asf-api)**, это ядро нашего интерфейса IPC, и позволяет работать всему остальному. Именно API вам стоит использовать в ваших собственных инструментах, утилитах и проектах для связи с ASF.

Средним уровнем является **[документация Swagger ](#user-content-Документация-swagger)**, которая представляет из себя графический интерфейс для ASF API. Это полная документация ASF API, а также инструмент, позволяющий использовать их более удобно. Вам стоит познакомиться с этим инструментом если вы планируете создать свой инструмент, утилиту или другой проект который должен обмениваться данными с ASF посредством API.

Самый высокий уровень это **[ASF-ui](#asf-ui)**, который основан на нашем ASF API и предоставляет дружественный к пользователю способ выполнять различные действия в ASF. Это интерфейс IPC используемый по умолчанию для конечных пользователей, и прекрасный пример того что вы можете создать на базе ASF API. Если захотите, вы можете использовать собственный веб-интерфейс для ASF, указав рабочую папку ASF с помощью **[аргумента командной строки](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-ru-RU#user-content-Аргументы)** `--path` с расположенной в нём папкой `www`.

---

# ASF-ui

ASF-ui это проект сообщества, цель которого создать дружественный к пользователю графический веб-интерфейс для конечных пользователей. Он служит интерфейсом для наших **[ASF API](#asf-api)**, позволяющим с лёгкостью выполнять различные действия. Это интерфейс поставляемый с ASF по умолчанию.

Как сказано выше, ASF-ui это проект разрабатываемый нашим сообществом, и поэтому он не поддерживается основными разработчиками ASF. У него свой собственный путь развития в **[репозитории ASF-ui](https://github.com/JustArchiNET/ASF-ui)**, и именно туда вам следует адресовать все вопросы, проблемы, сообщения об ошибках и предложения.

Вы можете использовать ASF-ui для общего управления процессом ASF. Он позволяет, например, управлять ботами, изменять настройки, отправлять команды и использовать некоторые другие функции, обычно доступных через ASF.

![ASF-ui](https://raw.githubusercontent.com/JustArchiNET/ASF-ui/main/.github/previews/bots.png)

---

# ASF API

Наш ASF API представляет собой веб-API, построенный с учётом **[REST](https://ru.wikipedia.org/wiki/REST)** и основанный на JSON в качестве основного формата данных. Мы стараемся максимально точно описать ответ, используя как коды состояний HTTP (когда это применимо), так и ответ, который вы можете самостоятельно разобрать чтобы выяснить, окончился ли запрос успешно, и если нет - то почему.

Доступ к нашему ASF API может быть получен путём отправки соответствующих запросов на соответствующие конечные точки `/Api`. Вы можете использовать эти конечные точки API для создания своих вспомогательных скриптов, утилит, интерфейсов и тому подобного. Именно это делает ASF-ui "под капотом", и любая утилита может делать то же самое. ASF API официально поддерживается и обслуживается основной командой ASF.

Полную документацию по всем доступным конечным точкам, описаниям, запросам, ответам, кодам состояния http и всему остальному, что относится к ASF API, вы найдёте в нашей **[документации Swagger](#user-content-Документация-swagger)**.

![ASF API](https://github.com/user-attachments/assets/08c3d4ad-ea77-403d-a18a-b75c3d0a3097)


---

# Пользовательская конфигурация

Наш интерфейс IPC поддерживает дополнительный файл конфигурации, `IPC.config`, который следует расположить в стандартной папке `config` вашего ASF.

При наличии, этот файл задаёт расширенную настройку используемого в ASF Kestrel http server, а также другие, связанные с IPC, настройки. Если у вас нет какой-то конкретной необходимости, вам нет смысла использовать этот файл, поскольку ASF уже использует разумные значения по умолчанию.

Конфигурационный файл основан на следующей структуре JSON:

```json
{
    "Kestrel": {
        "Endpoints": {
            "example-http4": {
                "Url": "http://127.0.0.1:1242"
            },
            "example-http6": {
                "Url": "http://[::1]:1242"
            },
            "example-https4": {
                "Url": "https://127.0.0.1:1242",
                "Certificate": {
                    "Path": "/path/to/certificate.pfx",
                    "Password": "passwordToPfxFileAbove"
                }
            },
            "example-https6": {
                "Url": "https://[::1]:1242",
                "Certificate": {
                    "Path": "/path/to/certificate.pfx",
                    "Password": "passwordToPfxFileAbove"
                }
            }
        },
        "KnownNetworks": [
            "10.0.0.0/8",
            "172.16.0.0/12",
            "192.168.0.0/16"
        ],
        "PathBase": "/"
    }
}
```

`Endpoints` - это массив конечных точек, каждая из которых имеет уникальное имя (такое как `example-http4`) и свойство `Url`, задающее адрес для ожидания запросов в формате `Protocol://Host:Port`. По умолчанию, ASF ожидает запросов по адресам http IPv4 и IPv6, но мы добавили примеры настроек для https, которые вы можете при необходимости использовать. Вам следует объявлять только те конечные точки, которые вам нужны, мы включили 4 в пример выше только чтобы вам было удобнее их редактировать.

`Host` принимает значение либо `localhost`, либо фиксированный IP адрес интерфейса, который он должен прослушивать (IPv4/IPv6), или `*` значение, которое связывает http сервер ASF со всеми доступными интерфейсами. Используя другие значения, такие как `mydomain.com` или `192.168.0.` действует то же самое, что и `*`, фильтрация IP не реализована, поэтому будьте предельно осторожны при использовании значений `Host`, которые позволяют удалённый доступ. Это позволит доступ к интерфейсу IPC ASF с других машин, что может представлять собой угрозу безопасности. Мы настоятельно рекомендуем в данном случае использовать **как минимум** `IPCPassword` (и желательно также ваш собственный брандмауэр).

`KnownNetworks` - это **необязательная** переменная указывает сетевые адреса, которые считаем заслуживающими доверия. По умолчанию ASF настроен на доверие интерфейсу обратной связи (`localhost`, **только** этому компьютеру). Это свойство используется двумя способами. Во-первых, если вы не измените `IPCPassword`, мы разрешим доступ к API ASF только компьютерам из доверенных сетей и откажемся от всех остальных в качестве меры безопасности. Во-вторых, это свойство имеет решающее значение для доступа к ASF через обратные прокси, поскольку ASF будет учитывать свои заголовки только в том случае, если сервер обратного прокси находится в пределах доверенных сетей. Соблюдение заголовков имеет решающее значение для механизма защиты от брутфорса ASF, поскольку вместо запрета обратного прокси-сервера, в случае проблемы он будет запрещать IP-адрес, указанный обратным прокси-сервером в качестве источника исходного сообщения. Будьте предельно осторожны с указанными здесь сетями, так как это допускает потенциальную атаку с подменой IP-адреса и несанкционированный доступ в случае взлома или неправильной настройки доверенной машины.

`PathBase` — это **необязательный** базовый путь, который будет использоваться интерфейсом IPC. По умолчанию используется `/`, и его не требуется изменять для большинства случаев использования. Изменив этот параметр вы разместите весь интерфейс IPC по заданному префиксу, например по адресу `http://localhost:1242/MyPrefix` вместо `http://localhost:1242`. Использование пользовательского `PathBase` может быть желательным в комбинации с обратным прокси, если вы хотите проксировать только отдельный URL, например `mydomain.com/ASF` вместо всего домена `mydomain.com` целиком. Обычно это требует от вас написать правило перезаписи для вашего веб-сервера, которое будет отображать `mydomain.com/ASF/Api/X` - > `localhost:1242/Api/X`, но вместо этого вы можете определить собственную `PathBase` для `/ASF` и упростить настройку `mydomain.com/ASF/Api/X` - > `localhost:1242/ASF/Api/X`.

Если у вас нет насущной необходимости задать пользовательский базовый путь, лучше оставить ему значение по умолчанию.

## Пример конфигурации

### Изменение порта по умолчанию

Следующая конфигурация просто меняет стандартный ASF порт с `1242` на `1337`. Вы можете выбрать любой порт, который вам нравится, но мы рекомендуем диапазон `1024-32767`, так как другие порты обычно **[зарегистрированы](https://en.wikipedia.org/wiki/Registered_port)** и могут, например, потребовать `root` доступ в Linux.

```json
{
    "Kestrel": {
        "Endpoints": {
            "HTTP4": {
                "Url": "http://127.0.0.1:1337"
            },
            "HTTP6": {
                "Url": "http://[::1]:1337"
            }
        }
    }
}
```

---

### Разрешить доступ со всех IP-адресов

Следующая конфигурация разрешит удаленный доступ из всех источников, поэтому вам следует ** убедиться, что вы прочитали и поняли наше уведомление о безопасности по этому поводу **, доступное выше.

```json
{
    "Kestrel": {
        "Endpoints": {
            "HTTP": {
                "Url": "http://*:1242"
            }
        }
    }
}
```

Если вам не требуется доступ из всех источников, а только, например, для вашей локальной сети, тогда лучше проверить локальный IP адрес сервера ASF, например `192.168.0.10` и используйте вместо `*` в примере конфигурации выше. Sadly that's only possible if your LAN address is always the same, as otherwise you'll probably have more satisfying results with `*` and your own firewall on top of that allowing only local subnets to access ASF's port.

---

# Аутентификация

Интерфейс IPC ASF по умолчанию не нуждается ни в какой аутентификации, поскольку `IPCPassword` установлено значение `null`. Однако, если вы активировали `IPCPassword`, указав ему не пустое значение, любой запрос к API ASF требует пароль, совпадающий с установленным в `IPCPassword`. Если вы пропустили аутентификацию или указали неверный пароль, вы получите ошибку `401 - Unauthorized`. После 5 неудачных попыток аутентификации (неверный пароль) вы будете временно заблокированы с ошибкой `403 - Forbidden`.

Аутентификация может быть выполнена двумя разными способами.

## Заголовок `Authentication`

Как правило, вам следует использовать **[заголовки HTTP-запросов](https://ru.wikipedia.org/wiki/Список_заголовков_HTTP)**, установив `Authentication`, в качестве значения которого указан ваш пароль. Как этого добиться зависит от того, каким инструментом вы пользуетесь для доступа к интерфейсу IPC ASF, например если вы пользуетесь `curl` вам следует добавить в качестве аргумента `-H 'Authentication: MyPassword'`. Таким способом аутентификация передаётся в заголовках запроса, где и должна быть.

## Параметр `password` в строке запроса

В качестве альтернативы вы можете добавить параметр `password` в конец URL, который хотите вызвать, например вызывая `/Api/ASF?password=MyPassword` вместо `/Api/ASF`. Этот подход достаточно хорош, но как видите он раскрывает пароль в открытом виде, что не всегда допустимо. В добавок это дополнительный параметр в строке запроса, что загромождает вид URL, и создаёт впечатление что он применим только к этому URL, хотя на самом деле пароль применяется ко всему обмену с ASF API.

---

Оба способа поддерживаются, и только вам решать каким из них пользоваться. Мы рекомендуем использовать заголовки HTTP везде где возможно, поскольку с точки зрения использования это более подходящий метод, чем строка запроса. Однако мы также поддерживаем строку запроса, в основном из-за различных ограничений, связанными с заголовками запроса. Хорошим примером этого служит отсутствие пользовательских заголовков при инициализации соединения websocket из javascript (хотя это совершенно корректно с точки зрения RFC). В этой ситуации строка запроса единственный способ аутентификации.

---

# Документация Swagger

Наш интерфейс IPC, в дополнение к ASF API и ASF-ui также включает в себя документацию Swagger, которая доступна по **[адресу](http://localhost:1242/swagger)** `/swagger` . Документация Swagger служит посредником между нашей реализацией API и другими инструментами, использующими их (например ASF-ui). Она содержит полную документацию и список всех доступных конечных точек API в **[спецификации](https://ru.wikipedia.org/wiki/OpenAPI_(%D1%81%D0%BF%D0%B5%D1%86%D0%B8%D1%84%D0%B8%D0%BA%D0%B0%D1%86%D0%B8%D1%8F))** **[OpenAPI](https://swagger.io/resources/open-api)**, которая легко может быть использована в других проектах, позволяя вам легко писать и тестировать ASF API.

Помимо использования документации Swagger как полного описания ASF API, вы можете использовать её также как дружественный к пользователю способ вызывать различные конечные точки API, в основном те, которые не реализованы в ASF-ui. Поскольку наша документация Swagger генерируется автоматически из кода ASF, она гарантировано будет актуальной и будет включать в себя все конечные точки API, доступные в вашей версии ASF.

![Документация Swagger](https://github.com/user-attachments/assets/ce998e08-f5db-46b0-a9e8-e6b69abd94bb)


---

# ЧАВО

### Безопасно ли использовать интерфейс IPC в ASF?

ASF по умолчанию ожидает соединений только по адресам `localhost`, а значит доступ к IPC ASF **невозможен** с любой машины, кроме той где запущен ASF. Если вы не изменяли адрес конечных точек, атакующему понадобится доступ к вашей машине чтобы получить доступ к IPC ASF, а значит он настолько безопасен насколько это возможно, и никто не может получить к нему доступ, даже из вашей локальной сети.

Однако, если вы решите изменить установленные по умолчанию адреса привязки `localhost` на что-то другое, то вам следует установить соответствующие правила брандмауэра **самостоятельно**, чтобы разрешить только доверенным IP-адресам доступ к интерфейсу ASF. В дополнение к этому вам нужно будет настроить `IPCPassword`, поскольку ASF откажется разрешать другим машинам доступ к ASF API без него, что добавляет еще один уровень дополнительной безопасности. Возможно вы также захотите в этом случае использовать интерфейс IPC ASF за обратным прокси, что подробно описано ниже.

### Могу ли я вызывать ASF API из своих утилит или скриптов?

Да, это именно то, для чего разрабатывался ASF API, вы можете вызывать их из любого инструмента способного отправлять HTTP запросы. Пользовательские скрипты в браузере следуют логике **[CORS](https://ru.wikipedia.org/wiki/Cross-origin_resource_sharing)**, и поэтому мы разрешаем для них доступ из любых источников (`*`) при условии что установлен `IPCPassword` в качестве дополнительной меры безопасности. Это позволяет вам выполнять различные авторизованные запросы ASF API, при этом не позволяя вредоносным скриптам делать это автоматически (поскольку им для этого понадобиться знать ваш `IPCPassword`).

### Могу я получить доступ к IPC ASF удалённо, например с другого компьютера?

Да, мы рекомендуем использовать обратный прокси для этого. Таким образом вы сможете получить доступ к вашему веб-серверу как обычно, а он в свою очередь получит доступ к IPC ASF на той же машине. Кроме того, если вы не хотите использовать обратный прокси, вы можете задать **[пользовательскую конфигурацию](#enabling-access-from-all-ips)** с соответствующим URL для этого. Например, если ваш компьютер подключен к VPN с адресом `10.8.0.1`, вы можете установить URL прослушивания `http://10.8.0.1:1242` в конфигурации IPC, который позволил бы доступ IPC изнутри вашей частной VPN, но не откуда-либо еще.

### Можно ли использовать IPC ASF за обратным прокси, таким как Apache или Nginx?

**Да**, наш IPC полностью совместим с такой конфигурацией, так что вы можете использовать также с другими утилитами для дополнительной безопасности или совместимости, если хотите. В целом, используемый в ASF Kestrel http server достаточно безопасен и не представляет риска при прямом подключении из интернета, но если вы поставите его позади обратного прокси, такого как Apache или Nginx, вы можете получить дополнительный функционал, недоступный в ином случае, такой как например защита интерфейса ASF с помощью **[Basic authentication](https://en.wikipedia.org/wiki/Basic_access_authentication)**.

Пример конфигурации Nginx вы можете найти ниже. Мы включили полный блок `server`, хотя вас в основном интересуют блоки `location`. Дальнейшую информацию вы можете найти в **[документации nginx](https://nginx.org/ru/docs/)**.

```nginx
server {
    listen *:443 ssl;
    server_name asf.mydomain.com;
    ssl_certificate /path/to/your/fullchain.pem;
    ssl_certificate_key /path/to/your/privkey.pem;

    location ~* /Api/NLog {
        proxy_pass http://127.0.0.1:1242;

        # Only if you need to override default host
#       proxy_set_header Host 127.0.0.1;

        # X-headers should always be specified when proxying requests to ASF
        # They're crucial for proper identification of original IP, allowing ASF to e.g. ban the actual offenders instead of your nginx server
        # Specifying them allows ASF to properly resolve IP addresses of users making requests - making nginx work as a reverse proxy
        # Not specifying them will cause ASF to treat your nginx as the client - nginx will act as a traditional proxy in this case
        # If you're unable to host nginx service on the same machine as ASF, you most likely want to set KnownNetworks appropriately in addition to those
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Host $host:$server_port;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Server $host;

        # We add those 3 extra options for websockets proxying, see https://nginx.org/en/docs/http/websocket.html
        proxy_http_version 1.1;
        proxy_set_header Connection "Upgrade";
        proxy_set_header Upgrade $http_upgrade;
    }

    location / {
        proxy_pass http://127.0.0.1:1242;

        # Only if you need to override default host
#       proxy_set_header Host 127.0.0.1;

        # X-headers should always be specified when proxying requests to ASF
        # They're crucial for proper identification of original IP, allowing ASF to e.g. ban the actual offenders instead of your nginx server
        # Specifying them allows ASF to properly resolve IP addresses of users making requests - making nginx work as a reverse proxy
        # Not specifying them will cause ASF to treat your nginx as the client - nginx will act as a traditional proxy in this case
        # If you're unable to host nginx service on the same machine as ASF, you most likely want to set KnownNetworks appropriately in addition to those
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Host $host:$server_port;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Server $host;
    }
}
```

Пример конфигурации Apache вы можете найти ниже. Пожалуйста, обратитесь к **[документации по apache](https://httpd.apache.org/docs)** если вам нужно больше объяснений.

```apache
<IfModule mod_ssl.c>
    <VirtualHost *:443>
        ServerName asf.mydomain.com

        SSLEngine On
        SSLCertificateFile /path/to/your/fullchain.pem
        SSLCertificateKeyFile /path/to/your/privkey.pem

        # TODO: Apache не может производить регистронезависимое сравнение, поэтому мы задали два наиболее используемых формата
        ProxyPass "/api/nlog" "ws://127.0.0.1:1242/api/nlog"
        ProxyPass "/Api/NLog" "ws://127.0.0.1:1242/Api/NLog"

        ProxyPass "/" "http://127.0.0.1:1242/"
    </VirtualHost>
</IfModule>
```

### Могу ли я получить доступ к IPC через протокол HTTPS?

**Да**, вы можете достичь его двумя разными способами. Рекомендуемый способ - использовать для этого обратный прокси-сервер, где вы можете получить доступ к своему веб-серверу через https, как обычно, и подключиться через него с интерфейсом IPC ASF на той же машине. Таким образом ваш трафик полностью зашифрован и вам не нужно вносить никаких изменений в настройки IPC для поддержки такой конфигурации.

Второй способ включает в себя задание **[пользовательской конфигурации](#custom-configuration)** для интерфейса IPC ASF, где вы можете включить конечную точку https, и предоставить соответствующий сертификат непосредственно нашему серверу Kestrel. Этот способ рекомендуется только если у вас нет дргугого веб-сервера и вы не хотите добавлять его только ради ASF. В остальных случаях гораздо проще получить удовлетворительную конфигурацию используя механизм обратного прокси.

---

### При запуске IPC я получаю ошибку: `System.IO.IOException: Failed to bind to address, An attempt was made to access a socket in a way forbidden by its access permissions`

Эта ошибка указывает на то, что на вашей машине что-то ещё либо уже использует этот порт, либо зарезервировало его для использования в будущем. Это можете быть вы, если вы пытаетесь запустить второй экземпляр ASF на той же машине, но чаще всего это Windows, исключая порт `1242` из вашего использования, поэтому вам придется перенести ASF на другой порт. Для этого следуйте **[примеру конфигурации](#changing-default-port)** выше, и просто попытайтесь выбрать другой порт, например `12420`.

Конечно, вы также можете попробовать выяснить, что блокирует порт `1242` от использования ASF, и удалить это, но это обычно гораздо сложнее, чем просто попросить ASF использовать другой порт, так что мы здесь опустим дальнейшее обсуждение этого.

---

### Почему я получаю `403 Forbidden` ошибку когда не использую `IPCPassword`?

ASF включает в себя дополнительные меры безопасности, которые по умолчанию дают доступ к ASF API только `localhost` (ваш собственный компьютер) без `IPCPassword` в конфигурации. Это связано с тем, что использование `IPCPassword` должно быть **минимальной** мерой безопасности, установленной каждым, кто решит использовать интерфейс ASF в дальнейшем.

Это изменение было вызвано тем фактом, что огромное количество ASF-ов, размещенных во всем мире неосведомлёнными пользователями, брались под контроль злоумышленниками, обычно оставляя людей без учетных записей и без вещей на них. Теперь мы могли бы сказать "они могли бы прочитать эту страницу перед открытием фермы на весь мир", но вместо этого имеет больше смысла запрещать небезопасные настройки ASF по умолчанию, и требовать от пользователей действия, если они явно хотят разрешить это, о чём мы поговорим ниже.

В частности, вы можете переопределить наше решение, указав сети, которым вы доверяете, разрешить подключаться к ASF без `IPCPassword`, вы можете установить их в свойстве `KnownNetworks` в пользовательской настройке. Однако, если вы **действительно** не знаете, что делаете, и не понимаете риски, вам следует вместо этого использовать `IPCPassword` как объявление `KnownNetworks`, которое позволит всем из этих сетей безоговорочно получить доступ к ASF API. Мы сёрьезно, люди уже стреляли в ногу веря, что свои обратные прокси и правила iptables были безопасными, но это не так, `IPCPassword` - первый, а иногда и последний защитник, если вы решите отказаться от этого простого, но очень эффективного и безопасного механизма, вам придется винить только себя.