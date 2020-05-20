# Inicio del proyecto

## Clase 3 - Agregamos funcionalidades al botón con un script

[Clase 3](https://platzi.com/clases/1642-javascript-profesional/22157-inicio-del-proyecto/)

Trabajaremos en el proyecto PlatziVideo, una plataforma de vídeo.

Esta es la base de nuestro proyecto y nos vamos a enfocar en el MediaPlayer. Durante el curso se desarrollarán plugins de forma modular para extender la funcionalidad del MediaPlayer.

El repositorio de este curso lo encuentras en: [https://github.com/platzi/javascript-profesional](https://github.com/platzi/javascript-profesional)

```html
<script>
      const video =  document.querySelector("video")
      const button = document.querySelector("button")

      function MediaPlayer(config) {
        this.media = config.el
      }

      MediaPlayer.prototype.play = function() {
        this.media.play();
      }

      MediaPlayer.prototype.pause = function(){
        this.media.pause()
      }

      const player = new MediaPlayer({ el: video });

      button.onclick = function (){
      (player.media.paused)
        ? player.play()
        : player.pause()
      console.log(player.media.paused)
    }

    </script>
```

## Clase 4 - Cómo llega un script al navegador

[Clase 4](https://platzi.com/clases/1642-javascript-profesional/22158-como-llega-un-script-al-navegador/)

El **DOM** es la representación que hace el navegador de un documento HTML.

El navegador interpreta el archivo HTML y cuando termina de transformarlo al DOM se dispara el evento DOMContentLoaded lo que significa que todo el documento está disponible para ser manipulado.

Todo script que carguemos en nuestra página tiene un llamado y una ejecución.

Tanto con async como defer podemos hacer llamados asíncronos pero tiene sus diferencias:

async. Con async podemos hacer la petición de forma asíncrona y no vamos a detener la carga del DOM hasta que se haga la ejecución del código.

defer. La petición es igual asíncrona como en el async pero va a deferir la ejecución del Javascript hasta el final de que se cargue todo el documento.

Hay que tener en cuenta que cuando carga una página y se encuentra un script a ejecutar toda la carga se detiene. Por eso se recomienda agregar tus scripts justo antes de cerrar el body para que todo el documento esté disponible.

### Script externo

Documento index.js

```html
 <script src="/assets/index.js"></script>
 ```

Documento externo en assets>index.js

```js
function MediaPlayer(config) {
    this.media = config.el;
  }

MediaPlayer.prototype.play = function() {
    this.media.play();
  }

MediaPlayer.prototype.pause = function() {
    this.media.pause()
  };

MediaPlayer.prototype.togglePlay = function() {
    if (this.media.paused) {
      this.play();
    } else {
      this.pause();
    }
  };

const video = document.querySelector('video');
const player = new MediaPlayer({ el: video });

const button = document.querySelector('button');
button.onclick = () => player.togglePlay();
```

## Clase 5 - Scope

[Clase 5](https://platzi.com/clases/1642-javascript-profesional/22159-scope/)

El Scope o ámbito es lo que define el tiempo de vida de una variable, en que partes de nuestro código pueden ser usadas.

### Global Scope

Variables disponibles de forma global se usa la palabra `var`, son accesibles por todos los scripts que se cargan en la página. Aquí hay mucho riesgo de sobreescritura.

```js
var message = 'Hello, Platzi!';

var $ = function (message) {
    console.log("Say: " + message);
    };
```

### Function Scope

Variables declaradas dentro de una función sólo visibles dentro de ella misma (incluyendo los argumentos que se pasan a la función).

```js
 function printNumbers() {
    var i;
    for(i = 0; i < 10; i++) {
    function eventuallyPrintNumber(n) {
        setTimeout(function() {
        console.log(n);
        }, 100);
    }
    eventuallyPrintNumber(i)
    }
};
printNumbers();
```

### Block Scope

Variables definidas dentro de un bloque, por ejemplo variables declaradas dentro un loop while o for. Se usa `let` y `const` para declarar este tipo de variables.

```js
function printNumbers2() {
    for(let i = 0; i < 10; i++) {
        setTimeout(function() {
        console.log(i);
        }, 100);
    }
};
printNumbers2();
```

### Module Scope

Cuando se denota un script de tipo module con el atributo `type="module"` las variables son limitadas al archivo en el que están declaradas.

#### Ejemplo

```html
<script type="module" src="/assets/index.js"></script>
```

Documento: index.js

```js
import MediaPlayer from './MediaPlayer.js';

const video = document.querySelector('video');
const player = new MediaPlayer({ el: video });

const button = document.querySelector('button');
button.onclick = () => player.togglePlay();

```

Documento: MediaPlayer.js

```js
function MediaPlayer(config) {
    this.media = config.el;
  }

MediaPlayer.prototype.play = function() {
    this.media.play();
  }

MediaPlayer.prototype.pause = function() {
    this.media.pause()
  };

MediaPlayer.prototype.togglePlay = function() {
    if (this.media.paused) {
      this.play();
    } else {
      this.pause();
    }
  };

export default MediaPlayer;
```

## Clase 6 - Closures

[Clase 6](https://platzi.com/clases/1642-javascript-profesional/22160-closures/)

Son funciones que regresan una función o un objeto con funciones que mantienen las variables que fueron declaradas fuera de su scope.

Los closures nos sirven para tener algo parecido a variables privadas, característica que no tiene JavaScript por default. Es decir encapsulan variables que no pueden ser modificadas directamente por otros objetos, sólo por funciones pertenecientes al mismo.

### Closures

```js

printColor

let color = 'red';

function printColor() {
  console.log(color);
}

printColor();

```

### IFFE

```js

(function() {
  let color = 'green iife';

  function printColor() {
  console.log(color);
  }

  printColor();
  })();

```

### Funciones que regresan funciones

```js
function makeColorPrinter(color) {
      let colorMessage = `The color is ${color}`;
      return function () {
        console.log(colorMessage)
      }
    }

let greenColorPrinter = makeColorPrinter('green');
console.log(greenColorPrinter());
```

### Variables privadas

```js
function makeCounter(n) {
  let count = n;

  return {
    increase: function () {
      count =  count + 1;
    },
    decrease: function () {
      count = count - 1;
    },
    getCount: function () {
      return count;
    },
  }
}

let counter = makeCounter(7);

console.log('The count is:', counter.getCount()); 
counter.increase();
console.log('The count is:', counter.getCount()); 
counter.decrease();
counter.decrease();
counter.decrease();
counter.decrease();
console.log('The count is:', counter.getCount());
counter.count = 0;
console.log('The count is:', counter.getCount());
  ```

## El primer plugin

[Clase 7](https://platzi.com/clases/1642-javascript-profesional/22161-el-primer-plugin/)

## Clase 8 - this

[Clase 9](https://platzi.com/clases/1642-javascript-profesional/22162-this/)

`this` se refiere a un objeto, ese objeto es el que actualmente está ejecutando un pedazo de código.

No se puede asignar un valor a this directamente y este depende de en que scope nos encontramos:

Cuando llamamos a this en el Global Scope o Function Scope, se hace referencia al objeto window. A excepción de cuando estamos en strict mode que nos regresará undefined.
Cuando llamamos a this desde una función que está contenida en un objeto, this se hace referencia a ese objeto.
Cuando llamamos a this desde una “clase”, se hace referencia a la instancia generada por el constructor.

// this en el scope global

```js
console.log(`this: ${this}`);
```

// this en el scope de una función

```js
function whoIsThis() {
  return this;
}

console.log(`whoIsThis(): ${whoIsThis()}`);
```

// this en el scope de una función en strict mode

```js
function whoIsThisStrict() {
  'use strict';
  return this;
}

console.log(`whoIsThisStrict(): ${whoIsThisStrict()}`);
```

// this en el contexto de un objeto

```js
const person = {
  name: 'Gabriel',
  saludar: function() {
    console.log(`Hola soy ${this.name}`);
  }
}

person.saludar();
```

// this cuando sacamos a una función de un objeto

```js
const accion = person.saludar;
accion();
```

// this en el contexto de una "clase"

```js
function Person (name) {
  this.name = name;
}

Person.prototype.saludar = function () {
  console.log(`Me llamo ${this.name}`);
}

const angela = new Person("Angela");
angela.saludar();
```

## Clase 9 - Los métodos call, apply y bind

[Clase 9](https://platzi.com/clases/1642-javascript-profesional/22163-los-metodos-call-apply-y-bind/)

Estas funciones nos sirven para establecer el valor de `this`, es decir cambiar el contexto que se va usar cuando la función sea llamada.

Las funciones call, apply y bind son parte del prototipo Function. Toda función esa este prototipo y por lo tanto tiene estas tres funciones.

- `functionName.call()` Ejecuta la función recibiendo como primero argumento el `this` y los siguientes son los argumentos que recibe la función que llamó a call.

- `functionName.apply()` Ejecuta la función recibiendo como primer argumento el `this` y como segundo un arreglo con los argumentos que reciba la función que llamó apply.

- `funtionName.bind()` Reciba como primer y único argumento el `this`. No ejecuta la función, sólo regresa otra función con el nuevo `this` integrado.

// Establece `this` usando `call`

```js
  function saludar() {
    console.log(`Hola, Soy ${this.name} ${this.apellido}`)
  }

  const carlos = {
    name: "Carlos",
    apellido: "Villalobos Arguello"
  }

    saludar.call(carlos);
  ```

// Establece `this` usando `call` y pasar argumentos a la función

```js
function caminar (metros, direccion) {
  console.log(`${this.name} camina ${metros} metros hacia ${direccion}.`)
}

caminar.call(carlos, 300, "norte");

```

// Establece `this` usando `apply` y pasar argumentos a la función

```js
  caminar.apply(carlos, [800, 'noreste']);
```

// caminar.apply(carlos, valores);

/*
  Call - comma
  Apply - arreglo
*/

// Establecer `this` en una nueva función usando `bind`

```js
const daniel = { name: "Daniel", apellido: "Sánchez" }
const danielSaluda = saludar.bind(daniel);
danielSaluda();

const danielCamina = caminar.bind(daniel)
danielCamina(1000, "este")

const danielCamina = caminar.bind(daniel, 2000, 'sur')
danielCamina()

const danielCamina = caminar.bind(daniel, 2000)
danielCamina('oeste');
```

// Cuándo es útil usar uno de estos métodos

```html

<body>
    <ul>
      <li><button class="call-to-action">Aprender</button></li>
      <li><button class="call-to-action">Aprender Más</button></li>
      <li><button class="call-to-action">¡Nunca parar de Aprender!</button></li>
    </ul>
    <script>
  
    const buttons = document.getElementsByClassName("call-to-action");
      // button.forEach(button => {
      //   button.onclick = () => alert('Nunca pares de aprender!')
      // })

      // NodeList
     Array.prototype.forEach.call(buttons, button => {
        button.onclick = () => alert('Nunca pares de aprender!');
      });

<script>

</body>
```

## Clase 10 - Prototype

[Clase 10](https://platzi.com/clases/1642-javascript-profesional/22164-prototype/)

En Javascript todo son objetos, no tenemos clases, no tenemos ese plano para crear objetos.

Todos los objetos “heredan” de un prototipo que a su vez hereda de otro prototipo y así sucesivamente creando lo que se llama la prototype chain.

La keyword new crea un nuevo objeto que “hereda” todas las propiedades del prototype de otro objeto. No confundir prototype con proto que es sólo una propiedad en cada instancía que apunta al prototipo del que hereda.