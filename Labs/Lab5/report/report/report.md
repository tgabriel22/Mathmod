---
## Front matter
title: "Отчет по лабораторной работе №5"
subtitle: "Модель хищник-жертва"
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

Постройте график зависимости численности хищников от численности жертв, а также графики изменения численности хищников и численности жертв при следующих начальных условиях: $$ x_0 = 12, y0 = 37 $$ Найдите стационарное состояние системы. @Lab

# Теоретическое введение

Простейшая модель взаимодействия двух видов типа «хищник — жертва» - модель Лотки-Вольтерры. Данная двувидовая модель основывается на следующих предположениях:

1.Численность популяции жертв x и хищников y зависят только от времени (модель не учитывает пространственное распределение популяции на занимаемой территории)

2.В отсутствии взаимодействия численность видов изменяется по модели Мальтуса, при этом число жертв увеличивается, а число хищников падает

3.Естественная смертность жертвы и естественная рождаемость хищника считаются несущественными

4.Эффект насыщения численности обеих популяций не учитывается

5.Скорость роста численности жертв уменьшается пропорционально численности хищников

$$
 \begin{cases}
   \frac{dx}{dt} = a*x(t) - b*x(t)*y(t)
   \\
   \frac{dy}{dt} = -c*y(t) + d*x(t)*y(t)
 \end{cases}
$$

В этой модели x — число жертв, y — число хищников. Коэффициент a описывает скорость естественного прироста числа жертв в отсутствие хищников, с — естественное вымирание хищников, лишенных пищи в виде жертв. Вероятность взаимодействия жертвы и хищника считается пропорциональной как количеству жертв, так и числу самих хищников (xy). Каждый акт взаимодействия уменьшает популяцию жертв, но способствует увеличению популяции хищников (члены — bxy и dxy в правой части уравнения).

![эволюция популяции жертв и хищников](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab5/report/report/image/Capture3.PNG){#fig:001 width=70%}

Математический анализ этой (жесткой) модели показывает, что имеется стационарное состояние (A на рис.1), всякое же другое начальное состояние (B) приводит к периодическому колебанию численности как жертв, так и хищников, так что по прошествии некоторого времени система возвращается в состояние B.

Стационарное состояние системы (положение равновесия, не зависящее от времени решение) будет в точке:

$$
x_0=\frac{c}{d},  y_0=\frac{a}{b}
$$

Если начальные значения задать в стационарном состоянии

$$
x(0)=x_0,y(0)=y_0
$$

то в любой момент времени численность популяций изменяться не будет. При малом отклонении от положения равновесия численности как хищника, так и жертвы с течением времени не возвращаются к равновесным значениям, а совершают периодические колебания вокруг стационарной точки. Амплитуда колебаний и их период определяется начальными значениями численностей x(0), y(0). Колебания совершаются в противофазе.

При малом изменении модели

$$
 \begin{cases}
   \frac{dx}{dt} = a*x(t) - b*x(t)*y(t)+\varepsilon f(x,y)
   \\
   \frac{dy}{dt} = -c*y(t) + d*x(t)*y(t)+\varepsilon g(x,y),\varepsilon \ll1
 \end{cases}
$$

(прибавление к правым частям малые члены, учитывающие, например, конкуренцию жертв за пищу и хищников за жертв), вывод о периодичности (возвращении системы в исходное состояние B), справедливый для жесткой системы Лотки-Вольтерры, теряет силу. Таким образом, мы получаем так называемую мягкую модель «хищник-жертва». В зависимости от вида малых поправок f и g возможны следующие сценарии 1-3 (рис.2).

![мягкая модель борьбы за существование](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab5/report/report/image/Capture4.PNG){#fig:002 width=70%}

В случае 1 равновесное состояние A устойчиво. При любых других начальных условиях через большое время устанавливается именно оно.

В случае 2 система стационарное состояние неустойчиво. Эволюция приводит то к резкому увеличению числа хищников, то к их почти полному вымиранию. Такая система в конце концов попадает в область столь больших или столь малых значений x и y, что модель перестает быть применимой.

В случае 3 в системе с неустойчивым стационарным состоянием A с течением времени устанавливается периодический режим. В отличие от исходной жесткой модели Лотки-Вольтерры, в этой модели установившийся периодический режим не зависит от начального условия. Первоначально незначительное отклонение от стационарного состояния A приводит не к малым колебаниям около A, как в модели Лотки-Вольтерры, а к колебаниям вполне определенной (и не зависящей от малости отклонения) амплитуды. Возможны и другие структурно устойчивые сценарии (например, с несколькими периодическими режимами).

Вывод: жесткую модель всегда надлежит исследовать на структурную устойчивость полученных при ее изучении результатов по отношению к малым изменениям модели (делающим ее мягкой).

В случае модели Лотки-Вольтерры для суждения о том, какой же из сценариев 1-3 (или иных возможных) реализуется в данной системе, совершенно необходима дополнительная информация о системе (о виде малых поправок f и g в нашей формуле). Математическая теория мягких моделей указывает, какую именно информацию для этого нужно иметь. Без этой информации жесткая модель может привести к качественно ошибочным предсказаниям. Доверять выводам, сделанным на основании жесткой модели, можно лишь тогда, когда они подтверждаются исследованием их структурной устойчивости.

# Выполнение лабораторной работы

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

![Sol (Julia)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab5/report/report/image/Capture6.PNG){#fig:003 width=90%}

```julia
sol = solve(prob, saveat = 0.05)
```

В результате работы программы получили следующие результат.

![Изменение числа хищников и хищников(JULIA)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab5/report/report/image/Capture5.PNG){#fig:004 width=90%}

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

![Изменение числа хищников и хищников(OM)](https://raw.githubusercontent.com/tgabriel22/mathmod/master/Labs/Lab5/report/report/image/Capture2.PNG){#fig:005 width=90%}

# Выводы

В ходе выполнения лабораторной работы я научился строить график зависимости численности хищников от численности жертв, а также графики изменения численности хищников и численности жертв при заданных начальных условиях. Нашел стационарное состояние системы.

# Список литературы{.unnumbered}

::: {#refs}
:::