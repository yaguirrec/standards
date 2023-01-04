# Guía de estilo de codificación extendida

## Descripción general

Esta especificación amplía y reemplaza PSR-2, la guía de estilo de codificación y
requiere el cumplimiento de [PSR-1][], el estándar de codificación básico.

Al igual que PSR-2, la intención de esta especificación es reducir la fricción cognitiva cuando se revisa el código de diferentes programadores. Lo hace enumerando un conjunto compartido de reglas y expectativas sobre cómo formatear el código PHP. Este PSR busca proporcionar una forma establecida de que las herramientas de estilo de codificación pueden implementarse, los proyectos pueden declarar adherencia y los desarrolladores puede relacionarse fácilmente entre diferentes proyectos. Cuando varios autores colaboran a través de múltiples proyectos, es útil tener un conjunto de pautas para ser utilizado entre todos esos proyectos. Por lo tanto, el beneficio de esta guía no está en las reglas mismas. sino el compartir esas reglas.

PSR-2 fue aceptado en 2012 y desde entonces se han realizado una serie de cambios en PHP lo que tiene implicaciones para las pautas de estilo de codificación. Si bien PSR-2 es muy completo de la funcionalidad de PHP que existía en el momento de escribir este artículo, la nueva funcionalidad es muy abierto a la interpretación. Este PSR, por lo tanto, busca aclarar el contenido del PSR-2 en
un contexto más moderno con nueva funcionalidad disponible, y hacer fe de las equvocaciones de PSR-2.

### Versiones anteriores del lenguaje

A lo largo de este documento, las instrucciones PUEDEN ignorarse si no existen en versiones
de PHP soportado por su proyecto.

### Ejemplo

Este ejemplo abarca algunas de las reglas a continuación como una descripción general rápida:

~~~php
<?php

declare(strict_types=1);

namespace Vendor\Package;

use Vendor\Package\{ClassA as A, ClassB, ClassC as C};
use Vendor\Package\SomeNamespace\ClassD as D;

use function Vendor\Package\{functionA, functionB, functionC};

use const Vendor\Package\{ConstantA, ConstantB, ConstantC};

class Foo extends Bar implements FooInterface
{
    public function sampleFunction(int $a, int $b = null): array
    {
        if ($a === $b) {
            bar();
        } elseif ($a > $b) {
            $foo->bar($arg1);
        } else {
            BazClass::bar($arg2, $arg3);
        }
    }

    final public static function bar()
    {
        // cuerpo del método
    }
}
~~~

## 2. General

### 2.1 Estándar de Codificación Básica

El código DEBE seguir todas las reglas descritas en [PSR-1].

El término 'StudlyCaps' en PSR-1 DEBE interpretarse como PascalCase donde la primera letra de
cada palabra está en mayúscula, incluida la primera letra.

### 2.2 Archivos

Todos los archivos PHP DEBEN utilizar únicamente el final de línea Unix LF (salto de línea).

Todos los archivos PHP DEBEN terminar con una línea que no esté en blanco, terminada con un solo LF.

La etiqueta de cierre `?>` DEBE omitirse de los archivos que contienen solo PHP.

### 2.3 Líneas

NO DEBE haber un límite estricto en la longitud de la línea.

El límite suave en la longitud de la línea DEBE ser de 120 caracteres.

Las líneas NO DEBEN tener más de 80 caracteres; líneas más largas que eso DEBERÍAN
dividirse en múltiples líneas subsiguientes de no más de 80 caracteres cada una.

NO DEBE haber espacios en blanco al final de las líneas.

SE PUEDEN agregar líneas en blanco para mejorar la legibilidad y para indicar
bloques de código excepto donde esté explícitamente prohibido.

NO DEBE haber más de una declaración por línea.

### 2.4 Indentación

El código DEBE usar una sangría de 4 espacios para cada nivel de sangría y NO DEBE usar tabs para sangría.

### 2.5 Palabras clave y Tipos

Todas las palabras clave reservadas de PHP y los tipos [[1]][keywords][[2]][types] DEBEN estar en minúsculas.

Cualquier nuevo tipo y palabra clave agregada a futuras versiones de PHP DEBE estar en minúsculas.

Se DEBE utilizar la forma abreviada de palabras clave de tipo, es decir, `bool` en lugar de `boolean`,
`int` en lugar de `integer`, etc.

