# Learning-TS

### Curso de OpenBootcamp para aprender Typescript.

En este readme voy a ir declarando el proceso de aprendizaje desarrollado mediante el curso de Open Bootcamp

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
- --esModuleInterop: nos permite definir si vamos a tener múltiples versiones de EcmaScript dentro de nuestro proyecto y proveerá la posibilidad de que estas versiones puedan operar entre sí sin problemas. Por defecto esta opción se activa en true.
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

