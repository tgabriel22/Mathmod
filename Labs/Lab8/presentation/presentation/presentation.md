---
## Front matter
title: Лабораторная работа №8
subtitle: Модель конкуренции двух фирм
author:
  - Габриэль Тьерри
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 31 марта 2023

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

Рассмотреть модель конкуренции двух фирм. Построить графики изменения оборотных средств.

## Материалы и методы

- Модель эффективности рекламы
- Язык программирования Julia
- Язык программирования Openmodelica

# Задание

Случай 1. Рассмотрим две фирмы, производящие взаимозаменяемые товары одинакового качества и находящиеся в одной рыночной нише. Считаем, что в рамках нашей модели конкурентная борьба ведётся только рыночными методами. То есть, конкуренты могут влиять на противника путем изменения параметров своего производства: себестоимость, время цикла, но не могут прямо вмешиваться в ситуацию на рынке («назначать» цену или влиять на потребителей каким-либо иным способом.) Будем считать, что постоянные издержки пренебрежимо малы, и в модели учитывать не будем. В этом случае динамика изменения объемов продаж фирмы 1 и фирмы 2 описывается следующей системой уравнений: $$ \frac{\partial M*1}{\partial\theta}=M_1-\frac{b}{c_1}M_1M_2-\frac{a_1}{c_1}M_1^2;\frac{\partial M_2}{\partial\theta}=\frac{c_2}{c_1}M_2-\frac{b}{c_1}M_1M_2- \frac{a_2}{c_1}M_2^2 $$ где $$ a_1=\frac{p*{cr}}{\tau_1^2\tilde{p}1^2Nq},a_2=\frac{p{cr}}{\tau_2^2\tilde{p}2^2Nq},b=\frac{p{cr}}{\tau_1^2\tilde{p}\_1^2\tau_2^2\tilde{p}2^2Nq},c_1=\frac{p{cr}-\tilde{p}\_1}{\tau_1\tilde{p}1},c_2=\frac{p{cr}-\tilde{p}\_2}{\tau_2\tilde{p}2} $$ Также введена нормировка $t=c_1\theta$<br>
Случай 2. Рассмотрим модель, когда, помимо экономического фактора влияния (изменение себестоимости, производственного цикла, использование кредита и т.п.), используются еще и социально-психологические факторы – формирование общественного предпочтения одного товара другому, не зависимо от их качества и цены. В этом случае взаимодействие двух фирм будет зависеть друг от друга, соответственно коэффициент перед $ M_1M_2 $ будет отличаться. Пусть в рамках рассматриваемой модели динамика изменения объемов продаж фирмы 1 и фирмы 2 описывается следующей системой уравнений: $$ \frac{\partial M_1}{\partial \theta}=M_1-\frac{b}{c_1}M_1M_2-\frac{a_1}{c_1}M_1^2;\frac{\partial M_2}{\partial\theta}=\frac{c_2}{c_1}M_2-(\frac{b}{c_1}+0.0015)M_1M_2-\frac{a_2}{c_1}M_2^2 $$ Для обоих случаев рассмотрим задачу со следующими начальными условиями и параметрами: $$ M_0^1=2.6,M_0^2=1.9,p{cr}=19,N=17.5,q=1,\tau_1=12,\tau_2=16,\tilde{p}\_1=10,\tilde{p}2=6.6 $$ **Замечание: **Значения $ p{cr},\tilde{p}\_1,\_2,N $ указаны в тысячах единиц, а значения $ M_1,\_2 $ указаны в млн. единиц.

Обозначения:<br> $ N $ – число потребителей производимого продукта <br>$ \tau $ – длительность производственного цикла <br>$ p $ – рыночная цена товара <br>$ \tilde{p} $ – себестоимость продукта, то есть переменные издержки на производство единицы продукции<br> $ q $ – максимальная потребность одного человека в продукте в единицу времени<br> $ \theta=\frac{t}{c_1} $ – безразмерное время

1.Постройте графики изменения оборотных средств фирмы 1 и фирмы 2 без учета постоянных издержек и с веденной нормировкой для случая 1.

2.Постройте графики изменения оборотных средств фирмы 1 и фирмы 2 без учета постоянных издержек и с веденной нормировкой для случая 2.

## Выполнение лабораторной работы

##### 1.1 Решение для случая 1 на Julia:

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
  N = 17.5
  p_cr = 19
  p1 = 10
  p2 = 6.6
  tau1 = 12
  tau2 = 16
  q = 1
  M_1 = 2.6
  M_2 = 1.9
end


begin
  a1 = p_cr/(tau1*tau1*p1*p1*N*q)
  a2 = p_cr/(tau2*tau2*p2*p2*N*q)
  b = p_cr/(tau1*tau1*tau2*tau2*p1*p1*p2*p2*N*q)
  c1 = (p_cr-p1)/(tau1*p1)
  c2 = (p_cr-p2)/(tau2*p2)