## 3. Delcaracion de Sentencias, Namespace, y Declaración de Importaciones

El encabezado de un archivo PHP puede consistir en varios bloques diferentes. Si está presente,
cada uno de los bloques a continuación DEBE estar separado por una sola línea en blanco, y NO DEBE contener una línea en blanco Cada bloque DEBE estar en el orden que se indica a continuación, aunque los bloques que son no relevante puede omitirse.

* Apertura de la etiqueta `<?php`.
* Docblock a nivel de archivo.
* Una o más sentencias declare.
* La declaración del namespace del archivo.
* Una o más sentencias de importación `use` basadas en clases.
* Una o más sentencias de importación `use` basadas en funciones.
* Una o más sentencias de importación `use` basadas en constantes.
* El resto del código en el archivo.

Cuando un archivo contiene una combinación de HTML y PHP, cualquiera de las secciones anteriores aún puede ser usado. Si es así, DEBEN estar presentes en la parte superior del archivo, incluso si el resto del código consiste en una etiqueta PHP de cierre y luego una mezcla de HTML y PHP.

Cuando la etiqueta de apertura `<?php` está en la primera línea del archivo, DEBE estar en su línea propia sin otras declaraciones a menos que sea un archivo que contenga marcado fuera de PHP Etiquetas de apertura y cierre.

Las declaraciones de importación nunca DEBEN comenzar con una barra invertida inicial, ya que siempre debe estar completamente calificado.

El siguiente ejemplo ilustra una lista completa de todos los bloques:

~~~php
<?php

/**
 * Este archivo contiene un ejemplo de estilos de codificación.
 */

declare(strict_types=1);

namespace Vendor\Package;

use Vendor\Package\{ClassA as A, ClassB, ClassC as C};
use Vendor\Package\SomeNamespace\ClassD as D;
use Vendor\Package\AnotherNamespace\ClassE as E;

use function Vendor\Package\{functionA, functionB, functionC};
use function Another\Vendor\functionD;

use const Vendor\Package\{CONSTANT_A, CONSTANT_B, CONSTANT_C};
use const Another\Vendor\CONSTANT_D;

/**
 * FooBar es una clase de ejemplo.
 */
class FooBar
{
    // ... código PHP adicional ...
}

~~~

NO DEBEN utilizarse espacios de nombres compuestos con una profundidad de más de dos. Por lo tanto, la siguiente es la profundidad de composición máxima permitida:
~~~php
<?php

use Vendor\Package\SomeNamespace\{
    SubnamespaceOne\ClassA,
    SubnamespaceOne\ClassB,
    SubnamespaceTwo\ClassY,
    ClassZ,
};
~~~

Y no estaría permitido lo siguiente:

~~~php
<?php

use Vendor\Package\SomeNamespace\{
    SubnamespaceOne\AnotherNamespace\ClassA,
    SubnamespaceOne\ClassB,
    ClassZ,
};
~~~

Cuando desee declarar tipos estrictos en archivos que contienen marcado fuera de PHP etiquetas de apertura y cierre, la declaración DEBE estar en la primera línea del archivo e incluir una etiqueta PHP de apertura, la declaración de tipos estrictos y una etiqueta de cierre.

For example:
~~~php
<?php declare(strict_types=1) ?>
<html>
<body>
    <?php
        // ... código PHP adicional ...
    ?>
</body>
</html>
~~~

Las declaraciones de sentencias NO DEBEN contener espacios y DEBEN ser exactamente `declare(strict_types=1)` (con un terminador de punto y coma opcional).

Las declaraciones de sentencias de bloque están permitidas y DEBEN tener el formato siguiente. Tenga en cuenta la posición de llaves y espaciado:
~~~php
declare(ticks=1) {
    // algún código
}
~~~

## 4. Clases, Propiedades, y Métodos

El término "clase" se refiere a todas las clases, interfaces y características.

Cualquier llave de cierre NO DEBE ir seguida de ningún comentario o declaración sobre el misma línea.

Al instanciar una nueva clase, los paréntesis siempre DEBEN estar presentes incluso cuando
no se pasan argumentos al constructor.

~~~php
new Foo();
~~~

### 4.1 Extends and Implements

Las palabras clave `extends` e `implements` DEBEN declararse en la misma línea que el nombre de la clase.

