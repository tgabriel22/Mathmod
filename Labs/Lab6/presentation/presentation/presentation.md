---
## Front matter
title: Лабораторная работа №6
subtitle: Задача об эпидемии
author:
  - Габриэль Тьерри
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 18 марта 2023

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

# Информация

## Докладчик

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

- Габриэль Тьерри
- студент НКНбд-01-20
- Факультет физико-математических и естественных наук
- Российский университет дружбы народов
- <https://github.com/tgabriel22/mathmod/tree/master/Labs>

:::
::::::::::::::

# Цель работы

Построить графики изменения числа особей в группах с помощью простейшей модели эпидемии, рассмотреть, как будет протекать эпидемия в различных случаях.

# Задание

На одном острове вспыхнула эпидемия. Известно, что из всех проживающих. на острове (N=14 041) в момент начала эпидемии (t=0) число заболевших людей (являющихся распространителями инфекции) I(0)=131, А число здоровых людей с иммунитетом к болезни R(0)=71. Таким образом, число людей восприимчивых к болезни, но пока здоровых, в начальный момент времени S(0)=N-I(0)- R(0).
Постройте графики изменения числа особей в каждой из трех групп.
Рассмотрите, как будет протекать эпидемия в случае:

1. если $I(0)\leq I^*$
2. если $I(0)> I^*$

## Материалы и методы

- Язык программирования Julia
- Язык программирования Modelica
- Пакеты Plots, DifferentialEquations

# Теоретическое введение

Рассмотрим простейшую модель эпидемии. Предположим, что некая
популяция, состоящая из N особей, (считаем, что популяция изолирована) подразделяется на три группы. Первая группа - это восприимчивые к болезни, но пока здоровые особи, обозначим их через S(t). Вторая группа – это число инфицированных особей, которые также при этом являются распространителями инфекции, обозначим их I(t). А третья группа, обозначающаяся через R(t) – это здоровые особи с иммунитетом к болезни. До того, как число заболевших не превышает критического значения I , считаем, что все больные изолированы и не заражают здоровых. Когда $I(t)> I^*$, тогда инфицирование способны заражать восприимчивых к болезни особей. Таким образом, скорость изменения числа S(t) меняется по следующему
закону:

$$
\frac{ds}{dt} =
\begin{cases}
   -\alpha{S},  I(t)> I^*
   \\
   0,  I(t)\leq I^*
 \end{cases}
$$

Поскольку каждая восприимчивая к болезни особь, которая, в конце концов, заболевает, сама становится инфекционной, то скорость изменения числа инфекционных особей представляет разность за единицу времени между заразившимися и теми, кто уже болеет и лечится, т.е.:

$$
  \frac{dI}{dt} =
  \begin{cases}
\alpha{S} - \beta {I},  I(t)> I^*
     \\
     -\beta {I}, I(t)\leq I^*
   \end{cases}
$$

А скорость изменения выздоравливающих особей (при это приобретающие иммунитет к болезни)

$$
  \frac{dR}{dt} = \beta {I}
$$

Постоянные пропорциональности $\alpha, \beta$ - это коэффициенты заболеваемости и выздоровления соответственно. Для того, чтобы решения соответствующих уравнений определялось однозначно, необходимо задать начальные условия .Считаем, что на начало эпидемии в момент времени t = 0 нет особей с иммунитетом к болезни R(0)=0, а число инфицированных и восприимчивых к болезни особей I(0) и S(0) соответственно. Для анализа картины протекания эпидемии необходимо рассмотреть два случая:
$I(0)\leq I^*$ и $I(0)> I^*$

## Выполнение лабораторной работы

##### 1.1 Решение для случая 1 на Julia: $I(0)\leq I^*$

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
    N = 14041.0 #общая численность популяции
    I0 = 131.0 #количество инфицированных особей в начальный момент времени
    R0 = 71.0   #количество здоровых особей с иммунитетом в начальный момент времени
    S0 = N-I0-R0 #количество восприимчивых к болезни особей в начальный момент времени
    t0 = 0.0 #начальное Время
    a = 0.01 #коэффициент заболеваемости
    b = 0.02 #коэффициент выздоровления
end

#случай, когда I(0)<=I*
function F!(du, u, p, t)
    du[1] = 0.0
    du[2] = -b*u[2]
    du[3] = b*u[2]
