---
## Front matter
title: " презентаця лабораторной работе №4"
subtitle: "Модель гармонических колебаний"
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

- Научиться решать линейное однородное дифференциальное уравнение второго порядка.
- Построить модель линейного гармонического осциллятора без затухания/ с затуханием/ с действием внешней силы.
- Построить фазовые портреты всех моделей.
- Отработать навыки решения систем дифференциальных уравнений на языке Julia, Openmodelica

# Задание

Постройте фазовый портрет гармонического осциллятора и решение уравнения
гармонического осциллятора для следующих случаев

1. Колебания гармонического осциллятора без затуханий и без действий внешней
   силы $$\ddot{x} + 14.4x = 0$$
2. Колебания гармонического осциллятора c затуханием и без действий внешней
   силы
   $$\ddot{x} + 17\dot{x} + x = 0$$
3. Колебания гармонического осциллятора c затуханием и под действием внешней
   силы
   $$\ddot{x} + 15\dot{x} + x = 0.7*sin(3*t)$$
   На интервале
   $$t \in [0; 31]$$
   (шаг 0.05) с начальными условиями
   x0 = 2, y0 = -0.2

# Теоретическое введение

**Гармонический осциллятор** - система, которая при выведении ее из положения равновесия испытывает действие возращающей силы $F$, пропорциональной смещению $x$: $$F = -kx$$

Если $F$ — единственная сила, действующая на систему, то систему называют **простым** или **консервативным гармоническим осциллятором**. Свободные колебания такой системы представляют собой периодическое движение около положения равновесия (гармонические колебания). Частота и амплитуда при этом постоянны, причём частота не зависит от амплитуды.

Если имеется ещё и сила трения (затухание), пропорциональная скорости движения (вязкое трение), то такую систему называют **затухающим** или **диссипативным осциллятором**. Если трение не слишком велико, то система совершает почти периодическое движение — синусоидальные колебания с постоянной частотой и экспоненциально убывающей амплитудой. Частота свободных колебаний затухающего осциллятора оказывается несколько ниже, чем у аналогичного осциллятора без трения.

Если осциллятор предоставлен сам себе, то говорят, что он совершает **свободные колебания**. Если же присутствует внешняя сила (зависящая от времени), то говорят, что осциллятор испытывает **вынужденные колебания**.

Механическими примерами гармонического осциллятора являются математический маятник (с малыми углами отклонения), груз на пружине, торсионный маятник и акустические системы. Среди немеханических аналогов гармонического осциллятора можно выделить электрический гармонический осциллятор. @Oscilliator

Для _консервативного гармонического осциллятора_, используя второй закон Ньютона, имеем @Lab:
$$F = -kx \iff a = -\frac{k}{m}x$$
Обозначим ${{\omega}_0}^2 = \frac{k}{m}$ и подставим
$$\ddot{x} + {{\omega}_0}^2x = 0$$

Для _затухающего гармонического осциллятора_, используя второй закон Ньютона, имеем @Lab:
$$F = -kx - \alpha{v} \iff a = -\frac{k}{m}x - \frac{\alpha}{m}v$$
Обозначим ${{\omega}_0}^2 = \frac{k}{m}$, $2\gamma = \frac{\alpha}{m}$ и подставим
$$\ddot{x} + {{\omega}_0}^2x + 2\gamma{\dot{x}}= 0$$

Для _вынужденных колебаний_, используя второй закон Ньютона, имеем @Oscilliator:
$$ma = -kx + F_0cos(\Omega{t}) \iff a = -\frac{k}{m} + \frac{F_0}{m}cos(\Omega{t})$$
Обозначим ${{\omega}_0}^2 = \frac{k}{m}$, ${\Phi}_0 = \frac{F_0}{m}$ и подставим
$$\ddot{x} + {{\omega}_0}^2x = {\Phi}_0cos(\Omega{t})$$

Для решения данных дифференциальных уравнений будем составлять систему, в которой обозначим $\dot{x}$ за $y$, будем оставлять без изменений.

