# Router 

## Bootstrapowanie routera: 
```
import {bootstrap} from 'angular2/angular2';
import {ROUTER_PROVIDERS} from 'angular2/router';

import {App} from './app';

bootstrap(App, [ROUTER_PROVIDERS]);  
```

Dodatkowo Angular będzie nas prosił o dodanie taga `<base href="/"></base>` do pliku `index.html`.

## Dodawanie pierwszej ścieżki i nawigacji

```
import {ROUTER_DIRECTIVES, RouteConfig} from 'angular2/router';
import {Home} from './home';

@Component({
  selector: 'app',
  template: `
    <nav>
      <ul>
        <li>
          <a [router-link]="['/home']" title="Home">Home</a>
          ...
        </li>
      </ul>
    </nav>
  `,
  directives: [ROUTER_DIRECTIVES]
})
@RouteConfig([
  { path: '/', redirectTo: '/home' },
  { path: '/home', component: Home, as: 'home' }
])
export class App {}
```

## Zagnieżdzone route'y

Modyfikując poprzedni przykład poprzez dodanie `/...` do ścieżki uzyskamy stan abstrakcyjny. 
```
...
@RouteConfig([
  { path: '/', redirectTo: '/home/...' },
  { path: '/home/...', component: Home, as: 'home' }
])
...
```

Dzięki temu w komponencie `Home` możemy utworzyć kolejny `@RouteConfig`, który przełoży się na finalny route.
```
import {ROUTER_DIRECTIVES, RouteConfig} from 'angular2/router';
import {HomeMain} from './home-main';
import {HomeInfo} from './home-info';

@Component({
  selector: 'home',
  tempalate: '<router-outlet></router-outlet>',
  directives: [ROUTER_DIRECTIVES]
})
@RouteConfig([
  { path: '/', component: HomeMain }, 
  { path: '/info', component: HomeInfo }
])
export class Home {}
```
`HomeMain` będzie dostępny pod routem `/home`, natomiast `HomeInfo` będzie dostępny pod routem `/home/info`.


## Parametry 

W przypadku poruszania się po liście obiektów potrzebne będą też szczegóły jednego z nich. Przekazywanie parametru (np. `id`) w Angularze 2 wygląda następująco: 
```
...
@RouteConfig([
  { path: '/', component: HomeMain }, 
  { path: '/info', component: HomeInfo },
  { path: '/info/:id', component: HomeInfoForSpecificId }
])
...
```
Dodatkowo, aby 'wydobyć' ten parametr potrzebne będzie wstrzyknięcie `RouteParams` i wykorzystanie funkcji `get`. Kod `HomeInfoForSpecificId` będzie wyglądał tak:
```
import {Component} from 'angular2/angular2';
import {RouteParams} from 'angular2/router';

@Component({
  selector: 'home-info-for-id',
  template: `
    HomeInfo no. {{ id }}
  `
})
export class HomeInfoForSpecificId {
  constructor(routeParams: RouteParams) {
    this.id = routeParams.get('id');
  }
}
```