end

begin
    U0 = [S0, I0, R0] #начальные значения
    T = [0.0, 200.0]
    prob = ODEProblem(F!, U0, T)
end
```

![sol №1(Julia)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab6/report/report/image/Julia_Sol1.PNG){#fig:001 width=70%}

```julia
sol = solve(prob, saveat = 0.01)
```

###### Результат

![Граф №1(Julia)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab6/report/report/image/Julia_Graph1.PNG){#fig:002 width=70%}

```julia
Plots.plot(sol, label =[L"$S(t)$" L"$I(t)$" L"$R(t)$"])
```

##### 1.2 Решение для случая 1 на Openmodelica: $I(0)\leq I^*$

```openmodelica
model LAB6
//случай, когда I(0)<=I*

constant Real a=0.01;//коэффицент заболевания
constant Real b=0.02;//коэфицент выздоровления
constant Real N=14041;//количество проживающих на острове

Real I;//инфицированные особи
Real R;//здоровые особи с иммунитетом к болезни
Real S;//здоровые особи, восприимчивые к болезни

initial equation
I=131;//количество инфицированных особей
R=71;//количество здоровых особей с иммунитетом к болезни
S=N-I-R;//количество здоровых особей, восприимчивых к болезни

equation
der(S)=0;//изменение количества здоровых особей, восприимчивых к болезни
der(I)=-b*I;//изменение количества инфицированных особей
der(R)=b*I;//изменение количества здоровых особей с иммунитетом

end LAB6;
```

###### Результат

![Граф №1(Openmodelica)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab6/report/report/image/OpenM1.png){#fig:003 width=70%}

##### 2.1 Решение для случая 2 на Julia: $I(0)> I^*$

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
    N = 14041.0 #общая численность популяции
    I0 = 131.0 #количество инфицированных особей в начальный момент времени
    R0 = 71.0   #количество здоровых особей с иммунитетом в начальный момент времени
    S0 = N-I0-R0 #количество восприимчивых к болезни особей в начальный момент времени
    t0 = 0.0 #начальное Время
    a = 0.01 #коэффициент заболеваемости
    b = 0.02 #коэффициент выздоровления
end

#случай, когда I(0)>I*
function F!(du, u, p, t)
    du[1] = -a*u[1]
    du[2] = a*u[1]-b*u[2]
    du[3] = b*u[2]
end

begin
    U0 = [S0, I0, R0] #начальные значения
    T = [0.0, 200.0]
    prob = ODEProblem(F!, U0, T)
end
```

![sol](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab6/report/report/image/Julia_Sol2.PNG){#fig:004 width=70%}

```julia
sol = solve(prob, saveat = 0.01)
```

###### Результат

![Граф №2(Julia)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab6/report/report/image/Julia_Graph2.PNG){#fig:005 width=70%}

```julia
Plots.plot(sol, label =[L"$S(t)$" L"$I(t)$" L"$R(t)$"])
```

##### 2.2Решение для случая 2 на Openmodelica: $I(0)> I^*$

```openmodelica
model LAB6_Part2
//случай, когда I(0)>I*

constant Real a=0.01;//коэффицент заболевания
constant Real b=0.02;//коэфицент выздоровления
constant Real N=14041;//количество проживающих на острове

Real I;//инфицированные особи
Real R;//здоровые особи с иммунитетом к болезни
Real S;//здоровые особи, восприимчивые к болезни

initial equation
I=131;//количество инфицированных особей
R=71;//количество здоровых особей с иммунитетом к болезни
S=N-I-R;//количество здоровых особей, восприимчивых к болезни

equation
der(S)=-a*S;//изменение количества здоровых особей, восприимчивых к болезни
der(I)=a*S-b*I;//изменение количества инфицированных особей
der(R)=b*I;//изменение количества здоровых особей с иммунитетом

end LAB6_Part2;
```

###### Результат

![Граф №2(Openmodelica)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab6/report/report/image/OpenM2.png){#fig:006 width=70%}

# Вывод

В ходе выполнения лабораторной работы я научился строить графики изменения числа особей в группах с помощью простейшей модели эпидемии, рассмотрел, как будет протекать эпидемия в различных случаях.
