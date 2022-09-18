# Javascript avanzado - Clase 04

##  AJAX

Async Javascript and XML

XML -> es una forma de comunicación, organiza la información a partir de etiquetas no inventadas, por ejemplo:

```sh
<factura>
    <cuit>244409423091</cuit>
</factura>
```

A través de AJAX, Javascript le tira peticiones al backend para que este le responda en XML.

Asincrónico -> tiene que ver cuando esto se va a ir a buscar, cuando tengo una hoja de estilo en mi HTML el navegador la va a encontrar instantáneamente:

```sh
setTimeout(() => {
    console.log("Hola, soy asincronica");
}, 1000); // va a tardar un segundo en ejecutarse
```

Sincrónica -> Cuando se carga el documento HTML este mismo se fija todas las peticiones de archivos que haya dentro de él, como lo son las imágenes, las hojas de estilo, los scripts, etc. Todo lo que tenga una ruta lo va a buscar después:

```sh
console.log("Hola soy sincrónica")
```

Algo importante también es que Javascript es monohilo por lo cual la interpretación del código se atiende por un único hilo de ejecución y los eventos se escuchan en secuencia, sin interrupción del anterior. Cuando le mandamos una petición al servidor (backend) yo no se cuanto va a tardar en responder y eso es un problema, entonces si hacemos una función AJAX para comunicarnos con el backend, esta entra en una cola de espera de respuesta del servidor pero mientras tanto las demás funciones del archivo se siguen cargando y el tráfico no se acumula.

Vamos a comenzar a trabajar con AJAX pero antes es necesario tener instalado la extensión "Live Server" ya que esto trabaja con el protocolo HTTP.

Lo primero que hay que hacer es instanciar un objeto de la clase XMLHttpRequest:

```sh
const xhr = new XMLHttpRequest; 
```

Este objeto tiene algo que se llama ReadyState el cual nos describe los 5 estados de carga del documento. 

* 0 -> Objeto creado.
* 1 -> Objeto configurado.
* 2 -> Headers enviados / Headers recibidos, ante cualquier inconveniencia que surja puedo abortar la petición.
* 3 -> Progreso de descarga.
* 4 -> Fin de la petición, pero esto no significa que el proceso se haya ejecutado como nosotros esperábamos, solo indica el fin de la petición y esto puede ser satisfactorio o no.

Ahora vamos a configurar el objeto haciendo uso del método .open() que detalla el medio por el cual me voy a comunicar recibiendo dos parámetros principales, el primero es un método "GET" en este caso que nos va a devolver la información y como segundo parámetro toma la ruta de la que voy a extraer la información, en este caso el archivo esta en servidor y carpeta, por lo que solamente pongo el nombre del archivo.

```sh
xhr.open("GET", "prueba.txt");  
```

Una vez que el objeto está configurado lo envío:

```sh
xhr.send();
```

Evento readystatechange -> se desata cuando el atributo readyState de un documento cambia

Este código devuelve el estado de carga del documento actual. Los estados de carga 0 y 1 no van a aparecer porque son estos son "regulados" por mi y no por el servidor.

```sh
xhr.addEventListener("readystatechange", ev => {
    console.log("ReadyState actual: " + xhr.readyState);
});
```

Teniendo en cuenta esto podemos jugar un poco con los estados de carga y reflejarlos de manera más explícita:

```sh
xhr.addEventListener("readystatechange", ev => {
    console.log("ReadyState actual: " + xhr.readyState);
    if (xhr.readyState === 2) {
        console.log("Headers recibidos");
    }

    if (xhr.readyState === 3) {
        console.log("Descarga en progreso...");
    }

    if (xhr.readyState === 4) {
        console.log("Peticion terminada");
    }
});
```

Mismo código pero haciendo uso de los eventos propios que nos brinda el objeto:

```sh
xhr.addEventListener("readystatechange", ev => {
    if (xhr.readyState === 2) {
        console.log("Headers recibidos");
    }
})

xhr.addEventListener("progress", ev => {
    console.log("Descarga en progreso");
})

xhr.addEventListener("load", ev => {
    console.log("Petición terminada");
})
```

Vamos a chequear el estado del documento:

