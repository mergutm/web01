# Cuerpo del documento y estilo del canvas
Además se agrega en el body un componente canvas con dimensiones de 500x500
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Laberinto </title>
    <style>
        canvas {
            background-color: lightgray;
            border: 1px solid #000;
            display: block;
            margin: 20px auto;
        }
    </style>
</head>
<body>
  <h1 style="text-align: center;">Laberinto Interactivo</h1>
  <canvas id="laberintoCanvas" width="500" height="500"></canvas>


</body>
</html>
```


Se verá una pantalla como la siguiente:

<img src='https://github.com/mergutm/web01/blob/main/animacion/img/lab01.png'>


# Creación del laberinto

Se declara dentro de una nueva sección de JavaScript una variable que contendrá la configuración del laberinto:

```javascript
 // Matriz que representa el laberinto
        const laberinto = [
            [0, 1, 0, 0, 0, 0, 0, 0, 0, 2],
            [0, 1, 1, 0, 0, 2, 2, 2, 0, 0],
            [0, 0, 0, 0, 3, 0, 0, 2, 2, 0],
            [0, 1, 1, 1, 3, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 3, 0, 1, 1, 1, 0],
            [0, 2, 2, 0, 3, 0, 0, 0, 0, 0],
            [0, 2, 0, 0, 0, 0, 1, 0, 0, 0],
            [0, 0, 0, 1, 1, 1, 1, 0, 0, 0],
            [0, 3, 0, 0, 0, 0, 0, 0, 2, 0],
            [0, 0, 0, 3, 3, 3, 3, 0, 0, 0]
        ];
```


## Obtención de la referencia al canvas


```javascript
        // referencia al canvas
        const canvas = document.getElementById('laberintoCanvas');
        const ctx = canvas.getContext('2d');
```
## Definición del tamaño de cada celda 

```javascript
        // Tamaño de cada celda en píxeles ancho y alto
        const cellWidth = 50; 
        const cellHeight = 50; 
```

### El código hasta este momento luce como el siguiente

Estos últimos cambios no afectan a lo que se puede ver en la página Web

```javascript
 <script>
        // Matriz que representa el laberinto
        const laberinto = [
            [0, 1, 0, 0, 0, 0, 0, 0, 0, 2],
            [0, 1, 1, 0, 0, 2, 2, 2, 0, 0],
            [0, 0, 0, 0, 3, 0, 0, 2, 2, 0],
            [0, 1, 1, 1, 3, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 3, 0, 1, 1, 1, 0],
            [0, 2, 2, 0, 3, 0, 0, 0, 0, 0],
            [0, 2, 0, 0, 0, 0, 1, 0, 0, 0],
            [0, 0, 0, 1, 1, 1, 1, 0, 0, 0],
            [0, 3, 0, 0, 0, 0, 0, 0, 2, 0],
            [0, 0, 0, 3, 3, 3, 3, 0, 0, 0]
        ];

        // referencia al canvas
        const canvas = document.getElementById('mazeCanvas');
        const ctx = canvas.getContext('2d');

        // Tamaño de cada celda en píxeles ancho y alto
        const cellWidth = 40; 
        const cellHeight = 40; 

    </script>
```

# Dibujado de los elementos del laberinto

Se definen los colores para los espacios vacíos, obstáculos y el jugador.

```javascript
        // Colores para cada tipo de celda
        const colores = {
            0: '#ffffff', // Espacios vacíos (blanco)
            1: '#ff9999', // Obstáculo tipo 1 (rojo claro)
            2: '#99ccff', // Obstáculo tipo 2 (azul claro)
            3: '#99ff99', // Obstáculo tipo 3 (verde claro)
            jugador: '#ffff00' // Posición actual del jugador (amarillo)
        };

```

## Función para detectar una posición valida inicial para el jugador

Se declara una función para recorrer por filas/columnas todas las posiciones del laberinto hasta encontrar alguna donde haya un 0.

```javascript
        // Encontrar una posición inicial válida (cualquier celda con valor 0)
        function obtener_posicion_inicial() {
            for (let row = 0; row < laberinto.length; row++) {
                for (let col = 0; col < laberinto[row].length; col++) {
                    if (laberinto[row][col] === 0) {
                        //posición detectada
                        return { row, col };
                    }
                }
            }
            // regresa la esquina superior izquierda si no hay alguna posición donde hay un 0
            return { row: 0, col: 0 }; 
        }

