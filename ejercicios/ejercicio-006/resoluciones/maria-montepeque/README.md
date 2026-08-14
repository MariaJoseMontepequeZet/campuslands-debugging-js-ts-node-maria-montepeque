# Ejercicio 006: Playlist de entrenamiento

## Nota sobre el ejercicio

El `README.md` base describe el objetivo como "filtrar canciones por energia sin perder
datos" (tema playlist de entrenamiento), pero el archivo real `codigo/playlist.js` y su
test `tests/playlist.test.js` no contienen logica de filtrado ni energia: son el mismo
contrato de `calcularResultado` (suma de puntos) y `ordenarRanking` (orden de mayor a
menor) que los ejercicios 001 a 005. Se corrigio el codigo tal como existe realmente,
sin modificar el test ni el archivo base.

## Error encontrado

1. `calcularResultado` devolvia `"10155"` (string concatenado con `join('')`) en vez de
   `30` (suma numerica).
2. `ordenarRanking` devolvia el ranking de menor a mayor puntaje en vez de mayor a menor.

## Causa raiz

1. La funcion usaba `datos.map(item => item.puntos).join('')`, que concatena los valores
   como texto sin separador, transformando `10 + 15 + 5` en el string `"10155"` en lugar
   de sumarlos numericamente.
2. El comparador del `sort` era `(a, b) => a.puntos - b.puntos` (orden ascendente). El
   ranking debe mostrar primero a quien tiene mas puntos, es decir, orden descendente.

## Cambio aplicado

1. Se reemplazo `.map(...).join('')` por `.reduce((total, item) => total + item.puntos, 0)`
   para sumar los puntos como numeros.
2. Se invirtio el comparador a `(a, b) => b.puntos - a.puntos`, manteniendo el spread
   `[...jugadores]` para no mutar el arreglo original.

## Comando usado para validar

```bash
npm test -- ejercicios/ejercicio-006/tests/playlist.test.js
```

Como el test importa el archivo fijo de `codigo/`, tambien se replico el caso manualmente
contra el archivo de esta carpeta:

```bash
node --input-type=module -e "
import { calcularResultado, ordenarRanking } from './ejercicios/ejercicio-006/resoluciones/maria-montepeque/playlist.js';
console.log(calcularResultado([{nombre:'ak47-master',puntos:10},{nombre:'rpg-tank',puntos:15},{nombre:'moto-racer',puntos:5}]));
console.log(ordenarRanking([{nombre:'novato',puntos:7},{nombre:'pro',puntos:22},{nombre:'elite',puntos:18}]).map(i=>i.nombre));
"
```

## Resultado final

- `calcularResultado(...)` devuelve `30`.
- `ordenarRanking(...)` devuelve el orden `['pro', 'elite', 'novato']`.
- Ambos casos del test coinciden con lo esperado.
