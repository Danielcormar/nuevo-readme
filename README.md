#Poincare

En este README.md se explica como funciona el código en lenguaje de Mathematica

En la primera linea se define el Hamiltoniano del sistema, escribiendolo de la siguiente manera

    
       H[px_, pz_, x_, z_] :=  1/2 (px^2 + pz^2) + 0.5*(0.75*x^2 + z^2 - 0.7*(x^2)*z + (9.81)^2)
    

Para despues en las siguientes dos lineas se hace su respectiva derivada respecto de x y de z, donde x es q1 y z es q2


    xpp = D[x[t], t, t] - 0.75*x[t]  + 0.7*(x[t])*(z[t]);
    zpp = D[z[t], t, t] - z[t] - 0.7*(x[t])^2;
    

Despues se define el número de pasos que hara el programa


    n = 140;
    list = Range[n];
    

despues de esto, se crea una lista de valores para usar dentro del Hamiltoniano, y de esa forma, tener una lista de valores dada por el Hamilotiano, tomando valores aleatorios reales entre un intervalo definido

    For[i = 1;, i < n + 1, i++, ics = {Random[Real, {-0.55, 0.55}], Random[Real, {-0.45, 0.45}], Random[Real, {-0.44, 0.9}], Random[Real, {-0.17, 0.9}]};
        inisol = Solve[H[ini, ics[[4]], ics[[1]], ics[[2]]] == 0.866, ini];
    inival = ini /. inisol;
	ics = {ics[[1]], ics[[2]], inival[[2]], ics[[4]]};

Para la solución ahora de las ecuaciones diferenciales, se define un tiempo inicial y un tiempo final de la siguiente forma

    TimeMin = 0;
    TimeMax = 12000;
    


Despues se realiza la solución de ecuaciones diferenciales con el comando `NDSolve`.

    Section =Reap[NDSolve[{xpp == 0, zpp == 0, x[0] == ics[[1]], z[0] == ics[[2]], x'[0]== ics[[3]], z'[0] == ics[[4]]}, {x, z}, {t, TimeMin, TimeMax}, MaxSteps -> \[Infinity], Method > {"EventLocator", "Event" -> x[t], "EventAction" :> Sow[{z[t], z'[t]}]}]][[2]];
	
Usando el Metodo de `EventLocator` siedo este un método en la libreria de Mathematica, ademas ahi mismo se definen las conidiciones iniciales para x, z, xpp, zpp. 

Al resolver el sistema de ecuaciones diferenciales, se hace una lista, la cual se lee con el comando `ListPlot``

     list[[i]] = ListPlot[Transpose[Table[Section, {j, Length[Section]}]][[1]][[1]],  PlotStyle -> {Black, Opacity[0.25], AbsolutePointSize[0.5]}]]
	 

ademas se grafica con el comando `Show`

    Show[list, PlotRange -> All, Frame -> True, Axes -> False, LabelStyle ->Directive[Black, Small], ImageSize -> Large, Background -> Lighter[Blue, 0.95]]

Dando tambien el estilo que se desea en la gráfica.
