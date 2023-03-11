---
## Front matter
title: Лабораторная работа №5
subtitle: Модель Лотки - Вольтерры
author:
  - Габриэль Тьерри
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 11 марта 2023

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: true # List of tables
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
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
  - \usepackage{amsmath}
---

## Докладчик

- Габриэль Тьерри
- студент НКНбд-01-20
- Факультет физико-математических и естественных наук
- Российский университет дружбы народов
- 1032204249

# Цель работы

Реализовать на языках программирования Julia и Openmodelica модель Лотки-Вольтерры, также известную как моедль взаимодействия "хищник-жертва".

# Задание

Для модели «хищник-жертва»:

$$
 \begin{cases}
   \frac{dx}{dt} = -0.47*x(t) + 0.021*x(t)*y(t)
   \\
   \frac{dy}{dt} = 0.57*y(t) - 0.044*x(t)*y(t)
 \end{cases}
$$

Постройте график зависимости численности хищников от численности жертв, а также графики изменения численности хищников и численности жертв при следующих начальных условиях: $$ x_0 = 12, y0 = 37 $$ Найдите стационарное состояние системы.

## Материалы и методы

- Язык программирования Julia
- Язык программирования Modelica
- Пакеты Plots, DifferentialEquations

# Ход работы

## Решение на языке Julia

1. На первом этапе смоедлировали задачу, используя язык программирования Julia. Получили следующий код:

```julia
begin
    import Pkg
    Pkg.add("LaTeXStrings")
    Pkg.activate()
    using DifferentialEquations
    using LaTeXStrings
    import Plots
end

begin
    X0 = 12.0
    Y0 = 37.0
    a = 0.47
    b = 0.021
    c = 0.57
    d = 0.044
end

function F!(du, u, p, t)
    du[1] = -a*u[1]+b*u[1]*u[2]
    du[2] = c*u[2]-d*u[1]*u[2]
end

begin
    U0 = [X0, Y0]
    T = [0.0, 100.0]
    prob = ODEProblem(F!, U0, T)
end
```

![Sol (Julia)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab5/report/report/image/Capture6.PNG){#fig:001 width=90%}

```julia
sol = solve(prob, saveat = 0.05)
```

В результате работы программы получили следующие результат.

### График

![Изменение числа хищников и жертва(JULIA)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab5/report/report/image/Capture5.PNG){#fig:002 width=90%}

```julia

 Plots.plot(sol)

 # Найдем стационарное состояние системы в точке x
begin
    x = c/d
end

# Найдем стационарное состояние системы в точке y
begin
    y = a/b
end
```

## Решение на языке Openmodelica

2. На втором этапе смоделировали задачу в среде моделирования Openmodelica. Получили следующие код:

```openmodelica
model LAB5
constant Real a = 0.47;    //значение a
constant Real b = 0.021;  //значение b
constant Real c = 0.57;  //значение c
constant Real d = 0.044;//значение d

Real x;//хищники
Real y;//жертвы

initial equation
x=12;//начальное количество хищников
y=37;//начальное количество жертв

equation
der(x)=a*x-b*x*y;//уравнение системы
der(y)=-c*y+d*x*y;//уравнение системы

end LAB5;
```

В результате работы программы получили следующие результат.

### График

![Изменение числа хищников и жертва(OM)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab5/report/report/image/Capture2.PNG){#fig:003 width=90%}

# Выводы

В ходе выполнения лабораторной работы я научился строить график зависимости численности хищников от численности жертв, а также графики изменения численности хищников и численности жертв при заданных начальных условиях. Нашел стационарное состояние системы.

# Список литературы{.unnumbered}

::: {#refs}
:::