```sh
xhr.addEventListener("load", ev => {
    if (xhr.status === 200) {
        console.log("Exito");
        document.querySelector("p").innerText = xhr.response // va a meter la espuesta dentro de nuestro HTML
    } else {
        console.log("Error");
    }
})
```

* NOTAS:
    * .response -> nos trae la respuesta del servidor.
    * .status -> nos devuelve el estado del documento en sí. El número 200 nos indica que está funcionando exitosamente, de no ser así expresa otro número. 

### Ejercicio

- Realizar una petición vía AJAX que obtenga un archivo de tipo HTML e insertarlo dentro de un div específico.

Solución:

Código HTML principal:

```sh
<!DOCTYPE html>
<html lang='en'>
<head>
    <meta charset='UTF-8'>
    <meta http-equiv='X-UA-Compatible' content='IE=edge'>
    <meta name='viewport' content='width=device-width, initial-scale=1.0'>
    <title></title>
    <link rel='shortcut icon' href='#'>
</head>
<body>
    <h1>Ejercicio</h1>
    <div id="objetivo"></div>

    <script src='ejercicio-clase04.js'></script>
</body>
</html>
```

Segundo archivo HTML con el texto a insertar en el principal:

```sh
<h2>Hola!</h2>
<p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Repellendus eos laboriosam odit nemo corporis, alias sed omnis expedita ipsa molestias?</p>
```

Código JS:

```sh
const xhr = new XMLHttpRequest;

xhr.open("GET", "archivo-de-referencia.html");

xhr.send();

xhr.addEventListener("load", ev => {
    document.getElementById("objetivo").innerText = xhr.response
})
```

Luego de realizar este ejercicio nos volcamos a la actividad que hicimos la clase anterior sobre la validación del formulario y nos centramos en realizar una única función de validación para todos los campos y no tener que repetir la misma función por cada escucha de evento que hagamos dicho campo.

Creamos la función para validar:

```sh
function validacion(target, callback) {
    const value = target.value
    const span = target.parentElement.querySelector(".errMsg")

    const msg = callback(value)

    target.setCustomValidity(msg)
    span.innerText = msg
}
```

La llamamos dentro del campo que queremos validar:

```sh
form.nombre.addEventListener("blur", ev => {
    const callback = (value) => {
        if (value.length < 2 || value.length > 45) {
            return "Debes contener entre 2 y 45 caracteres"
        }
        return ""
    }

    validacion(ev.target, callback)
})
```

La llamamos en otro campo más para validar:

```sh
form.apellido.addEventListener("blur", ev => {
    const callback = (value) => {
        if (value.length < 2 || value.length > 45) {
            return "Debes contener entre 2 y 45 caracteres"
        }
        return ""
    }

    validacion(ev.target, callback)
})
```

Y ahora ya nos queda un programa con un código más ligero y reutilizable

## Manipulación de Elementos

Nos adentramos de nuevo en el manejo de DOM pero esta vez creando elementos desde nuestro script e incorporándolos en el documento HTML.

Para crear un elemento:

```sh
const variable = document.createElement("selector");
const strong = document.createElement("strong");
```

Para verlo por consola:

```sh
console.dir(strong);
```

Lo modificamos:

```sh
strong.innerText = "Hola, soy un strong";
strong.style.color = "red";
strong.classList.add("ejemplo");
```

Generamos un nuevo elemento i que será el hijo de "strong" (elemento previamente creado):

```sh
const i = document.createElement("i");
i.innerText = "Soy un i";
strong.appendChild(i)
```

Para agregar los elementos que creamos a nuestro HTML es importante tener un ancla en el mismo, es decir, que si quiero añadir ciertos elementos primero tengo que tener un elemento ya generado en el HTML (esto se conoce como ancla) del cual me va a servir de sostén o de referencia para incluir a los demás. Por ejemplo, si yo creo un elemento "div" en el script, podría usar de ancla al "body" para asignar este componente al HTML.

```sh
const div = document.createElement("div");

document.querySelector("body").appendChild(div);
```

O bien siguiendo el ejemplo con el código que veníamos haciendo anteriormente:

```sh
document.querySelector("p").appendChild(strong);
```






