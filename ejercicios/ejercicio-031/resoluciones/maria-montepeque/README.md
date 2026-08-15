# Ejercicio 031: Autos de lujo

## Error encontrado

1. `calcularPromedio` devolvia `53.333...` en vez de `80`: el promedio se calculaba
   dividiendo la suma de puntos de los registros **activos** entre la cantidad **total**
   de registros (incluyendo los inactivos).
2. `obtenerMejor` devolvia `render-1` (puntos 40, el menor) en vez de `render-2` (puntos
   96, el mayor).

## Causa raiz

1. La funcion filtraba correctamente los registros activos y sumaba sus puntos
   (`activos.reduce(...)`), pero al dividir usaba `registros.length` (el total del
   arreglo original) en lugar de `activos.length` (solo los que realmente se contaron
   en la suma). Con 2 activos (90 y 70, suma 160) y 3 registros totales, `160 / 3` da
   `53.33`; lo correcto es `160 / 2 = 80`.
2. El comparador del `sort` era `(a, b) => a.puntos - b.puntos`, que ordena de forma
   ascendente. Al tomar el primer elemento (`[0]`) se obtenia el registro con **menor**
   puntaje en vez del de mayor.

## Cambio aplicado

1. Se cambio el divisor de `registros.length` a `activos.length`, para que el promedio
   se calcule sobre la misma poblacion que se sumo.
2. Se invirtio el comparador a `(a, b) => b.puntos - a.puntos` (orden descendente), de
   forma que `[0]` sea el registro con mayor puntaje. Se mantuvo el spread
   `[...registros]` para no mutar el arreglo original.

## Comando usado para validar

```bash
npm test -- ejercicios/ejercicio-031/tests/luxury-cars.test.ts
```

Como el test importa el archivo fijo de `codigo/`, tambien se valido copiando
temporalmente el test dentro de `tests/` apuntando a esta resolucion
(`../resoluciones/maria-montepeque/luxury-cars.ts`), ejecutando `vitest run` contra esa
copia y luego eliminandola (no se dejo ningun archivo extra fuera de esta carpeta).

## Resultado final

- `calcularPromedio(...)` devuelve `80` con el caso del test (90 y 70 activos, 30
  inactivo excluido).
- `obtenerMejor(...)` devuelve el registro `render-2` (puntos 96), el de mayor puntaje.
- Ambos casos del test coinciden con lo esperado.