end


function F!(du, u, p, t)
  du[1] = u[1] - (b/c1)*u[1]*u[2] - (a1/c1)*u[1]*u[1]
  du[2] = (c2/c1)*u[2] - (b/c1)*u[1]*u[2] - (a2/c1)*u[2]*u[2]
end


begin
  U0 = [2.6, 1.9]
  T = [0, 30]
  prob = ODEProblem(F!, U0, T)
end
```

![sol №1(Julia)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab8/report/report/image/Sol1_Julia.PNG){#fig:001 width=70%}

```julia
sol = solve(prob, saveat = 0.01)
```

![Граф №1(Julia)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab8/report/report/image/Graph1_Julia.PNG){#fig:002 width=70%}

```julia
Plots.plot(sol)
```

##### 1.2 Решение для случая 1 на Openmodelica:

```openmodelica
model Lab8Part1

constant Real N=17.5;
constant Real q=1;
constant Real p_cr=19;
constant Real p1=10;
constant Real p2=6.6;
constant Real tau1=12;
constant Real tau2=16;

constant Real a1 = p_cr/(tau1*tau1*p1*p1*N*q);
constant Real a2 = p_cr/(tau2*tau2*p2*p2*N*q);
constant Real b = p_cr/(tau1*tau1*tau2*tau2*p1*p1*p2*p2*N*q);
constant Real c1 = (p_cr-p1)/(tau1*p1);
constant Real c2 = (p_cr-p2)/(tau2*p2);

Real M1;
Real M2;

initial equation
M1=2.6;
M2=1.9;

equation
der(M1)=M1-(b/c1)*M1*M2-(a1/c1)*M1*M1;
der(M2)=(c2/c1)*M2-(b/c1)*M1*M2-(a2/c1)*M2*M2;
end Lab8Part1;
```

![Граф №1(OPM)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab8/report/report/image/OPM1.PNG){#fig:003 width=70%}

##### 2.1 Решение для случая 2 на Julia:

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
  N = 17.5
  p_cr = 19
  p1 = 10
  p2 = 6.6
  tau1 = 12
  tau2 = 16
  q = 1
  M_1 = 2.6
  M_2 = 1.9
end


begin
  a1 = p_cr/(tau1*tau1*p1*p1*N*q)
  a2 = p_cr/(tau2*tau2*p2*p2*N*q)
  b = p_cr/(tau1*tau1*tau2*tau2*p1*p1*p2*p2*N*q)
  c1 = (p_cr-p1)/(tau1*p1)
  c2 = (p_cr-p2)/(tau2*p2)
end


function F!(du, u, p, t)
  du[1] = u[1] - ((b/c1)+0.0015)*u[1]*u[2] - (a1/c1)*u[1]*u[1]
  du[2] = (c2/c1)*u[2] - (b/c1)*u[1]*u[2] - (a2/c1)*u[2]*u[2]
end


begin
  U0 = [2.6, 1.9]
  T = [0, 30]
  prob = ODEProblem(F!, U0, T)
end
```

![sol №2(Julia)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab8/report/report/image/Sol2_Julia.PNG){#fig:004 width=70%}

```julia
sol = solve(prob, saveat = 0.01)
```

![Граф №2(Julia)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab8/report/report/image/Graph2_Julia.PNG){#fig:005 width=70%}

```julia
Plots.plot(sol)
```

##### 2.2 Решение для случая 2 на Openmodelica:

```openmodelica
model Lab8Part2
constant Real N=17.5;
constant Real q=1;
constant Real p_cr=19;
constant Real p1=10;
constant Real p2=6.6;
constant Real tau1=12;
constant Real tau2=16;

constant Real a1 = p_cr/(tau1*tau1*p1*p1*N*q);
constant Real a2 = p_cr/(tau2*tau2*p2*p2*N*q);
constant Real b = p_cr/(tau1*tau1*tau2*tau2*p1*p1*p2*p2*N*q);
constant Real c1 = (p_cr-p1)/(tau1*p1);
constant Real c2 = (p_cr-p2)/(tau2*p2);

Real M1;
Real M2;

initial equation
M1=2.6;
M2=1.9;

equation
der(M1)=M1-((b/c1)+0.0015)*M1*M2-(a1/c1)*M1*M1;
der(M2)=(c2/c1)*M2-(b/c1)*M1*M2-(a2/c1)*M2*M2;
end Lab8Part2;
```

![Граф №2(OPM)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab8/report/report/image/OPM2.PNG){#fig:006 width=70%}

# Вывод

В ходе выполнения лабораторной работы я рассмотрел модель конкуренции двух фирм. Построил графики изменения оборотных средств и проанализировал их.
