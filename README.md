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

y realizamos las configuraciónes de nuestra preferencia para el proyecto (nombre, versión, descripción, etc).

Una vez inicializado nuestro proyecto npm, podemos proceder a instalar las dependencias necesaria para trabajar sobre el mismo. Esto lo hacemos desde la consola mediante el comando:

```
npm i --save-dev @types/node nodemon ts-node typescript rimraf
```

- --save-dev: Declaramos que las dependencias a instalar son dependecias de desarrollo.
- @types/node: Node trabaja sobre librerías de Javascript, por este motivo necesitamos @types/node para poder trabajar en proyectos typescript.
- nodemon: Utilizado para que cuando se haga un cambio, el proyecto se refresque automáticamente.
- ts-node: es una versión del run-time de node que nos permite ejecutar archivos de typescript.
- typescript: instalamos typescript en el proyecto.
- rimraf: permite generar aplicaciónes más potentes o más empaquetadas. Realiza un proceso similar a lo que hace webpack. Lo utilizamos para generar la build final del proyecto.

Es hora de inicializar typescript junto a su configuración. Para ello o hacemos mediante la consola con el siguiente comando:

```
npx tsc --init --rootDir src --outDir build --esModuleInterop --resolveJsonModule --lib es6 --module commonjs --allowjs true --noImplicitAny true
```

- npx: npx nos permite utilizar una librería sin necesidad de instalarla en nuestro proyecto.
- tsc --init: tsc typescript compiler. Será el encargado de transpilar todos nuestros archivos de TS a JS. El comando --init generará el archivo de configuración tsconfig.json y los comandos siguientes generarán configuraciones dentro de este archivo.
- --rootDir src: indica la carperta donde se encuentran los archivos ts que se tienen que transpilar.
- --outDir build: indica la carpeta donde se almacenarán los archivos js ya transpilados.
- --esModuleInterop: nos permite definir si vamos a tener múltiples versiones de ECMAScript dentro de nuestro proyecto y proveerá la posibilidad de que estas versiones puedan operar entre sí sin problemas. Por defecto esta opción se activa en true.
- --resolveJsonModule: habilita la posibilidad de utilizar archifo formato JSON e indica que todas las configuraciónes van a tener este formato que utiliza pares de clave valor.
- --lib es6: con el comando --lib indicamos qué versiones de Ecma Script vamos a utilizar, en este caso es6.
- --module commonjs: indica que voy a transpilar mi proyecto a common js para que todos los navegadores puedan leer correctamente el código.
- --allowjs true: permite que conviva ts con js dentro del mismo proyecto.
- --noImplicitAny true: restringe la posibilidad de aplicar el tipo de dato any de forma implícita.

Teniendo hecha la inicialización del proyecto y de las configuraciónes typescript, podemos empezar a adornar el proyecto con algunos scripts para un acceso mas fluído a determinadas funciónes.

Primero que nada debemos crear en la raíz del proyecto un archivo llamado nodemon.json, donde se escribirán las configuraciónes para indicarle nodemon cómo debe comportarse en este proyecto, de la siguiente manera:

```json
{
  "watch": ["src"],
  "ext": ".ts, .js",
  "ignore": [],
  "exec": "ts-node ./src/index.ts"
}
```
- watch: indica qué quiero que nodemosn esté escuchando.
- ext: especificamos el tipo de extensiónes que se van a escuchar o ejecutar.
- ignore: indica los archivos o carpetas que nodemosn debe ignorar cuando sena modificados.
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
- transpilation: realizará la transpilación de todos los archivos typescript a archivos javascript mediante la configuracion indicada anteriormente.
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

Tal como ya utilizamos en typescript, las variables se pueden declarar con las parlabras reservadas **var, let y const**. Pero typescript fomenta utilizar las palabras reservadas **let y const** ya que **var** declara una variable de ámbito global y **let y const** se utilizan para declarar variables con ámbito de nivel de bloque, lo cual evita que se declare la misma variable varias veces.

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
En este caso typescrip inferirá que hemos declarado una variable llamada nombre de tipo string con el valor "Pablo". Para relacionar, esta misma variable declarada con el tipo de forma explícita quedaría de la siguiente manera:

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

**Tipo any:** Todos los tipos de TypeScript son subtipos de un único tipo any. Éste es un tipo que puede representar cualquier valor de JavaScript sin restricciónes.

***Tipos primitivos:*** son los tipos *booblean, number, string, void, null y undefined*, junto con enumeración definida por el usuario o tipos *enum*. Forman los principales bloques de compilación para tipos más complejos.

El tipo *void* existe únicamente para indicar la ausencia de un valor, como en una funbción sin ningún valor devuelto. Los tipos *null* y *undefined* son subtipos de todos los demás tipos. No se puede hacer referencia explícita a ellos, sólo se puede hacer referencia a los valores de ellos mediante los literales *null* y *undefined*.

***Tipos de objeto y parámetros de tipo:*** Los tipos de objeto son todos los tipos de clase, de interfaz, de matriz y literales, todo lo que no sea un tipo primitivo.

Los tipos de clase e interfaz se introducen mediante declaraciónes de clase e interfaz, y se hace referencia a ellos con el nombre que se les ha asignado en sus declaraciónes. Los tipos clase e interfaz pueden ser tipos genéricos que tienen uno o más parámetros de tipo. Haremos un desarrollo más profundo de todos estos tipos a continuación.

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

Al igual que en JavaScript podemos utilizar los template strings, que permiten abarcar varias líneas y contener expresiónes insertadas. Por ejemplo: 

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

Supongamos que tenemos un tipo de dato enumeración llamado *Estados*, y que dentro de sí lista los elementos *Completo, Incompleto y Pendiente* de la siguente manera: 

```typescript
enum Estados {
    "Completo",
    "Incompleto",
    "Pendiente"
}
```
La enumeración es un tipo de dato que almacena pares de clave valor, sindo las claves las que nosotros declaramos en el ejemplo anterior. Por defecto el valor es numérico y en orden ascendente empezando desde 0, es decir, si vemos nuestro ejemplo la clave "Completo" va a tener un valor 0, "incompleto un valor 1 y "Pendiente" un valor 2.

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
Este tipo de estructuras nos sirve por ejemplo para declarar el estado de una tarea. Por ejemplo si declaramos:

```typescript
let estadoTarea: Estados = Estados.Completo
```
Hemos declarado una variable estado Tarea, de tipo Estados, y cuyo valor (según el último ejemplo) será "C".