```
Se obtiene en la variable pos_jugador la posición inicial, a partir de la búsqueda en la matriz original

```javascript
        pos_jugador = obtener_posicion_inicial();
        console.log(pos_jugador);
```


## Primera versión de dibujado del laberinto

En este código, se recorre toda la matriz por filas y columnas.

Del laberinto, dada la fila y columna, se obtiene el valor, y usando el diccionario de colores se tendría el color a usar para mostrar el rectángulo que determina cada obstáculo.

```javascript
 
// Dibujar el laberinto
function dibujar_laberinto1() {
    for (let row = 0; row < laberinto.length; row++) {
        for (let col = 0; col < laberinto[row].length; col++) {
            const valor_en_celda = laberinto[row][col];
            const x = col * cellWidth;
            const y = row * cellHeight;

            // Dibujar la celda
            ctx.fillStyle = colores[valor_en_celda];
            ctx.fillRect(x, y, cellWidth, cellHeight);

        }
    } 
}
```
* Las posiciones x, y se obtienen usando el índice de filas y columnas (col, row) que generan valores de 0,1,2..., etc. Adicionalmente se multiplican por el ancho y alto de cada celda.
* `ctx.fillStyle` determina el valor con el que se dibujará el rectángulo, usando el color tomado de la matriz.
* `ctx.fillRect(x, y, cellWidth, cellHeight);` dibuja el rectángulo en el canvas.


```javascript
// Dibujar la celda
ctx.fillStyle = colores[valor_en_celda];
ctx.fillRect(x, y, cellWidth, cellHeight);
```

Al ejecutar esta función, la salida obtenida del laberinto es:

<img src='https://github.com/mergutm/web01/blob/main/animacion/img/lab02.png'>


## Mejoras en bordes
Se pueden agregar bordesa cada celda del laberinto usando la función `strokeRect`.
Antes de dibujar cada borde, se especifica el color con el que se dibujará, en este caso es negro: `#000` o `#000000`

```javascript
// Dibujar bordes
ctx.strokeStyle = '#000';
ctx.strokeRect(x, y, cellSize, cellSize);
```



La función de dibujado del laberinto sería asi:


```javascript

// Dibujar el laberinto
function dibujar_laberinto2() {
    for (let row = 0; row < laberinto.length; row++) {
        for (let col = 0; col < laberinto[row].length; col++) {
            const valor_en_celda = laberinto[row][col];
            const x = col * cellWidth;
            const y = row * cellHeight;

            // Dibujar la celda
            ctx.fillStyle = colores[valor_en_celda];
            ctx.fillRect(x, y, cellWidth, cellHeight);

            // Dibujar bordes
            ctx.strokeStyle = '#000';
            ctx.strokeRect(x, y, cellWidth, cellHeight);
        }
    } 
}
```

Obteniendo la siguiente salida:

<img src='https://github.com/mergutm/web01/blob/main/animacion/img/lab03.png'>

### Posición del jugador

* Se obtiene la posición del jugador
* Se calcula la posición superior izquierda de la celda que identifica al jugador.
* Se dibuja el rectángulo usando el color que se definió para el jugador (amarillo).

```javascript
// Dibujar la posición del jugador después de mostrar el laberinto
const { row, col } = pos_jugador;
const x = col * cellWidth;
const y = row * cellHeight;
ctx.fillStyle = colores.jugador;
ctx.fillRect(x, y, cellWidth, cellHeight);
```
Al agregar todo a la función, se obtiene la siguiente función.

