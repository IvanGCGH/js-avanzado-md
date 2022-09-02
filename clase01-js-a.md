# Javascript avanzado - Clase 01

El profesor comenzó hablando de la importancia de llevarnos la parte lógica al aprender cualquier lenguaje. En este curso veremos JS Vanilla (sin frameworks).

Van a ser 3 clases por semana (lunes, miércoles y viernes) y se recomienda 4 horas de práctica fuera de la clase.

REST -> Es un standar de conexion de datos del servidor entre el back y el front.

### Constructores de variables:

Las variables let y const se almacenan en memoria, mientras que var se almacena en el objeto window.

let -> no se puede redeclarar pero si cambiarle el valor. El scope es con respecto al bloque donde esta variable se encuentre.

const -> no se puede redeclarar ni cambiar el valor. Es local, solo existe dentro del bloque de codigo donde se encuentre.

var -> accesible desde cualquier lado de la función, se puede redeclarar y redifinir. En cualquier bloque de codigo que se encuentre sigue siendo global. Menos en el caso de las funciones ya que solo existe en el ambito de la función y no por fuera de la misma.

Para let y const:

> "Si hay llaves, la variable solo existe dentro de esas llaves"

## Funciones:

Función tradicional: la puedo ejecutar en cualquier lugar del codigo, cuando arranca a interpretar el codigo js corta las funciones y las pone arriba leyendolas primero antes que todo lo demás.

```sh
// declarativo / se guarda en el objeto window = window.tradicional = function(){}
function tradicional(){

}

o bien:

// expresivo / se crea en el mismo momento en que la declaro, es decir que no puedo llamarla desde cualquier parte del código
const tradicional2 = function(){

}
```

Función flecha:

```sh
const flecha = () => {

}
```

La funcion flecha es más liviana ya que tiene menos propiedades que una común

Contexto -> palabra reservada 'this'. En el caso de las funciones flecha no tienen la palabra this. Recordar que this hace referencia al propio objeto de la clase.


Funciones de primer order -> son un tipo de dato, las puedo pasar dentro de un parametro, etc.

La funcion flecha tiene como proposito que sea mas liviana y de poder resumir lineas de codigo

Formas de escribirlas:

Lo que nunca puede faltar es la flecha característica.

```sh
let funcion

function = () => {}

// Según la cantidad de parametros que recibe

si tengo 0 parametros:

function = () => {}

con 1 parametro ():

function = param => {}

más de un parametro:

function = (param, param2) => {}

// Según la cantidad de lineas de codigo

+1 linea

function = () => {
    linea1
    linea2
}

Cuando es 1 sola linea puedo sacar las {} y obviar el return

function = () => console.log()

o bien:

function = (num) => num * 2
```

## BOM y DOM

Browser Object Model -> Representación en objeto de TODO el navegador, es una interfáz que en el fondo tiene una API (Application Program Interface, algo que está en el medio DE (una interfáz, en este caso un navegador) y nos sirve para comunicarme CON, por ejemplo un traductor entre dos lenguajes, podría ser el inglés). 

Un lenguaje puede estar hecho en C++, como en Python, etc, y el script está hecho en JS, ¿Como se podria hacer una relación entre ambos? Necesitamos que haya algo que esté entre medio de los dos (recordar que el navegador ya brinda una interfaz) y justamente esto es la API BOM que tiene dentro metodos tales como el alert(), prompt(), confirm(), document.write(), etc. Estos metodos anteriores son herramientas que me brindan el navegador para comunicarme con el lenguaje en el que esté fabricado el navegador.

JS -> metodo -> se consumen las instrucciones al lenguaje del navegador

Si bien hay una compilación, se trata de una "pre-compilacion" que realiza el navegador para saber que hacer frente a las instrucciones del script, pero no se realiza una compilación directa hacia el lenguaje en el que esté creado el navegador (puede ser C++, Python, Pascal, etc)

Entonces, ¿Qué es una API?

Es la posibilidad de manipular el navegador sin saber el lenguaje propio del navegador a través de la interfáz que se me brinda. Lenguaje base a un lenguaje X

### Objeto window

* Nota: el console es una entidad aparte, está por fuera del objeto window porque representa la consola de acceso.

Fuente sobre referencias hacia las API WEB:
<https://developer.mozilla.org/es/docs/Web/API>

El objeto window es una representación del objeto sobre todo el navegador.

### DOM (esta dentro del BOM)

Document Object Model -> Es una representación en objeto de TODO el documento HTML. Es una interfáz (API) que nos traduce la información del archivo HTML hacia el lenguaje del navegador. 

La interpretación es a través de una "cadena de arbol" (padres e hijos), el objeto comienza con Document, luego sigue el hijo que el <html>, luego los dos hijos <head> y <body> y así sucesivamente...

Para traer un elemento HTML a nuestro JS:

variable = document.querySelector("elemento");
const h1 = document.querySelector("h1");

JS interpreta a los elementos HTML como objetos, para visualizarlo mejor:

console.dir(variable_del_elemento)
console.dir(h1)

#### Notas

* classList -> pseudoAPI para manipular las clases de los elementos y modificarle o quitarle sus clases.

    * elemento.classList.toggle(clase) -> si el elemento no tiene una clase se la agrega y si ya tiene se la quita.

* style -> PseudoAPI con la cual le puedo modificar o agregar una propiedad en linea al elemento.

