### author webber (web-ber12@yandex.ru)


### evoBabel - мультиязычные сайты для modx evolution (версия 0.2)

### Состав:
---------
* 1. Папка assets/snippets/evoBabel, содержащая основной сниппет snippet.evoBabel.php (создание языковых версий) и необходимый для работы файл evoBabel.class.php
* 2. Сниппет evoBabel - для вывода результатов работы в ТВ ресурса
* 3. Сниппет lang - для выдачи перевода на нужном языке на сайте
* 4. Плагин evoBabel - для правильной отдачи 404 страницы, синхронизации значений выбранных ТВ, чтения в сессию актуальных переводов, парсинга языковых плейсхолдеров вида [%Главная страница%] - вместо сниппета [[lang? &a=`Главная страница`]] в версии 0.1
* 5. Модуль lexicon для evoBabel (файлы и текст модуля)


### Возможности:
---------
* 1. создание неграниченного количества языковых версий сайта
при этом каждая версия хранится в своем дереве, что не ограничивает нас никак
т.е. структура сайта на разных языках может отличаться (в отличие от YAMS), нет проблем с плейсхолдерами, другими сниппетами и т.п.
* 2. ресурсы разных языковых версий связаны между собой - что позволяет при навигации переключаться на нужную языковую версию ресурса
* 3. Возможна синхронизация значений нужных ТВ параметров для разных версий ресурса (id нужных ТВ и id нужных шаблонов задается в настройках плагина evoBabel)
* 4. Переводы в отдельном модуле Lexicon без перезагрузки и сразу для всех языков. Есть фильтрация, возможность добавления и удаления нужного языка там же


### Установка:
---------
* 1. Скачиваем архив со сниппетом
* 2. Помещаем в папку assets/snippets/ соответствующие папки
* 3. Создаем шаблон "Язык" и запоминаем его id - мы будем присваивать его каждой корневой папке языка
* 4. Создаем в корне сайта один (или несколько) папок для языков - например RU,EN и т.п. с шаблоном "Язык". Алиасы данных папок будут алиасами языков (ru,en,fr и т.п.)
* 5. Пусть у нас основным языком будет RU, создаем в папке RU главную страницу (задаем ее стартовой для сайта в конфигурации MODx)
* 6. ID данной страницы вносим в поле description нашего ресурса RU, поле longtitle ресурса RU пишем слово "Русский" - там будет содержаться название нашего языка для вывода в переключалке языков

* 7 Создаем новый ТВ-параметр (назовем его relation, заголовок - Языковые версии ресурса) с типом ввода Custom Input и в поле возможные значения вписываем код @EVAL return $modx->runSnippet("evoBabel");
id именно этого TV и нужно вносить в настройки плагина и сниппета как id языка, используемого для хранения связей.
* 8 Прикрепляем данный ТВ ко всем шаблонам кроме Языка.

* 9. Создаем новый сниппет evoBabel и помещаем в него код из соответствующего файла в папке install
* 10. На вкладке Свойства данного сниппета в поле "Значение по умолчанию" копируем текст настроек  
  
&lang_template_id=id шаблона языка;text; &rel_tv_id=id TV для хранения языковых связей;text; 

и вносим в появившееся поле нужный нам id шаблона языка и id тв для хранения языковых связей
* 11. Создаем новый сниппет lang и копируем код из соответствующего файла в папке install (у него нет никаких доп.настроек)
* 12. Создаем новый плагин evoBabel, копируем код из соответствующего файла в папке install
* 13. В конфигурации плагина вставляем 

&lang_template_id=id шаблона языка;text; &rel_tv_id=id TV языковых связей;text; &synch_TV=ids TV для синхронизации;text; &synch_template=ids шаблонов для синхронизации;text; &currlang=язык по умолчанию;text;ru

 и указываем нужный шаблон и нужные id TV для синхронизации через запятую,
задаем нужный id шаблона языка, id TV для хранения языковых связей и название языка по умолчанию согласно алиасу корневой папки
* 14. Системные события для плагина OnPageNotFound, OnDocFormSave, OnBeforeEmptyTrash, OnEmptyTrash, OnWebPageInit
* 15. Создаем новый модуль evoBabelLexicon и помещаем в него код из соответствующего файла в папке install
* 16. Создаем плагин evoBabelPlaceholder (событие OnParseDocument) и помещаем в него код из папки install для соответствующего плагина
(данный плагин используется для установки языковых плейсхолдеров [%плейсхолдер%] вместо запуска сниппета lang и необязателен


Настройка админки завершена. Теперь в каждом ресурсе мы должны получить в поле Языковые версии ресурса мы должны получить название текущего языка и список доступных языков.
Если перевод уже создан  - то будет кнопка "перейти", если еще нет - кнопка "создать".


### Использование на сайте - тут вообще все просто
---------
* 1. В нужном месте шаблона размещаем плейсхолдеры [+activeLang+] - для отдельного вывода текущего языка и [+switchLang+] - для вывода переключалки языков
* 2. Сами переводы в шаблонах и чанках получаем с помощью сниппета lang
//использование в шаблонах чанках и т.п.
// [[lang? &a=`Главная страница`]] - выведет перевод слов Главная страница для текущего языка из его чанка
// использование в сниппетах 
// [[DocLister? &parents=`[[lang? &a=`Папка каталог`]]` ...другие параметры ..]]
// если вы установили плагин evoBabelPlaceholder то вместо вызова [[lang? &a=`Главная страница`]] можно использовать языковой плейсхолдер [%Главная страница%]


### Пример работы
---------
<a href="http://evoBabel.sitex.by">Пример работы</a>

### Предупреждения и ограничения:
---------
* 1. Модуль поставляется "как есть", перед установкой изучите инструкцию и сделайте дамп базы сайта
* 2. При запуске модуля создается новая таблица "префикс_lexicon" в базе данных.
* 3. Поле description ресурса используется для хранения языковых связей. Если вы используете данное поле для других целей - предварительно необходимо вынести информацию из него в другое поле либо в TV параметр