```javascript
// Dibujar el laberinto
function dibujar_laberinto3() {
    for (let row = 0; row < laberinto.length; row++) {
        for (let col = 0; col < laberinto[row].length; col++) {
            const valor_en_celda = laberinto[row][col];
            const x = col * cellWidth;
            const y = row * cellHeight;

            // Dibujar la celda
            ctx.fillStyle = colores[valor_en_celda];
            ctx.fillRect(x, y, cellWidth, cellHeight);

            // Dibujar bordes
            ctx.strokeStyle = '#000';
            ctx.strokeRect(x, y, cellWidth, cellHeight);
        }
    } 
   
    // Dibujar la posición del jugador después de mostrar el laberinto
    const { row, col } = pos_jugador;
    console.log(pos_jugador);
    const x = col * cellWidth;
    const y = row * cellHeight;
    ctx.fillStyle = colores.jugador;
    ctx.fillRect(x, y, cellWidth, cellHeight);
}

```
Se apreciará el siguiente laberinto en pantalla: 

<img src='https://github.com/mergutm/web01/blob/main/animacion/img/lab04.png'>
Notar el rectángulo amarillo que indica la posición del jugador.


# Evento para capturar teclas 

Haciendo un paréntesis, existen 2 eventos que se pueden usar para detectar cuando se presiona una tecla en un componente de HTML. 

Este código puede probarse en la consola o agregarse al código en JS. No es parte de la versión final, pero puede agregarse para debugear.

```javascript
// Agregar un evento global para detectar la tecla presionada
document.addEventListener("keydown", (event) => {
    console.log(`Tecla presionada: ${event.key}`);
});

// Agregar un evento global para detectar la tecla presionada
document.addEventListener("keyup", (event) => {
    console.log(`Tecla liberada: ${event.key}`);
});
```

| Evento   | Cuándo se dispara                          | Detecta teclas especiales (Ctrl, Shift, etc.) |
|----------|--------------------------------------------|-----------------------------------------------|
| keydown  | Al presionar una tecla                     | Sí                                            |
| keyup    | Al soltar una tecla                        | Sí                                            |
| keypress | Al presionar una tecla que genera carácter | No (Obsoleto)                                 |

### Estos son los eventos que pueden usarse para detectar acciones por teclado.
* `keydown`: Se dispara cuando el usuario presiona una tecla.
Se activa una vez, incluso si la tecla se mantiene presionada.
Útil para capturar la tecla en el momento exacto en que comienza a presionarse.
* `keyup`: Se dispara cuando el usuario libera una tecla.
Útil para detectar acciones después de que se completa la pulsación.
* `keypress`: Similar a keydown, pero se activa sólo para teclas que generan un carácter visible. **Nota**: Este evento está obsoleto y se desaconseja su uso. Se sugiere usar keydown o keyup. 




## Detección de las teclas de dirección

El siguiente código permite detectar el evento de presionar una tecla de dirección, haciendo uso de una función para hacer cambios y validaciones en el laberinto.

```javascript
// Escuchar eventos de teclado
document.addEventListener('keydown', (event) => {
    if (event.key === 'ArrowUp') mover_jugador('up');
    else if (event.key === 'ArrowDown') mover_jugador('down');
    else if (event.key === 'ArrowLeft') mover_jugador('left');
    else if (event.key === 'ArrowRight') mover_jugador('right');
});
```

Cabe notar que al presionar una tecla de direccion se llama a una función que tiene por objetivo redibujar y validar la posición del jugador en el laberinto.


```javascript
function mover_jugador(direction) {
    const { row, col } = pos_jugador;
    let newRow = row;
    let newCol = col;

    // Calcular nueva posición
    if (direction === 'up') newRow--;
    else if (direction === 'down') newRow++;
    else if (direction === 'left') newCol--;
    else if (direction === 'right') newCol++;

    // Validar límites del laberinto y evitar obstáculos
    if (
        newRow >= 0 && newRow < laberinto.length &&
        newCol >= 0 && newCol < laberinto[0].length &&
        //solo se puede mover a sitios donde es "camino"
        laberinto[newRow][newCol] === 0
    ) {
        pos_jugador = { row: newRow, col: newCol };
    }

    // Redibujar el laberinto
    dibujar_laberinto3();
}
```




## Usar la función para desplegar el laberinto al final del código JS para poder mostrar el laberinto y la posición del jugador.

Sin esto, no se desplegará el laberinto.

```javascript
dibujar_laberinto3();
```

Haga los cambios que considere.
