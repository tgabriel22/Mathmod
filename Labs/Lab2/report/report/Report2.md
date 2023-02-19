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
  - \usepackage[T1]{fontenc}
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Использование языка Джулиан для выбора правильной стратегии при решении поисковых задач.

# Задание

На море в тумане катер береговой охраны преследует лодку браконьеров. Через определенный промежуток времени туман рассеивается, и лодка обнаруживается на расстоянии 25 км от катера. Затем лодка снова скрывается в тумане и уходит прямолинейно в неизвестном направлении. Известно, что скорость
катера в 6 раза больше скорости браконьерской лодки.


1. Запишите уравнение, описывающее движение катера, с начальными условиями для двух случаев (в зависимости от расположения катера  относительно лодки в начальный момент времени).
2. Постройте траекторию движения катера и лодки для двух случаев.
3. Найдите точку пересечения траектории катера и лодки

# Теоретическое введение

**Julia** — высокоуровневый высокопроизводительный свободный язык программирования с динамической типизацией, созданный для математических вы*числений. Эффективен также и для написания программ общего назначения. Синтаксис языка схож с синтаксисом других математических языков (например, MATLAB и Octave), однако имеет некоторые существенные отличия. Julia написан на Си, C++ и Scheme. Имеет встроенную поддержку многопоточности и распределённых вычислений, реализованные в том числе в стандартных конструкциях.

# Ход работы:

## Постановка задачи 

1. Примем за момент отсчета времени момент первого рассеивания тумана.

2. Введем полярные координаты с центром в точке нахождения браконьеров и осью, проходящей через катер береговой охраны. Тогда начальные координаты катера (25; 0). Обозначим скорость лодки v

3. Траектория катера должна быть такой, чтобы и катер, и лодка все время были на одном расстоянии от полюса, только в этом случае траектория катера пересечется с траекторией лодки. Поэтому для начала катер береговой охраны должен двигаться некоторое время прямолинейно, пока не окажется на том же расстоянии от полюса, что и лодка браконьеров. После этого катер береговой охраны должен двигаться вокруг полюса удаляясь от него с той же скоростью, что и лодка браконьеров.


4. Чтобы найти расстояние X (расстояние, после которого катер начнет двигаться вокруг полюса),
    необходимо составить простое уравнение. Пусть через время t катер и лодка окажутся на одном расстоянии x от полюса. За это время лодка пройдет x, а катер — k-x (или k+x в зависимости
    от начального положения катера относительно полюса). Время, за которое они пройдут это расстояние, вычисляется как x/v или k-x/6v (во втором случае k+x/6v). Так как время одно и то 
    же, то эти величины одинаковы. Тогда неизвестное расстояние x можно найти из следующего уравнения:x/v=(k-x)/3v в первом случае и x/v=(k+x)/3v во втором. Отсюда мы найдем два значения x_01=k/4 и x_02=k/2, задачу будем решать для двух случаев.


5. После того, как катер береговой охраны окажется на одном расстоянии от полюса, что и лодка, он должен сменить прямолинейную траекторию и начать двигаться вокруг полюса, удаляясь от него со скоростью лодки v. Для этого скорость катера раскладываем на две составляющие: v_r — радиальная 
    скорость и v_τ — тангенциальная скорость. Радиальная скорость - это скорость, с которой катер 
    удаляется от полюса, v_r=dr/dt. Нам нужно, чтобы эта скорость была равна скорости лодки, поэтому полагаем dr/dt=v. Тангенциальная скорость – это линейная скорость вращения катера
    относительно полюса. Она равна произведению угловой скорости ∂θ/∂t на радиус r, v_τ=r*∂θ/∂t
    v_τ=√(25v^2-v^2 )=√8*v (учитывая, что радиальная скорость равна v). Тогда получаем r*∂θ∂t=√8*v.

6. Решение исходной задачи сводится к решению системы из двух дифференциальных уравнений. Далее, исключая из полученной системы  производную по t, переходим к одному уравнению: ∂r/∂θ=r/√8. При этом, начальные условия остаются прежними. Решая это уравнение, мы получаем
траекторию движения катера в полярных координатах.

## Код

```Julia
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


#Create the function
function F(u, p, t)
    return u / sqrt(8)
end;


#Define the problem
prob1 = ODEProblem(F, x_01, t1)
prob2 = ODEProblem(F, x_02, t2)

#Solving the problems
sol1 = solve(prob1, Tsit5(), reltol=1e-8, abstol=1e-8)
sol2 = solve(prob2, Tsit5(), reltol=1e-8, abstol=1e-8)

#Analyzing the solution
plot(proj=:polar, sol1.t, linewidth=2, title="First Situation", label="yatch trajectory",
    color=:blue)

plot!(fill(sqrt(2) / 2, 4), collect(0:3), linewidth=2, label="Boat trajectory",
    color=:purple)

plot(proj=:polar, sol2.t, linewidth=2, title="Second Situation",
    label="yatch trajectory", color=:green)

plot!(fill(sqrt(2) / 2, 4), collect(0:3), linewidth=3, label="Boat trajectory",
    color=:yellow)
```

## Полученные графики

Первый случай (рис.1):

![первый случай](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab2/report/report/image/Capture1.PNG "рис.1")

Второй случай (рис.2):

![второй случай](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab2/report/report/image/Capture2.PNG "рис.2")

# Выводы

Использование языка Джулиан для выбора правильной стратегии при решении поисковых задач.
Узнал, как построить график с помощью функции plot и языка julia

### Список литературы
1. [DifferentialEquations.jl](https://docs.sciml.ai/DiffEqDocs/stable/basics/overview/)
2. [Julialang-Manual-Getting-Started](https://docs.julialang.org/en/v1/manual/getting-started/)

