# Gestión de Errores en Go

+++
    author = "David López Carrascal"
    username = "@davidlcarrascal"
    github = "davidlcarrascal"
    title = "Gestión de Errores en Go"
    tags = [go, golang, davidlcarrascal, friends of go, error handling, errors]
    categories = [Errores]
    crossPosting =
    externalCrossPosting = false
+++

La gestión de errores es una de las cosas con las que los programadores nos encontramos todos los días y hay que darle la importancia que merece. En este articulo hablaremos de las diferentes formas que tenemos de crear un error en Go, como capturarlo y personalizarlo. Todo esto con un toque de Juego de Tronos, para que no sea tan aburrido.

## Go, un lenguaje sin excepciones

 Actualmente, la mayoría de los lenguages de programación lo hacen con excepciones, pero este no es el caso de Go. El problema de las excepciones es el uso que se les da en otros lenguajes, ya que como su propio nombre indica, cuando una excepción se produce, el programa no debería de continuar sino pararse. [En este post](https://dave.cheney.net/2012/01/18/why-go-gets-exceptions-right), Dave Cheney lo explica muy bien, mostrando ejemplos de como C++ y Java lo hacen.

En Go esto no ocurre, porque las excepciones no existen. Lo que tenemos es una función bien conocida por todos, *panic*, que solo debemos usarla cuando queramos que el programa se pare y no deba continuar. Aquí abajo mostramos un ejemplo de como utilizar *panic*.

```Go
func main() {
    panic("No more GOT spoilers!")

    fmt.Println("this message will not be printed")
}
```

Por otro lado, tenemos la función *defer*, la cual sirve para enviar la instrucción que le pongamos al final del programa. Es muy usada para tareas de limpiezas como por ejemplo cerrar conexiones a la base de datos.

```Go
func main() {
    defer fmt.Println("Something is burning")
    fmt.Println("Dracarys")
}
```

Esto nos daría:  
*Dracarys  
Something is burning*.

Ahora que sabemos como se comportan las funciones *defer* y *panic*, vamos a ver como funciona *recover*. La función *defer* unida con la función *recover* se suele usar para capturar un *panic*. Aqui abajo vemos un ejemplo.

```Go
func main() {
    defer func() {
        r := recover()
        fmt.Println("recovered:", r)
        fmt.Println("John is alive")
    }()
    panic("John Snow died")
}
```

Esto nos daría:  
*recovered: John Snow died*.  
*John is alive*.

No deberíamos de usar la función *panic* muy a menudo en nuestros programas y mucho menos si estamos elaborando alguna librería que tenga que ser consumida por un tercero. Sin embargo, con lo aprendido hasta ahora, si nos encontramos con algún *panic* en una de las librerías que estemos usando, podriamos capturarlo.

## Control de errores

En Go las funciones pueden devolver más de un valor. Normalmente se utilizan para devolver el valor esperado y un error. En este caso hemos creado una función que traduce cualquier palabra que pasemos en español a alto valirio. Para ello, recibe una cadena de texto a traducir, devuelve una nueva cadena de texto ya traducida y un *error* en el caso de que lo hubiese.

```Go
func translateToHighValyrian(word string) (string, error)
```

El error puede ser *nil* o no, aquí está el truco. Habitualmente nos econtramos la siguiente estructura, comprobamos si el *error* no es *nil* y si es así lo controlamos.

```Go
res, err := translateToHighValyrian("aguja")
if err != nil {
    //Aquí controlamos el error
}
```

Esto se debe a que *error*, "el tipo" que devolvemos en nuestra función de arriba, se trata de una interfaz, que contiene un método *Error()*, el cual devuelve un *String*.

```Go
type error interface {
    Error() string
}
```

Como muy bien explicaron los compañeros [en el post sobre los *nil*](https://blog.friendsofgo.tech/posts/los_nil_seran_nil/) los tipos básicos y los *struct* no permiten ser nulos, solo podemos conseguir esto a través de los punteros o las interfaces como es el caso de *error*.

Una vez entendido como funciona, podemos crearnos un error de forma rápida usando el constructor de *errors* o a través del paquete *fmt*, por si queremos darle un formateo especial como mostramos en el ejemplo.

```Go
const errNotMoreGOTSpoilers = errors.New("No more GOT spoilers!")
const errTargaryenFireImmunity = fmt.Errorf("%s Targaryen is immune to fire!", name)
```

## Errores personalizados

También en Go podemos controlar los errores de una forma más personalizada. Para ello podemos crearnos nuestro propio *struct* que implemente la interface *error*, y así devolver nuestro error personalizado en el momento que queramos.

```Go
type ErrGOT struct {
    message string
    code int
    ...
}

func NewErrGOT(message string, code int) *ErrGOT {
    return &ErrGOT{
        message: message,
        code: code,
    }
}

func (e *ErrGOT) Error() string {
    return fmt.Sprintf("%s Error with code %d", e.message, e.code)
}

func translateToHighValyrian(word string) (string, error) {
    if word == "Cersei will..." {
        return "", NewErrGOT("No more GOT spoilers!", 1)
    } else if word == "Daenerys will ..." {
        return "", NewErrGOT("Daenerys Targaryen is immune to fire!", 2)
    }
    ...
}

func main() {
    t, err := translateToHighValyrian(word)
    if err != nil {
        fmt.Println(err)
    }

    fmt.Printf("translate: %s", t)
}
```

Esto nos daría el error *Daenerys Targaryen is immune to fire! Error with code 2*.

Esta forma de crear errores personalizados se suele utilizar cuando necesitas errores más complejos con códigos de errores, marcas de tiempo, etc.

## El futuro de los errores con Go 2

Si eres como yo, y te gusta conocer lo que va a venir en el futuro, ahora vamos a hablar de las funcionalidades que se proponen en la nueva versión de Go, de forma concreta en las novedades que trae en el control de errores.

Uno de los cambios que más están llamando la atención a los Gophers son las nuevas funciones que nos presentan para poder interactuar con los errores. La comunidad de Go piensa que lo mejor es usar errores personalizados, es por ello que estas nuevas funcionalidades van enfocadas en dar más herramientas para usarlos.

Una de las cosas que nos encontramos es la nueva *interface Wrapper*, esta deben implementarlas nuestros errores custom para poder usar las nuevas funciones *IS* y *AS* que a continuación os explicaremos. La propuesta de la nueva versión nos dice que todo error que envuelva a otro error debe implementar está *interface*.

```Go
type Wrapper interface {
        // Unwrap devuelve el siguiente error, en la cadena de errores.
        // Si el error no esiste, Unwrap devolverá nil.
        Unwrap() error
}
```

Vamos a hablar de la función *IS* que nos permite seguir la cadena de errores llamando a la función Unwrap y busca si alguno coincide con el valor que se le pasa. Será usada para controlar los errores sentinelas (sentinel errors), que es un tipo de error con valor único. Cualquier error puede implementar el método *Is* para sobrescribir el comportamiento por defecto.

```Go
// El método Is informa cuando cualquier error de la cadena de errores es igual al target.
//
// Un error coincide con el el target, si es igual al target o
// si implementa el método Is(error) bool de tal forma que Is(target) devuelve true.
func Is(err, target error) bool
```

La función *As*, que viene de *assert*, busca en la cadena de errores, un error cuyo tipo coincida con el valor que se le pasa. Cualquier error puede implementar el método *As* para sobrescribir el comportamiento por defecto.

```Go
// El método As encuentra el primer error en la cadena de errores que coincide con el tipo
// al que apunte el target, y si es así, este establece el target a su valor y devuelve true.
//
// Un error coincide con el el target, si este es asignable al target o
// si implementa el método As(interface{}) bool de tal forma que As(target) devuelve true.
func As(err error, target interface{}) bool
```

Estas creo que son las principales cosas a destacar en la propuesta de Go para los errores en su nueva versión, hay algunas más para formatear mejor los errores al imprimirlos, etc. Todas estas nuevas funcionalidades se pueden probar gracias [al paquete xerrors](https://godoc.org/golang.org/x/xerrors/) que la gente de Go ha sacado para que vayamos probando hasta que se hagan realidad estas funciones. Así que si te has quedado con ganas de más, puedes leer la propuesta y testearlo más a fondo.

Como siempre, estaremos encantados de recibir vuestro feedback en los comentarios del blog o vía Twitter.