La llave de apertura de la clase DEBE ir en su propia línea; la llave de cierre para la clase DEBE ir en la siguiente línea después del cuerpo.

Las llaves de apertura DEBEN estar en su propia línea y NO DEBEN ser precedidas o seguidas por una línea en blanco.

Las llaves de cierre DEBEN estar en su propia línea y NO DEBEN estar precedidas por un espacio en blanco
línea.

~~~php
<?php

namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements \ArrayAccess, \Countable
{
    // constantes, propiedades, métodos
}
~~~

Las listas de `implements`  y, en el caso de las interfaces, `extends` PUEDEN dividirse en varias líneas, donde cada línea subsiguiente se sangra una vez. al hacer por lo tanto, el primer elemento de la lista DEBE estar en la siguiente línea, y DEBE haber solo una interfaz por línea.

~~~php
<?php

namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements
    \ArrayAccess,
    \Countable,
    \Serializable
{
    // constantes, propiedades, métodos
}
~~~

### 4.2 Usando características

La palabra clave `use` utilizada dentro de las clases para implementar características DEBE ser declarado en la siguiente línea después de la llave de apertura.

~~~php
<?php

namespace Vendor\Package;

use Vendor\Package\FirstTrait;

class ClassName
{
    use FirstTrait;
}
~~~

Cada característica individual que se importa a una clase DEBE incluirse uno por línea y cada inclusión DEBE tener su propia declaración de importación `use`.

~~~php
<?php

namespace Vendor\Package;

use Vendor\Package\FirstTrait;
use Vendor\Package\SecondTrait;
use Vendor\Package\ThirdTrait;

class ClassName
{
    use FirstTrait;
    use SecondTrait;
    use ThirdTrait;
}
~~~

Cuando la clase no tiene nada después de la instrucción de importación `use`, la clase la llave de cierre DEBE estar en la siguiente línea después de la instrucción de importación `use`.

~~~php
<?php

namespace Vendor\Package;

use Vendor\Package\FirstTrait;

class ClassName
{
    use FirstTrait;
}
~~~

De lo contrario, DEBE tener una línea en blanco después de la instrucción de importación `use`.

~~~php
<?php

namespace Vendor\Package;

use Vendor\Package\FirstTrait;

class ClassName
{
    use FirstTrait;

    private $property;
}
~~~

Al usar los operadores `insteadof` y `as`, deben usarse de la siguiente manera teniendo
nota de sangría, espaciado y líneas nuevas.

~~~php
<?php

class Talker
{
    use A;
    use B {
        A::smallTalk insteadof B;
    }
    use C {
        B::bigTalk insteadof C;
        C::mediumTalk as FooBar;
    }
}
~~~

### 4.3 Propiedades y Constantes

La visibilidad DEBE declararse en todas las propiedades.

La visibilidad DEBE declararse en todas las constantes si su proyecto PHP es mínimo
La versión admite visibilidades constantes (PHP 7.1 o posterior).

La palabra clave `var` NO DEBE usarse para declarar una propiedad.

NO DEBE haber más de una propiedad declarada por línea.

Los nombres de propiedades NO DEBEN tener un prefijo con un solo guión bajo para indicar visibilidad protegida o privada. Es decir, un prefijo de subrayado tiene explícitamente sin significado.

DEBE haber un espacio entre la declaración de tipo y el nombre de la propiedad.

Una declaración de propiedad tiene el siguiente aspecto:

~~~php
<?php

namespace Vendor\Package;

class ClassName
{
    public $foo = null;
    public static int $bar = 0;
}
~~~

### 4.4 Métodos y Funciones

La visibilidad DEBE declararse en todos los métodos.

Los nombres de los métodos NO DEBEN tener un prefijo con un solo guión bajo para indicar visibilidad protegida o privada. Es decir, un prefijo de subrayado tiene explícitamente sin significado.

Los nombres de métodos y funciones NO DEBEN declararse con espacios después del nombre del método. Él la llave de apertura DEBE ir en su propia línea, y la llave de cierre DEBE ir en la misma línea. siguiente línea siguiendo el cuerpo. NO DEBE haber un espacio después de la apertura paréntesis, y NO DEBE haber un espacio antes del paréntesis de cierre.

Una declaración de método tiene el siguiente aspecto. Nótese la colocación de
paréntesis, comas, espacios y llaves:

