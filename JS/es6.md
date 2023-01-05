# Introducción a ECMAScript 2015 (es6)

ECMAScript es el estándar que define cómo debe de ser el lenguaje Javascript. Sin entrar en datos históricos, que puedes consultar otras fuentes como la [Wikipedia sobre ECMAScript](https://es.wikipedia.org/wiki/ECMAScript), cabe decir que es la especificación que los fabricantes deben seguir al crear intérpretes para Javascript.

Como sabes, Javascript es interpretado y procesado por multitud de plataformas, entre las que se encuentran los navegadores web, así como NodeJS y otros ámbitos más específicos como el desarrollo de aplicaciones para Windows y otros sistemas operativos. Todos los responsables de cada una de esas tecnologías se encargan de interpretar el lenguaje tal como dice el estándar ECMAScript.

## Let y const
### Let
La instrucción let declara una variable de alcance local con ámbito de bloque(blockscope), la cual, opcionalmente, puede ser inicializada con algún valor.

~~~js
let var1 [= valor1] [, var2 [= valor2]] [, ..., varN [= valorN]];
~~~

### Const
Las variables constantes presentan un ámbito de bloque (block scope) tal y como lo hacen las variables definidas usando la instrucción let, con la particularidad de que el valor de una constante no puede cambiarse a través de la reasignación. Las constantes no se pueden redeclarar.

~~~js
const varname1 = value1 [, varname2 = value2 [, varname3 = value3 [, ... [, varnameN = valueN]]]];
~~~

Más info MDN: [let][], [const][]

## Objetos literales

Un objeto literal es una lista de cero o más pares de nombres de propiedad y valores asociados de un objeto, entre llaves ({}).
~~~js
var sales = 'Toyota';

function carTypes(name) {
  if (name === 'Honda') {
    return name;
  } else {
    return "Lo sentimos, no vendemos " + name + ".";
  }
}

var car = { myCar: 'Saturn', getCar: carTypes('Honda'), special: sales };

console.log(car.myCar);   // Saturn
console.log(car.getCar);  // Honda
console.log(car.special); // Toyota
~~~

## Template String
Las plantillas literales son cadenas literales que habilitan el uso de expresiones incrustadas. Con ellas, es posible utilizar cadenas de caracteres de más de una línea, y funcionalidades de interpolación de cadenas de caracteres.

En ediciones anteriores de la especificación ES2015, solían llamarse "plantillas de cadenas de caracteres".

~~~js
// Creación básica de cadenas literales
`In JavaScript '\n' is a line-feed.`

// Cadenas multilínea
`In JavaScript this is
 not legal.`

// Interpolación de cadenas
var name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`

// Construir un prefijo de solicitud HTTP se usa para interpretar los reemplazos y la construcción
POST`http://foo.org/bar?a=${a}&b=${b}
     Content-Type: application/json
     X-Credentials: ${credentials}
     { "foo": ${foo},
       "bar": ${bar}}`(myOnReadyStateChangeHandler);
~~~

## Arrow Functions

Una expresión de función flecha es una alternativa compacta a una expresión de función tradicional, pero es limitada y no se puede utilizar en todas las situaciones.

Diferencias y limitaciones:

- No tiene sus propios enlaces a this o super y no se debe usar como métodos.
- No tiene argumentos o palabras clave new.target.
- No apta para los métodos call, apply y bind, que generalmente se basan en establecer un ámbito o alcance
- No se puede utilizar como constructor.
- No se puede utilizar yield dentro de su cuerpo.

~~~js
const materials = [
  'Hydrogen',
  'Helium',
  'Lithium',
  'Beryllium'
];

console.log(materials.map(material => material.length));
// resultado esperado: Array [8, 6, 7, 9]
~~~

##Comparación de funciones tradicionales con funciones flecha
Observa, paso a paso, la descomposición de una "función tradicional" hasta la "función flecha" más simple: Nota: Cada paso a lo largo del camino es una "función flecha" válida

~~~js
// Función tradicional
function (a){
  return a + 100;
}

// Desglose de la función flecha

// 1. Elimina la palabra "function" y coloca la flecha entre el argumento y el corchete de apertura.
(a) => {
  return a + 100;
}

// 2. Quita los corchetes del cuerpo y la palabra "return" — el return está implícito.
(a) => a + 100;

// 3. Suprime los paréntesis de los argumentos
a => a + 100;
~~~

## Parámetros con valores predeterminados en funciones

Parámetros predeterminados de función permiten que los parámetros con nombre se inicien con valores predeterminados si no se pasa ningún valor o undefined.

~~~js
function multiply(a, b = 1) {
  return a * b;
}

console.log(multiply(5, 2));
//  resultado esperado: 10

console.log(multiply(5));
//  resultado esperado: 5

~~~

## Operador Rest

La sintaxis de los parámetros rest nos permiten representar un número indefinido de argumentos como un array.

~~~js
function sum(...theArgs) {
  let total = 0;
  for (const arg of theArgs) {
    total += arg;
  }
  return total;
}

console.log(sum(1, 2, 3));
// resultado esperado: 6

console.log(sum(1, 2, 3, 4));
// resultado esperado: 10

~~~

## Operador Spread

La sintaxis extendida o spread syntax permite a un elemento iterable tal como un arreglo o cadena ser expandido en lugares donde cero o más argumentos (para llamadas de función) o elementos (para Array literales) son esperados, o a un objeto ser expandido en lugares donde cero o más pares de valores clave (para literales Tipo Objeto) son esperados.

~~~js
function sum(x, y, z) {
  return x + y + z;
}

const numbers = [1, 2, 3];

console.log(sum(...numbers));
// resultado esperado: 6

console.log(sum.apply(null, numbers));
// resultado esperado: 6

~~~

## Modulos

Soporte a nivel de idioma para módulos para la definición de componentes. Codifica patrones de cargadores de módulos JavaScript populares (AMD, CommonJS). Comportamiento en tiempo de ejecución definido por un cargador predeterminado definido por el host. Modelo implícitamente asíncrono: ningún código se ejecuta hasta que los módulos solicitados están disponibles y procesados.

~~~js
// lib/math.js
export function sum(x, y) {
  return x + y;
}
export var pi = 3.141593;
~~~

~~~js
// app.js
import * as math from "lib/math";
alert("2π = " + math.sum(math.pi, math.pi));
~~~

~~~js
// otherApp.js
import {sum, pi} from "lib/math";
alert("2π = " + sum(pi, pi));
~~~

Algunas funciones adicionales incluyen `export default` y `export *`:

~~~js
// lib/mathplusplus.js
export * from "lib/math";
export var e = 2.71828182846;
export default function(x) {
    return Math.log(x);
}
~~~

~~~js
// app.js
import ln, {pi, e} from "lib/mathplusplus";
alert("2π = " + ln(e)*pi*2);
~~~

## Promesas

Las promesas son una biblioteca para la programación asíncrona. Las promesas son una representación de primera clase de un valor que puede estar disponible en el futuro. Las promesas se utilizan en muchas bibliotecas de JavaScript existentes.

~~~js
function timeout(duration = 0) {
    return new Promise((resolve, reject) => {
        setTimeout(resolve, duration);
    })
}

var p = timeout(1000).then(() => {
    return timeout(2000);
}).then(() => {
    throw new Error("hmm");
}).catch(err => {
    return Promise.all([timeout(100), timeout(200)]);
})
~~~

## Programación Orientada a Objetos

Tener una sola forma declarativa conveniente hace que los patrones de clase sean más fáciles de usar y fomenta la interoperabilidad. Las clases admiten herencia basada en prototipos, superllamadas, métodos y constructores estáticos y de instancia.

~~~js
class SkinnedMesh extends THREE.Mesh {
  constructor(geometry, materials) {
    super(geometry, materials);

    this.idMatrix = SkinnedMesh.defaultMatrix();
    this.bones = [];
    this.boneMatrices = [];
    //...
  }
  update(camera) {
    //...
    super.update();
  }
  get boneCount() {
    return this.bones.length;
  }
  set matrixType(matrixType) {
    this.idMatrix = SkinnedMesh[matrixType]();
  }
  static defaultMatrix() {
    return new THREE.Matrix4();
  }
}
~~~

## Desestructuración

La sintaxis de desestructuración es una expresión de JavaScript que permite desempacar valores de arreglos o propiedades de objetos en distintas variables.

~~~js
// list matching
let [a, , b] = [1,2,3];

// object matching
let { op: a, lhs: { op: b }, rhs: c }
       = getASTNode()

// object matching shorthand
// binds `op`, `lhs` and `rhs` in scope
let {op, lhs, rhs} = getASTNode()

// Can be used in parameter position
function g({name: x}) {
  console.log(x);
}
g({name: 5})

// Fail-soft destructuring
let [a] = [];
a === undefined;

// Fail-soft destructuring with defaults
let [a = 1] = [];
a === 1;
~~~

## Iteradores + For..Of

Los objetos de iterador permiten la iteración personalizada como CLR IEnumerable o Java Iterable. Generalice for..in para personalizar la iteración basada en iteradores con for..of. No requiere realizar una matriz, lo que permite patrones de diseño perezosos como LINQ.

~~~js
let fibonacci = {
  [Symbol.iterator]() {
    let pre = 0, cur = 1;
    return {
      next() {
        [pre, cur] = [cur, pre + cur];
        return { done: false, value: cur }
      }
    }
  }
}

for (var n of fibonacci) {
  // truncate the sequence at 1000
  if (n > 1000)
    break;
  console.log(n);
}
~~~

## Map + Set + WeakMap + WeakSet

Estructuras de datos eficientes para algoritmos comunes. WeakMaps proporciona tablas auxiliares con clave de objeto sin fugas.

~~~js
// Sets
var s = new Set();
s.add("hello").add("goodbye").add("hello");
s.size === 2;
s.has("hello") === true;

// Maps
var m = new Map();
m.set("hello", 42);
m.set(s, 34);
m.get(s) == 34;

// Weak Maps
var wm = new WeakMap();
wm.set(s, { extra: 42 });
wm.size === undefined

// Weak Sets
var ws = new WeakSet();
ws.add({ data: 42 });
// Because the added object has no other references, it will not be held in the set
~~~

## Math + Number + String + Array + Object APIs

Muchas nuevas adiciones a la biblioteca, incluidas las bibliotecas matemáticas básicas, ayudantes de conversión de matriz, ayudantes de cadena y Object.assign para copiar.

~~~js
Number.EPSILON
Number.isInteger(Infinity) // false
Number.isNaN("NaN") // false

Math.acosh(3) // 1.762747174039086
Math.hypot(3, 4) // 5
Math.imul(Math.pow(2, 32) - 1, Math.pow(2, 32) - 2) // 2

"abcde".includes("cd") // true
"abc".repeat(3) // "abcabcabc"

Array.from(document.querySelectorAll('*')) // Returns a real Array
Array.of(1, 2, 3) // Similar to new Array(...), but without special one-arg behavior
[0, 0, 0].fill(7, 1) // [0,7,7]
[1, 2, 3].find(x => x == 3) // 3
[1, 2, 3].findIndex(x => x == 2) // 1
[1, 2, 3, 4, 5].copyWithin(3, 0) // [1, 2, 3, 1, 2]
["a", "b", "c"].entries() // iterator [0, "a"], [1,"b"], [2,"c"]
["a", "b", "c"].keys() // iterator 0, 1, 2
["a", "b", "c"].values() // iterator "a", "b", "c"

Object.assign(Point, { origin: new Point(0,0) })
~~~

## Binary and Octal Literals

Se agregan dos nuevas formas literales numéricas para binario (`b`) y octal (`o`).

~~~js
0b111110111 === 503 // true
0o767 === 503 // true
~~~


[let]: https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Statements/let
[const]: https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Statements/const
[objetos literales]: https://developer.mozilla.org/es/docs/Web/JavaScript/Guide/Grammar_and_types#objetos_literales