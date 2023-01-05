# Introducción a ECMAScript 2016 (es7)

Desde finales del 2014, incluso antes de que se terminara de definir ECMAScript 6 ya se estaba empezando a trabajar en la siguiente versión de ECMAScript, la versión 7, formalmente conocida como ECMAScript 2016, ya que se espera este lista en el 2016.

Al igual que ES6 en ES7 se van a venir varias incorporaciones al lenguajes, algunas van a ser nuevas funcionalidades y otras van a ser syntax sugar de características ya existentes.

Este artículo esta desactualizado, no todas las características aquí explicadas fueron incorporadas finalmente en ECMAScript 2016, algunas serán implementadas en ECMAScript 2017 o en el futuro. Además la versión de Babel usada al final esta desactualizada.

## Async/Await
La primera de las nuevas características que trae ES7 es conocida como Async/Await o simplemente funciones asíncronas. Estas nuevas funciones están construidas combinando las Promesas y los Generadores de ES6.

La idea de las funciones asíncronas es tener una forma más fácil de definir funciones que trabajen de forma asíncrona y permitir a los developers usar try/catch, return y throw como si fuesen funciones síncronas.

Para poder crear una función asíncrona necesitamos agregar la palabra clave async, esto ya convierte nuestra función en una función asíncrona y nos habilita a usar la palabra clave await dentro de nuestra función.

Await detiene la ejecución de nuestra función asíncrona hasta que otra función asíncrona se complete, para esto agregamos await adelante del nombre de la función a ejecutar, es necesario que la función que esperamos devuelve una promesa.

~~~js
import fetch from 'whatwg-fetch';
async function getData() {
  try {
    const response = await fetch('/api/resource');
    return response.body;
  } catch (error) {
    throw error;
  }
}
getData()
  .then(::console.log)
  .catch(::console.error);
~~~

En este ejemplo podemos ver una función asíncrona llamada getData la cual intenta hacer una petición AJAX usando fetch a /api/resource, si la petición devuelve un error se realiza un throw del error, en caso de que se complete correctamente la respuesta de la petición se asigna a la constante response y luego devolvemos el cuerpo de la respuesta.

Por último ejecutamos nuestra función asíncrona, todas las funciones asíncronas devuelven una promesa y podemos trabajar sobre estas como cualquier promesa, ya sea usando then y catch o ejecutándola desde otra función asíncrona como hicimos con fetch.

Esta funcionalidad ya esta soportada por defecto en Babel.js.

## Function Bind Syntax

Esta nueva característica consiste en un syntax sugar para la forma en que hacemos call sobre una función para ejecutarla y hacer un bind del this. Con esta nueva sintaxis realizar un call es tan simple como escribir lo que queremos usar de this seguido de :: y la función que queremos ejecutar, pasándole cualquier parámetro que queramos.

Para entenderlo bien veamos un ejemplo de la forma actual:

~~~js
const inputs = document.querySelectorAll('input');
const map = Array.prototype.map;
const values = map.call(inputs, element => element.value);
~~~

En este código estamos obteniendo un listado de todos los input del sitio, esto nos devuelve un objeto que es una instancia de un NodeList, al no ser una instancia de un Array no tenemos acceso a los métodos de estos.

Así que para poder mapear nuestro NodeList y obtener un nuevo Array con los valores de cada input estamos haciendo un call sobre el método map extraído del prototipo de un Array para ejecutarlo indicándole que use nuestro NodeList como su this y ejecute nuestra función que devuelve el value de cada input.

Ahora este mismo código realizado con la nueva sintaxis sería así:

~~~js
const inputs = document.querySelectorAll('input');
const map = Array.prototype.map;
const values = inputs::map(element => element.value);
~~~

Como se puede ver la nueva sintaxis nos permite ejecutar la función map de forma similar a un método, solo que en vez de usar . estamos usando ::. Esto ayuda a que sea más fácil entender lo que se esta realizando en el código

## Operador exponencial **

Esta es bastante simple, desde ES7 va a ser posible usar ** para elevar un número X a una potencia Y. Haciendo una comparación con la forma actual:

~~~js
const result = Math.pow(2,3);
~~~

Forma nueva:

~~~js
const result = 2 ** 3;
~~~

Esta funcionalidad ya esta soportada por defecto en Babel.js.

## Array.prototype.includes
Este nuevo método de los Array es un syntax sugar que nos permite fácilmente saber si un elemento X esta dentro de un array, algo que actualmente haríamos con un indexOf de la siguiente forma:

