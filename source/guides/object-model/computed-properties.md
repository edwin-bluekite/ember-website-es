## Que es el cálculo de propiedades(propiedades calculadas)?

En resumen, cálculo de propiedades permite declarar funciones como propiedades. Para crear una hay definir una propiedad calculada como una funcion, que Ember llamará automaticamente cuando se consulte la propiedad. Se puede utilizar de la misma manera que una propiedad estática normal.

It's super handy for taking one or more normal properties and transforming or manipulating their data to create a new value. 

### Popiedades calculadas en acción

Iniciaremos con un ejemplo sencillo[

```javascript
App.Person = Ember.Object.extend({
  // esto sera suministrado por `create`
  firstName: null,
  lastName: null,

  fullName: function() {
    return this.get('firstName') + ' ' + this.get('lastName');
  }.property('firstName', 'lastName')
});

var ironMan = App.Person.create({
  firstName: "Tony",
  lastName:  "Stark"
});

ironMan.get('fullName') // "Tony Stark"
```
Hay que notar que la función `fullName` realiza una llamada a `property`. Esto declara la función como una propiedad calculada, y los parametros le indican a Ember que depende de los atributos `firstName` y `lastName`.

Al acceder a la pripiedad `fullName`, esta funcion es llamada, y retorna el valor de la función, la cual simplemente ejecuta `firstName` + `lastName`.

### Modificando propiedades calculadas

Se puede utilizar las propiedades calculadas como valores para crear nuevas propiedades calculadas. Vamos a agregar la propiedad calculada `description` al ejemplo anterior, y utilizando la propiedad existente `fullName` y agregando otras:

```javascript
App.Person = Ember.Object.extend({
  firstName: null,
  lastName: null,
  age: null,
  country: null,

  fullName: function() {
    return this.get('firstName') + ' ' + this.get('lastName');
  }.property('firstName', 'lastName'),

  description: function() {
    return this.get('fullName') + '; Age: ' + this.get('age') + '; Country: ' + this.get('country');
  }.property('fullName', 'age', 'country')
});

var captainAmerica = App.Person.create({
  firstName: 'Steve',
  lastName: 'Rogers',
  age: 80,
  country: 'USA'
});

captainAmerica.get('description'); // "Steve Rogers; Age: 80; Country: USA"
```

### Actualización dinámica

Las propiedades calculadas, de forma predeterminada observan cualquier cambio que se realize a las propiedades de las que dependen y son actulizadas dinámicamente cuando son utilizadas. Vamos a utilizar las propiedades calculadas para una actualización dinámica.

```javascript
captainAmerica.set('firstName', 'William');

captainAmerica.get('description'); // "William Rogers; Age: 80; Country: USA"
```

El cambio a `firstName` fue observado por la propiedad calculada `fullName`, la cual a su vez a sido observada por la propiedad `description`.

Al establecer cualquier propiedad calculada esta se propagará atraves de cualquier propiedad calculada que dependa de ella, copletando toda la cadena de propiedades calculadas creadas.

### Estableciendo propiedades calculadas

Tambien es posible definir que es lo que Ember tiene que hacer cuando se le asigna valor a una propiedad calculada. Si intentas poner valor en una propiedad calculada, esta será invocada con la llave (nombre de la propiedad), el valor que se desea establecer y el valor anterior.

```javascript
App.Person = Ember.Object.extend({
  firstName: null,
  lastName: null,

  fullName: function(key, value) {
    // setter
    if (arguments.length > 1) {
      var nameParts = value.split(/\s+/);
      this.set('firstName', nameParts[0]);
      this.set('lastName',  nameParts[1]);
    }

    // getter
    return this.get('firstName') + ' ' + this.get('lastName');
  }.property('firstName', 'lastName')
});


var captainAmerica = App.Person.create();
captainAmerica.set('fullName', "William Burnside");
captainAmerica.get('firstName'); // William
captainAmerica.get('lastName'); // Burnside
```

Ember ejecutará un la llamada a la propiedad calculada para los setters y  getters, de manera que si deseas utilizar una propiedad calculada como un setter, será necesario verificar la cantidad de argumentos para determinar si está siendo llamada como un getter o un setter. Hay que notar que si un valor es retornado de un setter, este se almacena en caché como el valor de la propiedad.