~~~php
<?php

namespace Vendor\Package;

class ClassName
{
    public function fooBarBaz($arg1, &$arg2, $arg3 = [])
    {
        // cuerpo del método
    }
}
~~~

La declaración de una función tiene el siguiente aspecto. Nótese la colocación de paréntesis, comas, espacios y llaves:

~~~php
<?php

function fooBarBaz($arg1, &$arg2, $arg3 = [])
{
    // cuerpo de la función
}
~~~

### 4.5 Argumentos de Método y Función

En la lista de argumentos, NO DEBE haber un espacio antes de cada coma, y ​​no DEBE ser un espacio después de cada coma.

Los argumentos de método y función con valores predeterminados DEBEN ir al final del argumento lista.

~~~php
<?php

namespace Vendor\Package;

class ClassName
{
    public function foo(int $arg1, &$arg2, $arg3 = [])
    {
        // cuerpo del método
    }
}
~~~

Las listas de argumentos PUEDEN dividirse en varias líneas, donde cada línea subsiguiente se sangra una vez. Al hacerlo, el primer elemento de la lista DEBE estar en el siguiente línea, y DEBE haber solo un argumento por línea.

Cuando la lista de argumentos se divide en varias líneas, el paréntesis de cierre y la abrazadera de apertura DEBEN colocarse juntas en su propia línea con un espacio entre ellos.

~~~php
<?php

namespace Vendor\Package;

class ClassName
{
    public function aVeryLongMethodName(
        ClassTypeHint $arg1,
        &$arg2,
        array $arg3 = []
    ) {
        // cuerpo del método
    }
}
~~~

When you have a return type declaration present, there MUST be one space after
the colon followed by the type declaration. The colon and declaration MUST be
on the same line as the argument list closing parenthesis with no spaces between
the two characters.

Cuando tiene presente una declaración de tipo de retorno, DEBE haber un espacio después los dos puntos seguidos de la declaración de tipo. Los dos puntos y la declaración DEBEN ser en la misma línea que el paréntesis de cierre de la lista de argumentos sin espacios entre los dos carcateres

~~~php
<?php

declare(strict_types=1);

namespace Vendor\Package;

class ReturnTypeVariations
{
    public function functionName(int $arg1, $arg2): string
    {
        return 'foo';
    }

    public function anotherFunction(
        string $foo,
        string $bar,
        int $baz
    ): string {
        return 'foo';
    }
}
~~~

En las declaraciones de tipos nulos, NO DEBE haber un espacio entre el signo de interrogación y el tipo.

~~~php
<?php

declare(strict_types=1);

namespace Vendor\Package;

class ReturnTypeVariations
{
    public function functionName(?string $arg1, ?int &$arg2): ?string
    {
        return 'foo';
    }
}
~~~

Cuando se usa el operador de referencia `&` antes de un argumento, NO DEBE haber un espacio después, como en el ejemplo anterior.

NO DEBE haber un espacio entre el operador de tres puntos y el argumento nombre:

```php
public function process(string $algorithm, ...$parts)
{
    // procesando
}
```

Al combinar el operador de referencia y el operador de tres puntos,
NO DEBE haber ningún espacio entre los dos:

```php
public function process(string $algorithm, &...$parts)
{
    // procesando
}
```

### 4.6 `abstract`, `final`, y `static`

Cuando estén presentes, las declaraciones `abstract` y `final` DEBEN preceder a la declaración de visibilidad.

Cuando está presente, la declaración `static` DEBE ir después de la visibilidad declaración.

~~~php
<?php

namespace Vendor\Package;

abstract class ClassName
{
    protected static $foo;

    abstract protected function zim();

    final public static function bar()
    {
        // cuerpo del método
    }
}
~~~

### 4.7 Llamadas a Métodos y Funciones

Al realizar una llamada de método o función, NO DEBE haber un espacio entre el nombre del método o función y el paréntesis de apertura, NO DEBE haber un espacio después del paréntesis de apertura, y NO DEBE haber un espacio antes del paréntesis de cierre. En la lista de argumentos, NO DEBE haber un espacio antes cada coma, y ​​DEBE haber un espacio después de cada coma.

~~~php
<?php

bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
~~~