~~~js
const isInArray = [1,2,3].indexOf(2) !== -1;
~~~

Con el nuevo método quedaría más fácil de entender que hace el código:

~~~js
const isInArray = [1,2,3].includes(2);
~~~

Al igual que con el operador :: esto hace más fácil de entender que hace el código.

## Object.observe y Array.observe

Una de la más interesantes características nuevas, la cual incluso ya funciona en Chrome y Opera, es Object.observe y su equivalente Array.observe. Este método de los objetos Object y Array (en sus respectivas instancias no existen) nos permite definir funciones que se queden escuchando cada vez que haya un cambio sobre un objeto o array sin necesidad de que estos tengan un eventListener definido.

Para poder usar este método es simplemente ejecutarlo pasándole dos parámetros, el primero es el objeto o array a observer y el segundo es la función que se va a ejecutar cuando haya cambios, esta función va a recibir un array de todos los cambios que se hayan realizado.

~~~js
const persona = {
  nombre: ‘Freddy’,
  empresa: ‘Mejorando.la’,
};
Object.observe(persona, changes => {
  changes.forEach(console.log);
});
setTimeout(() => persona.empresa = ‘Platzi’, 2500);
~~~

En el ejemplo anterior creamos un objeto persona con dos propiedades, luego empezamos a escuchar los cambios que se realicen sobre este objeto y ejecutamos una función que va a recorrer el listado de cambios que recibimos y va a mostrarlos en consola. Por último después de 2.5 segundos modificamos una de las propiedades de nuestro objeto.

Ahora en nuestra consola vamos a ver un objeto similar a este:

~~~js
{
  name: ‘empresa’,
  object: {
    nombre: ‘Freddy’,
    empresa: ‘Platzi’
  },
  oldValue: ‘Mejorando.la’,
  type: ‘update’
}
~~~

Acá podemos ver el nombre de la propiedad que cambio, el estado del objeto actualmente con los cambios aplicados, el valor anterior de la propiedad que cambio y el tipo de cambio que se realizo.

En este caso el tipo fue update esto quiere decir que una de las propiedades del objeto se modificó, otros posibles tipos de cambios son add cuando se agrega una nueva propiedad (en este caso no existe la propiedad oldValue) y el otro es delete cuando se borra una propiedad del objeto.

El uso de Object.observe y Array.observe nos permitiría hacer data binding sobre objetos y arrays facilmente sin necesidad de librerías y combinado con MutationObserver sería posible hacer un two-way data binding sin necesidad de hacer dirty checking como hace Angular v1.x.

Como detalle extra, Polymer usa Object.observe internamente y carga un polyfill para que funciones en otros navegadores que no sean Chrome y Opera.

## Propiedades de clases

En ES6 se incorporaban clases al lenguaje, aunque estas son en realidad un simple syntax sugar de la forma tradicional de hacer POO en JS. Algo que no se podía hacer con las clases de ES6 es agregar propiedades a la clase directamente, en lugar de eso había que hacer algo así:

~~~js
class Persona {
  saludar() {
    return `Hola soy ${this.nombre}`;
  }
}

Persona.nombre = ‘Sergio’;
~~~

En ES7 se agrega una mejora a estas clase permitiéndonos agregar propiedades directo dentro de la definición de la clase de la siguiente forma:

~~~js
class Persona {
  nombre = ‘Sergio’
  saludar() {
    return `Hola soy ${this.nombre}`;
  }
}
~~~

## Decoradores

Otra mejora relacionada a las clases es la implementación de decoradores que nos permiten extender la funcionalidad de una clase en tiempo de ejecución fácilmente.

Para esto una vez tenemos nuestra clase (como la definida en el punto ejemplo anterior) podemos crear la función decoradora y aplicársela a la definición de la clase agregando @nombre antes de la definición de la clase.

~~~js
function agregarDespedida(target) {
  target.prototype.despedirse = (persona) => {
    return `Adios ${persona.nombre}!`;
  }
  return target;
}
@agregarDespedida
class Persona {
  nombre = ‘Sergio’
  saludar() {
    return `Hola soy ${this.nombre}`;
  }
}
const persona = new Persona();
console.log(persona.despedirse(persona));
~~~

Esta nueva funcionalidad en JS nos permite trabajar de forma más interesante con las clases incorporadas en ES6 para poder hacer código más reutilizable.