# Cabit
Event driven state of the app.

## API

### Emmit

```
var data = {
  a: 'a'
};

// Emmit start event
cabit.start('name', data);

data.a = 'b';

// Emmit end event
cabit.end('name', data);
```

### Subscribe

```
// Subscribe to start
cabit.onStart('name', function(data) {
  console.log(data);
});

// Subscribe to end
cabit.onEnd('name', function(data) {
  console.log(data);
});

// Subscribe to all
cabit.onAll('name', function(data) {
  console.log(data);
});
```

## Example

```
function MyFunction() {
  cabit.onStart('some', function(data) {
    console.log('MyFunction', 'constructor', 'start', data);
  });
}

MyFunction.prototype.someMethod = function() {
  setTimeout(function() {
    cabit.start('some', {
      a: 'a'
    });

    setTimeout(function() {
      cabit.end('some', {
        a: 'b'
      });
    }, 1000);
  }, 1000);
}

var myFunction = new MyFunction();
myFunction.someMethod();

function MyModule() {
  cabit.onStart('some', function(data) {
    console.log('MyModule', 'constructor', 'start', data);
  });
}

MyModule.prototype.someMethod = function() {
  cabit.onEnd('some', function(data) {
    console.log('MyModule', 'someMethod', 'end', data);
  });
}

cabit.onAll('some', function(data) {
  console.log('all', data);
});

var MyModule = new MyModule();
MyModule.someMethod();
```