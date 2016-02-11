---
layout: post
title: Breve introducción a los Web Components
feature-img: "img/sample_feature_img.png"
---
# Breve introducción a los Web Components

Hablemos un poco del futuro antes que sea demasiado tarde. Y es que el tema sobre Web Components cada día que pasa adquiere más tracción entre los desarrolladores web.

En un mundo ideal el lenguaje HTML sería suficiente para desarrollar interfaces de usuarios complejas que a su vez sean extensibles. Esto permitiría que otros desarrolladores puedan utilizarlas para complementar funciones que no están nativamente apoyadas en los navegadores. O simplemente para crear un producto más adaptable a las nuevas necesidades de los usuarios. Hoy día esto es “posible” gracias a los Web Components.

## ¿Qué son los Web Components?
Los Web Components son una colección de tecnologías que constantemente se refinan a través del W3C y eventualmente llegan a ser implementadas dentro de los navegadores que usamos. En pocas palabras este conjunto de estándares para la web nos permite crear elementos HTML adaptables a nuestras necesidades tal y como lo haría tu navegador.

Estos componentes consisten de varias tecnologías ya conocidas siendo las principales HTML, CSS y JavaScript. Podemos ver estos componentes como elementos reusables para crear users interfaces derivados de los estándares en los que está fundamentada la Web. Es por eso que básicamente todo lo que construyamos a través de la utilización de estos componentes puede ser reutilizable fácilmente.

Por el momento los Web Components no están implementados completamente en los navegadores de mayor uso. Esto no quiere decir que no podamos aprovecharnos de las ventajas que ofrecen este tipo de tecnologías. Utilizando alguno que otro polyfill podemos cubrir ese espacio en donde los navegadores aun no proveen dicha tecnología. Una librería poderosa para este cumplir este propósito es el proyecto Polymer de Google.

Los Web Components agrupa las siguientes cuatro tecnologías (aunque cada una puede ser utilizada de manera independiente):

- Custom Elements
- HTML Templates
- Shadow DOM
- HTML Imports

## Custom Elements
La idea básica de un custom element es crear un elemento que se pueda comportar exactamente igual, manteniendo sus características y propiedades. Esto lo vemos a través de los elementos de `<table/>`, `<input/>`, `<img/>`, `<video/>` por mencionar algunos. Pero hoy en día y a medida que avancemos al futuro la web crece y crece y los elementos nativos que provee el HTML estándar se va quedando corto en cuestión de funcionalidad y extensibilidad. No podemos continuar creyendo que todo lo vamos resolver a través de un espagueti de divs sazonados con un poco de javascript. Así que los custom elements nos brinda la oportunidad de llevar el HTML a un próximo nivel. Y si vamos a cocinar un espagueti que sea con las mejores herramientas e ingredientes.

Crear un custom element es bien sencillo. Solo debes pensar en una etiqueta y listo. Se me ocurre `<x-component/>`. Ya, fácil, he creado mi propia etiqueta. Las misma la puedo adornar con CSS y hacer cosillas a través de JavaScript. ¿Estúpidamente secillo no?  Puede parecer  pero esto realmente no emociona mucho. Primero, para evitar colisiones con etiquetas HTML existentes se recomienda añadir un guión como parte del nombre. Veámoslo a través de un ejemplo. Para utilizar la nueva etiqueta o elemento en JavaScript puedo hacer lo siguiente:

```
var XComponent = document.registerElement('x-component');
var dom = new XComponent();
document.body.appendChild(dom);
```

## HTML Templates
Venga, venimos escuchando la palabra template básicamente desde la fundación del mundo. Así que no creo pertinente discutir que es un template o plantilla. No importa el lenguaje que prefieras, PHP, C#, Ruby, etc cada uno tiene su mecanismo de templates. Pero al día de hoy no existe un mecanismo nativo que pueda ser utilizado para trabajar a base de templates en el navegador. Tener un mecanismo de templates a este nivel provee la flexibilidad para que los diferentes equipos o desarrolladores puedan dividir mejor el trabajo. Los diseñadores pueden concentrarse en trabajar los elementos visuales escritos en HTML y CSS mientras los desarrolladores encargados del back-end se enfocan en la lógica y la integración.

Recordemos que al día de hoy la arquitectura de la web ha sufrido grandes cambios. Los servidores están más dedicados a procesar data de manera más eficientes y la parte del cliente ha pasado a poseer mayor relevancia en procesar la manera en que los usuarios interactuan con nuestro sistema. Mejor no lo puede exponer Eiji Kitamura cuando dice que el modelo MVC ya no es un patrón que ocurre únicamente en la parte del back-end, ya se ha convirtiendo en algo que ocurre también en la parte del cliente.

