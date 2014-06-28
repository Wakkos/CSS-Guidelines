# Notas generales de CSS, consejos y directrices

---

## Traducciones

* [Russian](https://github.com/matmuchrapna/CSS-Guidelines/blob/master/README%20Russian.md)
* [Chinese](https://github.com/chadluo/CSS-Guidelines/blob/master/README.md)
* [French](https://github.com/flexbox/CSS-Guidelines/blob/master/README.md)
* [Japanese](https://github.com/kiwanami/CSS-Guidelines/blob/master/README.ja.md)
* [Inglés](https://github.com/csswizardry/CSS-Guidelines/blob/master/README.md)

---

Trabajando en proyectos extensos con muchos desarrolladores, es importante que
trabajemos de una manera unificada para, entre otras cosas:
* Mantener las hojas de estilos actualizadas
* Mantener el código limpio y legible
* Mantener las hojas de estilos escalables

Existen una variedad de técnicas que podemos implementar para cumplir estos objetivos.

La primera parte de este documento tratará la sintaxis, el formato y la anatomía
del CSS, la segunda parte tratará con el enfoque, concepción y postura a la hora de escribir
y organizar CSS.

##Contenido

* [Anatomía de un documento CSS](#anatom%C3%ADa-de-un-documento-css)
  * [General](#general)
  * [Un archivo vs. varios archivos](#un-archivo-vs-varios-archivos)
  * [Tabla de contenidos](#tabla-de-contenidos)
  * [Título de secciones](#título-de-secciones)
* [Orden del código](#orden-del-código)
* [Anatomía del conjunto de reglas](#anatom%C3%ADa-del-conjunto-de-reglas)
* [Convenciones para los nombres](#convenciones-para-los-nombres)
  * [JS Hooks](#js-hooks)
  * [Internacionalización](#internacionalización)
* [Comentarios](#comentarios)
  * [Comentarios Avanzados](#comentarios-avanzados)
    * [Selectores quasi-calificados](#selectores-quasi-calificados)
    * [Código de etiquetas](#código-de-etiquetas)
    * [Indicadores de Objetos/Extensiones](#indicadores-de-objetos/extensiones)
* [Escribiendo CSS](#escribiendo-css)
* [Construyendo nuevos componentes](#construyendo-nuevos-componentes)
* [OOCSS](#oocss)
* [Layout](#layout)
* [Tamaño UI](#tamano-ui)
  * [Tamaño de fuente](#tamano-de-fuente)
* [Taquigrafía](#taquigrafía)
* [IDs](#IDs)
* [Selectores](#selectores)
  * [Selectores sobre cualificados](#selectores-sobre-cualificados)
  * [Rendimiento de los selectores](#rendimiento-de-los-selectores)
* [Indentación de selectores CSS](#indentación-de-los-selectores)
* ['!important'](#important)
* [Números mágicos y absolutos](#numeros-mágicos-y-absolutos)
* [Hojas de estilos condicionales](#hojas-de-estilos-condicionales)
* [Depuración](#depuración)
* [Preprocesadores](#preprocesadores)

---

## Anatomía de un documento CSS

Sin importar el documento, debemos siempre intentar mantener un formato común.
Esto quiere decir comentarios consistentes, sintaxis consistente y nombres consistentes.

### General

Limita tu hoja de estilos a un máximo de 80 caracteres de largo a ser posible.
Las sintaxis de degradados y URL en comentarios pueden ser una excepción.
No pasa nada, no podemos hacer nada al respecto.

Prefiero cuatro (4) espacios indentados en tabs y escribir en múltiples líneas el CSS.

### Un archivo vs. varios archivos

Algunos prefieren trabajar con un único y gran archivo. Esto está bien, y si
nos ceñimos a estas directrices no encontraremos problemas. Desde que me he pasado
a preprocesadores he empezado a dividir mi hoja de estilos en muchos y pequeños includes.
Esto también es correcto... Cualquiera de los dos métodos que elijas es aplicable a las
reglas y directrices de este documento. La única diferencia notable se aplica a la tabla de contenidos
y los títulos de nuestras secciones. Si seguís leyendo lo entenderéis...



### Tabla de contenidos

Al inicio de mi hoja de estilos, mantengo una tabla de contenidos que detalla las
secciones mantenidas en el documento, por ejemplo:

    /*------------------------------------*\
        $CONTENIDO
    \*------------------------------------*/
    /**
     * CONTENIDO...........Lo estás leyendo!!
     * RESET...............Establece los valores iniciales
     * FONT-FACE...........Importamos fuentes de iconos y font-face
     */

Esto dice al siguiente desarrollados(es) que tome el proyecto exactamente que
puede esperar encontrar en este archivo. Cada item en la tabla de contenido marca
el título de una sección.

Si trabajas en una hoja de estilos muy grande, la sección correspondiente estará en ese archivo.
Sin embargo al trabajar con múltiples includes, como en el caso de preprocesadores, cada
elemento en la tabla de contenidos marcará el include que imprime esa sección.

### Título de secciones

La tabla de contenidos sería inútil a menos que tuviera su correspondientes títulos de secciones.
Denotad una sección de la siguiente manera:

    /*------------------------------------*\
        $RESET
    \*------------------------------------*/

Asignar el prefijo '$' al nombre de la sección nos permite efectuar una búsqueda ([Cmd|Ctrl]+F) por '$NOMBRE-SECCIÓN' y **limitar esa búsqueda a los títulos de sección solo**.

Si trabajas con hojas de estilo muy grandes, deja cinco (5) saltos de línea entre cada sección, así:

    /*------------------------------------*\
        $RESET
    \*------------------------------------*/
    [Estilos del
    reset
    del CSS]





    /*------------------------------------*\
        $FONT-FACE
    \*------------------------------------*/

Este amplio bloque en blanco permite ubicar las secciones cuando se hace un rápido scroll por documentos muy extensos.

Si estás trabajando en múltiples hojas de estilo, incluidas, empieza cada una de esas hojas con el título de la sección y no hay necesidad de saltos de línea o líneas en blanco.


## Orden del Código

Intenta escribir las hojas de estilo en un orden específico. Esto garantiza que aproveches al máximo la herencia y la primera <i>C</i> de CSS: La cascada.

Un correcto orden en la hoja de estilo sería algo como esto:

1. **Reset** – zona zero.
2. **Elementos** – `h1` sin clase, `ul` sin clase etc.
3. **Objetos y abstracciones** — Patrones de diseño genéricos y subyacentes.
4. **Componentes** – Componentes construídos a partir de objetos y sus extensiones.
5. **Texturas** – Estados de error, info, etc...

Esto implica que mientras vamos bajando cada una de las secciones del documento se construye y basa su herencia en la anterior. Deberíamos deshacer menos estilos, menos problemas en especificar estilos y una mejor arquitectura global de estilos.

Como lectura complementaria no podría recomendar lo suficiente el libro de Jonathan Snook [SMACSS](http://smacss.com).


## Anatomía del conjunto de reglas

    [selector]{
        [propiedad]:[valor];
        [<- Declaración ->]
    }


Tengo un gran número de estándares cuando estructuro un conjunto de reglas

* Usar un guión que delimite los nombres de las clases (excepto para notaciones BEM [ver abajo](#convenciones-para-los-nombres))
* 4 espacios de indentación.
* Multilínea
* Declaración en orden de relevancia (NO orden alfabético)
* Indentar los valores con prefijos de navegador, así sus valores quedan alineados.
* Indentar nuestras reglas para que asemeje la estructura del DOM
* Siempre incluir el punto y coma final en una declaración.

Un ejemplo corto:

    .widget{
        padding:10px;
        border:1px solid #BADA55;
        background-color:#C0FFEE;
        -webkit-border-radius:4px;
           -moz-border-radius:4px;
                border-radius:4px;
    }
        .widget-heading{
            font-size:1.5rem;
            line-height:1;
            font-weight:bold;
            color:#BADA55;
            margin-right:-10px;
            margin-left: -10px;
            padding:0.25em;
        }
Aquí podemos ver que '.widget-heading' debe ser un hijo de '.widget' ya que tenemos indentado '.widget-heading' un nivel más que '.widget'. Esta información es muy útil para los desarrolladores que ahora pueden hacerse una idea del conjunto con tan solo echar un corto vistazo a las indentaciones de nuestras reglas de estilos.

También podemos ver que las declaraciones de '.widget-heading' están declaradas por orden de relevancia; '.widget-heading' debe ser un elemento de texto y podemos empezar con nuestras reglas de texto, seguidas por todas las demás.

Una excepción a nuestra regla multi-línea sería en el siguiente caso:

    .t10    { width:10% }
    .t20    { width:20% }
    .t25    { width:25% }       /* 1/4 */
    .t30    { width:30% }
    .t33    { width:33.333% }   /* 1/3 */
    .t40    { width:40% }
    .t50    { width:50% }       /* 1/2 */
    .t60    { width:60% }
    .t66    { width:66.666% }   /* 2/3 */
    .t70    { width:70% }
    .t75    { width:75% }       /* 3/4*/
    .t80    { width:80% }
    .t90    { width:90% }

En este ejemplo (del [sistema de grids de inuit.css](https://github.com/csswizardry/inuit.css/blob/master/inuit.css/partials/base/_tables.scss#L88)) tiene más sentido dejar nuestro CSS en una sola línea.

## Convenciones para los nombres
Mayormente utilizo clases delimitadas por guiones (Ej: '.foo-bar' y no '.foo_bar' ni '.foobar'), sin embargo, en ciertos casos uso notaciones BEM (Block Element, Modifier - elemento en bloque, modificador).

<abbr title="Block, Element, Modifier">BEM</abbr> es una metodología para nombrar y clasificar selectores CSS de manera que los hacemos más estrictos, transparentes e informativos.

La convención de nombre sigue este patrón:

    .bloque{}
    .bloque__elemento{}
    .bloque--modificador{}

* '.bloque' representa el primer nivel de una abstracción o componente.
* '.bloque__element' representa un descendente de '.bloque' que se ayuda de '.bloque' como un conjunto.
* '.bloque--modificador' representa un estado diferente de '.bloque'.

Una **analogía** del funcionamiento de las clases BEM sería:

    .persona{}
    .persona--mujer{}
        .persona__mano{}
        .persona__mano--izquierda{}
        .persona__mano--derecha{}

Aquí vemos como el objeto básico que estamos describiendo es una persona, y que un tipo diferente de persona podría ser una mujer. También podemos ver que las personas tienen manos; son sub-partes de las personas, y que hay variaciones, como izquierda y derecha.

Ahora podemos nombrar nuestros selectores basado en sus objetos base y podemos también comunicar que función tiene el selector; es un sub-componente ('__') o una variación ('--')?

Entonces, '.page-wrapper' es un selector aislado; no forma parte de una abstracción o componente y como tal está nombrado correctamente. '.widget-heading' _está_ relacionado a un componente y es un hijo de '.widget', así que deberíamos renombrar esta clase a '.widget__heading'


BEM se ve un poco feo, y es mucho más verbal, pero nos garantiza   poder espigar las funciones y relaciones de los elementos con sus componentes. BEM a su vez comprime (gzip) muy bien y por ello el repetirlos nos viene bien en documentos comprimidos.

Independientemente que uses BEM o no, siempre asegúrate que las clases son nombradas muy claramente; mantenlas tan cortas como sea posible pero tan largas como sea necesario. Asegúrate que cualquier objeto o abstracción tienen nombres vagos (Ej: '.ui-list', '.media') para permitir su reutilización siempre que sea posible. Las extensiones de objetos deben ser nombradas mucho más explícitamente (Ej: '.user-avatar-link). No te preocupes por la longitud o cantidad de clases; gzip comprimirá todo el código bien escrito _increíblemente_ bien.


### Clases en HTML

Haz las cosas fáciles de leer, separa tus clases en HTML con dos (2) espacios en blanco:

    <div class="foo--bar  bar__baz">

Este espacio en blanco incrementado permitirá ubicar con mayor facilidad y mejorar la lectura de múltiples clases.

### JS hooks

**Nunca uses una clase de CSS _de estilos_ como un hook de JS.**
Adjuntar comportamientos de JS a clases de estilo quiere decir que nunca podremos tener una sin la otra

Si necesitas enlazar a alguna etiqueta usa una clase específica de JavaScript,. Simplemente es una clase que empiece por '.JS-' , Ej: '.js-toggle', '.js-drag-and-drop' y de esta manera podemos agregar ambas clases, JS y CSS, a nuestra etiqueta pero nunca se sobrepondrán una a la otra.

    <th class="is-sortable  js-is-sortable">
    </th>

El marcado anterior contiene dos clases; una a la cual podemos adjuntar estilos y otra que permite agregar la funcionalidad.

### Internacionalización

A pesar de ser británico - y escribir toda mi vida _colour_ en vez de _color_ - creo que, por el bien de la consistencia, es mejor usar siempre Inglés Americano en CSS. CSS, así como la mayoría (si no todos) de otros lenguajes, está escrito en Inglés Americano, con lo cual mezclar sintaxis como 'color:red' con clases como '.colour-picker {}' carece de consistencia.
Anteriormente he sugerido y defendido escribir clases bilingües, como por ejemplo:

    .color-picker,
    .colour-picker{
    }

Sin embargo, tras haber trabajado recientemente en un proyecto bastante largo en Sass donde habían docenas de variables de colores (Ej: '$brand-color', '$highlight-color' etc.) mantener dos versiones de cada variable se volvió agotador. Esto también quiere decir un doble trabajo con funciones como buscar y reemplazar.

En beneficio de la consistencia, siempre nombra clases y variables en el lenguaje en el que estás trabajando.

***Nota del traductor***
* En mi opinión siempre usaría inglés: nada impide a desarrolladores de otros idiomas continuar nuestro trabajo y necesitamos una consistencia.

## Comentarios

Uso comentarios estilo docBlock-esque a los que limito a 80 caracteres de longitud:

    /**
     * Esto es un comentario estilo docBlock
     *
     * Esta es una descripción más larga del comentario, describiendo el código con más
     * detalle. Limitamos estas líneas a 80 caracteres de longitud
     *
     * Podemos tener etiquetado en el comentario, y de hecho es recomendable:
     *
       <div class=foo>
           <p>Lorem</p>
       </div>
     *
     * No ponemos un asterisco como prefijo en las líneas de código, ya que inhibiría
     * copiar y pegar.
     *
     */

Deberías documentar y comentar tu código tanto como sea posible, lo que puede parecer transparente y que se explica por sí mismo podría no serlo para otro desarrollador.
Escribe un trozo de código y luego escribe sobre él.

### Comentarios avanzados

Hay algunas técnicas avanzadas que puedes utilizar con los comentarios, por ejemplo:

* Selectores Cuasi-calificados
* Código de etiquetas
* Indicadores de Objetos/Extensiones

#### Selectores Cuasi-calificados

Nunca deberías clasificar tu selector, esto es, nunca debemos escribir 'ul.na{}' si podemos tener '.nav{}'. Calificar selectores disminuye el rendimiento del selector, inhibe la posibilidad de rehusar una clase en un elemento diferente y disminuye su adecuación. Estas son cosas que debemos evitar a toda costa.

Sin embargo, a veces es útil comunicar al siguiente desarrollador(es) dónde pretendes usar esa clase. Tomemos '.product-page' por ejemplo; esta clase suena como si debiera ser usada en un contenedor principal, quizás el elemento 'body' o 'html' pero es difícil de decir de verdad dónde se debe usar.

Cuasi-calificarndo este selector (Ej: Comentando el selector principal) podemos comunicar dónde queremos que se use esta clase, algo como:

   /*html*/.product-page{}

Ahora podemos ver dónde usar esta clase exactamente pero sin las especificaciones que modifiquen la clase para evitar que sea rehusada.

Otro ejemplo puede ser:

    /*ol*/.breadcrumb{}
    /*p*/.intro{}
    /*ul*/.image-thumbs{}

Se puede ver dónde queremos usar esta clase pero sin que afecte al selector.


#### Código de etiquetas

Si escribes un nuevo componente entonces deja algunas etiquetas relativas a su función en un comentario, por ejemplo:

    /**
     * ^navigation ^lists
     */
    .nav{}

    /**
     * ^grids ^lists ^tables
     */
    .matrix{}

Estas etiquetas permiten a otros desarrolladores encontrar snippets de código buscando por su función; si un desarrollador necesita trabajar con listas, puede ejecutar una búsqueda por '^lists' y encontrar los objetos '.nav' y '.matrix. (y posiblemente más).

#### Indicadores de Objetos/Extensiones

Cuando trabajamos orientados a objetos encontrarás a menudo dos trozos de código CSS (siendo uno el esqueleto - el objeto - y otro siendo su skin o piel - la extensión -) que están relacionados, pero que habitan en sitios muy diferentes. Para establecer un vínculo entre el objeto y el uso de su extensión están los _Indicadores de Objetos/Extensiones_. Éstos son simples comentarios que funcionan así:

En tu hoja de estilos base:

    /**
     * Extiende `.foo` en theme.css
     */
     .foo{}

En tu hoja de tema, skin o layout:

    /**
     * Extendido de `.foo` en base.css
     */
     .bar{}

Hemos establecido una relación concreta entre dos trozos de código muy separados.

---

## Escribiendo CSS

Las secciones anteriores tratan acerca de la estructura y forma de nuestro CSS; son reglas y normas cuantificables. La siguiente sección es un poco más teórica y trata acerca de nuestra actitud y punto de vista.

## Construyendo nuevos componentes

Cuando construyamos nuevos componentes escribe el marcado (HTML) **antes** del CSS. Con esto podrás visualizar que propiedades CSS son heredadas y así evitar replicar estilos redundantes.

Escribiendo el marcado primero te enfocas en datos, contenido y semántica y _luego_ aplicas sólo las clases relevantes y el CSS.

## OOCSS

Yo trabajo bajo OOCSS; Divido los componentes en estructura (objetos) y skin (extension). Como **analogía** (No ejemplo) observad lo siguiente:

    .habitación{}

    .habitación--cocina{}
    .habitación--cuarto{}
    .habitación--baño{}

Tenemos diferentes tipos de habitaciones en una casa, pero todas ellas reciben un trato similar; todas tienen suelo, techo, paredes y puertas. Podemos compartir esta información con una clase abstracta '.habitación{}. Sin embargo, tenemos diferentes tipos de habitación que las difieren de otras; la cocina puede tener un suelo de baldosas y el dormitorio puede tener alfombra, un baño puede no tener ventana pero es muy probable que un cuarto si la tenga, y cada habitación puede tener las paredes de diferente color. OOCSS (Object Oriented CSS) nos enseña a abstraer los estilos compartidos en un objeto base y luego _extender_ esta información con clases más específicas que añadan estilo(s) único(s).

Entonces, en vez de construir docenas de componentes únicos, prueba a identificar patrones de diseño repetidos y abstráelos dentro de clases reusables; construye esqueletos como 'objetos' base y luego enclavija clases a éstos objetos para extender sus estilos en circunstancias más específicas o únicas.

Si tienes que construir nuevos componentes divídelos en estructura y skin; construye la estructura del componente usando clases muy genéricas de manera que se puedan rehusar y añadirle las clases más específicas para estilizar y añadir diseño.

## Layout

Todos los componentes que construyas no deben tener anchos (width); deben ser fluidos y su anchura regida por su contenedor o por su grid.

**Nunca** se deben aplicar alturas (height) a los elementos. La altura debe ser solo aplicada a los elementos con unas dimensiones dadas _antes_ de introducirse en el layout (Ej: sprites o imágenes). Nunca establezcas altura a 'p', 'ul', 'div', o a cualquier otro elemento. normalmente puedes obtener el efecto deseado con 'line-height' que es de lejos mucho más flexible.

Los sistemas de grids deben ser pensados como estanterías. Tienen contenido pero no son contenido _en si_. Tu montas las estanterías y luego las llenas con tus cosas. Configurar nuestras grids separadas de nuestros componentes nos permite mover componentes mucho más fácil que si tienen dimensiones configuradas; esto hace nuestro front-end mucho más adaptable y nos permite trabajarlo más rápido.

Nunca deberías aplicar estilos a un elemento del sistema de grid, son para efectos de disposición únicamente. Aplica estilos al contenido _dentro_ del elemento del grid. Nunca, bajo _ninguna_ circunstancia, debemos aplicar propiedades al modelo de caja de un elemento del grid.


## Medidas

Uso una combinación de métodos para los tamaños. Porcentajes, pixels, ems, rems y nada en absoluto.

El sistema de grids debe, idealmente, estar dado en porcentajes. Ya que uso el grid para controlar el ancho de columnas y páginas, puedo dejar componentes sin dimensiones definidas (tal como se expuso anteriormente).

Los tamaños de fuentes deben darse en rems con fallback en pixels. Así se aprovechan los beneficios de accesibilidad de los rems con la robustez de los pixels. Aquí os dejo un mixin de Sass para trabajar con rems y dejar un fallback en pixels (asume que has declarado una variable para el tamaño de fuente en algún lado):

    @mixin font-size($font-size){
        font-size:$font-size +px;
        font-size:$font-size / $base-font-size +rem;
    }

Y en Less:

	.font-size(@font-size: 16) {
        @rem: (@font-size / 10);
        font-size: @font-size * 1px;
        font-size: ~"@{rem}rem";
	}

Sólo uso pixels para elementos cuyas dimensiones están previamente definidas antes de entrar en el sitio. Esto incluye elementos como imágenes y sprites cuyas dimensiones están heredadas y configuradas en pixels.

### Tamaños de fuentes

Yo defino una serie de clases parecidas a un sistema de grids para los tamaños de fuentes. Estas clases pueden ser usadas para dar estilos a una tipografía en un doble encabezado. Para una completa explicación sobre como funciona, leer mi artículo [Pragmatic, practical font-sizing in CSS](http://csswizardry.com/2012/02/pragmatic-practical-font-sizing-in-css)

## Shorthand

**Los shorthand de CSS deben usarse con precaución.**

Es tentador usar declaraciones como 'background:red;' pero haciéndolo lo que de verdad dices es 'no quiero que la imagen haga scroll, que se alinee arriba a la izquierda, que se repita en el eje X y en el eje Y, y con un fondo rojo'. Nueve veces de 10 esto no causará ningún problema pero esa sola vez que lo causa es suficiente para evitar el uso de shorthands. En vez de ello, usa 'background-color:red;'.

Similarmente, declaraciones como 'margin:0;' están bien y son cortas, pero **sé explícito**. Si lo que quieres de verdad afectar es solo el margen en el inferior de un elemento entonces es más apropiado usar 'margin-bottom:0;'.

Sé explícito en que propiedad estableces y ten cuidado de no desconfigurar otras por error al usar un shorthand. Ej: Si quieres solo eliminar el margen inferior de un elemento no tiene sentido establecer todos los márgenes a 0 con 'margin:0;'.

Los shorthands son buenos, pero pueden hacer caer en malas prácticas fácilmente.

## ID

Una pequeña anotación acerca de IDs en CSS antes de entrar a ver selectores en general.

**NUNCA uséis IDs en CSS.**

Pueden ser usados en tu marcado para referencias de JS, pero usad  sólo clases para estilos. No queréis ver una sola ID en ninguna hoja de estilos!

Las clases vienen con el beneficio de ser reusables (aún si no queremos, podemos) y tienen un buen y bajo especificado. El especificado es una de las formas más rápidas de afrontar dificultades en proyectos y mantenerlo bajo en todo momento, es imperativo. Una ID es **255** veces más específico que una clase, por ello _nunca_ los uses en CSS.

## Selectores

Mantén los selectores cortos, eficientes y portables.

Selectores basados en su posición son malos por una variedad de razones. Por ejemplo, tomemos '.sidebar h3 span{}'. Este selector está basado en la posición y por ello no podemos mover ese 'span' fuera de un 'h3' fuera de un '.sidebar' y mantener el estilo.

Los selectores que son muy largos también introducen problemas de optimización. Cuantas más verificaciones hay en un selector (Ej: '.sidebar h3 span' tiene tres verificaciones, '.content ul p a' tiene cuatro), más trabajo tiene el navegador.

Siempre que sea posible nos aseguraremos que los estilos no dependan de la posición en el CSS, así como nos aseguraremos que los selectores son buenos y cortos.

Los selectores en general deben mantenerse cortos (Ej: una clase de profundidad) pero los nombres de las clases en sí mismas deben ser tan largas como lo necesiten. Una clase de '.user-avatar' siempre es mejor que '.usr-avt'

**Recordad:** Las clases no son semánticas o dejan de serlo; ellas son sensibles o insensibles! Dejad de peocuparos por la 'semántica' de los nombres de las clases y elige algo sensato y previsor.

### Selectores sobre-calificados

Como ya comentamos anteriormente, selectores calificados son malas noticias.

Un selector sobre calificado es como 'div.promo'. Probablemente podrías obtener el mismo resultado usando sólo '.promo'. Claro, algunas veces querrás calificar una clase con un elemento (Ej: si tienes una clase genérica '.error' que debe verse diferente cuando se aplica a un elemento diferente (Ej: '.error{color:red;}' 'div.error{padding:14px;}')), pero generalmente se evita siempre que sea posible.

Otro ejemplo de un selector sobre calificado sería 'ul.nav li a {}'. Al igual que arriba, podemos eliminar instantáneamente el 'ul' y ya que sabemos que '.nav' es una lista, podemos saber que cualquier 'a' _debe_ ir en un 'li', así que podemos cambiar 'ul.nav li a{}' por '.nav a{}'.


### Rendimiento de los selectores

Aunque es verdad que los navegadores son cada vez más rápidos renderizando CSS, la eficiencia es algo en lo que siempre deberíamos tener un ojo puesto. Selectores cortos, no anidados, sin usar el selector universal ('*{}') y evitando selectores complejos de CSS3 ayudarán a evitar problemas de rendimiento.


## Propósito de los selectores CSS

En vez de utilizar selectores para perforar el DOM hasta un elemento, muchas veces es mejor poner una clase en el elemento que queremos estilizar o modificar. Tomemos un ejemplo en concreto con un selector como '.header ul{}'...

Imaginemos que 'ul' es la navegación principal de nuestro sitio. Habita en el header como es de esperar y actualmente es el único 'ul' ahí.
'.header ul' funcionará, pero no es ideal o aconsejable. No es muy seguro en futuros cambios y ciertamente no es lo suficiente explícito. Apenas agreguemos otro 'ul' a ese encabezado adoptará el estilo de nuestra navegación principal y lo más probable es que no queramos eso. Esto quiere decir que tendríamos que reestructurar código _o_ deshacer mucho estilo en los subsecuentes 'ul' dentro de ese '.header'.

El propósito de tu selector debe coincidir con el motivo de estilizar algo; pregúntate a ti mismo **'¿Estoy seleccionando esto porque es un 'ul' dentro de '.header' o porque es la navegación principal de mi sitio?'**. La respuesta a esto determinará tu selector.


Asegúrate que tu selector clave nunca es una clase tipo/elemento u objeto/abstracción. No queremos ver clases como '.sidebar ul{}' o '.footer .media{}' en nuestros estilos.

Sé explícito; selecciona el elemento al que quieres afectar, no su padre o contenedor. Nunca asumas que el marcado no va a cambiar. **Escribe selectores que hagan objetivo a lo que quieres, no a lo ya está ahí.**

Para una lectura más profunda en el propósito de los selectores:
[Shoot to kill; CSS selector intent](http://csswizardry.com/2012/07/shoot-to-kill-css-selector-intent/)

## '!important'

Está bien utilizar '!important' sólo en clases _helper_. Añadir '!important' preventivamente es correcto, Ej: '.error{ color:red!important;}', como ya sabrás **siempre** querrás que esta regla tenga prioridad.

Usar '!important' sensiblemente, Ej: para salir de malas situaciones de especificación, no es recomendado. Rehaz tu CSS e intenta combatir estos problemas reconstruyendo tus selectores. Mantener tus selectores cortos y evitando IDs ayudará enormemente.

## Números mágicos y absolutos

Un número mágico es un número usado porque 'funciona'. Son malos porque raramente funcionan de verdad y no son muy seguros o flexibles/indulgentes. Tienden a solucionar síntomas y no problemas.

Por ejemplo, usar '.dropdown-nav li:hover ul{ top:37px; }' para mover un desplegable abajo de la navegación en _hover_ está mal, ya que 37px es un número mágico. 37px sólo funciona aquí porque en este escenario en particular el '.dropdown-nav' tiene 37px de alto.

En vez de eso, deberías usar '.dropdown-nav li:hover ul{ top:100%; }' que quiere decir que no importa el alto de '.dropdown.nav', el desplegable siempre se colocará a 100% del borde superior.

Piensa cada vez que escribes un número para que _encaje_ en el layout; si puedes evitarlo usando 'aliases' (Ej: 'top:100%' para indicar 'todo a partir del borde superior') o &mdash;aún mejor&mdash; sin medida alguna.

Escribir medidas para que _encajen_ en el layout es un compromiso que no necesariamente querrás mantener.

## Hojas de estilos condicionales

Las hojas de estilos para IE pueden, sobradamente, ser evitadas en su totalidad. La única vez que puede necesitarse una hoja de estilo alternativa para IE es para eludir falta de soporte (Ej: ajustes para PNG).

Como regla general, todas las reglas de tu layout y modelo de cajas (box-model) pueden funcionar y _funcionarán_ sin una hoja de estilos para IE si reestructuras y trabajas tu CSS. Esto quiere decir que no queremos ver '<!--[if IE 7]> elemento{ margin-left:-9px; } < ![endif]-->' u otra parecida que claramente tiene un uso arbitrario para hacer que 'las cosas funcionen'.

## Depuración

Si te encuentras con un problema de CSS **quita código antes de empezar a agregar más** en un intento por solucionarlo. El problema existe en el CSS ya escrito, más CSS no es la solución adecuada.


Borra trozos de CSS y marcado hasta que el problema desaparezca, así puedes determinar en que parte o línea reside el problema.

Puede ser tentador poner un 'overflow:hidden;' en un elemento para ocultar los efectos de un error en el layout, pero overflow nunca fue probablemente el problema. **Arregla el problema, no sus síntomas.**

## Preprocesadores

### **Nota del traductor:** Harry Roberts (@csswizardry), autor de este texto, usa Sass, la idea e incluso las técnicas que se exponen a continuación son perfectamente adaptables a cualquier otro preprocesador //

Sass es mi preprocesador elegido. **Usadlo con cuidado.** Usa preprocesadores para mejorar la creación de CSS pero evita anidaciones exageradas! Anida sólo cuando sea necesario en vanilla CSS, Ej:

    .header{}
    .header .site-nav{}
    .header .site-nav li{}
    .header .site-nav li a{}

Sería totalmente innecesario en un CSS normal, de manera que lo que sigue estaría **mal**

Sass/Less:

    .header{
        .site-nav{
            li{
                a{}
            }
        }
    }

Lo correcto sería escribirlo así:

    .header{}
    .site-nav{
        li{}
        a{}
    }
