# NEURITO --- Mi cerebro artificial

## Experimento 1. Mantener dos necesidades fijas y variar la tercera

Se plantearon cuatro pruebas para observar si la acción cambia cuando
aumenta una necesidad.

Prueba     Hambre   Energía   Aburrimiento Qué se buscaba observar

\---

A             0.2       0.9            0.2 Estado tranquilo inicial
B             0.8       0.9            0.2 Si empieza a predominar COMER
C             0.2       0.1            0.2 Si empieza a predominar DORMIR
D             0.2       0.9            0.8 Si empieza a predominar JUGAR

**Interpretación:** la prueba modifica una necesidad respecto al estado
inicial y permite comprobar si cambia la predicción. La regla sintética
favorece COMER cuando el hambre es la puntuación más alta, DORMIR cuando
`1 - energía` es la más alta y JUGAR cuando predomina el aburrimiento.

**Nota sobre los resultados:** los datos compartidos describen las
pruebas y lo que se pretendía observar, pero no incluyen una anotación
inequívoca de la acción final obtenida en cada caso. Se recomienda
completar esta sección con las observaciones de la interfaz, sin asumir
que la acción esperada fue necesariamente la predicha.

## Experimento 2. Comparar prioridades casi iguales

Se compararon tres combinaciones para analizar cómo cambia la salida de
Softmax cuando las puntuaciones ideales de las acciones son similares.

\---

Caso              Hambre       Energía   Aburrimiento Puntuaciones   Resultado
ideales:       registrado
comer, dormir,
jugar

\---

A                   0.60          0.40           0.20 0.60, 0.60,    COMER: 49.8
0.20           %

B                   0.60          0.50           0.20 0.60, 0.50,    COMER: 86.6
0.20           %

C                   0.60          0.30           0.20 0.60, 0.70,    DORMIR: 86.9
0.20           %
---

En el caso A, COMER y DORMIR tienen la misma puntuación ideal. En B, la
puntuación ideal de COMER es ligeramente superior a la de DORMIR. En C,
DORMIR tiene una puntuación ideal mayor.

**Interpretación:** los resultados registrados muestran que la acción
que encabeza la predicción cambia entre los casos. Las puntuaciones
ideales no son lo mismo que las probabilidades producidas por la red: la
red aprende una aproximación de la regla a partir de sus pesos y sesgos.

> En los resultados compartidos solo se indicó la probabilidad de la
> acción con mayor puntuación para cada caso. Los porcentajes de las
> tres salidas de Softmax, cuando se registren completos, deben sumar
> aproximadamente el 100 %.

## Experimento 3. Cambiar el número de neuronas ocultas

Se mantuvieron **1200 épocas** y se compararon cuatro tamaños de la capa
oculta.

&#x20;   Neuronas ocultas   Épocas   Pérdida final   Precisión de prueba

\---

&#x20;                  3     1200          0.1618               96.30 %
                   6     1200          0.1224               99.40 %
                  10     1200          0.1261               99.30 %
                  20     1200          0.1220               98.80 %


### Análisis

* Con **3 neuronas**, se obtuvo la precisión más baja de las cuatro
pruebas: `96.30 %`.
* Con **6 neuronas**, se logró una precisión de `99.40 %` y una
pérdida de `0.1224`.
* Con **10 neuronas**, la precisión fue de `99.30 %`, muy cercana a la
obtenida con 6.
* Con **20 neuronas**, se obtuvo la pérdida más baja (`0.1220`), pero
la precisión fue de `98.80 %`, menor que la de 6 neuronas.

**Conclusión:** aumentar el número de neuronas no garantiza una mejora
de la precisión. Para estos datos sintéticos, la configuración de 6
neuronas tuvo la mayor precisión registrada. Pequeñas diferencias entre
ejecuciones también pueden depender de la inicialización aleatoria de
los pesos y de los datos de prueba.

## Experimento 4. Cambiar las épocas y la tasa de aprendizaje

### A. Variar el número de épocas

Se mantuvieron la tasa de aprendizaje en `0.1` y la capa oculta en 6
neuronas.

&#x20;   Épocas   Pérdida final   Precisión de prueba

\---

&#x20;      100          0.8059               77.30 %
       500          0.2220               98.20 %
      1200          0.1224               99.40 %
      2000          0.0898               99.50 %


