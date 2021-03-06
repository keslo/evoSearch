### author webber (web-ber12@yandex.ru)

### набор evoSearch - индексирование и поиск с учетом морфологии (версия 0.1)

### DONATE
---------
если считаете данный продукт полезным и хотите отблагодарить автора материально,
либо просто пожертвовать немного средств на развитие проекта - 
можете сделать это на любой удобный Вам электронный кошелек<br><br>
<strong>Яндекс.Деньги</strong> 410011544038803<br>
<strong>Webmoney WMR:</strong> R133161482227<br>
<strong>Webmoney WMZ:</strong> Z202420836069<br><br>
с необязательной пометкой от кого и за что именно


### содержит:
- плагин evoSearch - для индексирования (индексируются pagetitle,longtitle,description,introtext,content и указанные явно в плагине ТВ (по именам через запятую)). При использовании для построения списка возможных значений TV сниппета multiParams (из комплекта eFilter) индексируются не id ресурсов, а их заголовки.
- сниппет evoSearch - для вывода результатов поиска (использует DocLister, но может работать в режиме &action=`ids`. В этом случае лишь генерируется и возвращается список id ресурсов, подходящих под условия поиска, которые могут использоваться в любом другом сниппете (параллельно устанавливается плейсхолдер evoSearchIDs - массив тех же id)).


### обязательные дополнительные компоненты:
- сниппет DocLister

### установка
устанавливаем плагин и сниппет. Для плагина событие onDocFormSave, 
параметры  - &offset=Первая строка переиндексации;text;0 &rowsperonce=Строк за сеанс индексировать;text;1 &reindex=Переиндексировать все;text;0 &excludeTmpls=Исключить шаблоны;text; &excludeIDs=Исключить ID ресурсов;text; &TvNames=Имена TV для поиска;text; &unpublished=Индексировать неопубликованные;text;0 &deleted=Индексировать удаленные;text;0 &dicts=Использовать словари;text;rus,eng

 * до первого запуска сниппета на фронтэнде сайта необходимо запустить индексацию (сохранить любой ресурс в админке)
 * создание необходимых полей content_with_tv и content_with_tv_index, а также нужных индексов производится автоматически (!!!) при первом запуске индексации
 * индексация запускается сохранением любого ресурса (вызовом события onDocFormSave)
 
при первом запуске или необходимости переиндексации необходимо выставить параметр "Переиндексировать все" = 1, начальные строки и количество строк за сеанс устанавливаются в зависимости от 
возможностей вашего хостинга (например 0 и 10 000 соответственно - проиндексирует строки с 0 в количестве 10 000 штук в БД
необходимо открыть и пересохранить любой документ для создания события onDocFormSave

для последующей работы установите "Переиндексировать все" = 0, "Строк за сеанс индексировать" = 1 
при этом происходит переиндксация только того документа, который сохраняется

### пример вызова
    для вывода результатов [!evoSearch? &tpl=`evoSearch`!], 

### ПАРАМЕТРЫ СНИППЕТА
параметры DocLister - т.к. это обертка для DL, то он понимает все параметры DocLister - paginate, display, addWhereList (ограничения на вывод в результатах поиска шаблонов, id и всего чего угодно)
 + &action = `ids` - возвращает список найденных ids, которые можно подставить в другой сниппет. По-умолчанию - отрабатывает полностью с выводом результатов
 + &noResult = `Ничего не найдено`- шаблон строки, которая выводится при отсутствии результата поиска (по-умолчанию "По запросу <u>[+stat_request+]</u> ничего не найдено. Смягчите условия поиска")
 + &extract = `0` - отключить экстрактор формирует нужную часть текста с подсветкой из результатов поиска (плейсхолдер [+extract+] в чанке вывода результатов DocLister) - по умолчанию 1 (не извлекать)
 + &maxlength = `300` - максимальная длина извлекаемой части текста в резуьлтатах поиска (по умолчанию 350)
 + &show_stat = `0` - отключаем показ статистики "найдено....показано...с...по...". По умолчанию - 1 - показ включен
 + &statTpl = `` - шаблон показа статистики (по умолчанию - <div class="evoSearch_info">По запросу <b>[+stat_request+]</b> найдено всего <b>[+stat_total+]</b>. Показано <b>[+stat_display+]</b>, c [+stat_from+] по [+stat_to+]</div> ), где
              [+stat_request+] - запрос из строки $_GET['search']
              [+stat_total+] - найдено всего
              [+stat_display+] - показано на текущей странице с [+stat_from+] по [+stat_to+] 
 + &rel = `1` - релевантность поиска, по умолчанию 0.01, чем выше цифра - тем более релевантные результаты и тем их меньше
 + &search_field = `search2` - поле $_GET для запроса (по-умолчанию запрос ищется в $_GET['search'])
 + &minlength = `3` - минимальная длина слова, которое будет участвовать в полнотекстовом поиске (по-умолчанию 2)

подсветка найденных слов в pagetitle и extract в результатах поиска осуществляется тегом <span class="evoSearch_highlight"> - т.е. возможна ее стилизация через css-файлы
 
### информация. 
    Т.к. при полнотекстовом поиске MySQL без дополнительных настроект обрабатываются только слова не короче 4 символов, для улучшения результатов поиска используется дополнительный поиск 
    средствами фильтров DocLister (что улучшает результаты, особенно при их отстутствии в результате обычного поиска).
    Совместим с DocLister версии 1.4.1 и ниже, 1.4.8 и выше. В версиях 1.4.5, 1.4.6, 1.4.7 встречается некорректный сброс строки $_GET, из-за чего некорректно срабатывает обработка пустых результатов


### Сотрудничество:
---------
По вопросам сотрудничества обращайтесь на электронный ящик web-ber12@yandex.ru