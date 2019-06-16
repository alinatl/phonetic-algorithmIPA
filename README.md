# phonetic-algorithmIPA

```
pip install phonetic-algorithmIPA
```

Фонетический алгоритм - это алгоритм сравнения слов на основе их произношения. Идеей данного алгоритма является приведение строк, записанных с помощью графем, к фонетическому представлению и их сравнение с помощью различных метрик поиска расстояния.  Универсальность алгоритма достигается за счет использования Международного Фонетического Алфавита (МФА). Алгоритм представляет каждый символ транскрипции, записанной с помощью МФА, в виде вектора, описывающего звук с точки зрения фонетических признаков. В качестве такого описания нами была выбрана фонологическая система Хомского и Халле. Расстояние между транскрипциями рассчитывается с помощью модифицированного варианта расстояния Дамерау-Левенштейна, где стоимость замены равна расстоянию между векторами символов транскрипции.
Пакет работает на стандартной библиотеке Python.

Note: пакет еще находится в разработке, следите за версиями. 

Current version: 0.2.2<br>
Сайт: http://stonewebsite.pythonanywhere.com/


## Обозначение ударения 
#### V1…Vn_n - основное ударение (mou_2se)
где V1…Vn - это гласные, находящиеся под ударением, «_» - знак основного ударения, а n - это количество соответствующих ударных гласных.
#### V1…Vn=n - побочное ударение 
где V1…Vn - это гласные, находящиеся под ударением, «=» - знак побочного ударения, а n - это количество ударных гласных.

Обозначения, принятые в МФА не учитываются!!!
«ˈ» - для обозначения основного ударения (mother [ˈmʌðə])
«ˌ» - для обозначения побочного ударения (pronunciation [prəˌnʌnsɪˈeɪʃn])

#### Учитываемые диакритические и супрасегментные символы 
ⁿ, ʲ, ʷ, ʰ, ˤ, ʼ, ẽ, s̬, n̥ / ŋ̊, e̯, m̩, e̘, e̙, u̟, e̠, d͡ʑ/d͜ʑ

#### Запись для дифтонга и аффрикаты

1) Аффриката - [t͡ʃ] и [t͜ʃ]
2) Дифтонги: <br>
[uɪ̯]  или [u̯ɪ] - с использованием неслогового диакритического знака; <br>
[u͡ɪ] - c помощью дуги, использующийся для обозначения аффрикат


# Getting started
Пакет включает в себя модуль phonetic_distance. Чтобы приступить к работе, инициализируйте класс PhoneticAlgorithmIPA() из модуля ipa_distances.

```
>>> from phonetic_algorithmIPA import ipa_distances
>>> ipa = ipa_distances.PhoneticAlgorithmIPA()
```

Класс имеет следующий функционал:


## Описание пакета

### phonetic_distance(path, delimiter=';', typ='Non LS', irrelevant_features=[], normalize=False)
Функция осуществляет расчет расстояний между транскприпциями, записанными с помощью МФА. Аргументы: 
1) path - путь к csv-файлу со строками, между которыми должны быть посчитаны расстояния (2 колонки без названий)

2) delimiter - разделитель для файла

3) typ - тип задачи: LS или Non LS

LS - Тип задачи, когда необходимо не учитывать признаки, которые являются нерелевантными для конкретной исследовательской задачи. Определение нерелевантных признаков может производится двумя способами: 

1) Пользователь сам указывает те признаки, которые не нужно учитывать (агрумент irrelevant_features).
2) Программа анализирует звуки, между которыми должны быть рассчитаны расстояния, определяет признаки, которые не встретились ни у одного элемента и удаляет их из признаковой таблицы.

В случае Non LS никаких изменений в данных не происходит, расстояния считаются с учетом всех значений признаков. 

4) normalize - типа нормализации: <br>
False - Без нормализации;<br>
'mean' - Расчет расстояний с нормализацией по среднему значению длин слов;<br>
'max' - Расчет расстояний с нормализацией по самому длинному слову;<br>
'min' - Расчет расстояний с нормализацией по длине самого короткого слова;<br>
 
