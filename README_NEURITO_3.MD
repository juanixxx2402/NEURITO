

## Experimento 1. Comportamiento sin entrenamiento

  -----------------------------------------------------------------------
  Indicador                                                     Resultado
  ------------------------------ ----------------------------------------
  Exactitud                                                        21.1 %

  Llega a la comida                                                    No

  Comportamiento                       Choca frecuentemente con paredes y
                                                       repite movimientos
  -----------------------------------------------------------------------

**Conclusión:** sin entrenamiento la red todavía no ha aprendido una
correspondencia útil entre el estado del laberinto y el movimiento
recomendado. Esta prueba funciona como línea base.

## Experimento 2. Influencia del número de épocas

    Épocas   Pérdida   Exactitud
  -------- --------- -----------
       100     0.039      90.5 %
       200     0.041      89.1 %
       400     0.011      91.3 %

**Observación:** la exactitud baja ligeramente entre 100 y 200 épocas y
luego aumenta con 400. La pérdida más baja también corresponde a 400
épocas.

**Conclusión:** 400 épocas ofrecieron el mejor resultado de los tres
valores evaluados. Más épocas no garantizan una mejora en cada punto
intermedio.

## Experimento 3. Red pura frente a navegación asistida

Configuración: 400 épocas.

  Indicador               Asistida        Red pura
  --------------------- ---------- ---------------
  Épocas                       400             400
  Tiempo reportado           1 min   No registrado
  Pérdida                    0.010           0.011
  Exactitud                 91.2 %          90.8 %
  Comidas conseguidas           65              33

**Observación:** el modo asistido consiguió 65 comidas frente a 33 de la
red pura. La diferencia de exactitud fue pequeña, pero el número de
comidas fue considerablemente mayor con las reglas de asistencia.

**Conclusión:** la exactitud por sí sola no describe todo el desempeño.
Las reglas que evitan paredes y penalizan visitas repetidas pueden
mejorar la navegación.

> El tiempo de un minuto solo fue reportado para la prueba asistida; no
> es una comparación de tiempos entre ambos modos.

## Experimento 4. Número de neuronas ocultas

Configuración: 400 épocas, tasa de aprendizaje 0.45 y semilla 42.

    Neuronas ocultas   Pérdida   Exactitud
  ------------------ --------- -----------
                  16     0.034      87.4 %
                  32     0.016      89.2 %
                  96     0.010      91.5 %
                 128     0.010      90.5 %

**Observación:** al pasar de 16 a 96 neuronas mejora la exactitud y
disminuye la pérdida. Con 128 neuronas la pérdida se mantiene, pero la
exactitud baja respecto a 96.

**Conclusión:** 96 neuronas ofrecieron el mejor resultado de los tamaños
probados. Aumentar el tamaño de la capa oculta no produjo una mejora
adicional.

## Experimento 5. Tasa de aprendizaje

Configuración: 400 épocas, 96 neuronas ocultas y semilla 42.

    Tasa de aprendizaje   Pérdida   Exactitud
  --------------------- --------- -----------
                   0.01    0.6818      79.3 %
                    0.1     0.149      87.2 %
                   0.45     0.010      91.5 %
                    0.8     0.004      93.0 %

**Observación:** en estas ejecuciones, el aumento de la tasa desde 0.01
hasta 0.8 estuvo acompañado por una reducción de la pérdida y un aumento
de la exactitud.

**Conclusión:** 0.8 fue el mejor valor entre los evaluados. Esto no
demuestra que tasas aún mayores sean mejores; habría que probarlas y
revisar la estabilidad.

## Experimento 6. Generalización entre mapas

Configuración reportada: 400 épocas, 96 neuronas, tasa 0.45, semilla 42
y tiempo aproximado de un minuto.

    Mapa   Pérdida reportada   Exactitud   Comidas conseguidas
  ------ ------------------- ----------- ---------------------
       1               0.011      91.5 %                    24
       2               0.011      89.8 %                    24
       3              0.10\*      90.8 %                    24

\* Se conserva el valor `0.10` tal como fue comunicado. Conviene
verificar si el valor correcto era `0.010` o `0.10`, porque la
diferencia es significativa.

**Observación:** los tres mapas registraron 24 comidas conseguidas. La
exactitud fue mayor en el mapa 1 y menor en el mapa 2.

**Conclusión:** el agente obtuvo resultados similares de navegación en
los tres mapas incluidos en el entrenamiento. Esto demuestra
generalización a pares posición-meta no vistos, no a laberintos
completamente desconocidos.

## Experimento 7. Comparación de BFS, DFS y A\*

Objetivo: estudiar cómo influye el algoritmo que genera las etiquetas de
entrenamiento.

  Algoritmo profesor       Pérdida   Exactitud   Comidas conseguidas
  -------------------- ----------- ----------- ---------------------
  BFS                    Pendiente   Pendiente             Pendiente
  DFS                    Pendiente   Pendiente             Pendiente
  A\*                    Pendiente   Pendiente             Pendiente

**Estado:** pendiente de ejecución. No se incluyen resultados hasta
realizar las tres pruebas en condiciones comparables.

------------------------------------------------------------------------

## Conclusiones preliminares

1.  **El entrenamiento es fundamental:** la red sin entrenar obtuvo 21.1
    % de exactitud y no llegó a la comida en la prueba observada.
2.  **400 épocas dieron el mejor resultado** entre 100, 200 y 400 épocas
    en la comparación registrada.
3.  **La asistencia mejoró la navegación:** consiguió 65 comidas frente
    a 33 de la red pura en la prueba reportada.
4.  **96 neuronas ofrecieron el mejor resultado** entre los tamaños
    evaluados.
5.  **La tasa 0.8 fue la mejor probada**, con 93.0 % de exactitud y
    pérdida de 0.004.
6.  **Los tres mapas registraron 24 comidas** en la evaluación
    reportada, aunque hubo diferencias de exactitud.

Los resultados son preliminares y corresponden a las ejecuciones
registradas. Para afirmar que una configuración es consistentemente
superior, conviene repetir cada experimento con varias semillas y
comparar promedios.





