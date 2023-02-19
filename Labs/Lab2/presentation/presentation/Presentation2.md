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
## Объект и предмет исследования

- Задача о погоне
- Язык программирования Julia
- Система моделирования Openmodelica

## Цели и задачи

- Решить задачу о погоне с определенными входными данными
- Овладеть языком программирования Julia
- Построить график траектории движения катера в полярных координатах

## Материалы и методы

- Язык программирования Julia
- Пакеты "Plots", "DifferentialEquations

# Ход работы:

## Подключение библиотек и установка начальных значений

```julia
using DifferentialEquations
using Plots

#the initial distance from the boat to the yatch
D = 25;

fi = 3 * pi / 4

#initial condition
x_01 = D / 2;
x_02 = D / 4;
t1 = (0, pi)
t2 = (-pi, 0)
```

# функция, описывающая движение катера береговой охраны 
```julia
#Create the function
function F(u, p, t)
    return u / sqrt(8)
end;
```

### определите проблему (Решение дифференцильных уравнений)
```julia
#Solving the problems
#Define the problem
prob1 = ODEProblem(F, x_01, t1)
prob2 = ODEProblem(F, x_02, t2)
sol1 = solve(prob1, Tsit5(), reltol=1e-8, abstol=1e-8)
sol2 = solve(prob2, Tsit5(), reltol=1e-8, abstol=1e-8)
```

## графика для первого случая
```julia
#Analyzing the solution
plot(proj=:polar, sol1.t, linewidth=2, title="First Situation", label="yatch trajectory",
    color=:blue)

```julia
plot!(fill(sqrt(2) / 2, 4), collect(0:3), linewidth=2, label="Boat trajectory",
    color=:purple)
```

## графика для второго случая
```julia
plot(proj=:polar, sol2.t, linewidth=2, title="Second Situation",
    label="yatch trajectory", color=:green)

plot!(fill(sqrt(2) / 2, 4), collect(0:3), linewidth=3, label="Boat trajectory",
    color=:yellow)
```

# Результаты

## Полученные графики

Первый случай (рис.1):

![первый случай](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab2/report/report/image/Capture1.PNG "рис.1")

Второй случай (рис.2):

![второй случай](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab2/report/report/image/Capture2.PNG "рис.2")

# Выводы

Использование языка Джулиан для выбора правильной стратегии при решении поисковых задач.
Узнал, как построить график с помощью функции plot и языка julia