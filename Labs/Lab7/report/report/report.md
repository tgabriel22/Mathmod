---
## Front matter
title: "Отчет по лабораторной работе №7"
subtitle: "Эффективность рекламы"
author: "Габриэль Тьерри"

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

# Цель работы

Построить графики распространения рекламы, определить в какой момент времени скорость распространения рекламы будет иметь максимальное значение.

# Задание

Постройте график распространения рекламы, математическая модель которой описывается следующим уравнением:

$$
\frac{\partial n}{\partial t}=(0.895+0.0000433n(t))(N-n(t))
$$

$$
\frac{\partial n}{\partial t}=(0.0000145+0.295n(t))(N-n(t))
$$

$$
\frac{\partial n}{\partial t}=(0.196sin(t)+0.699cos(t)n(t))(N-n(t))
$$

При этом объем аудитории N=1170 , в начальный момент о товаре знает 7 человек. Для случая 2 определите в какой момент времени скорость распространения рекламы будет иметь максимальное значение.

# Теоретическое введение

Организуется рекламная кампания нового товара или услуги. Необходимо, чтобы прибыль будущих продаж с избытком покрывала издержки на рекламу. Вначале расходы могут превышать прибыль, поскольку лишь малая часть потенциальных покупателей будет информирована о новинке. Затем, при увеличении числа продаж, возрастает и прибыль, и, наконец, наступит момент, когда рынок насытиться, и рекламировать товар станет бесполезным.

Предположим, что торговыми учреждениями реализуется некоторая продукция, о которой в момент времени t из числа потенциальных покупателей N знает лишь n покупателей. Для ускорения сбыта продукции запускается реклама по радио, телевидению и других средств массовой информации. После запуска рекламной кампании информация о продукции начнет распространяться среди потенциальных покупателей путем общения друг с другом. Таким образом, после запуска рекламных объявлений скорость изменения числа знающих о продукции людей пропорциональна как числу знающих о товаре покупателей, так и числу покупателей о нем незнающих.

Модель рекламной кампании описывается следующими величинами. Считаем, что dn/dt - скорость изменения со временем числа потребителей, узнавших о товаре и готовых его купить, t - время, прошедшее с начала рекламной кампании, n(t) - число уже информированных клиентов. Эта величина пропорциональна числу покупателей, еще не знающих о нем, это описывается следующим образом: $\alpha_1(t)(N-n(t))$ где N - общее число потенциальных платежеспособных покупателей, $\alpha_1(t)>0$ -характеризует интенсивность рекламной кампании (зависит от затрат на рекламу в данный момент времени). Помимо этого, узнавшие о товаре потребители также распространяют полученную информацию среди потенциальных покупателей, не знающих о нем (в этом случае работает т.н. сарафанное радио). Этот вклад в рекламу описывается величиной $\alpha_2(t)n(t)(N-n(t))$, эта величина увеличивается с увеличением потребителей узнавших о товаре. Математическая модель распространения рекламы описывается уравнением:

$$
\frac{\partial n}{\partial t}=(\alpha_1(t)+\alpha_2(t)n(t))(N-n(t))
$$

При $\alpha_1(t) >> \alpha_2(t)$ получается модель типа модели Мальтуса, решение которой имеет вид:

