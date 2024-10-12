## Задача 1
Вывести служебную информацию о пакете matplotlib (Python). Разобрать основные элементы содержимого файла со служебной информацией из пакета. Как получить пакет без менеджера пакетов, прямо из репозитория?
```
(venv) C:\Users\lordp\OneDrive\Рабочий стол\confupr\task_2>pip3 show matplotlib
Name: matplotlib
Version: 3.9.2
Summary: Python plotting package
Home-page:
Author: John D. Hunter, Michael Droettboom
Author-email: Unknown <matplotlib-users@python.org>
License: License agreement for matplotlib versions 1.3.0 and later
=========================================================
Location: C:\Users\lordp\OneDrive\Рабочий стол\confupr\task_2\venv\Lib\site-packages
Requires: contourpy, cycler, fonttools, kiwisolver, numpy, packaging, pillow, pyparsing, python-dateutil
Required-by:
```
Установка без pip, тоже самое надо сделать со всеми зависимостями
```
pip install git+https://github.com/matplotlib/matplotlib.git
cd matplotlib
python setup.py install
```
### Задача 2
Вывести служебную информацию о пакете express (JavaScript). Разобрать основные элементы содержимого файла со служебной информацией из пакета. Как получить пакет без менеджера пакетов, прямо из репозитория?
```
>npm show express

express@4.21.0 | MIT | deps: 31 | versions: 279
Fast, unopinionated, minimalist web framework
http://expressjs.com/

keywords: express, framework, sinatra, web, http, rest, restful, router, app, api

dist
.tarball: https://registry.npmjs.org/express/-/express-4.21.0.tgz
.shasum: d57cb706d49623d4ac27833f1cbc466b668eb915
.integrity: sha512-VqcNGcj/Id5ZT1LZ/cfihi3ttTn+NJmkli2eZADigjq29qTlWi/hAQ43t/VLPq8+UX06FCEx3ByOYet6ZFblng==
.unpackedSize: 220.8 kB

dependencies:
accepts: ~1.3.8            cookie: 0.6.0              finalhandler: 1.3.1        parseurl: ~1.3.3
array-flatten: 1.1.1       debug: 2.6.9               fresh: 0.5.2               path-to-regexp: 0.1.10
body-parser: 1.20.3        depd: 2.0.0                http-errors: 2.0.0         proxy-addr: ~2.0.7
content-disposition: 0.5.4 encodeurl: ~2.0.0          merge-descriptors: 1.0.3   qs: 6.13.0
content-type: ~1.0.4       escape-html: ~1.0.3        methods: ~1.1.2            range-parser: ~1.2.1
cookie-signature: 1.0.6    etag: ~1.8.1               on-finished: 2.4.1         safe-buffer: 5.2.1
(...and 7 more.)

maintainers:
- wesleytodd <wes@wesleytodd.com>
- dougwilson <doug@somethingdoug.com>
- linusu <linus@folkdatorn.se>
- sheplu <jean.burellier@gmail.com>
- blakeembrey <hello@blakeembrey.com>
- ulisesgascon <ulisesgascondev@gmail.com>
- mikeal <mikeal.rogers@gmail.com>

dist-tags:
latest: 4.21.0  next: 5.0.0

published a week ago by wesleytodd <wes@wesleytodd.com>
Установка без npm

git clone https://github.com/expressjs/express.git
cd express
```
## Задача 3
Сформировать graphviz-код и получить изображения зависимостей matplotlib и express.
```
pipdeptree --packages matplotlib --graph-output dot > matplotlib_deps.dot
Screenshot from 2024-09-26 16-57-39
```
## Задача 4
Решить на MiniZinc задачу о счастливых билетах. Добавить ограничение на то, что все цифры билета должны быть различными (подсказка: используйте all_different). Найти минимальное решение для суммы 3 цифр.
```
include "globals.mzn";  % Подключение библиотеки глобальных ограничений
% Модель для задачи о счастливых билетах с уникальными цифрами

% Билет состоит из 6 цифр от 0 до 9
array[1..6] of var 0..9: digits;

% Условие: все цифры билета должны быть различными
constraint all_different(digits);

% Условие счастливого билета: сумма первых трех цифр равна сумме последних трех
constraint
  digits[1] + digits[2] + digits[3] = digits[4] + digits[5] + digits[6];

% Минимизируем сумму первых трех цифр
var int: sum_digits = digits[1] + digits[2] + digits[3];
solve minimize sum_digits;

% Выводим решение
output [
  "Билет: \(show(digits[1]))\(show(digits[2]))\(show(digits[3]))\(show(digits[4]))\(show(digits[5]))\(show(digits[6]))\n",
  "Сумма цифр: \(sum_digits)\n"
];
```