**Análisis:** al aumentar las épocas, la pérdida disminuyó y la
precisión mejoró en las pruebas registradas. El avance fue especialmente
notable entre 100 y 500 épocas. Entre 1200 y 2000 épocas, la mejora fue
pequeña: la precisión pasó de `99.40 %` a `99.50 %`.

### B. Variar la tasa de aprendizaje

Se mantuvieron 1200 épocas y 6 neuronas ocultas.

&#x20;   Tasa de aprendizaje   Pérdida final   Precisión de prueba

\---

&#x20;                 0.001          1.0333               41.80 %
                   0.01          0.7372               82.90 %
                    0.1          0.1224               99.40 %
                    1.0          0.0447               98.70 %


**Análisis:** con una tasa de `0.001`, el aprendizaje fue insuficiente
durante las 1200 épocas, con pérdida alta y precisión baja. Al subirla a
`0.01` y `0.1`, el rendimiento mejoró notablemente. Con `1.0`, la
pérdida registrada fue la menor, pero la precisión (`98.70 %`) quedó por
debajo de la obtenida con `0.1`.

**Conclusión:** una pérdida menor no siempre implica la mayor precisión
de prueba. La tasa de aprendizaje debe elegirse considerando el
comportamiento del entrenamiento y el desempeño en datos no usados para
ajustar los pesos.

## Experimento 5. ¿Qué datos y etiquetas necesitaría una cuarta acción?

Una cuarta acción posible sería **RECIBIR CARICIAS**. Para aprenderla,
la red necesitaría ejemplos con las tres entradas habituales ---hambre,
energía y aburrimiento--- y una etiqueta que indique cuándo debe
seleccionarse esa acción.

Ejemplo ilustrativo:

&#x20;   Hambre   Energía   Aburrimiento Etiqueta

\---

&#x20;      0.9       0.7            0.2 COMER
       0.2       0.1            0.3 DORMIR
       0.2       0.8            0.9 JUGAR
       0.1       0.8            0.2 RECIBIR CARICIAS


Los valores son solo ejemplos. Para que la cuarta acción tenga sentido,
habría que definir una regla clara o recopilar ejemplos etiquetados que
indiquen cuándo corresponde elegirla.

Los cambios de implementación incluirían:

1. Crear ejemplos y etiquetas para la cuarta acción.
2. Modificar la generación de `targets` para admitir cuatro clases.
3. Cambiar la capa de salida de tres a cuatro neuronas.
4. Actualizar la lista de acciones y la interfaz para mostrar la nueva
salida.
5. Entrenar y evaluar nuevamente la red con ejemplos representativos de
las cuatro clases.

Por ejemplo, la lista podría quedar así:

``` python
actions = ("eat", "sleep", "play", "be\_petted")
W2 = np.random.randn(hidden\_neurons, 4) \* 0.5
b2 = np.zeros((1, 4))
```

Este fragmento no completa por sí solo la implementación: también se
deben adaptar las etiquetas y el resto del código que depende de las
tres clases.

## Preguntas de análisis

### 1\. ¿En qué se diferencian los pesos y los sesgos?

Los pesos determinan la influencia de las conexiones entre entradas y
neuronas. Los sesgos son valores adicionales que se suman al cálculo de
cada neurona y permiten desplazar su activación. Ambos se ajustan
durante el entrenamiento para reducir la pérdida.

### 2\. ¿Por qué el cansancio es `1 - energía`?

La energía está normalizada entre 0 y 1. Al calcular `1 - energía`, se
obtiene una medida inversa: cuando la energía es baja, el valor de
cansancio es alto; cuando la energía es alta, el cansancio es bajo. Esto
permite que la política asigne mayor prioridad a DORMIR cuando queda
poca energía.

### 3\. ¿Qué hace ReLU y qué hace Softmax?

ReLU convierte los valores negativos en cero y conserva los positivos,
introduciendo no linealidad en la capa oculta. Softmax transforma las
salidas de la red en probabilidades que suman aproximadamente el 100 %,
facilitando la comparación entre COMER, DORMIR y JUGAR.

### 4\. ¿Por qué una precisión alta puede ser engañosa?

Una precisión alta indica que la red coincide con las etiquetas de
prueba, pero en este proyecto esas etiquetas se generan con una regla
sintética sencilla. Por eso, una precisión del 99 % demuestra que el
modelo aprendió a imitar esa política, pero no garantiza que tome buenas
decisiones en situaciones reales. También conviene analizar la pérdida,
los errores por clase y la representatividad de los datos.

## 

