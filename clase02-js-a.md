# Javascript avanzado - Clase 02

> Repaso sobre la clase anterior, scope de las variables, arrays y objetos como datos mutables, etc.

## Tipos de datos

Datos simples o primitivos:

* string
* number
* boolean
* undefined
* null
* NaN

Datos complejos u objetos:

* Array (indice numérico) 
* Obj (asociativo/string)
* Funciones
* Expresiones Regulares

Datos primitivos o simples -> se guarda propiamente el valor en el espacio de memoria de la variable.

Datos complejos u objetos -> se guarda una referencia. Se crea un espacio de memoria para guardar el objeto pero el objeto no se puede guardar en una variable ya que es muy pesado, por esto es que se guarda solo una referencia.

```sh
const a = {x: 123}; // se guarda la referencia
a.x = 555 -> modifico la referencia principal

const b = a // No se duplicó el objeto, sino que se copió la referencia
b.x = 888 -> modifico la referencia principal

function ejemplo(variable){
    variable.x = 999;
}

ejemplo(a) // con esta funcion pasa lo mismo, se realiza una copia de la referencia, por lo que las modificaciones afectan directamente sobre el objeto principal
```

## Eventos - continuación

### Propagación

#### Código CSS:

```sh
#padre{
    height: 300px;
    width: 300px;
    background-color: seagreen;
}

#hijo{
    height: 250px;
    width: 250px;
    background-color: cadetblue;
}

#nieto{
    height: 200px;
    width: 200px;
    background-color: indianred;
}
```

#### Código HTML:

```sh
<div id="padre">
	PADRE
		<div id="hijo">
			HIJO
			    <div id="nieto">NIETO</div>
		</div>
	</div>
```

#### Código JS:

```sh
const padre = document.querySelector("#padre");

// dos formas distintas de traer un elemento
const hijo = padre.querySelector("#hijo"); // esto no se rompe, se adapta
const hijo = padre.children[0]; // atada al esquema html y su orden, si agrego un nuevo hijo esto se va a romper

const nieto = hijo.children[0];

padre.addEventListener("click", ev => {
    ev.stopPropagation();
    console.log("PADRE");
})

hijo.addEventListener("click", ev => {
    ev.stopPropagation();
    console.log("HIJO");
})

nieto.addEventListener("click", ev => {
    ev.stopPropagation(); // detenemos la propagacion en el evento
    console.log("NIETO");
})
```

Notas:

* En el parámetro "ev" estamos haciendo referencia al "objeto evento" el cual guarda toda la información del evento, es decir, en que momento se hizo click, etc.

* Cuando hago click en nieto, voy a estar tambien haciendo click en hijo y en padre, esto es la propagación y se debe a que nieto está contenido dentro de hijo y el hijo está contenido dentro de padre. Comúnmente la propagación se deshabilita ya que a veces puede ser peligrosa.

#### Hay dos tipos de propagación:

* Desde Adentro hacia afuera -> El que ya vimos anteriormente.

* Desde Afuera hacia adentro -> poniendole un booleano (true) en la escucha de evento indicamos que la ejecucion será a la inversa, es decir, de afuera hacia adentro:

```sh
nieto.addEventListener("click", ev => {
    console.log("NIETO");
}, true)
```

### ¿Como prevenir el comportamiento por defecto de un elemento?

Primero traemos el elemento HTML a nuestro JS, luego realizamos una escucha de evento seleccionando el evento que queremos afectar y por ultimo lanzamos una función callback (en este caso es de tipo flecha) tomando como parametro al objeto evento y pasandole al mismo la función .preventDefault()

```sh
document.querySelector("a").addEventListener("click", ev => {
	ev.preventDefault()
})

document.querySelector("form").addEventListener("submit", ev => {
	ev.preventDefault()
})

document.querySelector("input[type='text']").addEventListener("keydown", ev => {
	ev.preventDefault()
})

document.querySelector("input[type='checkbox']").addEventListener("click", ev => {
	ev.preventDefault()
})	
```

* Nota: Los elementos que traemos desde el HTML los almacenamos normalmente en una constante, pese a esto podemos modificar el objeto e incluso eliminar dicha referencia, lo que no se puede hacer es cambiar esa referencia por otra.