Las listas de argumentos PUEDEN dividirse en varias líneas, donde cada línea subsiguiente se sangra una vez. Al hacerlo, el primer elemento de la lista DEBE estar en el siguiente línea, y DEBE haber solo un argumento por línea. Siendo un solo argumento dividido en varias líneas (como podría ser el caso con una función anónima o array) no constituye dividir la lista de argumentos en sí.

~~~php
<?php

$foo->bar(
    $longArgument,
    $longerArgument,
    $muchLongerArgument
);
~~~

~~~php
<?php

somefunction($foo, $bar, [
  // ...
], $baz);

$app->get('/hello/{name}', function ($name) use ($app) {
    return 'Hello ' . $app->escape($name);
});
~~~

## 5. Estructuras de control

Las reglas generales de estilo para las estructuras de control son las siguientes:

- DEBE haber un espacio después de la palabra clave de estructura de control
- NO DEBE haber un espacio después del paréntesis de apertura
- NO DEBE haber un espacio antes del paréntesis de cierre
- DEBE haber un espacio entre el paréntesis de cierre y el de apertura
  abrazadera
- El cuerpo de la estructura DEBE sangrarse una vez
- El cuerpo DEBE estar en la siguiente línea después de la llave de apertura
- La llave de cierre DEBE estar en la siguiente línea después del cuerpo

El cuerpo de cada estructura DEBE estar encerrado entre llaves. Esto estandariza cómo las estructuras se ven y reduce la probabilidad de introducir errores como nuevos se añaden líneas al cuerpo.

### 5.1 `if`, `elseif`, `else`

Una estructura `if` se parece a lo siguiente. Tenga en cuenta la ubicación de los paréntesis, espacios y llaves; y que `else` y `elseif` están en la misma línea que la llave de cierre del cuerpo anterior.

~~~php
<?php

if ($expr1) {
    // cuerpo del if
} elseif ($expr2) {
    // cuerpo del elseif
} else {
    // cuerpo del else;
}
~~~

La palabra clave `elseif` DEBERÍA usarse en lugar de `else if` para que todo el control las palabras clave parecen palabras sueltas.

Las expresiones entre paréntesis PUEDEN dividirse en varias líneas, donde cada la línea siguiente se sangra al menos una vez. Al hacerlo, la primera condición DEBE estar en la siguiente línea. El paréntesis de cierre y la llave de apertura DEBEN ser colocados juntos en su propia línea con un espacio entre ellos. Los operadores booleanos entre condiciones DEBEN estar siempre al principio o al final de la línea, no una mezcla de ambos.

~~~php
<?php

if (
    $expr1
    && $expr2
) {
    // cuerpo del if
} elseif (
    $expr3
    && $expr4
) {
    // cuerpo del elseif
}
~~~

### 5.2 `switch`, `case`

Una estructura `switch` se parece a lo siguiente. Nótese la colocación de
paréntesis, espacios y llaves. La declaración `case` DEBE estar sangrada una vez de `switch`, y la palabra clave `break` (u otras palabras clave de terminación) DEBEN ser sangrado al mismo nivel que el cuerpo del `case`. DEBE haber un comentario como
`// no break` cuando la falla es intencional en un cuerpo `case` no vacío.

~~~php
<?php

switch ($expr) {
    case 0:
        echo 'Primer caso, con un break';
        break;
    case 1:
        echo 'Segundo caso, que fracasa';
        // no break
    case 2:
    case 3:
    case 4:
        echo 'Tercer caso, return en lugar de break';
        return;
    default:
        echo 'case por defecto';
        break;
}
~~~

Las expresiones entre paréntesis PUEDEN dividirse en varias líneas, donde cada la línea siguiente se sangra al menos una vez. Al hacerlo, la primera condición DEBE estar en la siguiente línea. El paréntesis de cierre y la llave de apertura DEBEN ser colocados juntos en su propia línea con un espacio entre ellos. Los operadores booleanos entre condiciones DEBEN estar siempre al principio o al final de la línea, no una mezcla de ambos.

~~~php
<?php

switch (
    $expr1
    && $expr2
) {
    // cuerpo de la estructura
}
~~~

### 5.3 `while`, `do while`

Una instrucción `while` tiene el siguiente aspecto. Nótese la colocación de
paréntesis, espacios y llaves.

~~~php
<?php

while ($expr) {
    // cuerpo de la estructura
}
~~~

