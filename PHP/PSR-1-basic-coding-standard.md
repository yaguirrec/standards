# Estándar Básico de Codificación

Esta sección del estándar comprende lo que debe considerarse como los elementos de codificación estandar que se requieren para garantizar un alto nivel de interoperavilidad entre código PHP compartido.

[PSR-4]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader.md

## 1. Descripción general

- Los archivos DEBEN usarl solo las etiquetas `<?php` y `<?=`.

- Los namespaces y clases DEBEN seguir un PSR de "carga automática": [PSR-4].

- Los nombres de las clases DEBEN declararse en `StudlyCaps`.

- Las constantes de clase DEBEN declararse en mayúsculas con separadores de guiones bajos.

- Los nombres de los métodos DEBEN declararse en `camelCase`.

## 2. Archivos

### 2.1. Etiquetas PHP

El código PHP DEBE usar las etiquetas largas `<?php ?>` o las etiquetas cortas de echo `<?= ?>` = `<?php echo ?>`; NO DEBE usar las otras variaciones de etiquetas.

### 2.2. Codificación de caracteres

El código PHP DEBE usar solo UTF-8 sin BOM.

### 2.3. Efectos secundarios

Un archivo DEBE declarar nuevos símbolos (clases, funciones, constantes,
etc.) y no causa otros efectos secundarios, o DEBE ejecutar lógica con efectos secundarios, pero NO DEBE hacer ambos.

La frase "efectos secundarios" significa ejecución de lógica no directamente relacionada con
declarar clases, funciones, constantes, etc.

Los "efectos secundarios" incluyen pero no se limitan a: generación de impresión de código, explícito uso de `require` o `include`, conexión a servicios externos, modificación de configuración .ini , emitir errores o excepciones, modificar variables globales o estáticas, leer o escribir en un archivo, y así sucesivamente.

El siguiente es un ejemplo de un archivo con declaraciones y efectos secundarios;
es decir, un ejemplo de lo que debe evitar:

```php
<?php
// efecto secundario: modificación de configuración .ini
ini_set('error_reporting', E_ALL);

// efecto secundario: cargar un archivo
include "file.php";

// efecto secundario: generación de impresión de código
echo "<html>\n";

// declaración
function foo()
{
    // cuerpo de la fucnión
}
```

El siguiente ejemplo es de un archivo que contiene declaraciones sin efectos secundarios; es decir, un ejemplo de qué emular:

```php
<?php
// declaración
function foo()
{
    // function body
}

// La declaración condicional *no* es un efecto secundario
if (! function_exists('bar')) {
    function bar()
    {
        // cuerpo de la fucnión
    }
}
```

## 3. Namespace y Nombres de Clases

Los namespaces y clases DEBEN seguir un PSR de "carga automática": [PSR-4].

Esto significa que cada clase está en un archivo por sí misma y está en un espacio de nombres de al menos un nivel: un nombre de proveedor de nivel superior.

Los nombres de las clases DEBEN declararse en `StudlyCaps`.

El código escrito para PHP 5.3 y posterior DEBE usar espacios de nombres formales.

Por ejemplo:

```php
<?php
// PHP 5.3 y posteriores:
namespace Vendor\Model;

class Foo
{
}
```

El código escrito para 5.2.x y anteriores DEBERÍA usar la convención de pseudo-namepacing de prefijos en nombres de clase. `Vendor_`.

```php
<?php
// PHP 5.2.x y anteriores:
class Vendor_Model_Foo
{
}
```

## 4. Constantes de Clase, Propiedades, y métodos

El término "clase" se refiere a todas las clases, interfaces y métodos.

### 4.1. Constantes

Las constantes de clase DEBEN declararse en mayúsculas con separadores de guiones bajos
Por ejemplo:

```php
<?php
namespace Vendor\Model;

class Foo
{
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
```

### 4.2. Propiedades

Los nombres de las propiedades DEBEN declararse en `$camelCase` o `$under_score` pero solo usar una forma por clase.

```php
<?php
namespace Vendor\Model;

class Foo
{
    $camelCase;
    $under_score;
}
```

### 4.3. Métodos

Los nombres de los métodos DEBEN declararse en `camelCase()`.

```php
<?php
namespace Vendor\Model;

class Foo
{
    funciton camelCase()
    {

    }
}
```