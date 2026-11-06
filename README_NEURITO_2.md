# Neurito 2.0: experimentos de aprendizaje de saltos

## Descripción

En esta segunda parte se evalúa una red neuronal que aprende a decidir
cuándo saltar obstáculos que se aproximan a diferentes velocidades. La
red recibe tres características normalizadas: distancia al obstáculo,
velocidad y tiempo estimado hasta alcanzarlo. Su arquitectura inicial es
**3 → 12 → 1**, con ReLU en la capa oculta y sigmoide en la salida. El
aprendizaje es supervisado mediante ejemplos sintéticos etiquetados con
una ventana de tiempo de referencia.

## Experimento 1. Juego sin entrenamiento

  Indicador                               Resultado
  ------------------------ ------------------------
  Obstáculos encontrados                         20
  Obstáculos esquivados                           0
  Colisiones                                     20
  Épocas                                          0
  Precisión registrada                       44.9 %
  Pérdida                             No registrada
  Comportamiento             Sin entrenar; no salta

**Análisis:** sin entrenamiento, Neurito no aprendió a saltar y chocó
con los 20 obstáculos encontrados.

![Juego sin entrenamiento](images/salto-sin-entrenamiento.png)

## Experimento 2. Entrenamiento progresivo

    Épocas   Precisión         Pérdida Comportamiento
  -------- ----------- --------------- ------------------------
         0      44.9 %   No registrada Sin entrenar; no salta
       100     91.75 %          0.2476 Salta correctamente
       200      91.9 %          0.2416 Salta correctamente
       300      92.5 %          0.2216 Salta correctamente
       400      92.8 %          0.2162 Salta correctamente
       500      95.0 %          0.1213 Salta correctamente

**Análisis:** al aumentar las épocas, la pérdida disminuyó y la
precisión aumentó. En las pruebas registradas, Neurito comenzó a saltar
correctamente después del entrenamiento.

![Entrenamiento progresivo](images/salto-entrenamiento.png)

## Experimento 3. Número de neuronas ocultas

**Condición:** 200 épocas de entrenamiento.

    Neuronas ocultas   Pérdida   Precisión
  ------------------ --------- -----------
                   3    0.2356      91.9 %
                  12    0.2306      91.9 %
                  24    0.2384      92.3 %

**Análisis:** los resultados fueron cercanos. La red de 24 neuronas
obtuvo la precisión más alta (92.3 %), mientras que la de 12 neuronas
tuvo la pérdida más baja (0.2306). Una red más grande no fue
automáticamente mejor en todas las métricas.

![Comparación de neuronas](images/salto-neuronas.png)

## Experimento 4. Tasa de aprendizaje

**Condición:** 200 épocas de entrenamiento.

    Tasa de aprendizaje   Pérdida   Precisión
  --------------------- --------- -----------
                   0.01    0.6010      85.7 %
                    0.8    0.2306      91.9 %
                    1.2    0.2165      91.2 %

**Análisis:** con 0.01 el aprendizaje fue menos efectivo en las 200
épocas. La tasa 1.2 produjo la pérdida más baja, mientras que 0.8 obtuvo
la precisión más alta. La mejor configuración depende de la métrica que
se observe.

![Comparación de tasas](images/salto-tasa-aprendizaje.png)

## Experimento 5. Ventana de entrenamiento

**Objetivo:** comprobar cómo cambia la política de salto al modificar el
intervalo de tiempo hasta el obstáculo utilizado para generar las
etiquetas.

  Ventana (fotogramas)     Pérdida   Precisión Resultado en el juego
  ---------------------- --------- ----------- ----------------------------
  7--13                     0.2293      92.3 % Salta sin dificultad
  5--10                     0.4000      68.5 % Salta bien, casi sin error
  10--16                    0.4381      73.9 % No salta

**Análisis:** la ventana original de 7--13 fotogramas obtuvo la menor
pérdida y mayor precisión, además de un comportamiento adecuado. Con
5--10 la precisión bajó, aunque el comportamiento observado fue bueno.
Con 10--16 Neurito no saltó. Esto evidencia que las etiquetas influyen
en la política aprendida y deben ser compatibles con la física del
juego.

![Comparación de ventanas](images/salto-ventana-entrenamiento.png)

## Pregunta central

### ¿Por qué una red puede clasificar ejemplos con buena precisión y aun así chocar?

Una red puede obtener buena precisión porque clasifica correctamente
muchos ejemplos del conjunto de prueba, pero eso no garantiza que
siempre tome la decisión adecuada durante el juego. Evitar una colisión
también depende de la distancia y velocidad del obstáculo, del momento
en que se decide saltar, de la duración del salto, la gravedad y la
detección de colisiones. Además, los datos sintéticos representan
situaciones limitadas y el juego puede presentar combinaciones que la
red no aprendió. Por eso se deben evaluar tanto las métricas como el
comportamiento real en Pygame.

## Conclusiones

1.  Sin entrenamiento, Neurito no logró esquivar los obstáculos de la
    prueba inicial.
2.  El entrenamiento progresivo mejoró la precisión y redujo la pérdida;
    se observó que saltaba correctamente desde las primeras 100 épocas.
3.  Cambiar las neuronas ocultas produjo diferencias pequeñas; aumentar
    la red no garantizó mejores resultados en todas las métricas.
4.  La tasa 1.2 tuvo la menor pérdida, pero 0.8 alcanzó la mayor
    precisión en esta comparación.
5.  La ventana 7--13 fotogramas ofreció el mejor resultado conjunto de
    las tres ventanas probadas.
6.  La precisión por sí sola no demuestra que el juego funcione
    correctamente: también importa la interacción entre la decisión de
    la red y la física del salto.

## Tecnologías

-   Python
-   NumPy
-   Pygame
-   uv

## Ejecución

``` bash
uv sync
uv run saltar.py
```

## Controles

  Tecla       Función
  ----------- -----------------------------------------
  `1`         Entrenar una época
  `2`         Entrenar cien épocas
  `3`         Activar o pausar entrenamiento continuo
  `A`         Alternar control automático y manual
  `Espacio`   Saltar manualmente
  `C`         Reiniciar pista y contadores
  `R`         Reiniciar pesos y contadores
  `Esc`       Salir

## Evidencias

Guarda las capturas de los experimentos en `images/` con estos nombres:

-   `salto-sin-entrenamiento.png`
-   `salto-entrenamiento.png`
-   `salto-neuronas.png`
-   `salto-tasa-aprendizaje.png`
-   `salto-ventana-entrenamiento.png`

## Estructura sugerida

``` text
Neurito/
├── README.md
├── README_NEURITO_2.md
├── main.py
├── red-sencilla.py
├── salto_modelo.py
├── salto_juego.py
├── saltar.py
├── GUIA_SALTO.md
├── assets/
│   └── jugar_1.png
└── images/
    ├── salto-sin-entrenamiento.png
    ├── salto-entrenamiento.png
    ├── salto-neuronas.png
    ├── salto-tasa-aprendizaje.png
    └── salto-ventana-entrenamiento.png
```
