# Angular 1.x dyrektywy vs Angular 2 komponenty 

Dyrektywy nadal istnieją w Angularze 2. Komponenty są natomiast najważniejszym typem dyrektyw. Posiadają one dodatkowo template. Same komponenty są podstawą Angulara 2. 



## Angular 1.x
```
<home-component person="{ name: 'John' }"></home-component>
```
```
var app = angular.module('app', []);

app.directive('homeComponent', function(){
  return {
    restrict: 'E',
    template: '{{ homeController.sayHello() }}',
    controller: 'homeController',
    controllerAs: 'homeController',
    bindToController: true,
    scope: { 
      person: '='
    }
  }
});

angular.module('app').controller('homeController', function() {
  this.sayHello = function() {
    return 'Hello ' + this.person.name;
  };
});
```

## Angular 2

Największa zmiana w htmlowym wywołaniu dyrektywy to przekazanie parametru do dyrektywy - nawiasy []. Nie ma tutaj już bindowania w dwie strony. Teraz trzeba użyć innych nawiasów - (), aby przechwycić zdarzenie wychodzące z dyrektywy.

```  
<home-component [person]="{ name: 'John' }"></home-component>
```

W samym kodzie nie ma już kontrolerów. Wszystko dzieje się w komponencie. `restrict` został zamieniony na `selector`, a dane wejściowe można teraz przekazać tworząc property klasy z dekoratorem `@Input()`.

```
import {Component, Input} from 'angular2/angular2'

@Component({
  selector: 'home-component',
  template: `
    {{ sayHello() }}
  `
})
export class HomeComponent {
  @Input() person:Object;
  
  sayHello() {
    return `Hello ${this.person.name}`;
  }
}
```