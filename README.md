# Описание решения кейса

Данный репозиторий содержит решение тестового задания от компании ИНЛАЙН.

Полное решение можно также запустить в <b>Google Collab</b>: <a href="https://colab.research.google.com/drive/1MA8PLnxTnJY_aQ8xh-HGBqvTGTNfdb6T#scrollTo=pgIkPO2l2zs7&uniqifier=1">Inline_test.ipynb</a>

## Описание задачи

Создайте модель, обрабатывающую фрагмент текста и определяющую
какой вид продукции в нём содержится.

Выполненное задание пришлите ссылкой на Google Collab.
Датасет поделите на 80% / 20% - обучающая/тестовая выборки.

Используйте библиотеку PyTorch

Датасет: <a href='https://axe.inline-ltd.ru/data/meatinfo.csv'>meatinfo.csv</a>

Виды продукции (брать только виды продукции, для которых в датасете есть не менее 500 примеров):

* Баранина
* Ягнятина
* Индейка
* Говядина
* Свинина
* Кура
* Цыплено
* Гусь
* Буйволятина
* Оленина
* Конина
* Телятина
* Кролик
* Утка
* Куропатка
* Перепел
* Глухарь
* Страус
* Заяц
* Кенгуру
* Изюбр
* Кабан
* Коза
* Косуля
* Лось
* Марал
* Медвежатина
* Бобер
* Цесарка
* Нутрия
* Рябчик
* Тетерев
* Фазан
* Як


<b>Примеры входных текстов и видов продукции:</b>

Набор для бульона свиной Набор для бульона свиной, в наличии, 76р/кг. -> Свинина


Мясо премиум Предлагаем котлетное мясо мраморной говядины. -> Говядина

спинка цб -> Цыпленок.


<b> Проверьте вашу модель на образцах</b>

Говядина блочная 2 сорт в наличии ООО "АгроСоюз" реализует блочную говядину 2 сорт (80/20)
Свободный объем 8 тонн Самовывоз или доставка. Все подробности по телефону.

Куриная разделка Продам кур и куриную разделку гост и халяль по хорошей цене .Тел:

Говяжью мукозу Продам говяжью мукозу в охл и замороженном виде. Есть объем.

<br>
<hr>
<br>

## Разработка решения

Одним из условий реализации модели, было использовать библиотеку `PyTorch`. 

Далее необходимо было определить, как обрабатывать текстовый столбец. На начальном этапе 3 значения из столбца с целевым признаком `mtype` были названы с ошибками. Например <b>"свиниеа"</b> вместо <b>"свинина"</b>.

После приведения значений столбца `mtype` в правильный вид, необходимо было по условию задачи оставить только те признаки, количество которых не менее 500. В результате осталось 6 классов, которые закодировали с помощью `LabelEncoder`.

Обработка тестов проходила следующие этапы.

Лемматизация -> Очистка тестов от любых символов, кроме буквенных записей -> Векторизация текста -> модель

<b>Важное замечание.</b> Данный датасет имеет очень сильный дисбаланс классов

| Название      |    Кол-во     |
| ------------- | ------------- |
| говядина      |    8424       |
| свинина       |    3054       |
| кура          |    1571       |
| индейка       |    1339       |
| баранина      |    1116       |
| цыпленок      |    944        |


## Архитектура 
Для данной задачи была построена 3-х слойная полносвязная нейронная сеть. 

## Предварительные результаты

Данную выборку необходимо было поделить на тренировочную и тестовую в соотношении 80/20. Далее тренировочную было решено ещё разделить на валидационную в соотношении 90/10

Параметры обучения:

<ul>
    <li>epoch = 15</li>
    <li>batch = 16</li>
    <li>drop = 0.2</li>
</ul>  

В результате, модель имеет значение метрики accuracy = 0.94, F1 weighted avg = 0.94. Что в целом является неплохим результатом, но как было сказано выше, данные имеют высокий дисбаланс классов и из-за этого модель плохо распознает классы, которые имеют мало обучающих данных. Расчёт метрик представлен на рисунке 1.
<br>
<img src = "https://github.com/YarickVodila/Inlain_NLP/blob/master/classification_report.png">
<br>
Также стоит отметить, что пробовалась борьба с дисбалансом классов алгоритмом `SMOTE` который реализует метод `upsampling`. Но в конечном итоге это не дало никаких результатов. 