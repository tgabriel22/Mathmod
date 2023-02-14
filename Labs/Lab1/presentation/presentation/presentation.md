---
## Front matter
title: "РОССИЙСКИЙ УНИВЕРСИТЕТ ДРУЖБЫ НАРОДОВ"
subtitle:
- Факультет физико-математических и естественных наук.

- Математическое моделирование.

- Презентация по лабораторной работе № 1.

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

- Работа с git
- Получите знания о том, как настроить git, pandoc, chocolatey, TexLive, Julia, Openmodelica на вашем компьютере

1. **Работа с git:**

- Прежде всего, я удостоверяюсь, что у меня есть учетная запись на github.
- я создал локальный репозиторий.
- Чтобы иметь возможность клонировать существующий репозиторий git по SSH-ссылке, нам необходимо установить службу SSH-Agent для Windows.
- A также настроил пару ключей SSH для доступа к удаленному провайдеру Git.
- В GitLab, в разделе SSH Keys пользовательских настроек, добавил открытый ключ SSH.

- создал новый репозиторий с именем mathmod в своей Github, используя существующий шаблон, и клонировал этот репозиторий в свой локальный, используя ssh-ссылку

- в папке mathmod создал общий каталог лабораторных работ с именем Labs и подпапку с именем Lab1.

- Добавил изменения, внесенные в локальном репозитории, на сервере github

2. **Установка языков / текстовых редакторов:**

- Настроил git, pandoc, chocolatey, TexLive, Julia, Openmodelica

# Результаты:

- Выполнив эти задания, я изучил идеологию и применение версии git, и как я настроил git, pandoc, chocolatey, TexLive, Julia, Openmodelica
