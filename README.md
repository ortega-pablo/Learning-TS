# Learning-TS

## Curso de OpenBootcamp para aprender Typescript.

En este readme voy a ir declarando el proceso de aprendizaje desarrollado mediante el curso de Open Bootcamp sumado a la documentación y curso de Microsoft Learn.

### ¿Qué es Typescript?

Typescript es un lenguaje de código abierto desarrollado por Microsoft. Es un supraconjunto de JavaScript, es decir que nos brinda todas las características conocidas de JavaScript agregando determinadas características que no estaban disponibles.

La principal ventaja de TypeScript es que permite agregar tipos estáticos al código JavaScript.

El tipado estático en tiempo de compilación de TypeScript modela estrechamente el tipado dinámico en tiempo de ejecución de JavaScript. El análisis de tipos de TypeScript se produce completamente en tiempo de compilación y no agrega sobrecarga en tiempo de ejecución al programa.

***
## Inicialización del proyecto

En primer lugar creamos la carpeta donde estará alojado todo el proyecto, en este caso Learning-TS.

Dentro del fichero raíz del proyecto, vamos a inicializar un proyecto de node modules ejecutando por consola el comando:

```
npm init
```

y realizamos las configuraciones de nuestra preferencia para el proyecto (nombre, versión, descripción, etc).

Una vez inicializado nuestro proyecto npm, podemos proceder a instalar las dependencias necesaria para trabajar sobre el mismo. Esto lo hacemos desde la consola mediante el comando:

```
npm i --save-dev @types/node nodemon ts-node typescript rimraf
```

- --save-dev: Declaramos que las dependencias a instalar son dependencias de desarrollo.
- @types/node: Node trabaja sobre librerías de Javascript, por este motivo necesitamos @types/node para poder trabajar en proyectos typescript.
- nodemon: Utilizado para que cuando se haga un cambio, el proyecto se refresque automáticamente.
- ts-node: es una versión del run-time de node que nos permite ejecutar archivos de typescript.
- typescript: instalamos typescript en el proyecto.
- rimraf: permite generar aplicaciones más potentes o más empaquetadas. Realiza un proceso similar a lo que hace webpack. Lo utilizamos para generar la build final del proyecto.

Es hora de inicializar typescript junto a su configuración. Para ello o hacemos mediante la consola con el siguiente comando:

```
npx tsc --init --rootDir src --outDir build --esModuleInterop --resolveJsonModule --lib es6 --module commonjs --allowjs true --noImplicitAny true
```

- npx: npx nos permite utilizar una librería sin necesidad de instalarla en nuestro proyecto.
- tsc --init: tsc typescript compiler. Será el encargado de transpilar todos nuestros archivos de TS a JS. El comando --init generará el archivo de configuración tsconfig.json y los comandos siguientes generarán configuraciones dentro de este archivo.
- --rootDir src: indica la carpeta donde se encuentran los archivos ts que se tienen que transpilar.
- --outDir build: indica la carpeta donde se almacenarán los archivos js ya transpilados.
- --esModuleInterop: nos permite definir si vamos a tener múltiples versiones de ECMAScript dentro de nuestro proyecto y proveerá la posibilidad de que estas versiones puedan operar entre sí sin problemas. Por defecto esta opción se activa en true.
- --resolveJsonModule: habilita la posibilidad de utilizar archivos formato JSON e indica que todas las configuraciones van a tener este formato que utiliza pares de clave valor.
- --lib es6: con el comando --lib indicamos qué versiones de ECMAScript vamos a utilizar, en este caso es6.
- --module commonjs: indica que voy a transpilar mi proyecto a common js para que todos los navegadores puedan leer correctamente el código.
- --allowjs true: permite que conviva ts con js dentro del mismo proyecto.
- --noImplicitAny true: restringe la posibilidad de aplicar el tipo de dato any de forma implícita.

Teniendo hecha la inicialización del proyecto y de las configuraciones typescript, podemos empezar a adornar el proyecto con algunos scripts para un acceso mas fluido a determinadas funciones.

Primero que nada debemos crear en la raíz del proyecto un archivo llamado nodemon.json, donde se escribirán las configuraciones para indicarle nodemon cómo debe comportarse en este proyecto, de la siguiente manera:

```json
{
  "watch": ["src"],
  "ext": ".ts, .js",
  "ignore": [],
  "exec": "ts-node ./src/index.ts"
}
```
- watch: indica qué quiero que nodemon esté escuchando.
- ext: especificamos el tipo de extensiones que se van a escuchar o ejecutar.
- ignore: indica los archivos o carpetas que nodemon debe ignorar cuando sena modificados.
- exec: indicamos el comando que nodemon debe ejecutar.

