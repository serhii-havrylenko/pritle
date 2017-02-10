# Event handling via pure JS

Event handling using pure JavaScript can be realized with addEventListener/dispatchEvent/removeEventListener functions. All modern browsers support these functions and allow to add standard event listeners as well as custom events on DOM objects:

```javascript
var event = new Event('build');
elem.addEventListener('build', function (e) { ... }, false);
elem.dispatchEvent(event);
```

For custom events CustomEvent constructor should be used instead of Event:
```javascript
var event = new CustomEvent('build', { 'detail': elem.dataset.time });
```

# Custom event handling frameworks

Custom JS frameworks for handling events:
* [Backbone.Events](http://backbonejs.org/#Events)
* [RxJS from Microsoft](https://github.com/Reactive-Extensions/RxJS)
* [EventEmitter](https://github.com/Olical/EventEmitter)
* [AngularJS $emit/$on](https://docs.angularjs.org/api/ng/type/$rootScope.Scope#$emit)

# Examples

I have an experience only with AngularJS events handling via $scope.$emit/$on. However realization via other custom frameworks/libraries is quite simple and not so complicated.

## AngularJS $emit/$broadcast/$on

The realization of custom event handling via $on/$emit is quite simple:
```javascript
$scope.$on('someEvent', (event, data) => {
  console.log(data);
});

$scope.$emit('someEvent', 'Message');

$scope.$broadcast('someEvent', {
  foo: 'bar'
});
```

There are two ways how to fire events on $scope:
* $scope.$emit for firing event upwards
* $scope.$broadcast for firing event downwards

In this case when app fire event it has access only to parent scope and all child scopes (no access to the sibling scopes). The event will stop propagating if one of the listeners cancels it.

For rootScope situation is quite different:
* $rootScope.$emit fire an event for all $rootScope.$on listeners only.
* $rootScope.$broadcast notify all $rootScope.$on as well as $scope.$on listeners.

## EventEmitter

Even handling via EventEmitter:
```javascript

var ee = new EventEmitter();

ee.addListener('foo', showFoo);
ee.addListener('bar', showBar);

function showFoo(message) {
  console.log(`showFoo event: ${message}`)
  ee.emitEvent('bar', arguments);
}

function showBar(text) {
   console.log(`showBar event, message: ${text}`);
}

ee.emitEvent('foo', ['Fooooooo'])

```
