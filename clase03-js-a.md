# Javascript avanzado - Clase 03

## Expresiones regulares

Llamando al constructor:

```sh
let pattern = 'love'
let flag = 'gi'
let regEx = new RegExp(pattern, flag)
let regEx = new RegExp('love','gi')
```

Sin usar el constructor:

```sh
let regEx= /love/gi
```

Son patrones de búsqueda dentro de cadenas de texto

* Literal -> "asd", "estacadena"

* Metacaracteres -> \w -> alfanumérico

Hay dos formas de declarar una expresion regular:

```sh
let regExp
regExp = /a/ -> "H-a-bía una vez"
regExp = /ab/ -> "H+ab+ia una vez
```

Para buscar:

```sh
regExp = /\w/ // -> alfanuméricos
regExp = /\d/ // -> numeros
regExp = /\s/ // -> espacios o tabulaciones
```

Para buscar por una o mas coincidencias:

```sh
regExp = /\w+/ // 
```

Para cero o ninguna coincidencia:

```sh
regExp = /\w?/ 
```

Para 2 coincidencias:

```sh
regExp = /\w{2}?/ 
```

Para más de dos coincidencias:

```sh
regExp = /\w{2,}?/
```

Entre 2 y 5 coincidencias:

```sh
regExp = /\w{2,5}?/
```

Para buscar 3 coincidencias de texto que vaya desde la A a la Z junto a 3 coincidencias de números:

```sh
"asd123" -> /[a-z]{3}\d{3}/
```

Lo que pasa con lo anterior es que pese a que encuentre esa coincidencia, el texto que se ponga después también va a figurar como "matcheado". Para establecer un límite hacemos uso de ^ (le decimos que "empiece con esto") y $ ("que termine con esto"):

```sh
"asd123" -> /^[a-z]{3}\d{3}$/
```

Cadena:

```sh
"dominio.com/users/2"
```

Expresión regular:

```sh
/\/users\/(\d)/
```

### Algunos metodos:

#### .test()

El primero es .test, lo que va a ser es que se va a fijar si el valor que le pasamos por parámetro coincide con el que estamos buscando dentro de la cadena. Lo que devuelve es un booleano:

```sh
let text = "This is some text"; 
let pattern = /text/;
let result = pattern.test(text);
```

#### .match()

También busca por coincidencias como .test() pero lo que hace es que a dicha coincidencia la mete dentro de un array.

```sh
let text = "The rain in SPAIN stays mainly in the plain";
text.match(/ain/g);
```

### Volvemos al ejercicio

Lo que nos quedó pendiente del ejercicio anterior es la validación del email con expresiones regulares.

```
const regExp = /^[\w._-]+@\w+\.\w+$/
```

Lo que hacen los [] es un equivalente al OR, dentro de esto están los carácteres que yo permito que se ingresen, en este caso NO son alfanuméricos tales como el ".", "_" y "-"

Ya tenemos nuestra expresión regular así que lo que nos queda hacer es un condicional para validar:

```sh
if (!regExp.test(value)) {
    msg = "";
}
```

## Métodos de arrays y objetos (funciones de orden superior)

#### .foreach()

Recibe entre 1 y 3 parámetros, en el primero recibe cada valor del array (elemento). Realiza una operación con cada valor del array.

```sh
const numeros = [100, 200, 300, 400]

numeros.forEach((numero, index) => {
    console.log(numero)    
})
```

Para entender un poco más de lo que va el método forEach vamos a hacerla nosotros mismos:

```sh
const numeros = [100, 200, 300, 400]

function forEach(array, callback) {
    const length = array.length
    for(let i = 0; i < length; i++) {
        const value = array[i];
        callback(value, i)
    } 
}
```

Lo que hicimos anteriormente de forma "tradicional" podría traducirse usando el método forEach de la siguiente forma:

```sh
forEach(numeros, (numero, index) => {
    console.log(numero)
})
```

#### .map()

Fabrica un nuevo array según la operación de la función. El map se maneja con el "return". En el ejemplo siguiente se fabrica un nuevo array con los valores multiplicados por 3 del array con respecto al que se creó anteriormente

```sh
const numeros2 = numeros.map(numero => {
    return numero*3
})
```

#### .filter()

Fabrica un nuevo array filtrando valores. Si retorno true se guarda el valor original, si retorno false NO se guarda el valor. Siempre trabaja sobre los valores originales, no realiza modificaciones.

```sh
const numerosFilter = numeros.filter(numero => {
    return numero !== 300
})
```

#### for ... in 

Lo que hace este bucle es iterar sobre cada propiedad del objeto, por ejemplo:

```sh
const object = { a: 1, b: 2, c: 3 };

for (const property in object) {
  console.log(property);
}

// salida:
// "a"
// "b"
// "c"
```

Y ya que estamos también lo podemos usar para acceder al valor de las propiedades de tal objeto:

```sh
const object = { a: 1, b: 2, c: 3 };

for (const property in object) {
  console.log(`${property}: ${object[property]}`);
}

// salida:
// "a: 1"
// "b: 2"
// "c: 3"
```

#### for ... of

Itera sobre cada elemento de un array:

```sh
let iterable = [10, 20, 30];

for (let value of iterable) {
  value += 1;
  console.log(value);
}
// 11
// 21
// 31
```