Los dos principales problemas que ataca este componente está relacionado a como utilizamos HTML para esconder algún detalle. Usualmente esto lo hacemos a través de un div y los escondemos usando CSS. En el ejemplo, la imagen del logo será cargada por el navegador aún cuando no se vaya a utilizar, gastando ancho de banda de forma innecesaria.

```
<div style="display:none;"> 
    <div> 
        <h1>Web Components</h1> 
        <img src="http://webcomponents.org/img/logo.svg"> 
    </div> 
</div>
```

Otra forma de lograr eso es usando la etiqueta de `<script/>`. Pero este método puede comprometer la seguridad del web app exponiéndola a un cross site scripting vulnerability si no se verifica adecuadamente la lógica del mismo.

```
<script type="text/template"> 
    <div> 
        <h1>Web Components</h1> 
        <img src="http://webcomponents.org/img/logo.svg">
    </div> 
</script>
```

Utilizando HTML Templates podemos lograr lo siguiente:

```
<template id="template">
  <style>
    ...
  </style>
  <div>
    <h1>Web Components</h1>
    <img src="http://webcomponents.org/img/logo.svg">
  </div>
</template>
```

Y con un poco de JavaScript podemos implementarlo en nuestro web app.

```
<script>
  var template = document.querySelector('#template');
  var clone = document.importNode(template.content, true);
  var host = document.querySelector('#host');
  host.appendChild(clone);
</script>
<div id="host"></div>
```

## Shadow DOM
Una de las funcionalidades principales del Shadow DOM es proveer la encapsulación necesaria al modelo de objeto de documento (DOM en inglés) para limitar su alcance. A través del Shadow DOM estos elementos pueden ser asociados de forma diferente. A estos nuevos nodos se le conocen como shadow root. Esto se logra separando estos elementos del DOM principal del documento. Aclaremos el asunto a través de una imagen.

shadow-dom

Para crear un Shadow DOM necesitamos conectar o pegar el siguiente método de Javascript, .createShadowRoot(), a cualquier elemento, sea HTML nativo o custom element. Este elemento es lo que se conoce como shadow host. Ahora bien, todo elemento creado bajo el shadow host es lo que se conoce como shadow root y es actualmente lo que el navegador muestra al momento de presentar el Shadow Dom.

En términos generales creamos un Shadow Dom cuando invocamos el método .createShadowRoot() en X elemento para convertirlo en el shadow host y así poder añadir a este elemento lo que será el shadow root. Para abundar un poco mas sobre el tema y jugar con varios ejemplos te recomiendo el artículo Shadow DOM 101 por Dominic Cooney.

## HTML Imports
Este componente se encarga de enlazar elementos de un documento HTML como una referencia externa a otro documento HTML. De esta manera podemos rehusar elementos que tengamos en un documento HTML X en multiples documentos. Si te suena a SSI (Server Side Includes) pues no estas muy lejos, ya que es muy similar el fin y su uso. Veamos un ejemplo:

Digamos que tenemos un documento llamado index.html. Para hacer uso del HTML Import utilizamos el tag link con el keyword import como atributo del rel. Y por último en el atributo href indicamos el documento html que queremos importar.

```
<link rel="import" href="component.html" >
```

Dentro de ese documento, component.html, puedes utilizar cualquier recurso incluyendo js, css, y web fonts, tal y como lo harías con cualquier documento html:

component.html

```
<link rel="stylesheet" href="css/style.css">
<script src="js/script.js"></script>
```

Este documento no necesita del típico encabezado que se utiliza para definir un documento HTML. Basta con definir los recursos y listo.

Es muy lamentable que de los cuatro componentes este es el que menos apoyo está recibiendo de los principales navegadores. Siendo la posición de Mozilla darle más espacio hasta ver cómo JavaScript implementa el sistema de módulos en las nuevas versiones (ES6, 7, etc). Y digo que es lamentable porque el HTML Imports puede verse como el pegamento que nos ayudaría a combinar lo que son el HTML Template, Shadow DOM y los Custom Elements. Cosa que las más famosas librerías actualmente hacen a través de polyfills. ¿No seria nítido que lo pudiéramos hacer de manera nativa?

## El futuro hoy
Hoy día muchos de los elementos que se agrupan bajo lo que son los Web Components se utilizan en los principales frameworks y librerías de desarrollo (React, Angular, Vuejs por mencionar algunas). O al menos los desarrolladores están persiguiendo que haya una buena integración con las funcionalidades principales tal y como se definen en los specs. Sin duda alguna los Web Components son un buen punto de partida para entender hacia donde se dirige el desarrollo web y como ir poco a poco adaptándonos esos cambios para que no nos tomen por sorpresa.

## Referencias

- WebComponents.org: a place to discuss and evolve web component best-practices
- W3C: Web Components Current Status
- What happened to Web Components?
- Supercharge your HTML powers with web components
- The state of Web Components