![ График решения уравнения модели Мальтуса](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab7/report/report/image/Teo1.PNG){#fig:001 width=70%}
График решения уравнения модели Мальтуса

В обратном случае, при $\alpha_1(t) << \alpha_2(t)$ получаем уравнение логистической кривой:
![График логистической кривой](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab7/report/report/image/Teo2.PNG){#fig:002 width=70%}
График логистической кривой

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
    N = 1170.0
    n0 = 7.0
    a1 = 0.895
    a2 = 0.0000433
    t0 = 0.0
    tmax = 30.0
end

begin
    U0 = [n0]
    T = [t0, tmax]
    prob = ODEProblem(F!, U0, T)
end

function F!(du, u, p, t)
    du[1] = (0.895 + 0.0000433*u[1])*(N-u[1])
end
```

![sol №1(Julia)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab7/report/report/image/JuliaPart1Sol.PNG){#fig:003 width=70%}

```julia
sol = solve(prob, saveat = 0.01)
```

![Граф №1(Julia)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab7/report/report/image/JuliaPart1Graph.PNG){#fig:004 width=70%}

```julia
Plots.plot(sol)
```

##### 1.2 Решение для случая 1 на Openmodelica:

```openmodelica
model Lab7Part1

constant Real a1 = 0.895;
constant Real a2 = 0.0000433;
constant Real N = 1170;

Real n;

initial equation
n = 7;

equation
der(n) = (a1+a2*n)*(N-n);

end Lab7Part1;
```

![Граф №1(Openmodelica)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab7/report/report/image/OM1.PNG){#fig:005 width=70%}

##### 1.3 Решение для случая 2 на Julia:

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
    N = 1170.0
    n0 = 7.0
    a1 = 0.0000145
    a2 = 0.295
    t0 = 0.0
    tmax = 30.0
end

begin
    U0 = [n0]
    T = [t0, tmax]
    prob = ODEProblem(F!, U0, T)
end


function F!(du, u, p, t)
    du[1] = (0.0000145 + 0.295*u[1])*(N-u[1])
end
```

![sol №2(Julia)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab7/report/report/image/JuliaPart2Sol.PNG){#fig:006 width=70%}

```julia
 sol = solve(prob, saveat = 0.01)
```

![Граф №2(Julia)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab7/report/report/image/JuliaPart2Graph.PNG){#fig:007 width=70%}

```julia
Plots.plot(sol)
```

Максимальное значение n достигается при time=0.006.

##### 1.4 Решение для случая 2 на Openmodelica:

```openmodelica
model Lab1Part2

constant Real a1 = 0.0000145;
constant Real a2 = 0.295;
constant Real N = 1170;

Real n;

initial equation
n = 7;

equation
der(n) = (a1+a2*n)*(N-n);

end Lab1Part2;
```

![Граф №2(Openmodelica)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab7/report/report/image/MO2.png){#fig:008 width=70%}

Максимальное значение n достигается при time=0.006.

##### 1.5 Решение для случая 3 на Julia:

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
    N = 1170.0
    n0 = 7.0
    t0 = 0.0
    tmax = 30.0
end


begin
    U0 = [n0]
    T = [t0, tmax]
    prob = ODEProblem(F!, U0, T)
end

function F!(du, u, p, t)
    du[1] = (0.196sin(t) + 0.699cos(t)*u[1])*(N-u[1])
end

```

![sol №3(Julia)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab7/report/report/image/JuliaPart3Sol.PNG){#fig:009 width=70%}

```julia
sol = solve(prob, saveat = 0.01)
```

![Граф №3(Julia)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab7/report/report/image/JuliaPart3Graph.PNG){#fig:010 width=70%}

```julia
 Plots.plot(sol)
```

##### 1.6 Решение для случая 3 на Openmodelica:

```openmodelica
model Lab7Part3

Real a1;
Real a2;
constant Real N = 1170;

Real n;

initial equation
n = 7;

equation
a1 = 0.196*sin(time);
a2 = 0.699*cos(time);
der(n) = (a1+a2*n)*(N-n);

end Lab7Part3;
```

![Граф №3(Openmodelica)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab7/report/report/image/OM3.PNG){#fig:011 width=70%}

# Вопросы к лабораторной работе

1.Записать модель Мальтуса (дать пояснение, где используется данная модель).

$$
\frac{\partial N}{\partial t}=rN
$$

где N — исходная численность населения, r — коэффициент пропорциональности, для которого r = b - d (b — коэффициент рождаемости, d — коэффициент смертности), t — время. Модель используется в экологии для расчета изменения популяции особей животных.

2.Записать уравнение логистической кривой (дать пояснение, что описывает данное уравнение).

$$
\frac{\partial P}{\partial t}=rP(1-\frac{P}{K})
$$

где r — характеризует скорость роста (размножения), K — поддерживающая ёмкость среды (то есть, максимально возможная численность популяции). Исходные предположения для вывода уравнения при рассмотрении популяционной динамики выглядят следующим образом: скорость размножения популяции пропорциональна её текущей численности, при прочих равных условиях; скорость размножения популяции пропорциональна количеству доступных ресурсов, при прочих равных условиях. Таким образом, второй член уравнения отражает конкуренцию за ресурсы, которая ограничивает рост популяции.

3.На что влияет коэффициенты $\alpha_1(t);\alpha_2(t)$ в модели распространения рекламы. $\alpha_1(t)$ — интенсивность рекламной кампании, зависящая от затрат.$\alpha_2(t)$ — интенсивность рекламной кампании, зависящая от сарафанного радио.

4.Как ведет себя рассматриваемая модель при $\alpha_1(t)>>\alpha_2(t)$ При данный условиях получается модель типа модели Мальтуса, решение которой имеет вид:

![ График решения уравнения модели Мальтуса](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab7/report/report/image/Teo1.PNG){#fig:012 width=70%}

5.Как ведет себя рассматриваемая модель при
$\alpha_1(t)<<\alpha_2(t)$
При данных условиях получаем уравнение логистической кривой.

![График логистической кривой](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab7/report/report/image/Teo2.PNG){#fig:013 width=70%}

# Вывод

В ходе выполнения лабораторной работы я научился строить графики распространения рекламы, определять в какой момент времени скорость распространения рекламы будет иметь максимальное значение.