```
>>> with open('test2.csv', 'r', encoding='utf-8') as f:
       arr = f.read()
>>> arr
‘haʊ_2s;maʊ_2s’

>>> c = ipa.phonetic_distance(‘test2.csv’)
[[0.36]]
```

### phonetic_transformer(data_path, rules_path, delimiter=';', typ='Non LS', irrelevant_features=[], normalize=False, cons_u=[], vows_u=[])
Функция осуществляет автоматическую трансформацию строк в фонетические представления. Функция очищает входные строки от всех знаков препинания и пробелов, поэтому рекоминдуется сравнивать слова, а не предложения, если для преобразований позиции в конце и начале слова. 

1) data_path - путь к csv-файлу со строками, между которыми должны быть посчитаны расстояния (2 колонки без названий).

2) rules_path -  путь к таблице соответствий графем и звуков и контекстов, в которых данные соответствия реализуются, и списки гласных (vows_u) и согласных графем для конкретного языка (cons_u). 

3) delimiter - разделитель для файлов. Он должен быть одинаковым для обоих файлов.

4) typ - тип задачи: LS или Non LS

LS - Тип задачи, когда необходимо не учитывать признаки, которые являются нерелевантными для конкретной исследовательской задачи. Определение нерелевантных признаков может производится двумя способами: 

1) Пользователь сам указывает те признаки, которые не нужно учитывать (агрумент irrelevant_features).
2) Программа анализирует звуки, между которыми должны быть рассчитаны расстояния, определяет признаки, которые не встретились ни у одного элемента и удаляет их из признаковой таблицы.

В случае Non LS никаких изменений в данных не происходит, расстояния считаются с учетом всех значений признаков. 

5) normalize - типа нормализации: <br>
False - Без нормализации;<br>
mean - Расчет расстояний с нормализацией по среднему значению длин слов;<br>
max - Расчет расстояний с нормализацией по самому длинному слову;<br>
min - Расчет расстояний с нормализацией по длине самого короткого слова;<br>
 
```
>>> with open('test3.csv', 'r', encoding='utf-8') as f:
       arr = f.read()
>>> arr
‘hou_2se;mou_2se’

>>> with open('rules.csv', 'r', encoding='utf-8') as f:
       arr_r = f.read()
>>> arr_r
‘m;m\no;a;_u\nu;ʊ;o_\ns;s\ne;;_#\nh;h;'

>>> c = ipa.phonetic_distance(‘test3.csv’, 'rules.csv')
[[0.36]]
```

###### Типы контектов для правил:

Допустим есть графема А и ее МФА эквивалент В

1) X_Y - А реализуется как В в позиции между Х и Y<br>
2) #_  - А реализуется как В в позиции начала слова<br>
3) _# - А реализуется как В в позиции конца слова<br>
4) _Y - А реализуется как В  в позиции перед Y<br>
5) X_ - А реализуется как В в позиции после X<br>
6) пустая строчка - А реализуется как В в любой позиции<br>
7) X_Y - А выпадает в позиции между Х и Y<br>
8) _* - А реализуется как В в позиции перед ударным гласным<br>
9) @_ - А реализуется как В в позиции перед гласным<br>
10) &_& - А реализуется как В в позиции между двумя согласными<br>


### add_columns(dict)
Функция позволяет добавить новые признаки в артикуляционную систему. На вход программе подается словарь, в котором ключом является название признака, а значением - список значений длиной, равной количеству звуков в системе. Признаки могут принимать значения «+», «-» и «0». Например, {‘sonorant’: [+, -, 0]}.
 
### add_rows(dict)
add_rows позволяет добавить символ МФА в артикуляционную систему. На вход программе подается словарь, в котором ключом является символ, а значением - список значений длиной, равной количеству признаков в системе. Признаки могут принимать значения «+», «-» и «0». Например, {‘a’: [+, -, 0]}.

### add_diacritics(dict)
add_diacritics позволяет добавить диакритический или супрасегментный символ в систему. На вход программе подается словарь, в котором ключом является символ, а значением - список, состоящий из позиции символа относительно звука и словаря признаков, которые он меняет. Позиция может принимать значения post, pre, between. Позицией для диакритик, которые располагаются над или под звуками, является post. Например, {‘ⁿ’: [‘post’, {‘nasal’: ‘+’}]}.

