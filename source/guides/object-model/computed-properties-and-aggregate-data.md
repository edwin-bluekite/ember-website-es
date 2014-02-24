A menudo, se tiene una propiedad calculada que está basada en todos los elementos de un arreglo para determinar su valor. Por ejemplo, se puede necesitar contar todos los elementos TODO de un controllador para determinar cuantos de estos han sido completados.

Así es como se observaría una propiedad calculada:

```javascript
App.TodosController = Ember.Controller.extend({
  todos: [
    Ember.Object.create({ isDone: false })
  ],

  remaining: function() {
    var todos = this.get('todos');
    return todos.filterBy('isDone', false).get('length');
  }.property('todos.@each.isDone')
});
```

Hay que notar aca que la llave dependiente (`todos.@each.isDone`) contiene la llave especial `@each`.
Esto le indica e Ember.js que actualize los enlaces y ejecute los observers para esta propiedad calculada cuando alguno de estos cuatro eventos ocurre:

1. La alguna propiedad `isDone` del arreglo de objetos `todos` cambia.
2. Un elemento es agregado al arreglo de `todos`.
3. Un elemento es eliminado del arreglo de `todos`.
4. La propiedad `todos` del controllador es cambiada a un arreglo distinto.

En el ejemplo anterior, el contador `remaining` es `1`:

```javascript
App.todosController = App.TodosController.create();
App.todosController.get('remaining');
// 1
```

Si se cambia la pripieadad `isDone`, la propiedad `remaining` es actualizada automáticamente:

```javascript
var todos = App.todosController.get('todos');
var todo = todos.objectAt(0);
todo.set('isDone', true);

App.todosController.get('remaining');
// 0

todo = Ember.Object.create({ isDone: false });
todos.pushObject(todo);

App.todosController.get('remaining');
// 1
```

Hay que notar que `@each` solo trabaja a primer nivel de profundidad. No es posible utilizarlo de maneras anidadas como:
`todos.@each.owner.name` or `todos.@each.owner.@each.name`.