* Nota: Cuando se pone input[type='text'] esto se refiere a todo lo que se puede escribir dentro de un input, ya sea una password, un email, etc.

#### Código HTML:
 
```sh
<body>
	<h1>Eventos 3 - Continuacion</h1>

	<input type="text">

	<input type="checkbox" value="acepto" >

	<select>
		<option value="" disabled selected>Seleccione una opcion</option>
		<option>A</option>
		<option>B</option>
	</select>
</body>
```

#### Código JS:

Traemos los elementos al JS:

```sh
const text = document.querySelector("input[type='text']")
const check = document.querySelector("input[type='checkbox']")
const select = document.querySelector("select")
```

Realizamos una escucha del evento keydown, el cual se desata cuando se presiona una tecla. Mediante el método 'key' accedemos a la información de la tecla que se pulsó. 

```sh
text.addEventListener("keydown", ev => {
	if(ev.key === "%"){
		ev.preventDefault()
	}
})
```

En el siguiente código se genera una escucha del evento input el cual se desata cuando se escribe algo nuevo. En este caso accedemos al valor que tiene dicho elemento (lo que se le escribió dentro) y contamos la cantidad de carácteres que posee haciendo uso del método .length. 

```sh
text.addEventListener("input", ev => {
	console.log("Cantidad de caracteres " + ev.target.value.length)
})
```

Una posible representación de los eventos "keydown" e "input" sería:

    -> presiono la tecla
        -> recibe la info de la tecla
            (se desata el keydown)
        
            -> dibuja el caracter
                (se desata el input)

* El target es la propiedad del objeto evento que hace referencia al elemento que fue afectado por dicho evento, es decir que al escribir "objetoEvento.target" estamos indicando "objetoEvento.elemento".

* El value indica el valor que hay dentro del input. Una forma de acceder al mismo sería como en el ejemplo anterior, pásandolo como propiedad del target: 

```sh
console.log("Cantidad de caracteres " + ev.target.value.length)
```

#### Evento focus 

Se desata al entrar al input:

```sh
text.addEventListener("focus", ev => {
    console.log("FOCUS");
})
```

#### Evento blur:

Se desata al salir del input:

```sh
text.addEventListener("blur", ev => {
    console.log("BLUR");
});
```

O bien se puede plantear un condicional:

```sh
text.addEventListener("blur", ev => {
    if (ev.target.value.length > 10) {
        console.log("Error");
    }
});
```

#### Evento change

Siempre que yo haga un cambio en el input se ejecuta el change, si no modifico nada el change no se desata

```sh
select.addEventListener("change", ev => {
    console.log(ev.target.value);
})
```

#### Propiedad checked

En un input de tipo "checkbox" se puede hacer uso de la propiedad ".checked" la cual devuelve un booleano en base a si está o no "checkeada" o "seleccionada" la opción.  

```sh
check.addEventListener("click", ev => {
    console.log(ev.target.checked);
});
```

## Validaciones

#### Código HTML:

```sh
<body>
	<h1>Validaciones</h1>

	<form action="">
		<div class="campo">
			<label for="ejemplo">Ejemplo</label>
			<input type="text" id="ejemplo" name="ejemplo">
			<span class="errMsg"></span>
		</div>

		<button type="submit">Enviar</button>
	</form>
</body>
```

* Nota: En los formularios los elementos hijos se pueden recorrer con un bucle for o acceder directamente a ellos mediante la nomenclatura del punto (form.elementoHijo)

#### Código JS:

Traemos al ámbito global el formulario de HTML:

```sh
const form = document.querySelector("form");
```

Ahora ya podemos empezar a trabajar con el formulario:

```sh
form.ejemplo.addEventListener("blur", ev => {
    const value = ev.target.value;

    if (value.length < 2){
        console.warn("Error: debe escribir mas de 2 caracteres");
    } else if (value.length > 8) {
        console.warn("Error: debe escribir menos de 8 caracteres")
    } else {
        console.log("OK")
    }

    form.addEventListener("submit", ev => {
        ev.preventDefault()
    })
})
```

