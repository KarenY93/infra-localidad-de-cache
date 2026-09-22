# Respuestas

Nombre y código: Karen Andrea Sanabria Gonzalez 202459413

## Parte 3: el paso

¿Hasta qué paso el tiempo baja menos que las operaciones, y a partir de cuál
baja al mismo ritmo? Relacione ese punto con el tamaño de un `int` y de una
línea de caché.

El tiempo baja menos que las operaciones hasta el paso 8-16 (el peor caso es la transición 8→16, donde el tiempo casi no cae: de 14,6 ms a 14,0 ms, mientras las operaciones se reducen a la mitad). A partir del paso 16 el tiempo empieza a bajar de forma más proporcional a las operaciones (por ejemplo 32→64 cae de 11,7 a 6,1 ms, casi a la mitad). Esto coincide con que 16 = tamaño de línea de caché (64 bytes) / tamaño de un `int` (4 bytes): por debajo de ese paso varios enteros comparten la misma línea traída a caché (hay reutilización), y desde ese paso en adelante cada acceso ya cae en una línea distinta, así que cada operación cuesta aproximadamente lo mismo (un miss) sin importar cuánto más grande sea el paso.

## Parte 4: speedup, eficiencia y la ley de Amdahl

Con los tiempos de `amdahl.txt`:

| Hilos | Total medido | Speedup medido | Eficiencia | Speedup según Amdahl |
|---:|---:|---:|---:|---:|
| 1 | 1684,2 | 1,00 | 1,00 | 1,00 |
| 2 | 1055,2 | 1,60 | 0,80 | 1,61 |
| 4 | 742,2 | 2,27 | 0,57 | 2,30 |
| 8 | 607,0 | 2,77 | 0,35 | 2,94 |

Fracción paralelizable `p` (parte paralela sobre el total, con un hilo):

p = 1270,5 / 1684,2
p = 0,754

Techo del speedup con esa `p`, `1 / (1 - p)`:

techo = 1 / (1 - 0,754)
techo = 1 / 0,246
techo = 4,07

¿Dónde se separa la columna medida de la que predice Amdahl, y qué lo
explica?

Comparando la columna de Speedup medido con el Speedup según Amdahl, en 2 hilos casi coinciden (1,60 vs 1,61), pero desde 4 hilos ya se nota la diferencia (2,27 vs 2,30) y en 8 hilos se hace más clara (2,77 vs 2,94), alejándose cada vez más del techo teórico de 4,07.

La causa es porque Amdahl asume que la parte paralela se divide en partes exactamente iguales entre los hilos, pero en la práctica eso no sucede. Ya que no toma en cuenta el overhead de sincronización entre hilos y la contención de memoria caché al acceder varios hilos a datos compartidos.

Calculos 

Sn = T1/Tn
En = Sn/n
Sn A = 1 / ( (1 - p) + p/n )

S2 = 1684,2 / 1055,2
S2 = 1,60
E2 = 1,60 / 2
E2 = 0,80
S2 A = 1 / ( (1 - 0,754) + 0,754/2)
S2 A = 1,61

S4 = 1684,2 / 742,2
S4 = 2,27
E4 = 2,27 / 4
E4 = 0,57
S4 A = 1 / ( (1 - 0,754) + 0,754/4)
S4 A = 2,30

S8 = 1684,2 / 607,0
S8 = 2,77
E8 = 2,77 / 8
E8 = 0,35
S8 A = 1 / ( (1 - 0,754) + 0,754/8)
S8 A = 2,94