Una vez hecha esta configuración podemos generar los scripts necesarios en el archivo package.json como vemos a continuación:

```json
{
  "test": "echo \"Error: no test specified\" && exit 1",
  "start": "nodemon",
  "transpilation": "tsc",
  "build:prod": "rimraf .build && tsc",
  "start:prod": "npm run build:prod && node build/index.js"
}
```
- test: test por el momento no tiene ninguna configuración, (se le dará más adelante).
- start: va a indicarle a nodemon que se debe iniciar.
- transpilation: realizará la transpilación de todos los archivos typescript a archivos javascript mediante la configuración indicada anteriormente.
- build:prod: genera una versión de nuestro proyecto lista para producción y luego la transpila.
- start:prod: ejecuta la aplicación habiendo realizado el build para producción.

Para ejecutar cualquiera de estos scripts debemos introducir por consola el comando:

```
npm run "script que quiero ejecutar"
```

***

## Sintaxis, variables y estructuras de control

Ya hemos hecho la inicialización del proyecto junto a su configuración principal. Es hora de empezar a introducirnos en este lenguaje.

Como ya sabemos, TypeScript es un supraconjunto de JavaScript, lo que nos permite utilizar toda la sintaxis de ECMAScript junto a la sintaxis específica de TypeScript.

### Declaración de variables

Tal como ya utilizamos en JavaScript, las variables se pueden declarar con las palabras reservadas **var, let y const**. Pero typescript fomenta utilizar las palabras reservadas **let y const** ya que **var** declara una variable de ámbito global y **let y const** se utilizan para declarar variables con ámbito de nivel de bloque, lo cual evita que se declare la misma variable varias veces.

Ejemplo de declaración de variables:

```typescript
var variable1 = "Variable1"
let variable2 = "Variable2"
const variable3 = "Variable3"
```

### Tipado de variables

El tipo de una variable se puede expresar mediante una anotación explícita o implícita (inferencia).

El tipado explícito no es obligatorio, pero sí recomendad. Para declararlo, se utiliza la sintaxis *nombreVariable: tipo*. Por ejemplo, podemos declarar una variable llamada miNumero de tipo number de la siguiente manera:

```typescript
let miNumero: number = 12 
```
Por otro lado, si no colocamos el tipo, typescript intentará inferirlo mediante el valor que le demos a nuestra variable. Por ejemplo:

```typescript
let nombre = "Pablo"
```
En este caso Typescript inferirá que hemos declarado una variable llamada nombre de tipo string con el valor "Pablo". Para relacionar, esta misma variable declarada con el tipo de forma explícita quedaría de la siguiente manera:

```typescript
let nombre: string = "Pablo"
```
En la siguiente tabla podemos observar los diferentes tipos de datos que nos permite utilizar TypeScript, y luego desarrollaremos un poquito sobre cada uno:

- Tipo any
    - Tipos primitivos
        - Tipo booleano
        - Tipo número
        - Tipo cadena
        - Tipo enumeración
        - Tipo void
    - Tipos de objeto
        - clase
        - Interfaz
        - Matriz
        - Literales
    - Tipos parámetro

**Tipo any:** Todos los tipos de TypeScript son subtipos de un único tipo any. Éste es un tipo que puede representar cualquier valor de JavaScript sin restricciones.

***Tipos primitivos:*** son los tipos *boolean, number, string, void, null y undefined*, junto con enumeración definida por el usuario o tipos *enum*. Forman los principales bloques de compilación para tipos más complejos.

El tipo *void* existe únicamente para indicar la ausencia de un valor, como en una función sin ningún valor devuelto. Los tipos *null* y *undefined* son subtipos de todos los demás tipos. No se puede hacer referencia explícita a ellos, sólo se puede hacer referencia a los valores de ellos mediante los literales *null* y *undefined*.

***Tipos de objeto y parámetros de tipo:*** Los tipos de objeto son todos los tipos de clase, de interfaz, de matriz y literales, todo lo que no sea un tipo primitivo.

Los tipos de clase e interfaz se introducen mediante declaraciones de clase e interfaz, y se hace referencia a ellos con el nombre que se les ha asignado en sus declaraciones. Los tipos clase e interfaz pueden ser tipos genéricos que tienen uno o más parámetros de tipo. Haremos un desarrollo más profundo de todos estos tipos a continuación.