Las expresiones entre paréntesis PUEDEN dividirse en varias líneas, donde cada la línea siguiente se sangra al menos una vez. Al hacerlo, la primera condición DEBE estar en la siguiente línea. El paréntesis de cierre y la llave de apertura DEBEN ser colocados juntos en su propia línea con un espacio entre ellos. Los operadores booleanos entre condiciones DEBEN estar siempre al principio o al final de la línea, no una mezcla de ambos.

~~~php
<?php

while (
    $expr1
    && $expr2
) {
    // cuerpo de la estructura
}
~~~

De manera similar, una instrucción `do while` tiene el siguiente aspecto. Tenga en cuenta la colocación de paréntesis, espacios y llaves.

~~~php
<?php

do {
    // cuerpo de la estructura;
} while ($expr);
~~~

Las expresiones entre paréntesis PUEDEN dividirse en varias líneas, donde cada la línea siguiente se sangra al menos una vez. Al hacerlo, la primera condición DEBE estar en la siguiente línea. Los operadores booleanos entre condiciones DEBEN estar siempre al principio o al final de la línea, no una mezcla de ambos.

~~~php
<?php

do {
    // cuerpo de la estructura;
} while (
    $expr1
    && $expr2
);
~~~

### 5.4 `for`

Una instrucción `for` tiene el siguiente aspecto. Tenga en cuenta la ubicación de los paréntesis, espacios y llaves.

~~~php
<?php

for ($i = 0; $i < 10; $i++) {
    // cuerpo del for
}
~~~

Las expresiones entre paréntesis PUEDEN dividirse en varias líneas, donde cada la línea siguiente se sangra al menos una vez. Al hacerlo, la primera expresión DEBE estar en la siguiente línea. El paréntesis de cierre y la llave de apertura DEBEN ser colocados juntos en su propia línea con un espacio entre ellos.

~~~php
<?php

for (
    $i = 0;
    $i < 10;
    $i++
) {
    // cuerpo del for
}
~~~

### 5.5 `foreach`

Una instrucción `foreach` tiene el siguiente aspecto. Tenga en cuenta la ubicación de los paréntesis, espacios y llaves.

~~~php
<?php

foreach ($iterable as $key => $value) {
    // cuerpo del foreach
}
~~~

### 5.6 `try`, `catch`, `finally`

Una instrucción `try-catch-finally` tiene el siguiente aspecto. Tenga en cuenta la ubicación de los paréntesis, espacios y llaves.

~~~php
<?php

try {
    // cuerpo del try
} catch (FirstThrowableType $e) {
    // cuerpo del catch
} catch (OtherThrowableType | AnotherThrowableType $e) {
    // cuerpo del catch
} finally {
    // cuerpo del finally
}
~~~

## 6. Operadores

Las reglas de estilo para los operadores se agrupan por aridad (el número de operandos que toman).

Cuando se permite espacio alrededor de un operador, se PUEDEN habilitar varios espacios. utilizado con fines de legibilidad.

Todos los operadores no descritos aquí se dejan sin definir.

### 6.1. Operadores unarios

Los operadores de incremento/decremento NO DEBEN tener ningún espacio entre
el operador y el operando.
~~~php
$i++;
++$j;
~~~

Los operadores de fundición tipográfica NO DEBEN tener ningún espacio entre paréntesis:
~~~php
$intValue = (int) $input;
~~~

### 6.2. Operadores binarios

All binary [arithmetic][], [comparison][], [assignment][], [bitwise][],
[logical][], [string][], and [type][] operators MUST be preceded and
followed by at least one space:

Todo los operadores binarios [aritméticos][], [comparación][], [asignación][], [bit a bit][], [lógicos][], [strings][] y [tipo][] DEBEN estar precedidos y seguido de al menos un espacio:
~~~php
if ($a === $b) {
    $foo = $bar ?? $a ?? $b;
} elseif ($a > $b) {
    $foo = $a + $b * $c;
}
~~~

### 6.3. Operadores ternarios

El operador condicional, también conocido simplemente como operador ternario, DEBE ser precedido y seguido por al menos un espacio alrededor de caracteres `?` y `:`:
~~~php
$variable = $foo ? 'foo' : 'bar';
~~~