* innerText -> recupera y establece el contenido de la etiqueta como texto plano. Trabaja con nodos de texto.

* innerHTML -> recupera y establece el mismo contenido pero en formato HTML. Trabaja con nodos de texto y de elemento. Un ejemplo de esto sería:

```sh
elemento.innerHTML = "<elemento>Me permite agregar un elemento hijo como este</elemento>"
```

Un nodo es un punto de intersección, en el HTML representa como una sección del documento puede contener otra, es decir, que representa una parte de lo que es el "arbol de cadena". Por ejemplo, el primer nodo sería el "document", el segundo sería el "html" el cual tiene otros dos nodos los cuales son el "head" y el "body", este "body" puede tener hijos como un "h1", "span" en donde los hijos de estos elementos puede ser el "text" que tienen contenido dentro.

## Eventos

Generamos un evento:

```sh
const h1 = document.querySelector("h1");

h1.onclick = function (){
    console.log("Click al h1);
}
```

Estó lo que hará es que cuando hagamos click en el h1 que se encuentra en el documento HTML imprimirá por consola el mensaje de "Click al h1".

Cuando hago click lo que pasa que es estoy entrando en el objeto del elemento HTML y estoy asignandole a la propiedad del evento onclick la función ya escrita en el codigo anterior. Esto quiere decir que no pueden coexistir dos llamadas a eventos sin que una sobreescriba a la otra.


```sh
h1.addEventListener("click", () => {
    console.log("Click listener 1")
})

h1.addEventListener("click", () => {
    console.log("Click listener 2")
})

h1.addEventListener("click", () => {
    console.log("Click listener 3")
})
```

En este caso al ser escuchas de evento no sobreescribe un evento por otro, sino que se asocian y se guardan afuera, en un objeto externo que almacena todas las funciones callback de los eventos, por lo que estas funciones no saben nada de una respecto a la otra. Un ejemplo de como se guardarían sería algo así: 

```sh
events = {
    element: [
        h1: {
            "click": [
                () => {},
                () => {},
                () => {}
            ]
        }
    ]
}
```

Ahora lo que hacemos es traer un boton del HTML a nuestro JS y jugar con sus clases.

En el documento HTML:

```sh
<body>
    <button>Boton<button>
</body>
```

En el archivo CSS:

```sh
.dark {
    background-color: darkslategrey;
    color: white;
}

.dark button{
    background-color: dark;
    color: white;
}
```

En el archivo JS:

```sh
document
    .querySelector("#btnCambiarClase")
    .addEventListener("click", ev => {
        h1.classList.toggle("dark") // sacamos y agregamos una clase
        document.body.classList.toggle("dark") 
    })    
```

### EJERCICIO:

* Generar un html con header (h1, nav(a*4)), main y footer (p)

El main deberá ser así:

main ->
    section -> titulo h2 y 3 parrafos
    añadir un boton que nos permita cambiar el color de fondo
    PLUS -> 3 posibilidades
            clase light
            clase dark
            clase rainbow

* Vincular un JS externo
* Vincular un CSS externo

* Pista: document.body.classList.contains("dark"); // se fija si tiene asignada o no dicha clase y en base a eso devuelve un booleano.

> Formula para escribir elementos en HTML más rápido:  (p>lorem)*3

SOLUCIÓN:

* En el archivo HTML:

```sh
<!DOCTYPE html>
<html lang='en'>
<head>
    <meta charset='UTF-8'>
    <meta http-equiv='X-UA-Compatible' content='IE=edge'>
    <meta name='viewport' content='width=device-width, initial-scale=1.0'>
    <title>Ejercicio</title>
    <link rel='shortcut icon' href='#'>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>Esto es un titulo</h1>
        <nav>
            <a href="">Link 1 |</a>
            <a href="">Link 2 |</a>
            <a href="">Link 3 |</a>
            <a href="">Link 4</a>
        </nav>
    </header>
    <main>
        <section>
            <h2>Esto es el segundo titulo</h2> <button id="cambiar">Cambiar</button>
            <p>Lorem ipsum dolor sit amet consectetur, adipisicing elit. Iste, maxime.</p>
            <p>Lorem ipsum dolor sit amet consectetur, adipisicing elit. Perspiciatis, culpa?</p>
            <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Tempore, ullam!</p>
        </section>
    </main>
    <footer>
        
    </footer>
        <p>&copy; @2022</p>
    <script src='main.js'></script>
</body>
</html>
```

* En el archivo CSS:

```sh
*{
	margin: 0;
	padding: 0;
	box-sizing: border-box;

	font-family: calibri;
}

.light{
	background-color: #e7f0e4;
	color: #272e25;
}

.dark{
	background-color: #272e25;
	color: #e7f0e4;
}

.rainbow{
	background-image: linear-gradient( 45deg, #ff1b6b, #45caff, #00ff87 );
}
```

* En el archivo JS:

```sh
document.querySelector("#btnCambiar").addEventListener("click", ev => {
    if (document.body.classList.contains("light")) {
        document.body.classList.remove("light")
        document.body.classList.add("dark")
    }
    else if (document.body.classList.contains("dark")) {
        document.body.classList.remove("dark")
        document.body.classList.add("rainbow")
    }
    else if (document.body.classList.contains("rainbow")) {
        document.body.classList.remove("rainbow")
        document.body.classList.add("light")
    }
})
```
