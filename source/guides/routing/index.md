## Rutas

A medida que los usuarios interactuan con la aplicación, esta pasará por diferentes estados. Ember.js proporciona herramientas que ayudarán a gestionar los distintos estados para que escalen junto a la aplicación.

Para entender porqué esto es importante, supondremos que estamos escribiendo una aplicación para gestionar un blog. En cualquier momento deberá de poderse responder preguntas como: _¿Está el usuario actual logeado? ¿Es un usuario con rol administrativo? ¿Qué es lo que está viendo? ¿Está abierta la ventana de configuraciones? ¿Está editando el artículo actual?_

En Ember.js, cada uno de los posibles estados de una aplicación son representados por una URL. Porque todas las preguntas que se realizaron anteriormente-_¿Está el usuario actual logeado? ¿Es un usuario con rol administrativo?_ —están encapsuladas en los manejadores de ruta (route handlers) de las URL's, reponderlas es simple y preciso.

En un momento dado, la aplicación tendrá uno o más _manejadores de ruta activos (active route handlers)_. Los manejadores de ruta pueden cambiar por una de dos razones:

1. El usuario interactuó con una vista, que generó un evento que causo que la URL cambiara.
2. El usuario modificó la ruta manualmente (Ej. con el botón atrás), o la página fue cargada por primera vez.

Cuando la URL cambia, los manejadores de la nueva ruta activa pueden realizar una o más de las acciones siguientes:

1. Redirigir a una nueva URL condicionalmente.
2. Actualizar un controlador para que represente un modelo particular.
3. Cambiar la plantilla en la pantalla, o colocar una nueva plantilla en un "outlet".

###Logear cambios de ruta

A medida que una aplicación se incremente en complejidad, puede ser útil saber que es lo que esta sucediendo en la ruta. Para que Ember muestre el log de eventos de transición, simplemente hay que modificar la variable `Ember.Application`:

```javascript
App = Ember.Application.create({
  LOG_TRANSITIONS: true
});
```

###Especificar la URL de ruta
Si la aplicación de Ember es una de varias aplicaciones web, servidas desde el mismo dominio, puede ser necesario indicar al enrutador cual es la URL raíz de la aplicación. De manera predeterminada, Ember asumirá que esta siendo servido desde la URL raíz de la aplicación.

Si por ejemplo, es necesaro servir la aplicación de blog desde www.emberjs.com/blog/, sera necesario especificar la ruta `/blog/`.

Esto puede ser alcanzado especificando la variable rootURL en el enrutador.

```js
App.Router.reopen({
  rootURL: '/blog/'
});
```