### Tipos primitivos en typescript



#### Tipo booleano

Es el tipo de dato más básico y su valor puede ser *true* o *false*

```typescript
let flag: boolean   //tipo boolean explícito
let yes = true      //tipo boolean inferido
let no = false      //tipo boolean inferido
```

#### Tipos numéricos

En TypeScript todos los valore numéricos son de tipo flotante (*float*) o BigInteger (*bigint*).

>Además de los literales hexadecimales y decimales, Typescript también admite los literales binarios y octales introducidos en ECMAScript 2015

```typescript
let x: number               //tipo number explícito
let y = 10                  //tipo number inferido
let z: number = 123.456     //tipo number explícito
let big: bigint = 100n      //tipo BigInteger explícito
```

#### Tipo cadena

Utilizan la palabra calve *string*, y representan secuencias de caracteres almacenados como unidades de código Unicode UTF-16.

Se pueden utilizar comillas dobles (") o comillas simples (') al igual que en JavaScript.

```typescript
let s: string       //tipo string explícito
let empty = ""      //tipo string inferido
let abc = 'abc'     //tipo string inferido
```

Al igual que en JavaScript podemos utilizar los template strings, que permiten abarcar varias líneas y contener expresiones insertadas. Por ejemplo: 

```typescript
let firsName: string = "Pablo"                      //tipo string explícito
let template: string = `Mi nombre es ${firstName}.
    Soy nuevo utilizando TypeScript`                //tipo string explícito como template
```

#### Tipo enumeración

La palabra clave reservada para este tipo es *enum*.

Ofrece una manera sencilla para trabajar con conjuntos de constantes relacionadas.

Permiten especificar una lista de opciones disponibles. Son muy útiles cuando se tiene un conjunto de valores que puede tomar un tipo de variables determinado. 

Podemos explicarlo de una manera más entendible usando un ejemplo:

Supongamos que tenemos un tipo de dato enumeración llamado *Estados*, y que dentro de sí lista los elementos *Completo, Incompleto y Pendiente* de la siguiente manera: 

```typescript
enum Estados {
    "Completo",
    "Incompleto",
    "Pendiente"
}
```
La enumeración es un tipo de dato que almacena pares de clave valor, siendo las claves las que nosotros declaramos en el ejemplo anterior. Por defecto el valor es numérico y en orden ascendente empezando desde 0, es decir, si vemos nuestro ejemplo la clave "Completo" va a tener un valor 0, "incompleto un valor 1 y "Pendiente" un valor 2.

La enumeración nos permite inicializar la secuencia de valores desde cualquier número. Por ejemplo:

```typescript
enum Estados {
    "Completo" = 5,
    "Incompleto",
    "Pendiente"
}
```
En este caso las claves "Completo", "Incompleto" y "Pendiente", van a tener valores 5, 6 y 7 respectivamente.

De la misma manera podemos asignarle valores que ya no sigan una secuencia de la siguiente manera:

```typescript
enum Estados {
    "Completo" = "C",
    "Incompleto" = "I",
    "Pendiente" = "P"
}
```
Este tipo de estructuras nos sirve por ejemplo para declarar el estado de una tarea. Veamos, si declaramos:

```typescript
let estadoTarea: Estados = Estados.Completo
```
Hemos declarado una variable estado Tarea, de tipo Estados, y cuyo valor (según el último ejemplo) será "C".

### Tipos any y unknown

En determinadas ocasiones debemos trabajar con valores que son desconocidos al momento de desarrollar el código. En estos casos posemos utilizar los tipos *any* y *unknown*, así como también podemos utilizar la aserción de tipos y las restricciones de tipo para poder controlar el tipado de los elementos.

#### Tipo any

*any* es un tipo que puede representar cualquier valor de JavaScript sin ninguna restricción. Esto puede ser de mucha utilidad para algunos casos, por ejemplo, cuando se espera un valor de una biblioteca de terceros o entradas de usuario en las que el valor es dinámico, ya que el tipo any permitirá volver a asignar distintos tipos de valores.

Como podemos ver en este ejemplo, podemos declarar una variable de tipo any y luego asignarle diferentes tipos de valores:

```typescript
let variable: any = 10
variable = "Mateo"
variable = true
```
El uso de tipo any permite llamar a diferentes métodos, elementos, propiedades, etc. Por ejemplo:

- Permite llamar una propiedad que no exista para el tipo.
- Permite llamar a la variable como si fuese una función.
- Permite llamar métodos que solo se aplican a un tipo String.

Este tipo de permisión sin restricciones hacen que el tipo any a veces sea muy complicado de manejar ya que muchas acciones sobre este tipo **no** generarán problemas en tiempo de compilación pero puede que si lo hagan en tiempo de ejecución.

Por este motivo debemos evitar utilizar el tipo *any* siempre que sea posible.

#### Tipo unknown

Aunque es flexible, el tipo *any* puede producir errores inesperados. Para solucionar esto, TypeScript introdujo el tipo *unknown*.

El tipo *unknown* también puede recibir cualquier tipo de valor, sin embargo, no se puede acceder a las propiedades de un tipo *unknown*, tampoco se puede llamar ni construir.

> La diferencia principal entre los tipos *any* y *unknown* es que no se puede interactuar con una variable de tipo *unknown*; si lo hacemos, se generará un error de compilación. *any* omite las comprobaciones en tiempo de compilación y el elemento se evalúa en tiempo de ejecución. Si el método o propiedad que queremos llamar de un elemento de tipo *any* existe, el código se comportará según lo esperado.

### Aserción de tipos

Una aserción de tipos permite tratar una variable como un tipo de datos diferente. Indica a TypeScript que se ha realizado cualquier comprobación necesaria antes de llamar a la instrucción. Es como decirle al compilador "Estoy seguro de lo que hago".

Una aserción de tipos es como una conversión de tipos en otros lenguajes, pero no realiza ninguna comprobación especial reestructuración de los datos. La utiliza sólo el compilador, por lo que no tiene ningún efecto en tiempo de ejecución

Las aserciones de tipos se pueden definir de dos formas. Como lo hacemos en el siguiente ejemplo sobre la variable llamada variable1:

```typescript
(variable1 as string).toUpperCase() //sintaxis de as
(<string>variable1).toUpperCase     // sintaxis de corchetes angulares
```

> *as* es la sintaxis preferida. La sintaxis de corchetes angulares puede generar conflictos con algunas aplicaciones de TypeScript como JSX.

En el siguiente ejemplo podemos ver las comprobaciones necesarias para poder usar la aserción de tipos sobre la variable variable1 y de esta forma poder utilizar el método toUpperCase:

```typescript
let variable1: unknown = 10

variable1 = true
variable1 = "Mateo"

if (typeof variable1 === "string") {
    console.log((variable1 as string).toUpperCase()) // retornará Mateo
} else {
    console.log("Error - Se esperaba un sting")  //retornará un mensaje de error
}
```

TypeScript ahora da por supuesto que realizamos la comprobación necesaria. La aserción de tipos indica que se debe tratar al elemento como un string y luego realizar el método toUpperCase.

### Restricción de tipos

En el ejemplo anterior se muestra el uso de typeof en el bloque if para examinar el tipo de una expresión en tiempo de ejecución. A esto de le llama **restricción de tipos**.

Se puede utilizar las siguientes condiciones para descubrir el tipo de una variable:

| Tipo      | Expresión                         |
|-----------|-----------------------------------|
| string    | typeof s === "string"             |
| number    | typeof n === "number"             |
| boolean   | typeof b === "boolean"            |
| undefined | typeof undefined === "undefined"  |
| function  | typeof f === "function"           |
| array     | Array.isArray(a)                  |

#### Tipos de unión, intersección y literales

Los tipos de unión e intersección permiten controlar situaciones en las que un tipo se compone de dos o más tipos posibles, mientras que los tipos literales permiten restringir los valores asignados a un tipo, a una lista reducida de opciones.

#### Tipos de unión

Un tipo unión describe un valor que puede ser uno de entre varios tipos. Esto puede ser muy util cuando no se tenga controlado un valor, como por ejemplo los valores de una biblioteca o una API.

Los tipos de unión restringen la asignación de los valores a los tipos especificados.

El tipo unión, utiliza la barra vertical para separar cada tipo. En el siguiente ejemplo vemos una variable tipoMultiple que puede valores tanto de tipo number como string, y todas las asignaciones siguientes son correctas:

```typescript
let tipoMultiple: number | string;

tipoMultiple = 20
tipoMultiple = "Mateo"
```
#### Tipos de intersección

Están estrechamente relacionados con los tipos de unión pero se utilizan de maneras muy diferentes.

Un tipo de intersección combina dos o más tipos para crear uno que tenga todas las propiedades de los tipos existentes. Esto permite agregar tipos existentes de forma conjunta para obtener un tipo único que tenga todas las características que necesita.

El tipo de intersección utiliza un & para separar cada tipo.

Los tipos de intersección se usan con mayor frecuencia en interfaces (tema que veremos más adelante). En el siguiente ejemplo se definen dos interfaces (Empleado y Gerente), y luego se crea un tipo de intersección llamado gerenteEmpleado que combina las propiedades de ambas interfaces:

```typescript
interface Empleado {
    empleadoId: number,
    edad: number
}

interface Gerente {
    planAcciones: boolean
}

type GerenteEmpleado = Empleado & Gerente

let nuevoGerente: GerenteEmpleado = {
    empleadoId: 12345,
    edad: 35,
    planAcciones: true
}
```

#### Tipos literales

Un literal es un subtipo más concreto de un tipo colectivo. Esto significa que "Hola Mundo" es un elemento *string*, pero un elemento *string* no es "Hola Mundo" dentro del sistema de tipos.

Hay tres conjuntos de tipos literales disponibles en typescript: *string*, *number* y *boolean*.

Los tipos literales se escriben como objetos, matrices, funciones o literales de tipo constructor y se usan para crear tipos a partir de ellos.

En el siguiente ejemplo realizamos una definición de tipo que crea un tipo literal denominado resultadoTest, que puede contener uno de estos tres valores string:

```typescript
type resultadoTest = "aprobado" | "desaprobado" | "incompleto"

let miResultado: resultadoTest

miResultado = "incompleto"
miResultado = "aprobado"
miResultado = "desaprobado"
```
### Matrices

Las matrices se pueden escribir de dos maneras. En la primera se utiliza el tipo de los elementos seguidos de corchetes:

```typescript
let lista: number[] = [1, 2, 3]
```

En el segundo caso, se usa un tipo *array* genérico con la sintaxis Array<type>:

```typescript
let lista: Array<number> = [1, 2, 3]
```

### Tuplas

Tener una matriz de los mismos tipos de valor es útil, pero a veces tiene una matriz que contiene valores de tipos mixtos. Para este propósito, TypeScript proporciona el tipo de tupla. Para declarar una tupla, se usa la sintaxis variableTupla: [type, type, type, ...].

### Interfaces

En TypeScript, las interfaces se utilizan de la misma forma que en la programación orientada a objetos, pero también se pueden utilizar para definir tipos de objetos.

En palabras simples, las interfaces definen la estructura del objeto, es decir, define cómo debería verse. En typescript, una interfaz solo contiene la definición de métodos y propiedades, no su implementación.

Para crear una interfaz, debemos utilizar la palabra reservada *interface*, la cual debe ser seguida del nombre de la interfaz, luego entre llaves se debe declarar un conjunto de elementos clave valor donde se declaran las propiedades y tipos de la misma de la siguiente manera:

```typescript
interface Tarea {
    nombre: string,
    estado: Estados,
    urgencia: number
}
```

El tipo Estados utilizado corresponde a la enumeración declarada en el ejemplo donde explicamos enumeración.

Las interfaces nos permiten declarar variables con su estructura teniendo ya definido el tipo de sus elementos, por ejemplo, podemos declarar la variable tarea1 de la siguiente manera:

```typescript
let tarea1: Tarea {
    nombre: "Tarea 1",
    estado: Estados.pendiente,
    urgencia: 10
}
```

Podemos detallar algunos de los usos de las interfaces en la siguiente lista: 

- Crear nombres abreviados para tipos de uso frecuente. Esto nos permite utilizar las ventajas de IntelliSense y la comprobación de tipos.

- Controlar la coherencia de un conjunto de objetos porque cada objeto que implementa la interfaz funciona bajo las mismas definiciones de tipo.

- Describir las Api de JavaScript existentes, y aclarar los parámetros de función y los tipos del valor devueltos. Una interfaz puede proporcionar una idea clara de lo que espera de una función y lo que devolverá, sin necesidad de consultar repetidamente la documentación.

Las interfaces se pueden extender unas a otras, lo que permite copiar los elementos de una interfaz en otra. Esto ofrece mayor flexibilidad a la hora de separar las interfaces en componentes reutilizables.

Para extender una interfaz en otra utilizamos la palabra reservada *extends*, por ejemplo, la siguiente interfaz llamada Detalle tiene todas las características de la interfaz Tarea junto a sus nuevas propiedades de detalle y propietario:

```typescript
interface Detalle extends Tarea {
    detalle: string,
    propietario?: string
}
```

El símbolo ? determina que el parámetro es opcional.