Например, для затухающих гармонических колебаний имеем:

$$
\begin{cases}
   \dot{x} = y
   \\
   \dot{y} = -{{\omega}_0}^2x - 2\gamma{y}
 \end{cases}
$$

**Фазовая плоскость** — координатная плоскость, в которой по осям координат откладываются какие-либо две переменные (фазовые координаты), однозначно определяющие состояние системы второго порядка.

Каждая точка фазовой плоскости отражает одно некоторое состояние системы и называется _фазовой, изображающей или представляющей_ точкой

Изменение состояния системы отображается на фазовой плоскости движением этой точки. След от движения изображающей точки называется **фазовой траекторией**.

Полная совокупность всевозможных различных фазовых траекторий — это **фазовый портрет**.

В нашей задаче на оси абсцисс фазовой плоскости откладываются значения параметра $x$, например, величина отклонения от равновесия; на оси ординат откладываются значения первой производной $x$ по времени($y$) — скорости перемещения. @Phase

# Выполнение лабораторной работы

1. Построим решение уравнения гармонического осциллятора без затухания. В нашем случае оно имеет вид $$\ddot{x} + 14.4x = 0$$
   Для того, чтобы решить данное уравнение перепишем его в виде системы из двух дифференциальных уравнений первого порядка:
   $$
   \begin{cases}
      \dot{x} = y
      \\
      \dot{y} = -14.4x
    \end{cases}
   $$
   Далее запишем начальные условия, которые в нашем случае имеют вид:
   $$
   \begin{cases}
      x(0) = 2
      \\
      y(0) = -0.2
    \end{cases}
   $$

Теперь используем язык программирования Julia для получения численного решения и построения фазового портрета

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
    X0 = 2.0
    Y0 = -0.2
    w = 14.4
    t0 = 0.0
    tmax = 31.0
    tspan = (t0, tmax)
end

function F!(du, u, p, t)
    du[1] = u[2]
    du[2] = -w*u[1]
end

begin
    U0 = [X0, Y0]
    T = [t0, tmax]
    prob = ODEProblem(F!, U0, T)
end
```

![sol](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab4/report/report/image/Capture1.PNG){#fig:001 width=70%}

```julia
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
    #график
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