## Задание 5
## проход по максимальному 
![image](https://github.com/user-attachments/assets/15e486cd-54ad-4185-b7fa-f123c671c5a9)

```
set of int: MenuVersions = 1..6;
set of int: DropdownVersions = 1..5;
set of int: IconVersions = 1..2;

array[MenuVersions] of int: menu = [150, 140, 130, 120, 110, 100];
array[DropdownVersions] of int: dropdown = [230, 220, 210, 200, 180];
array[IconVersions] of int: icons = [200, 100];

var MenuVersions: selected_menu;
var DropdownVersions: selected_dropdown;
var IconVersions: selected_icons;

constraint
    (selected_menu = 1 -> selected_dropdown in 1..3) /\
    (selected_menu = 2 -> selected_dropdown in 2..4) /\
    (selected_menu = 3 -> selected_dropdown in 3..5) /\
    (selected_menu = 4 -> selected_dropdown in 4..5) /\
    (selected_menu = 5 -> selected_dropdown = 5) /\
    (selected_dropdown = 1 -> selected_icons = 1) /\
    (selected_dropdown = 2 -> selected_icons in 1..2) /\
    (selected_dropdown = 3 -> selected_icons in 1..2) /\
    (selected_dropdown = 4 -> selected_icons in 1..2) /\
    (selected_dropdown = 5 -> selected_icons in 1..2);

solve satisfy;

output [
    "Selected menu version: \(menu[selected_menu])\n",
    "Selected dropdown version: \(dropdown[selected_dropdown])\n",
    "Selected icon version: \(icons[selected_icons])\n"
];
```
![image](https://github.com/user-attachments/assets/2a906ace-d567-4e8a-a558-a2cbdcc9b729)

## Задание 6

![image](https://github.com/user-attachments/assets/96573961-6926-405a-a0d7-d990715440cf)

```
set of int: FooVersions = 1..2;
set of int: LeftVersions = 1..1;
set of int: RightVersions = 1..1;
set of int: SharedVersions = 1..2;
set of int: TargetVersions = 1..2;
set of int: RootVersions = 1..1; 

var FooVersions: selected_foo;
var LeftVersions: selected_left;
var RightVersions: selected_right;
var SharedVersions: selected_shared;
var TargetVersions: selected_target;
var RootVersions: selected_root;

constraint selected_root = 1;
constraint selected_foo = 1;
constraint selected_target = 2;
constraint selected_left = 1;
constraint selected_right = 1;
constraint selected_shared >= 1;
constraint selected_shared < 2;

solve satisfy;

output [
  "Версия foo: ", show(selected_foo),".0.0", "\n",
  "Версия left: ", show(selected_left),".0.0", "\n",
  "Версия right: ", show(selected_right),".0.0", "\n",
  "Версия shared: ", show(selected_shared),".0.0", "\n",
  "Версия target: ", show(selected_target),".0.0", "\n",
  "Версия root: ", show(selected_root),".0.0", "\n"
];
```

![image](https://github.com/user-attachments/assets/449090b6-3b39-4eeb-97a7-ea69c0fdb521)

## Задание 7
Представить на MiniZinc задачу о зависимостях пакетов в общей форме, чтобы конкретный экземпляр задачи описывался только своим набором данных.

```
int: num_packages;

set of int: Packages = 1..num_packages;

array[Packages] of set of int: Versions;

array[Packages] of var int: selected_version;

array[Packages] of set of int: dependencies;

array[Packages, Packages] of int: min_version;

array[Packages, Packages] of int: max_version;

constraint
  forall(i in Packages) (
    forall(dep in dependencies[i]) (
      selected_version[dep] >= min_version[i, dep] /\
      selected_version[dep] <= max_version[i, dep]
    )
  );

solve satisfy;
```