##### !!!
Диакритиками не могут быть: "_", "=", "@", "#" и цифры

### change_feature_table(dict)
Функция позволяет изменить значения существующих в системе признаков следующими тремя способами: 

{sound : {feature: value}} - изменение значения определенного признака у определенных звуков; 

{sound: [values]} - изменение всех значений определенного звука;

{feature: [values]} - изменение всех значений определенного признака.

### default_settings()
default_settings обеспечивает возврат к первоначальным артикуляционной системе признаков.

# Дополнительно
В пакете также есть функции: transcription_splitter, sound_dist, lev_distance.

###  transcription_splitter(word, diacrit, vows, cons)

type(word) - str <br>
type(diacrit) - dict<br>
type(vows) - list <br>
type(cons) - list <br>

1) Функция осуществляет автоматическое разделение строки, записанной с помощью МФА, на составляющие и их представление с помощью артикуляционных признаков.
2) Функция получает на вход саму строку, списки гласных и согласных, которые могут встретиться в транскрипции, и информацию о диакритиках в следующем виде:
```
{'ⁿ': ['post', {'nasal': '+'}]}
```
где 'ⁿ' - диакритический символ, 'post' - позиция символа относительно звука, к которому он относится (могут быть: pre, post, between), {'nasal': '+'} - словарь признаков, которые диакритический знак меняет в векторе признаков звука. 

Можно использовать дефолтные списки, для этого используйте: 
```
>>> diacrit = ipa_distances.diacrit
>>> cons = ipa_distances.cons
>>> vows = ipa_distances.vows
>>> vows
['a', 'e', 'i', 'o', 'u', 'y', 'ã', 'ø', 'œ', 'ɑ', 'ɒ', 'ɔ', 'ɘ', 'ə', 'ɛ', 'ɞ', 'ɤ', 'ɨ', 'ɪ', 'ɯ', 'ɵ', 'ɶ', 'ʉ', 'ʊ', 'ʏ']
```
 
3) Результатом работы функции является массив соответствующих векторов. Если в строке встретились символы, отсутствующие в наборе используемых программой символов, будет вызвана ошибка.
 ```
>>> ipa = PhoneticAlgorithmIPA()
>>> ipa.transcription_splitter(‘m’, diacrit, vows, cons)
[[‘-’, ‘0’, ‘0’, ‘-’, ‘+’, ‘+’, ‘-’, ‘0’, ‘-’, ‘-’, ‘-’, ‘+’, ‘+’, ‘-’, ‘-’, ‘+’, ‘-’, ‘-’, ‘-’, ‘0’, ‘0’, ‘0’, ‘-’, ‘-’, ‘0’, ‘0’, ‘0’, ‘0’, ‘0’, ‘-’, ‘0’, ‘0’, ‘-’, ‘-’, ‘-’, ‘-’]]
```


###  sound_dist(vector1, vector2)
Функция рассчитывает модифицированное расстояние между двумя векторными представлениями по формуле:

type(vector1) и  type(vector2) == tuple

1 - Similarities/ Common + (Uncommon  2)

где Similarities - число релевантных признаков, в которых два вектора имеют одинаковые значения, Common - число всех релевантных признаков для обоих звуков, Uncommon в свою очередь равно сумме нерелевантных признаков.
```
>>> c = ipa.transcription_splitter(‘m’, diacrit, vows, cons)
>>> b = ipa.transcription_splitter(‘n’, diacrit, vows, cons)
>>> ipa.sound_dist(c[0], b[0])
0.27586206896551724
```

###  lev_distance(list1, list2)
Функция вычисляет расстояние по модифицированному алгоритму Дамерау-Левенштейна между двумя массивами артикуляционных векторов. Стоимость замены рассчитывается с помощью функции sound_dist как расстояние между двумя векторами звуков. 

list1 и list2 - массивы картежей (tuple)
```
>>> c = ipa.transcription_splitter(‘haʊ_2s’, diacrit, vows, cons)
>>> b = ipa.transcription_splitter(‘maʊ_2s’, diacrit, vows, cons)
>>> ipa.lev_distance(c, b)
0.36
```
