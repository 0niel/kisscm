# Конфигурационные языки

## Инфраструктура как код

Параметры конфигурации ПО обычно хранятся в специальном виде, доступном для редактирования пользователями и другими программами. Для хранения может использоваться специальная база данных, но наиболее распространенным вариантом хранения настроек программы являются конфигурационные файлы, содержимое которых написано на одном из конфигурационных языков. Многие программные инструменты конфигурирования ПО используют конфигурационные языки. 

**Конфигурационный язык** – язык описания параметров конфигурации ПО. **Конфигурационный файл** представлен на конфигурационном языке и предназначен для редактирования пользователями и другими программами.

К преимуществам конфигурационных языков относят:

* удобный человеко-читаемый синтаксис, который дает возможность внести изменения в конфигурацию быстрее, чем при использовании GUI-инструментов;
* удобный машинно-читаемый синтаксис для автоматической обработки параметров конфигурационного файла внешними программами;
* ограниченные средства программируемости, позволяющие автоматизировать рутинные действия.

В качестве конфигурационных языков могут использоваться:

* непрограммируемые форматы описания конфигурации,
* языки программирования общего назначения,
* программируемые предметно-ориентированные языки для задач конфигурационного управления.

Современный подход «инфраструктура как код» (infrastructure as code) предполагает широкое использование в конфигурационном управлении конфигурационных языков.

## Формальные языки и грамматики

Компьютерные языки являются подмножеством формальных языков.

**Формальный язык** состоит из множества строк конечной длины, называемых словами. Слова состоят из символов, а символы содержатся в алфавите – конечном множестве символов.

В этом определении предполагается, что для установления принадлежности некоторой строки формальному языку необходимо сначала перечислить все возможные строки на этом языке, что, разумеется, является непрактичным подходом.

Рассмотрим иной способ определения языка. **Регулярный язык**, являющийся теоретической основой регулярных выражений, может быть описан следующим образом:

1. Пустая строка ε или одиночный символ из алфавита являются регулярным языком.
1. Если $A$ это регулярный язык, то $A^{*}$ также является регулярным языком. Операция $A^{*}$ обозначает повторение $R$ 0 или более раз.
1. Если $A$ и $B$ – регулярные языки, то $A | B$ тоже является регулярным языком. Операция $A | B$ обозначает выбор из $A$ или $B$.
1. Если $A$ и $B$ – регулярные языки, то $A B$ тоже является регулярным языком. Операция $A B$ обозначает последовательность: $A$, за которым следует $B$.

Введенных базовых операций достаточно, чтобы определить дополнительные конструкции, использующиеся в регулярных выражениях. Например, конструкцию `A+` можно определить, как `AA*`, а конструкция `A?` заменяется конструкцией `A|ε`.

Мощности регулярных языков, тем не менее, не хватает для описания многих важных компьютерных языков. В частности, с помощью регулярного языка невозможно описать конструкции произвольной вложенности. Это, в том числе, касается разбора и HTML-кода, и даже простого выражения, в котором присутствуют лишь правильно расставленные скобки с произвольной глубиной вложенности.

Другой способ определения формального языка – формальная грамматика, задающая общие правила построения языка. Этот способ используется для определения синтаксиса компьютерных языков. Формальная грамматика – метаязык, то есть язык, который определяет язык. В частности, для описания регулярных языков может быть задана регулярная грамматика.

Формальные грамматики на практике используются для следующих целей:

* лексический и синтаксический разбор компьютерного языка;
* порождение случайных фраз на компьютерном языке, например, для задач тестирования.

Более мощным, чем регулярные языки, формализмом описания компьютерных языков является **контекстно-свободная грамматика**.

Контекстно-свободную грамматику можно представить себе в виде программы на ограниченном языке программирования. В этом языке есть «имена функций» (нетерминалы, имена правил) и «определения функций» (тела правил). В определениях содержатся «вызовы функций» и «вывод символов» определяемого языка (терминалов). Работа программы начинается с «главной функции», называемой начальным нетерминалом.

Благодаря возможности использования рекурсии в грамматических правилах контекстно-свободные грамматики широко используются для определения синтаксиса компьютерных языков.

Контекстно-свободную грамматику можно представить в форме так называемой железнодорожной или синтаксической диаграммы.

На @fig:dsl1 показан пример грамматики арифметического выражения.

