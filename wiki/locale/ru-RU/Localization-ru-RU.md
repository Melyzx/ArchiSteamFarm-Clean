# Локализация

Перевод ASF осуществляется с помощью сервиса Crowdin, который даёт любому возможность участвовать в переводе ASF на любой язык мира. Для более подробного объяснения, как работает Crowdin, прочтите пожалуйста **[введение в Crowdin](https://support.crowdin.com/crowdin-intro)**.

Если вам интересно, как проходит локализация, вы можете посмотреть **[Ленту активности ASF в Crowdin](https://crowdin.com/project/archisteamfarm/activity_stream)**.

---

## Цели и задачи

Наша платформа поддерживает локализацию основной программы ASF, а также полную локализацию содержимого, поставляемого вместе с ним. В частности, это включает в себя ASF-WebConfigGenerator, ASF-ui, а также эту wiki. Всё это можно переводить через удобный интерфейс Crowdin.

---

## Регистрация

Если вы хотите помочь ASF как переводчик, рецензент или корректор, пожалуйста, зарегистрируйтесь на **[странице нашего проекта в Crowdin](https://crowdin.com/project/archisteamfarm)**. Регистрация очень простая и абсолютно бесплатна! После того, как вы зарегистрируетесь и войдёте, вы можете выбрать языки, с которыми вы хотите работать, а затем перейти к строкам ASF и помочь остальному сообществу с переводом ASF на все наиболее популярные языки!

---

### Перевод

Если выбранный вами язык ещё имеет непереведенные строки, вы можете взять их и начать работу над переводом. Мы постарались сделать перевод максимально гибким, поэтому многие строки включают в себя дополнительные переменные, которые ASF подставляет во время работы - они выглядят как число в фигурных скобках, например `{0}`. Это позволяет вам менять исходную формулировку строки в ASF, например перемещая пременную ASF в место, соответствующее языку перевода, без жёсткой привязки к контексту и формату. Это особенно важно для языков с письмом справа налево, таким как иврит.

Например, у вас может быть такая строка:

> У нас {0} игр для фарма.

Но из-за структуры вашего языка, следующее предложение будет иметь больше смысла:

> Количество игр для фарма равно {0}.

Или:

> {0} игр для фарма.

Эта гибкость специально дана вам чтобы вы могли перефразировать предложения ASF согласно вашему языку, и переместить число (или другую информацию) подставляемое ASF в подходящее для перевода место (а не переводить каждую часть отдельно). Это улучшает общее качество перевода.

---

### Рецензирование

Если строка уже была переведена кем-то, вы можете голосовать за неё. Голосование позволяет выбрать лучший вариант перевода, не привязываясь к первому предложенному - это позволяет сделать качество перевода ещё лучше. Вы можете голосовать за имеющиеся предложения, или предложить собственный перевод, который будет участвовать в том же процессе. В конце концов, окончательной будет выбрана версия набравшая большинство голосов, или же выбранная корректором этого языка, который персонально одобряет перевод (основываясь, в том числе, на количестве голосов).

**Вам не нужно одобрение корректора чтобы увидеть перевод строки в ASF**. Одобрение означает только что человек, которому мы доверяем, проверил содержимое и утвердил окончательную версию перевода. Совершенно нормально, если созданный сообществом перевод не был одобрен, если путём голосования был выбран лучший вариант. Если предложение переведено - это главное! Если же вы думаете, что текущий перевод плох, вы всегда можете голосовать за тот, который лучше, или сами предложить лучший вариант.

---

### Корректура

Лучше иметь единообразный перевод, даже если для этого может понадобиться ограничить свободу сообщества в рецензировании/голосовании за переводы описанном выше. Основная причина в том, что неправильный перевод (который не обязательно плох) может набрать так много голосов, что не удастся предложить лучший перевод, даже если у кого-то он есть.

Если у вас есть положительная история участия на Crowdin или других платформах по переводу, которую мы можем проверить чтобы считать вас надёжным, мы будем рады дать вам роль корректора в выбранном языке, чтобы вы могли утверждать перевод и делать его единообразным. Корректура не простая задача, особенно учитывая что ASF может иногда использовать достаточно "технический" язык или сленг, и могут возникать сложности перевода, но мы считаем что она нужна для идеального перевода. Поэтому если вы хотели бы помочь в качестве корректора какого-то языка, **[дайте нам знать](https://crowdin.com/messages/create/13177432/240376)**, но помните что вам нужно будет подкрепить своё предложение прошлым участием в локализациях (например, участием в локализации ASF на Crowdin, или в любом другом проекте). Мы можем также позволить более продвинутым пользователям проводить начальную корректуру, если мы знаем их лично или они способны взаимодействовать с остальным сообществом с целью локализации ASF наилучшим для данного языка образом.

Основное правила корректуры - не торопитесь, прислушивайтесь к мнению пользователей, взаимодействуйте с управляющим проекта, решайте возникшие проблемы, и следите за тем чтобы состояние перевода улучшалось, а не ухудшалось.

---

### Проблемы

Если вы видите проблему с переводом какой-то конкретной строки, например, вы не знаете как её перевести, или утверждённый перевод неверен, или вам нужно уточнить контекст, и тому подобное - напишите пожалуйста комментарий к этой строке и пометьте его как [X] Issue.

**Пожалуйста, избегайте ставить отметку "issue" если вы нуждаетесь в технической консультации или действии администрации**. Вы можете использовать комментарии для обсуждений связанных с переводом для данной строки, но сообщения о проблемах (с пометкой "issue") следует использовать только если вам нужны подробное техническое пояснение или коррекция со стороны администрации, и обычно в обработке таких сообщений будет участвовать кто-то, кто даже не говорит на языке, на который вы переводите, поэтому пожалуйста пишите по-английски если вы отмечаете комментарий как сообщение о проблеме (чтобы мы могли понять, в чём собственно проблема).

На данный момент поддерживается 4 вида сообщений о проблемах:
- General question - Общие вопросы. Для всего, что не попадает в одну из категорий ниже. В общем случае этого типа сообщений о проблемах **следует избегать**, поскольку если ваша проблема не попадает в одну из категорий, то с большой вероятностью это **не** проблема перевода. Однако, этот вариант доступен для всех прочих случаев.
- Current translation is wrong - Текущий перевод неверен. Этот тип сообщения должен использоваться **только** если перевод был уже утверждён корректором, и вы считаете что он неверен, например в нём присутствуют опечатки или у вас есть предложения как можно этот перевод улучшить. Этот тип сообщения никогда не следует использовать для переводов, выбранных путём голосования сообщества, поскольку в таком случае вам следует связаться с автором перевода и попросить его внести исправления, или просто проголосовать за лучший перевод, как описано в разделе про рецензирование. Мы отменим одобрение перевода и уведомим соответствующего корректора, отвечающего за язык, чтобы он учел ваш комментарий и проверил его снова.
- Lack of contextual information  - Недостаточно контекстной информации. Этот тип сообщения следует использовать, если вы не уверены, в каком случае ASF использует это сообщение, в каком контексте, или с какой целью. Этот тип должен использоваться только при разработке ASF, и означает что вам нужна техническая консультация, потому что вы не уверены, как следует перевести данную строку.
- Mistake in the source string - Ошибка в исходной строке. Этот тип должен использоваться только если вы полагаете что исходная (английская) строка неверна. Это довольно редкий случай, но так как не все из нас говорят по-английски, не стесняйтесь пользоваться этим типом сообщений есть у вас идеи по улучшению. В качестве альтернативы, поскольку это непосредственно связано с разработкой, вы можете использовать для этой цели наши **[выпуски GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/issues/new/choose)**, если хотите.

---

### Прогресс перевода

Для каждого языка есть две стадии завершенности - перевод и корректура.

Содержимое ASF считается **переведенным** на какой-то язык если прогресс перевода достигает 100%. На этом этапе каждая локализованная строка в ASF имеет правильное значение, и это здорово. Однако это не означает, что нет места улучшениям - голосование сообщества остаётся активным, и вы можете предлагать лучший перевод для уже переведенных частей, и голосовать за существующие. Обратите внимание, что даже языки, перевод на которые был завершён, могут снова упасть ниже 100% если мы внесём изменения в исходные строки или добавим новые в процессе разработки. Вы можете настроить уведомления crowdin если хотите получать e-mail когда такое происходит.

Часть языков может иметь назначенных корректоров, которые проверяют перевод и утверждают окончательную версию. Это последний этап после перевода, позволяющий дополнительно улучшить локализацию.

Переведенные языки будут включены в ASF **настолько быстро, насколько это возможно**, то есть для этого не нужно чтобы перевод был утверждён или даже закончен на 100%. В программу будут всегда включены строки, набравшие максимум голосов, если корректор не решил иначе (что случается редко). Таким образом вы увидите результаты ваших усилий уже в следующей версии ASF - наши автоматические системы проводят слияние переводов с Crowdin с репозиторием ASF ежедневно.

---

## Отсутствующие языки

По умолчанию в проекте ASF был открыт перевод только 30 наиболее используемых в мире языков. Если вы хотите добавить ещё одни (или местный диалект уже существующего), пожалуйста **[дайте нам знать](https://crowdin.com/messages/create/13177432/240376)** и мы добавим его как можно скорее. Мы не хотим открывать несколько сотен разных языков если никто не собирается их переводить, поэтому мы ограничились каким-то разумным числом. Не стесняйтесь связаться с нами если вы хотите перевести ASF на какой-то отсутствующий в списке язык, добавить новый язык очень просто. Просто убедитесь что у вас хватит воли и целеустремлённости переводить ASF на ваш язык, прежде чем обращаться к нам.

Чтобы увидеть полный список языков, на которыые может быть переведен ASF, **[нажмите здесь](https://developer.crowdin.com/language-codes)**.

---

## Множественное число

В каждом языке свои правила использования слов во множественном числе. Эти правила можно найти в **[CLDR](https://unicode-org.github.io/cldr-staging/charts/latest/supplemental/language_plural_rules.html)** (Общий Репозиторий Языковых Данных), в котором указано их число и конкретные языковые условия.

Мы прилагаем все усилия чтобы предоставить возможность гибкой локализации, и там где возможно это включает в себя правила для множественного числа. Например, мы переводим следующую строку на польский язык:

> Released {PLURAL:n|{n} month|{n} months} ago

Ключевое слово `PLURAL` обрабатывается особым образом, позволяя вам указать все формы множественного числа, существующие в вашем языке. Если взглянуть на CLDR, мы видим что в английском языке только 2 формы для количественных числительных (cardinal) - "one" и "other". И как вы видите выше, обе формы заданы - `{n} month` и `{n} months`.

Однако, в польском языке таких форм уже 4 - "one", "few", "many" и "other". Это значит что для полноты перевода мы должны задать их все. Наши инструменты локализации достаточно умны чтобы выбрать нужную форму множественного числа на основе правил языка, поэтому вам нужно только задать их все в переводе:

> Wydany {PLURAL:n|{n} miesiąc|{n} miesiące|{n} miesięcy|{n} miesiąca} temu

Таким образом мы задаём все 4 формы множественного числа для польского языка, и поскольку наша библиотека для локализации уже знает точные правила, она будет использовать правильную форму для подставляемого в `{n}` числа.

Не обязательно указывать все формы множественного числа, используемые в вашем языке. Если какой-то формы не хватает, наша библиотека локализации будет использовать вместо неё последнюю из заданных. Стоит задавать все формы множественного числа, используемые в вашем языка, но в некоторых случаях формы множественного числа могут совпадать, и тогда нет смысла их повторять. В примере выше это было необходимо, поскольку форма "other" в польском языке "miesiąca", а не "miesięcy", как в форме "many".

---

## Вики

Платформа Crowdin также позволяет нам локализовать даже саму эту wiki. Это очень мощный инструмент, поскольку это позволяет перевести целиком всю документацию ASF на ваш родной язык, полностью решая задачу локализации ASF. В совокупности с переводом программы и всех её компонентов, это делает локализацию завершённой.

Wiki в этом плане несколько особенная, поскольку это онлайн помощь в которой не нужно жёстко придерживаться исходных предложений. Это означает, что вы можете писать максимально естественно, передавая исходный смысл - не обязательно придерживаясь оригинальных строк, использованных формулировок или пунктуации. Не бойтесь переписывать строки во что-то более естественное для вашего языка, главное сохранить общее направление и помощь содержащуюся в предложении.

---

### Глобальные ссылки

Платформа Crowdin также позволяет адаптировать ссылки в исходном тексте, чтобы они указывали на новые (локализованные) страницы.

В ASF есть ссылки почти на каждой странице для облегчения навигации, а также боковую панель справа. Хорошая новость в том, что всё это можно редактировать, "исправляя" ссылки, чтобы они указывали на переведенные на ваш язык страницы. При этом следует соблюдать осторожность, но это возможно.

Например, **[домашняя страница](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home)** ASF включает в себя следующий текст:

> Если вы здесь впервые, рекомендуем начать с инструкции по **[установке](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up-ru-RU)**.

Которая исходно написана как:

```markdown
If you're a new user, we recommend starting with **[setting up](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up)** guide.
```

В системе Crowdin, первое что вам надо сделать это зайти в настройки и убедиться, что у вас отображаются теги HTML ("HTML Tags Displaying" - "Show"). Это очень важно, если вы собираетесь заниматься локализацией wiki.

---

![Crowdin](https://i.imgur.com/YqAxiZ4.png)

---

Теперь, в процессе перевода на Crowdin, в зависимости от форматирования вы можете встретить ссылки в виде:

* Строки для перевода с тегами HTML (большинство строк, где только часть предложения является ссылкой)
* Отдельной строки для перевода, со ссылкой приведенной в разделе `Hidden texts` -> `Link addresses` (Скрытый текст -> Адреса ссылок) (в редких случаях, когда вся строка является ссылкой, в основном встречается в боковой панели)

В примере выше представлен первый случай (поскольку только слова "setting up" это ссылка), поэтому на Crowdin мы увидим эту строку как:

---

![Crowdin 2](https://i.imgur.com/Li5RzS3.png)

---

Независимо от случая, сначала нужно скопировать содержимое исходной строки и перевести строку как обычно, оставив весь HTML (если он присутствует) без изменений. Вот пример перевода для Польского языка:

---

![Crowdin 3](https://i.imgur.com/NpKwfka.png)

---

Теперь, если это обычная ссылка, ведущая куда-то за пределы wiki (например, на последнюю версию ASF), вы можете её оставить как есть. Можно просто сохранить строку и двигаться дальше.

Однако, если это ссылка на другую страницу **внутри** wiki, как в примере выше, вы можете исправить её чтобы она вела на новую (локализованную) страницу. Вы можете это сделать аккуратно добавив `-locale` к URL в теге `<a>`, как показано ниже:

---

![Crowdin 4](https://i.imgur.com/TL8uwmb.png)

---

Будьте предельно осторожны делая это, убедитесь что URL который вы создали действительно существует, если вы ошибётесь - ссылка перестанет работать. Если вы всё сделали правильно, у вас получиться полностью работоспособная ссылка, указывающая на переведенную страницу (в нашем примере - `Setting-up-pl-PL`).

После того как вы проделали все вышеописанные шаги, перевод будет штатно конвертирован из созданного нами HTML назад в markdown:

```markdown
Jeśli jesteś nowym użytkownikiem, zalecamy rozpoczęcie od korzystania z **[przewodnika po konfiguracji](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up-pl-PL)**.
```

И, наконец, в текст в wiki:

> Jeśli jesteś nowym użytkownikiem, zalecamy rozpoczęcie od korzystania z **[przewodnika po konfiguracji](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up-pl-PL)**.

Если в тексте отсутствуют HTML-теги (второй случай), всё ещё проще - вам нужно просто перейти в раздел `Hidden texts` -> `Link addresses` (Скрытый текст -> Адреса ссылок).

---

![Crowdin 5](https://i.imgur.com/ZmgavCM.png)

---

Там вы можете легко исправить ссылки чтобы они указывали на новое место, вообще не заботясь о HTML:

---

![Crowdin 6](https://i.imgur.com/maG7kSm.png)

---

### Локальные ссылки

На страницах wiki вы можете также встретить локальные ссылки, которые указывают на определённый раздел документа. Эти ссылки включают в себя символ `#`, который указывает веб-браузеру что он должен перейти к определённому разделу документа.

Эти ссылки - особый случай, поскольку они зависят от заголовков раздела в текущем документе. Если для глобальных URL у нас есть общий принцип добавление `-locale` в конец URL, и это работает везде, то названия разделов могут быть переведены вами и другими людьми, так что придётся проверять что ссылка ведёт в правильное место.

Например, вы можете встретить ссылку `#introduction` в разделе **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#introduction)**:

---

![Crowdin 7](https://i.imgur.com/EEHSPtK.png)

---

Если для польского языка "Introduction" перевели как "Wprowadzenie", нам придётся изменить эту ссылку, иначе она перестанет работать.

---

![Crowdin 8](https://i.imgur.com/JMJegO7.png)

---

Таким образом локальная ссылка продолжит работать, потому что теперь она указывает на правильный раздел статьи. Ссылки в HTML тегах можно редактировать аналогичным образом.

---

### Блоки кода

Будьте очень осторожны при переводе предложений, содержащих блоки `<code></code>`. Блоки кода используются чтобы показать фиксированные имена в коде ASF, или иные термины, не подлежащие переводу. Например:

> This is especially useful if you have a lot of keys to redeem and you're guaranteed to hit <code>RateLimited</code> status before you're done with your entire batch.

Как видите, слово `RateLimited` заключено здесь в блок кода, и описывает внутреннее состояние в коде ASF - его не нужно переводить. Аналогичным образом не следует переводить и другие блоки кода, такие как названия свойств конфигов (например, `TradingPreferences`), членов перечислений (например, `Stable` и `PreRelease` опции `UpdateChannel`) и т.п.

Однако то, что что эти слова не нужно переводить, не означает что вы не можете добавить соответствующий перевод рядом с ними, например в скобках.

> Эта функция особенно полезна в связке с множеством ключей, которые нужно активировать, когда Вы гарантировано получите статус <code>RateLimited</code> (слишком много попыток активации) до того, как вы закончите активацию всего набора.

Как видите, мы добавили пояснение, "слишком много попыток активации" рядом с `RateLimited`, чтобы перевести значение статуса, и в то же время сохранить используемый в ASF текст, который будет выводиться при использовании программы. Таким же образом вы можете переводить/пояснять слова и целые фразы и предложения в похожих случаях.

Если вы считаете, что в блок кода включено что-то неподходящее, или что текст не заключённый в блок кода должен быть в нём - не стесняйтесь спросить нас на crowdin создав комментарий с отметкой **[issue](#Проблемы)**. Это также служит практическим примером использования локальной ссылки.

---

## Доска почёта

Мы хотели бы выразить нашу бесконечную благодарность людям, которые потратили значительную часть личного времени в стремлении сделать локализацию ASF лучше. Их усилия невероятны, и вы можете наслаждаться полным переводом, включая wiki, в основном благодаря им. Как знак признательности, всем перечисленным здесь людям предоставляется бесплатный доступ к **[`MatchActively`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin#matchactively)** после **[запроса](https://crowdin.com/messages/create/13177432/240376)**.

| Участник                                                   | Языки                   |
| ---------------------------------------------------------- | ----------------------- |
| **[Astaroth](https://crowdin.com/profile/astaroth2012)**   | LOLCAT, Испанский       |
| **[Dead_Sam](https://crowdin.com/profile/Dead_Sam)**       | Португальский (BR)      |
| **[deluxghost](https://crowdin.com/profile/deluxghost)**   | Китайский (CN)          |
| **[DragonTaki](https://crowdin.com/profile/dragontaki)**   | Китайский (Тайваньский) |
| **[LittleFreak](https://crowdin.com/profile/littlefreak)** | Немецкий                |
| **[Ryzhehvost](https://crowdin.com/profile/Ryzhehvost)**   | Русский, Украинский     |
| **[MrBurrBurr](https://crowdin.com/profile/MrBurrBurr)**   | LOLCAT, Немецкий        |
| **[XinxingChen](https://crowdin.com/profile/XinxingChen)** | Китайский (HK)          |

Спасибо вам всем за улучшение качества локализации ASF!