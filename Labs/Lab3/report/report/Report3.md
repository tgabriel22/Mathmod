---
## Front matter
title: "РОССИЙСКИЙ УНИВЕРСИТЕТ ДРУЖБЫ НАРОДОВ"
subtitle:
- Факультет физико-математических и естественных наук.

- Математическое моделирование.

- ОТЧЕТ по лабораторной работе № 3.

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

Изучить и отработать навыки работы с языками программирования Julia и Openmodelica. Освоить основные библиотеки данных языков для решения дифференциальных уравнений и построения графиков. Закрепить на практике полученные знания. Решить математическую задачу моделирования боевых действий.

# Задание

Между страной $X$ и страной $Y$ идет война. Численность состава войск исчисляется от начала войны, и являются временными функциями $x(t)$ и $y(t)$. В начальный момент времени страна $X$ имеет армию **_80000_**, а в распоряжении страны $Y$ армия численностью **_60000_** человек. Для упрощения модели считаем, что коэффициенты $a,b,c,h$ постоянны. Также считаем $P(t)$ и $Q(t)$ непрерывные функции.

Постройте графики изменения численности войск армии $X$ и армии $Y$ для следующих случаев:

1. Модель боевых действий между регулярными войсками.

$$\frac{dx}{dt} = -0.21x(t) - 0.855y(t) + sin(t)+2$$
$$\frac{dy}{dt} = -0.455x(t) - 0.32y(t) + cos(t)+2$$

2. Модель ведения боевых действий с участием регулярных войск и партизанских отрядов.

$$\frac{dx}{dt} = -0.267x(t) - 0.687y(t) + abs(sin(2t))$$
$$\frac{dy}{dt} = -0.349x(t)y(t) - 0.49y(t) + 2abs(cos(t)$$

# Теоретическое введение

**Julia** — высокоуровневый высокопроизводительный свободный язык про- граммирования с динамической типизацией, созданный для математических вычислений. Эффективен также и для написания программ общего назначения. Синтаксис языка схож с синтаксисом других математических языков (например, MATLAB и Octave), однако имеет некоторые существенные отличия. Julia написан на Си, C++ и Scheme. Имеет встроенную поддержку многопоточности и распределённых вычислений, реализованные в том числе в стандартных конструкциях.[1]

**Законы Ланчестера (законы Осипова — Ланчестера)** — математическая формула для расчета относительных сил пары сражающихся сторон — подразделений вооруженных сил. В статье «Влияние численности сражающихся сторон на их потери», опубликованной журналом «Военный сборник» в 1915 году, генерал-майор Корпуса военных топографов М. П. Осипов описал математическую модель глобального вооружённого противостояния, практически применяемую в военном деле при описании убыли сражающихся сторон с течением времени и, входящую в математическую теорию исследования операций, на год опередив английского математика Ф. У. Ланчестера. Мировая война, две революции в России не позволили новой власти заявить в установленном в научной среде порядке об открытии царского офицера.

# Выполнение лабораторной работы

Для моделирования данной задачи используем языки Julia и пакеты DifferentialEquations, Plots.

## Реализация на Julia

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
  X0 = 80000.0
  Y0 = 60000.0
  a = 0.21
  b = 0.855
  c = 0.455
  h = 0.32
end


function F!(du, u, p, t)
  du[1] = -a*u[1]-b*u[2]+sin(t)+2
  du[2] = -c*u[1]-h*u[2]+cos(t)+2
end

begin
  U0 = [X0, Y0]
  T = [0.0, 2.0]
  prob = ODEProblem(F!, U0, T)
end

sol = solve(prob, saveat = 0.05)

begin
  Time = sol.t
  const X = Float64[]
  const Y = Float64[]
  for u in sol.u
      x, y = u
      push!(X, x)
      push!(Y, y)
  end
  X,Y
end

begin
  #проста заготовка для будущих графикоф
  fig = Plots.plot(
    layout = (1, 1),
    dpi = 150,
    grid =:xy,
    gridcolor =:black,
    gridwidth = 1,
    background_color=:antiquewhite,
    # aspect_ratio=: equal,
    size = (800, 400),
    plot_title="графиk",
  )
  Plots.plot!(
    fig[1],
    Time,
    [X Y],
    xlabel = L"$t$",
    ylabel = L"$x(t)$, $y(t)$",
    color =[ :red :blue ],
    label = [L"$x(t)$" L"$y(t)$"]
  )
end


```

![Модель боевых действий #1(Julia)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab3/report/report/image/Part1.7.PNG){#fig:001 width=70%}

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
    X0 = 80000.0
    Y0 = 60000.0
    a = 0.267
    b = 0.687
    c = 0.349
    h = 0.491
end


function F!(du, u, p, t)
    du[1] = -a*u[1]-b*u[2]+abs(sin(2*t))
    du[2] = -c*u[1]*u[2]-h*u[2]+2*abs(cos(t))
end

begin
  U0 = [X0, Y0]
  T = [0.0, 2.0]
  prob = ODEProblem(F!, U0, T)
end

sol = solve(prob, saveat = 0.05)

begin
  Time = sol.t
  const X = Float64[]
  const Y = Float64[]
  for u in sol.u
      x, y = u
      push!(X, x)
      push!(Y, y)
  end
  X,Y
end

begin
  #проста заготовка для будущих графикоф
  fig = Plots.plot(
    layout = (1, 1),
    dpi = 150,
    grid =:xy,
    gridcolor =:black,
    gridwidth = 1,
    background_color=:antiquewhite,
    # aspect_ratio=: equal,
    size = (800, 400),
    plot_title="график",
  )
  Plots.plot!(
    fig[1],
    Time,
    [X Y],
    xlabel = L"$t$",
    ylabel = L"$x(t)$, $y(t)$",
    color =[ :red :blue ],
    label = [L"$x(t)$" L"$y(t)$"]
  )
end
```

![Модель боевых действий #2(Julia)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab3/report/report/image/Part2.4.PNG){#fig:002 width=70%}

# Выводы

Произведено численное моделирование модели боевых действий для двух случаев: без партизан и с партизанским движением. Для этого были применены языки программирования Julia и пакеты DifferentialEquations, Plots. Отработали навыки работы с вышеназванными языками программирования.