![Синтаксическая диаграмма грамматики арифметического выражения](dsl1.svg){#fig:dsl1}

В текстовой форме контекстно-свободная грамматика определяется с помощью формы Бэкуса-Наура (БНФ). На практике, в БНФ часто добавляются конструкции регулярных выражений для упрощения описания синтаксиса.

Ниже представлен пример грамматики арифметического выражения в БНФ:

```bnf
<Значение> ::= <Число> | <Переменная>
<Операция> ::= "+" | "-" | "*" | "/"
<Выражение> ::= <Значение> | <Выражение> <Операция> <Выражение> | "(" <Выражение> ")"
```

Для автоматизации задач лексического (уровень базовых лексических единиц) и синтаксического (уровень иерархических конструкций) разбора существуют специальные инструменты. Примерами таких инструментов являются ANTLR @antlr для Java, SLY @sly для Python и Flex/Bison для C++.

## Компьютерные языки

Рассмотрим представленную на @fig:dsl2 упрощенную классификацию компьютерных языков.

![Упрощенная классификация компьютерных языков](dsl2.svg){#fig:dsl2}

Тьюринг-полными называются языки, на которых можно реализовать модель машины Тьюринга, а значит – реализовать любой алгоритм. К таким языкам относится, в частности, большинство языков программирования.

Тьюринг-полнота необязательно является полезным свойством языка. Для Тьюринг-неполных, ограниченных языков может быть легче осуществить статический анализ и преобразования на уровне компилятора.

Предметно-ориентированные языки (domain-specific language, DSL) ориентированы задачи некоторого узкого класса, поэтому, зачастую являются Тьюринг-неполными. Основное достоинство DSL – использование предметно-ориентированной нотации. 

Известно множество специализированных нотаций. Например, нотация химических формул или нотная нотация. Математики древних цивилизаций описывали математические задачи на естественном языке с использованием сложных систем счисления. Поэтому установление даже некоторых тривиальных для современного школьника, оперирующего алгебраической нотацией, математических фактов могло было затруднено для математика древности. Заслуживает внимания гипотеза лингвистической относительности, которая предполагает наличие влияния структуры языка на мышление. 

## Простые форматы описания конфигурации

Одним из старейших форматов описания как кода, так и данных являются **S-выражения** языка Lisp. Это нотация для древовидных структур, реализованных в виде вложенных списков. 

Грамматика S-выражений в БНФ:

```bnf 
<s-exp> ::= <atom> |  '(' <s-exp-list> ')'
<s-exp-list> ::= <sexp> <s-exp-list> |  
<atom> ::= <symbol> |  <integer> |  #t  |  #f
```
 
Пример S-выражения:

```lisp
(users
   ((uid 1)   (name root) (gid 1))
   ((uid 108) (name peter) (gid 108))
   ((uid 109) (name alex) (gid 109)))
```

К недостаткам S-выражений для описания конфигурации относятся:

* сложность чтения для человека синтаксиса с большим числом скобок;
* нет стандарта S-выражений для описания конфигурационных данных.

Классическим способом представления файлов конфигурации является **формат INI** из Windows, а также схожие с ним варианты conf из Unix. В основе формата – последовательность пара ключ-значение. Пары схожего назначения объединяются в секции:

```ini 
[секция_1]
параметр1=значение1
параметр2=значение2
 
[секция_2]
параметр1=значение1
параметр2=значение2
``` 

В (расширенной) БНФ:
 
```bnf
ini ::= {section}
section ::= "[" name "]" "\n" {entry}
entry ::= key "=" value "\n"
```

Здесь фигурные скобки обозначают повторение 0 или более раз.

Пример INI-файла (system.ini):
 
```ini
; for 16-bit app support
[386Enh]
woafont=dosapp.fon
EGA80WOA.FON=EGA80WOA.FON
EGA40WOA.FON=EGA40WOA.FON
CGA80WOA.FON=CGA80WOA.FON
CGA40WOA.FON=CGA40WOA.FON
 
[drivers]
wave=mmdrv.dll
timer=timer.drv
 
[mci]
[network]
Bios=1438818414
SSID=29519693
```
 
К недостаткам INI для описания конфигурации относятся:

* отсутствие поддержки вложенных конструкций;
* отсутствие стандарта INI для описания конфигурационных данных.

Еще недавно наибольшую популярность среди форматов конфигурационных данных имел **XML** (eXtensible Markup Language).

XML разрабатывался как язык с простым формальным синтаксисом, удобный для создания и обработки документов программами и одновременно удобный для чтения и создания документов человеком, с подчеркиванием нацеленности на использование в Интернете.

В самом базовом виде XML напоминает S-выражения, в которых вместо скобок используются открывающийся и закрывающийся именованные теги. 

Пример XML-данных:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<shiporder orderid="889923"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:noNamespaceSchemaLocation="shiporder.xsd">
   <orderperson>John Smith</orderperson>
   <shipto>
      <name>Ola Nordmann</name>
      <address>Langgt 23</address>
      <city>4000 Stavanger</city>
      <country>Norway</country>
   </shipto>
   <item>
      <title>Hide your heart</title>
      <quantity>1</quantity>
      <price>9.90</price>
   </item>
</shiporder>
``` 

Важной особенностью XML является поддержка специальных схем для определения корректности XML-данных: DTD (Document Type Definition), XML Schema и другие.
DTD определяет:

* состав элементов, которые могут использоваться в XML документе;
* описание моделей содержания, т.е. правил вхождения одних элементов в другие;
* состав атрибутов, с какими элементами XML документа они могут использоваться;
* каким образом атрибуты могут применяться в элементах;
* описание сущностей, включаемых в XML документ.

В XML Schema дополнительно введены типы данных для элементов. Фрагмент XML Schema описания элемента item из примера выше:

```xml
<xs:element name="item" maxOccurs="unbounded">
  <xs:complexType>
    <xs:sequence>
      <xs:element name="title" type="xs:string"/>
      <xs:element name="note" type="xs:string" minOccurs="0"/>
      <xs:element name="quantity" type="xs:positiveInteger"/>
      <xs:element name="price" type="xs:decimal"/>
    </xs:sequence>
  </xs:complexType>
</xs:element>
```

К недостаткам XML для описания конфигурации относится высокая сложность формата, как для чтения человеком, так и для программного разбора.

Формат **JSON** (JavaScript Object Notation) основан на синтаксисе JavaScript. Изначально он использовался в качестве текстового формата обмена данными, но со временем все чаще стал применяться и в качестве формата описания конфигурации приложения.

В JSON  используются следующие типы данных:

* число в формате double;
* строка;
* логическое значение true или false;
* массив значений любого поддерживаемого типа;
* объект – словарь пар ключ-значение, где ключ – строка, а значение – любой поддерживаемый тип;
* специальное значение null.

Пример JSON-данных:

```json
{"menu": {
  "id": "file",
  "value": "File",
  "popup": {
    "menuitem": [
      {"value": "New", "onclick": "CreateNewDoc()"},
      {"value": "Open", "onclick": "OpenDoc()"},
      {"value": "Close", "onclick": "CloseDoc()"}
    ]
  }
}}
```

Для проверки корректности JSON-данных в формат позже была добавлена схема – JSON Schema, которая во многом схожа с XML Schema.

К недостаткам JSON для описания конфигурации относится отсутствие поддержки комментариев.

Формат **YAML** (YAML Ain't Markup Language) предназначен для сериализации данных, но также часто используется в качестве конфигурационного языка. YAML с точки зрения синтаксиса имеет сходство с языком Python – для определения вложенности конструкций используются отступы. Особенностью YAML является возможность создания ( с помощью `&`) и использования (с помощью `*`) ссылок на элементы документа. Это позволяет не приводить полностью повторно встречающиеся данные.

Пример YAML-файла:
 
```yaml
–
receipt:     Oz-Ware Purchase Invoice
date:        2012-08-06
customer:
    first_name:   Dorothy
    family_name:  Gale
 
items:
    - part_no:   A4786
      descrip:   Water Bucket (Filled)
      price:     1.47
      quantity:  4
 
    - part_no:   E1628
      descrip:   High Heeled "Ruby" Slippers
      size:      8
      price:     133.7
      quantity:  1
 
bill-to:  &id001
    street: |
            123 Tornado Alley
            Suite 16
    city:   East Centerville
    state:  KS
 
ship-to:  *id001
...
```

К недостаткам YAML для описания конфигурации относятся:

* сложность редактирования сильно вложенных элементов, проблемы с отступом;
* сложность стандарта YAML (размер файла спецификации больше, чем у XML).

Формат **TOML** (Tom's Obvious, Minimal Language) специально предназначен для использования в конфигурационных файлах. Его синтаксис основан на INI.
[Спецификация TOML](https://toml.io/en/v1.0.0-rc.1#spec) не имеет БНФ-описания.

Пример TOML-данных:
 
```ini
# This is a TOML document
 
title = "TOML Example"
 
[owner]
name = "Tom Preston-Werner"
dob = 1979-05-27T07:32:00-08:00
 
[database]
enabled = true
ports = [ 8001, 8001, 8002 ]
data = [ ["delta", "phi"], [3.14] ]
temp_targets = { cpu = 79.5, case = 72.0 }
 
[servers]
 
  [servers.alpha]
  ip = "10.0.0.1"
  role = "frontend"
 
  [servers.beta]
  ip = "10.0.0.2"
  role = "backend"
```

К недостаткам TOML для описания конфигурации относится сложность спецификации языка.

## Языки общего назначения как конфигурационные

Использование языка программирования общего назначения для описания конфигурации ПО обладает рядом преимуществ по сравнению с простыми непрограммируемыми конфигурационными форматами:

* поддержка функций;
* поддержка циклов;
* поддержка типов данных;
* поддержка импорта файлов.

Благодаря поддержке функций, циклов и разбиению файлов на модули достигается принцип DRY (Don't repeat yourself) – не допускать повторения одной и той же информации  в программе. В результате конфигурационные файлы имеют компактную, удобную для редактирования форму.

Поддержка типов данных, в том числе пользовательских, позволяет осуществить на уровне типов проверку корректности программы – то, для чего в таких форматах, как XML, используются схемы.

Некоторые примеры использования языков общего назначения в качестве конфигурационных языков:

* В текстовом редакторе Emacs используется встроенный язык [Elisp](https://www.gnu.org/software/emacs/manual/html_node/emacs/Saving-Customizations.html) для описания конфигурации;
* В БД [Tarantool](https://www.tarantool.io/en/doc/1.10/reference/configuration/#index-init-label) используется Lua;
* Конфигурации на языке Python используются в [setuptools](https://setuptools.readthedocs.io/en/latest/setuptools.html) и Jupyter;
* В [Gradle](https://docs.gradle.org/current/userguide/tutorial_using_tasks.html#sec:build_scripts_are_code) конфигурация сборки может быть описана на Groovy и Kotlin.
 
К недостаткам языков общего назначения для задач конфигурирования относятся:

* небезопасное исполнение стороннего кода;
* сложность, чрезмерная выразительность языка (Тьюринг-полнота), мешающая задачам анализа и порождения конфигурационных данных.
  
## Программируемые конфигурационные языки

К основным DRY-элементам программируемых конфигурационных языков, можно отнести:

* переменные;
* арифметические операции;
* функции в математическом смысле;
* ветвления;
* операции построения многоэлементных массивов и иных составных объектов;
* операции импорта сторонних модулей;

**Jsonnet** является расширенным вариантом JSON и представляет собой Тьюринг-полный конфигурационный язык с динамической типизацией. 

Пример описания данных на Jsonnet:

```js
local makeUser(user) = {
  local home = std.format("/home/%s", user),
  local privateKey = std.format("%s/.ssh/id_ed25519", home),
  local publicKey  = std.format("%s.pub", privateKey),
  home: home,
  privateKey: privateKey,
  publicKey: publicKey
};

[
  makeUser("bill"),
  makeUser("jane")
]
 
```

Результат преобразования в JSON:

```json 
[
  {
    "home": "/home/bill",
    "privateKey": "/home/bill/.ssh/id_ed25519",
    "publicKey": "/home/bill/.ssh/id_ed25519.pub"
  },
  {
    "home": "/home/jane",
    "privateKey": "/home/jane/.ssh/id_ed25519",
    "publicKey": "/home/jane/.ssh/id_ed25519.pub"
  }
]
```

Еще одним программируемым конфигурационным языком является **Dhall**. Этот язык со статической типизацией, основанный на принципах функционального программирования. Авторы определяют Dhall, как JSON + функции + типы + импорты. Важной особенностью Dhall является тот факт, что это Тьюринг-неполный язык. В Dhall имеется режим нормализации представления, при котором все языковые абстракции разворачиваются и результат представляет собой уровень представления JSON или YAML.

Пример описания данных на Dhall:
 
```haskell
let makeUser = \(user : Text) ->
      let home       = "/home/${user}"
      let privateKey = "${home}/.ssh/id_ed25519"
      let publicKey  = "${privateKey}.pub"
      in  { home = home
          , privateKey = privateKey
          , publicKey = publicKey
          }

in  [ makeUser "bill"
    , makeUser "jane"
    ]
``` 