Cuando se sale de la validación del input, se setea el error (si es que hay) pero me voy a dar cuenta de estos errores cuando haga un "submit" de los datos. Lo práctico de esto es que el formulario no se enviará si es que ocurre un error, para ello también es que prevenimos el comportamiento por defecto del formulario con .preventDefault().

En el siguiente código hacemos uso de la función .setCustomValidity() la cual lo que hace es mostrar una señal de error en el caso de que se le pase un mensaje por argumento, por ejemplo:

```sh
objetoEvento.target.SetCustomValidity("Esto es un mensaje de error y por lo tanto se mostrará un mensaje de error");
```

O bien se puede omitir el error pasándole por argumento una cadena vacía. 

```sh
objetoEvento.target.SetCustomValidity("");
```

Algo muy importante es que cuando se haga uso de la función .SetCustomValidity() para señalar un error, si o si tiene que haber un caso en el que se llamé a la función pero se le pase una cadena vacía. Esto es para evitar que el error persista durante toda la ejecución pese a que no haya ningún error. Un ejemplo de este uso es:

```sh
form.ejemplo.addEventListener("blur", ev => {
    const value = ev.target.value;

    if (value.length < 2){
        ev.target.setCustomValidity("Error: debe escribir más de 2 caracteres")
    } else if (value.length > 8) {
        ev.target.setCustomValidity("Error: debe escribir menos de 8 caracteres")
    } else {
        ev.target.setCustomValidity("")
    }

    form.addEventListener("submit", ev => {
        ev.preventDefault()
    })
})
```

El siguiente código tiene el mismo fin que el código anterior pero con la salvedad de que acá hacemos uso de un elemento "span" para mostrarle o indicarle al usuario el mensaje de error en caso de que este ocurra.

```sh
form.ejemplo.addEventListener("blur", ev => {
    const value = ev.target.value;
    const span = ev.target.parentElement.querySelector(".errMsg");

    if (value.length < 2){
        span.innerText = "Error: debe escribir mas de 2 caracteres"
    } else if (value.length > 8) {
        span.innerText = "Error: debe escribir menos de 8 caracteres";
    } else {
        span.innerText = "";
    }

    form.addEventListener("submit", ev => {
        ev.preventDefault()
    })
})
```

Lo mismo que lo anterior pero de una forma más abreviada y óptima:

```sh
const form = document.querySelector("form");

form.ejemplo.addEventListener("blur", ev => {
    const value = ev.target.value;
    const span = ev.target.parentElement.querySelector(".errMsg");

    let msg = "";

    if (value.length < 2){
        msg = "Error: debe escribir mas de 2 caracteres"
    } else if (value.length > 8) {
        msg = "Error: debe escribir menos de 8 caracteres"
    } 

    ev.target.setCustomValidity(msg)
    span.innerText = msg;

    form.addEventListener("submit", ev => {
        ev.preventDefault()
    })
})
```

Ahora realizando una validación de email en base a si el texto tiene un "@":

```sh
const form = document.querySelector("form");

form.ejemplo.addEventListener("blur", ev => {
    const value = ev.target.value;
    const span = ev.target.parentElement.querySelector(".errMsg");

    let msg = "";

    if (!value.includes("@")){
        msg = "Debe tener un @"
    }

    ev.target.setCustomValidity(msg)
    span.innerText = msg;

    form.addEventListener("submit", ev => {
        ev.preventDefault()
    })
})
```

## Ejercicio:

Crear un formulario para crear un usuario

* Nombre -> text -> blur -> +2 y -45 
    PLUS -> Evitar que escriba NO alfanumericos, solo permitir alfanumericos, para esto se podria manejar el evento keydown
* Apellido -> text -> Mismas restricciones que el nombre
* Edad -> text -> +18 y -120 / que sea solo numero (para esto podemos usar la función Number())
* Email -> text -> texto@texto.texto a@a.a
    Funciones que podemos usar para esto -> includes, split, indexOf 
* Categoria -> A, B, C (select)
    Está habilitado? -> checkbox -> true -> OK
    Si está deshabilitado -> false -> Error



