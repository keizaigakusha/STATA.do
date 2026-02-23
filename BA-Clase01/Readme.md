* Sesion 1
* STATA Basico 
* INEI
* Version Febrero 2026
******************************

cls
clear all
sysdescribe auto.dta

* esto es un comentario
* str18 ---> str = string-->cadena
* 18 el numero de caracteres

sysuse auto.dta

* sys --- sistema
* sysdescribe auto.dta

br
help sum

sum price
sum price if price<= 6165.257

sum price
sum rep78

help ed


replace price = . in 1
replace trunk = . in 11
replace weight = . in 20
replace weight = . in 21
replace weight = . in 22

sum
sum , detail
sum price, detail
hist price

* pregunta 1
* evaluacion docente
* ranKing
* 10 profesores
* nota para cada uno de los profesores
* ¿Cual es un buena medida de nota hacia el profesor la media o la mediana?
* cual es el test que conocen que mida si una variable tiene distribucion normal o no?

* test de normalidad
* Ho: Normalidad
* H1: No normalidad
* Test jarque bera: jb
* nivel maximo permitido
* nivel de error
* 5%, 10% o 1%
help jb
findit jb
help jb
jb price
* 0.0000000081
* nivel 5%
* 0.05
* Regla de decision
* Si probabilidad = p_value<0.05
* si 0.0000000081< 0.05 aceptar h1
*  H1: No normalidad
summarize price
summarize mpg  rep78 weight price
jb price
jb rep78
jb mpg
hist mpg
sum mpg, detail
hist mpg
jb mpg
histogram mpg
graph box mpg

* segunda pregunta?
* probreza a nivel nacional es del 30%
* tesis sobre pobreza
* distrito xdt
* pobreza en el distrito xdt es del 25%
*¿El resultado es confiable??
* un estimador es una carateristica
* tiene que cumplir ciertos requisitos
* requisitos
* 1) insesgado
* 2) consistente
* 3) eficiente
sum price
sum mpg
cls
clear all
ed

*Creando nueva tabla ¿El consumo de alcohol afecta la productividad en agricultores?
set obs 1
generate var1 = 1 in 1
set obs 2
replace var1 = 1.5 in 2
set obs 3
replace var1 = 0.5 in 3
set obs 4
replace var1 = 0.8 in 4
set obs 5
replace var1 = 2 in 5
set obs 6
replace var1 = 4 in 6
set obs 7
replace var1 = 1.2 in 7
set obs 8
replace var1 = 0.8 in 8
set obs 9
replace var1 = 1 in 9
set obs 10
replace var1 = 1.5 in 10
generate var2 = 1000 in 1
replace var2 = 2000 in 2
replace var2 = 500 in 3
replace var2 = 600 in 4
replace var2 = 800 in 5
replace var2 = 500 in 6
replace var2 = 900 in 7
replace var2 = 500 in 7
replace var2 = 600 in 8
replace var2 = 800 in 9
replace var2 = 200 in 10

rename var1 litros_cons
label variable litros_cons "litros de consumo promedio de alcohol"
rename var2 hact
label variable hact "kilos cosechados por hectarea"

ed
tab litros_cons
tab hact
tabulate litros_cons hact
tabulate litros_cons hact, cell
tabulate litros_cons hact, cell nofreq
tabulate litros_cons hact, row nofreq
tabulate litros_cons hact, col nofreq
tabulate litros_cons hact, row col cell nofreq

* Correlaciones
* grado de asociacion
* correlacion = pearson
* si p(rho) se aproxima a +1, existe una fuerte asociacion positiva
* si p(rho) se aproxima a -1, existe una fuerte asociacion negativa
* si p(rho) se aproxima a 0, no existe ni asociacion positiva ni negativa
cor litros_cons hact

* test de chi-cuadrado de person
* H0: varaiables independientes no se asocian, no tienen relacion
* H1: dependencia
* 5% de error

tabulate litros_cons hact, cell nofreq chi
* Rd: Si p_value = 0.345< 0.05 aceptaria H1, pero es mayor aceptar H0.
* Ho indepencia

save "C:\Users\user\Desktop\Stata_Basico\base_1_prueba.dta", replace


cd
cd "C:\Users\user\Desktop\Stata_Basico"
cls
clear all
sysuse auto.dta
twoway (line price mpg)

* generar una variable
help generate
g id = _n
ed

* graficos sencillos
twoway (line price mpg) (line price id)
twoway (line price id)
twoway (line mpg id)
twoway (line mpg id) (line price id)
twoway (scatter price mpg)
cor price mpg
g dd = price+ mpg
replace dd = price + weight

* pregunta 3
* cual es tu edad? encuesta abierta
* podrias decime en que rango esta tu edad? 1) de 20- 30a, 2) 31 a + años
* Pregunta?? cual usarian en su encuesta y porque??
*Yo Steven usaria la encuesta abierta para tener los datos exactos y ya luego poder categorizar la variable edad 
sum mpg
sum mpg, detail
g mpg_rango = 1 if mpg<20
replace mpg_rango = 2 if mpg>=20 & mpg<25
replace mpg_rango = 3 if mpg>=25
tab mpg_rango
ed

* -----→
label variable mpg_rango "rango de la variable mpg"
label define rango 1 "Desde 0 a 20 mpg" 2 "Mas de 20 y menos de 25 mpg" 3 "mas de 25 mpg"
label values mpg_rango rango
* ------
tab mpg_rango
findit jb
help jb
hist mpg

*Estableciendo formato de gráficos scheme 
help scheme
findit scheme
set scheme sj
hist mpg
set scheme economist
hist mpg

* guardar
graph save "Graph" "C:\Users\user\Desktop\Stata_Basico\Sesion_1\Graph0_01.gph"
graph matrix price mpg
 graph matrix price mpg weight turn displacement
graph pie rep78


help egen
egen promedio = mean(price)
ed
ed
move promedio make
ed
tab rep78
sum rep78
help egen
sum rep78
return list
ed
scalar define d = r(N)
scalar list d
scalar define T = r(sum)
scalar list T
mean price
proportion rep78
ratio (price/mpg), fvwrap(1)
total price
total mpg
