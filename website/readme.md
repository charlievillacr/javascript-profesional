# Inicio del proyecto

[Clase 3](https://platzi.com/clases/1642-javascript-profesional/22157-inicio-del-proyecto/)

Trabajaremos en el proyecto PlatziVideo, una plataforma de vídeo.

Esta es la base de nuestro proyecto y nos vamos a enfocar en el MediaPlayer. Durante el curso se desarrollarán plugins de forma modular para extender la funcionalidad del MediaPlayer.

El repositorio de este curso lo encuentras en: [https://github.com/platzi/javascript-profesional](https://github.com/platzi/javascript-profesional)

## Agregamos funcionalidades al botón con un script

```html
<script>
      const video =  document.querySelector("video")
      const button = document.querySelector("button")

      function MediaPlayer(config) {
        this.media = config.el
      }

      MediaPlayer.prototype.play = function() {
        this.media.play();
      }

      MediaPlayer.prototype.pause = function(){
        this.media.pause()
      }

      const player = new MediaPlayer({ el: video });

      button.onclick = function (){
      (player.media.paused)
        ? player.play()
        : player.pause()
      console.log(player.media.paused)
    }   

    </script>
```

## Cómo llega un script al navegador

El **DOM** es la representación que hace el navegador de un documento HTML.

El navegador interpreta el archivo HTML y cuando termina de transformarlo al DOM se dispara el evento DOMContentLoaded lo que significa que todo el documento está disponible para ser manipulado.

Todo script que carguemos en nuestra página tiene un llamado y una ejecución.

Tanto con async como defer podemos hacer llamados asíncronos pero tiene sus diferencias:

async. Con async podemos hacer la petición de forma asíncrona y no vamos a detener la carga del DOM hasta que se haga la ejecución del código.

defer. La petición es igual asíncrona como en el async pero va a deferir la ejecución del Javascript hasta el final de que se cargue todo el documento.

Hay que tener en cuenta que cuando carga una página y se encuentra un script a ejecutar toda la carga se detiene. Por eso se recomienda agregar tus scripts justo antes de cerrar el body para que todo el documento esté disponible.