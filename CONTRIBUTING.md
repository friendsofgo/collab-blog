# ¿Cómo contribuir?

Hemos creído conveniente crear estas pautas para poder contribuir en el blog.

## Normas básicas

Hay unas normas básicas que seguir para escribir un artículo en **Friends of Go**:

* El artículo debe estar escrito en español, intentando ser lo más neutro posible ya que abarcamos España y América Latina.
* Si utilizamos términos en inglés, al menos la primera vez que se usen deben ser traducidos al español, entre paréntesis, ej: Dependency Injection (Inyección de Dependencias).
* Tratar sobre temas concisos y con buena representación práctica. Intentar evitar los dogmas.
* No faltar el respeto, no todos tienen el mismo nivel y no por saber más se es mejor.

Además de esto se debe de cumplir con el [Código de Conducta](https://github.com/friendsofgo/collab-blog/blob/master/CODE_OF_CONDUCT.md)

## ¿Cómo se escribe un artículo?

Todos los artículos deben ser escritos utilizando [Markdown](https://www.markdownguide.org/), para los **snippets** de código utilizad los bloques de código que facilita `Markdown`:

<pre>
    ```{tu lenguaje aquí}
        package main

        func main() {

        }
    ```
</pre>

Todo artículo debe venir con una cabecera que cumpla la siguiente plantilla:

<pre>
+++
    author = "{Nombre que quieras que aparezca en el artículo}"
    username = "{usuario de twitter}"
    title = "{Título del artículo}"    
    tags = [
        "{array de tags que creáis que es útil para vuestro artículo}"
    ]    
    categories = [
        "{categoría o categorías donde puede encajar vuestro artículo}"
    ]
    crossPosting = "{url si aplica, se explica más abajo}"
    externalCrossPosting = {true|false}
+++
</pre>

Si es la primera vez que publicáis en el blog, necesitaremos una foto vuestra, nos la podéis enviar a contact@friendsofgo.tech con el asunto `[Blog][Foto] {usuario de twitter}` o sino utilizaremos la que tengáis en `Twitter`, además podéis incluir en el email una pequeña descripción, para que aparezca bajo vuestro nombre.

Todas las **PR deberan de tener el artículo bajo el directorio draft**, el cual nosotros moveremos a published una vez publicado.

## ¿Cuándo se publicará mi artículo?

De momento publicamos artículos cada lunes, ya que no tenemos el suficiente contenido como para hacerlo con más frecuencia.

Tú artículo será publicado si cumple con lo especificado anteriormente, y no está repetido. Igualmente se dará feedback durante la PR si hay algo a modificar o por algún motivo no vemos publicable el artículo para el blog.

Por otro lado no esperes que tu artículo sea publicado antes porque hayas llegado antes que otra persona, aquí premia el contenido y el valor aportado a nuestros lectores, obviamente esto es un punto completamente subjetivo, pero si creemos que un artículo puede ser interesante sacarlo antes que otro, es lo que haremos, igualmente todo artículo que sea **mergeado en master en draft** será publicado tarde o temprano.

Todo artículo publicado vendrá acompañado del tweet correspondiente, mencionando al autor.

## ¿Puedo hacer CrossPosting?

¿Tienes ya un blog de Golang y quieres además publicar tú artículo en **Friends of Go**? No hay problema, puedes aplicar para que tu artículo sea publicado en nuestro blog, recuerda que si tu otro medio es en inglés deberás de traducir el artículo al español.

Para indicarnos que tu artículo es CrossPosting, en la plantilla mencionada anteriormente añade lo siguiente:

<pre>
...
    crossPosting = "{url de tu artículo}"
...
</pre>

Así podremos añadir una cabecera de donde ha sido publicado previamente el artículo, no olvides que debes tener la autoría del artículo en cuestión para esto.

## CrossPosting de Friends of Go

Actualmente tenemos un acuerdo de publicación mensual con los chicos de [Betabeers](https://betabeers.com/blog/), donde hacemos **crossposting de los artículos de nuestro blog**. 

Os brindamos la oportunidad de que participéis en dicho acuerdo también de manera completamente voluntaria, es decir, si cuando llegue la fecha de publicación en **Betabeers** podemos utilizar uno de vuestros artículos para compartirlo en la plataforma.
La autoría se sigue respetando, podéis ver un ejemplo aquí:

[https://betabeers.com/blog/como-estructurar-tus-proyectos-go-400/](https://betabeers.com/blog/como-estructurar-tus-proyectos-go-400/)

Para indicarnos que queréis participar en Betabeers así como en otros posibles crossPosting nuestros, deberéis de rellenar con `true` o `false` la variable `externalCrossPosting` de la plantilla anterior.

## Dudas

Para cualquier duda relacionada con la colabroación del blog puedes abrir un [Issue](https://github.com/friendsofgo/collab-blog/issues/new) o escribirnos a
contact@friendsofgo.tech con el asunto: `[Blog][Colaboración] {Asunto de tu duda aquí}` y estaremos encantados de ayudarte.