Cuando se omite el operando medio del operador condicional, el operador
DEBE seguir las mismas reglas de estilo que otros operadores binarios de [comparación][]:
~~~php
$variable = $foo ?: 'bar';
~~~

## 7. Closures

Los Closures DEBEN declararse con un espacio después de la palabra clave `función` y un espacio antes y después de la palabra clave `use`.

La llave de apertura DEBE ir en la misma línea, y la llave de cierre DEBE ir en la siguiente línea después del cuerpo.

NO DEBE haber un espacio después del paréntesis de apertura de la lista de argumentos o lista de variables, y NO DEBE haber un espacio antes del paréntesis de cierre de la lista de argumentos o lista de variables.

En la lista de argumentos y la lista de variables, NO DEBE haber un espacio antes de cada coma, y ​​DEBE haber un espacio después de cada coma.

Los argumentos de cierre con valores predeterminados DEBEN ir al final del argumento lista.

Si un tipo de retorno está presente, DEBE seguir las mismas reglas que con normal funciones y métodos; si la palabra clave `use` está presente, los dos puntos DEBEN seguir la lista `use` cierra paréntesis sin espacios entre los dos caracteres.

Una declaración de cierre tiene el siguiente aspecto. Nótese la colocación de
paréntesis, comas, espacios y llaves:

~~~php
<?php

$closureWithArgs = function ($arg1, $arg2) {
    // cuerpo
};

$closureWithArgsAndVars = function ($arg1, $arg2) use ($var1, $var2) {
    // cuerpo
};

$closureWithArgsVarsAndReturn = function ($arg1, $arg2) use ($var1, $var2): bool {
    // cuerpo
};
~~~

Las listas de argumentos y las listas de variables PUEDEN dividirse en varias líneas, donde cada línea subsiguiente se sangra una vez. Al hacerlo, el primer elemento en el la lista DEBE estar en la siguiente línea, y DEBE haber solo un argumento o variable por línea.

Cuando la lista final (ya sea de argumentos o variables) se divide en varias líneas, el paréntesis de cierre y la llave de apertura DEBEN colocarse juntos en su propia línea con un espacio entre ellos.

Los siguientes son ejemplos de cierres con y sin listas de argumentos y listas de variables divididas en varias líneas.

~~~php
<?php

$longArgs_noVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) {
   // cuerpo
};

$noArgs_longVars = function () use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // cuerpo
};

$longArgs_longVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // cuerpo
};

$longArgs_shortVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use ($var1) {
   // cuerpo
};

$shortArgs_longVars = function ($arg) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // cuerpo
};
~~~

Tenga en cuenta que las reglas de formato también se aplican cuando el cierre se usa directamente en una función o llamada de método como argumento.

~~~php
<?php

$foo->bar(
    $arg1,
    function ($arg2) use ($var1) {
        // cuerpo
    },
    $arg3
);
~~~

## 8. Clases Anónimas

Las clases anónimas DEBEN seguir las mismas pautas y principios que los cierres en la sección anterior.

~~~php
<?php

$instance = new class {};
~~~

La llave de apertura PUEDE estar en la misma línea que la palabra clave `class` siempre que la lista de interfaces de `implements` no se ajusta. Si la lista de interfaces vueltas, el corsé DEBE colocarse en la línea que sigue inmediatamente a la última interfaz.

~~~php
<?php

// Llave en la misma línea
$instance = new class extends \Foo implements \HandleableInterface {
    // Contenido de la clase
};

// Llave en la siguiente línea
$instance = new class extends \Foo implements
    \ArrayAccess,
    \Countable,
    \Serializable
{
    // Contenido de la clase
};
~~~

[PSR-1]: https://github.com/yaguirrec/standards/blob/main/PHP/PSR-1-basic-coding-standard.md
[keywords]: http://php.net/manual/es/reserved.keywords.php
[types]: http://php.net/manual/es/reserved.other-reserved-words.php
[aritméticos]: http://php.net/manual/es/language.operators.arithmetic.php
[asignación]: http://php.net/manual/es/language.operators.assignment.php
[comparación]: http://php.net/manual/es/language.operators.comparison.php
[bit a bit]: http://php.net/manual/es/language.operators.bitwise.php
[lógicos]: http://php.net/manual/es/language.operators.logical.php
[strings]: http://php.net/manual/es/language.operators.string.php
[tipo]: http://php.net/manual/es/language.operators.type.php