![Фазовый портрет №1(Julia)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab4/report/report/image/Capture2.PNG){#fig:002 width=70%}

проделаем те же самые действия в Openmodelica

```modelica
model Lab4
Real x(start=2);
Real y(start=-0.2);
parameter Real w =14.4;
equation
der(x) = y;
der(y) = -w*x;
end Lab4;
```

Получим фазовый портрет
![Фазовый портрет №1](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab4/report/report/image/Open1.PNG){#fig:003 width=70%}

2. Построим решение уравнения для колебаний гармонического осциллятора с затуханием и без действий внешней силы.
   В нашем случае оно имеет вид $$\ddot{x} + 17\dot{x} + x = 0$$
   Для того, чтобы решить данное уравнение перепишем его в виде системы из двух дифференциальных уравнений первого порядка:
   $$
   \begin{cases}
      \dot{x} = y
      \\
      \dot{y} = -17y - x
    \end{cases}
   $$
   Далее запишем начальные условия, которые в нашем случае имеют вид:
   $$
   \begin{cases}
      x(0) = 2
      \\
      y(0) = -0.2
    \end{cases}
   $$
   Теперь используем язык программирования Julia для получения численного решения и построения фазового портрета

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
    X0 = 2.0
    Y0 = -0.2
    g = 17.0
    w = 1.0
    t0 = 0.0
    tmax = 31.0
    tspan = (t0, tmax)
end

function F!(du, u, p, t)
    du[1] = u[2]
    du[2] = -g*u[2]-w*u[1]
end

begin
    U0 = [X0, Y0]
    T = [t0, tmax]
    prob = ODEProblem(F!, U0, T)
end
```

![Sol](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab4/report/report/image/Capture2.1.PNG){#fig:004 width=70%}

```julia
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
    #график
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

![Фазовый портрет №2(Julia)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab4/report/report/image/Capture2.2.PNG){#fig:005 width=70%}

проделаем те же самые действия в Openmodelica

```modelica
model Lab4Pt2
Real x(start=2);
Real y(start=-0.2);
parameter Real g = 17;
parameter Real w =1;
equation
der(x) = y;
der(y) = -g*y-w*x;
end Lab4Pt2;
```

Получим фазовый портрет
![Фазовый портрет №2](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab4/report/report/image/Open2.PNG){#fig:006 width=70%}

3. Построим решение уравнения для колебаний гармонического осциллятора с затуханием и под действием внешней силы.
   В нашем случае оно имеет вид $$\ddot{x} + 15\dot{x} + x = 0.7*sin(3*t)$$
   Для того, чтобы решить данное уравнение перепишем его в виде системы из двух дифференциальных уравнений первого порядка:
   $$
   \begin{cases}
      \dot{x} = y
      \\
      \dot{y} = -15y - x + 0.7*sin(3*t)
    \end{cases}
   $$
   Далее запишем начальные условия, которые в нашем случае имеют вид:
   $$
   \begin{cases}
      x(0) = 2
      \\
      y(0) = -0.2
    \end{cases}
   $$
   Теперь используем язык программирования Julia для получения численного решения и построения фазового портрета

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
    X0 = 2.0
    Y0 = -0.2
    g = 15.0
    w = 1.0
    t0 = 0.0
    tmax = 31.0
    tspan = (t0, tmax)
end

function P(t)
    return 0.7*sin(3*t)
end

function F!(du, u, p, t)
    du[1] = u[2]
    du[2] = -g*u[2]-w*u[1] + P(t)
end

begin
    U0 = [X0, Y0]
    T = [t0, tmax]
    prob = ODEProblem(F!, U0, T)
end
```

![Sol](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab4/report/report/image/Capture3.1.PNG){#fig:007 width=70%}

```julia

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
    #график
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

![Фазовый портрет №3(Julia)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab4/report/report/image/Capture3.2.PNG){#fig:008 width=70%}

проделаем те же самые действия в Openmodelica

```modelica
model Lab4Pt3
Real x(start=2);
Real y(start=-0.2);
parameter Real g = 15;
parameter Real w = 1;
equation
der(x) = y;
der(y) = -g*y-w*x + 0.7*sin(3*time);
end Lab4Pt3;
```

Получим фазовый портрет
![Фазовый портрет №3](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab4/report/report/image/Open3.PNG){#fig:009 width=70%}

# Ответы на вопросы

1. Простейшая модель гармонических колебаний:
   $$x = A*cos(\omega{t} + {\phi}_0)$$
2. Осциллятор - система, совершающая колебания, то есть показатели которой периодически повторяются во времени.
3. Модель математического маятника:
   $$\ddot{\theta} + \frac{g}{L}sin(\theta) = 0$$
4. Для перехода от дифференциального уравнения второго порядка к системе дифференциальных уравнений первого порядка, необходимо в уравнении заменить все производные первого порядка на новую переменную, а также записать дополнительное уравнения, в котором будет обозначено, что первая производная равна новой переменной
5. **Фазовый портрет** — это геометрическое представление траекторий динамической системы на фазовой плоскости

6. **Фазовая траектория** - кривая в фазовом пространстве, составленная из точек, представляющих состояние динамической системы в последовательные моменты времени в течение всего времени эволюции.

# Выводы

- Научились решать линейное однородное дифференциальное уравнение второго порядка.
- Построиили модель линейного гармонического осциллятора без затухания/ с затуханием/ с действием внешней силы.
- Построили фазовые портреты всех моделей. Увидели, что при реализации на Julia и Openmodelica портреты совпадают.
- Отработали навыки решения систем дифференциальных уравнений на языке Julia, Openmodelica

# Список литературы{.unnumbered}

::: {#refs}
:::
