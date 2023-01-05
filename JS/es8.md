# Introducción a ECMAScript 2017 (es8)

## Object.values()

Personalmente llevo mucho tiempo usando Object.keys() y no puedo vivir sin él, así que poder disponer ahora de su hermano, Object.values() seguro que también nos va a resultar muy útil y a permitir ahorrarnos muchos bucles for!

~~~js
const quirks = {
  midoriya: 'One For All',
  uraraka: 'Zero Gravity',
  bakugo: 'Explosion'
};

console.log(Object.values(quirks));
// > ['One For All', 'Zero Gravity', 'Explosion']
~~~

## Object.entries()

Este nuevo método del prototype Object devuelve un array de 2 dimensiones con cada key-value del objeto en sí, unido al array destructuring seguro que nos va a resultar realmente útil! 😄

~~~js
const quirks = {
  midoriya: 'One For All',
  uraraka: 'Zero Gravity',
  bakugo: 'Explosion'
};

console.log(Object.entries(quirks));
// > [['midoriya', 'One For All'], ['uraraka', 'Zero Gravity'], ['bakugo', 'Explosion']]

console.log(Object.entries(quirks).map(([hero, quirk]) => `Hero: ${hero}, Quirk: ${quirk}`));
// > [
// >  "Hero: midoriya, Quirk: One For All",
// >  "Hero: uraraka, Quirk: Zero Gravity",
// >  "Hero: bakugo, Quirk: Explosion"
// > ]
~~~

## String.prototype.padStart() / String.prototype.padEnd()

¿Nunca has tenido que mostrar un string o un número con un formato determinado (horas, minutos, IBAN, etc…)? ¡Seguro que éstas 2 nuevas funciones del prototype String te vienen al pelo! Básicamente, rellenan un string por el principio o por el final (con el carácter especificado, por defecto: un espacio “ “) hasta que llegue a la longitud especificada. 🤔

~~~js
// String.prototype.padStart()
'abc'.padStart(10);         // "       abc"
'abc'.padStart(10, "foo");  // "foofoofabc"
'abc'.padStart(6,"123465"); // "123abc"
'abc'.padStart(8, "0");     // "00000abc"
'abc'.padStart(1);          // "abc"

// String.prototype.padEnd()
'abc'.padEnd(10);          // "abc       "
'abc'.padEnd(10, "foo");   // "abcfoofoof"
'abc'.padEnd(6, "123456"); // "abc123"
'abc'.padEnd(1);           // "abc"
~~~

## Object.getOwnPropertyDescriptors()

Recibe como parámetro un objeto y devuelve otro objeto con la definición de todas las propiedades de ese objeto (devuelve un objeto vacío si no las tiene).

~~~js
const obj = {
    id: 123,
    get bar() { return 'abc' },
};
console.log(Object.getOwnPropertyDescriptors(obj));

>   { 
      id: { 
        value: 123,
        writable: true,
        enumerable: true,
        configurable: true 
      },
      bar: { 
        get: [Function: bar],
        set: undefined,
        enumerable: true,
        configurable: true 
      } 
    }
~~~


## Comas al final de arrays, objetos y llamadas a funciones

Zanjada por fin la discusión que generaron los linters sobre si es correcto o no añadir comas al final de arrays, objetos y llamadas a funciones (trailing commas)! 👏🏼👏🏼👏🏼

~~~js
// Todo lo que ves es JavaScript válido 🎉🎉🎉

const arr = [
  1,
  2,
  3,
];

const hero = {
  lastname: 'Izuku',
  lastname: 'Midoriya',
};

const plusUltra = (one, for, all,) => ({});

plusUltra(1, 2, 3,);
~~~

¿Por qué es este cambio importante? En realidad sólo es una frikada para mejorar los git diffs y evitar que tengas que añadir una coma cada vez que tengas que añadir un nuevo elemento/parámetro/argumento pero a mi me hace feliz 😜