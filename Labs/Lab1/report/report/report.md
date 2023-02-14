---
## Front matter
title: "РОССИЙСКИЙ УНИВЕРСИТЕТ ДРУЖБЫ НАРОДОВ"
subtitle:
- Факультет физико-математических и естественных наук.

- Математическое моделирование.

- ОТЧЕТ по лабораторной работе № 1.

- НКНбд 01-20.

author: "Габриэль Тьерри"
date:  "МОСКВА 2023 г."

## Generic otions
lang: ru-RU

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Misc options
indent: true
header-includes:
  - \usepackage[T1]{fontenc}
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы :

- Создайте или подтвердите, что у вас есть учетная запись на Github.

- Настройте git, pandoc, chocolatey, TexLive, Julia, Openmodelica на своем компьютере
  Составьте отчет

# Ход работы:

Поскольку у меня уже есть учетная запись, заходим по [ссылке](https://github.com/tgabriel22) и попадаем в наш аккаунт github. .( рисунок 1)

![](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab1/report/report/image/Capture001.PNG "Рисунок1")

Cоздадим локальный репозиторий
( рисунок 2)

![](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab1/report/report/image/Capture01.PNG "Рисунок2")

Чтобы иметь возможность клонировать существующий репозиторий git по SSH-ссылке, нам необходимо установить службу SSH-Agent для Windows.
A также настроим пару ключей SSH для доступа к удаленному провайдеру Git. ( рисунок 3,4)

![](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab1/report/report/image/Capture1.PNG "Рисунок3")

![](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab1/report/report/image/Capture2.PNG "Рисунок4")

В GitLab, в разделе SSH Keys пользовательских настроек, мы добавляем открытый ключ SSH.( рисунок 5)

![](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab1/report/report/image/Capture3.PNG "Рисунок5")

Мы создаем новый репозиторий с именем mathmod в нашем Github, используя существующий шаблон, и клонируем этот репозиторий в наш локальный, используя ssh-ссылку. ( рисунок 6, 7)

![](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab1/report/report/image/Capture4.PNG "Рисунок6")

![](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab1/report/report/image/Capture5.PNG "Рисунок7")

в папке mathmod мы создаем общий каталог лабораторных работ с именем Labs и подпапку с именем Lab1. ( рисунок 8)

![](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab1/report/report/image/Capture6.PNG "Рисунок8")

Добавляем изменения, внесенные в локальном репозитории, на сервер github. ( рисунок 9, 10)

![](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab1/report/report/image/Capture7.PNG "Рисунок9")

![](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab1/report/report/image/Capture10.PNG "Рисунок10")

Настраиваем git, pandoc, chocolatey, TexLive, Julia, Openmodelica
Добавляем изменения, внесенные в локальном репозитории, на сервер github. ( рисунок 11, 12, 13, 14, 15, 16)

![](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab1/report/report/image/Choco.PNG "Рисунок11")

![](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab1/report/report/image/Julia.PNG "Рисунок12")

![](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab1/report/report/image/Make.PNG "Рисунок13")

![](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab1/report/report/image/Pandoc.PNG "Рисунок14")

![](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab1/report/report/image/TexLive.PNG "Рисунок15")

![](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab1/report/report/image/openmodelica.PNG "Рисунок16")

# Вывод:

- изучил идеологию и применение средств контроля версий.

- и как настроил git, pandoc, chocolatey, TexLive, Julia, Openmodelica
