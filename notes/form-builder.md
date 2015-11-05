# Form Builder

W Angularze 2 mamy do dyspozycji nowy system tworzenie formularzy. Wcześniej wszystko opierało się na `ng-model`, teraz jest jeszcze `FormBuilder`. W tym przypadku grupę oraz walidatory tworzymy w kodzie, a nie w htmlu (jak wcześniek). Pozwala to na przede wszystkim na testowanie kodu.

## Model formularza

Cały model w pliku html tworzony jest poprzez dodanie na formularzu `[ng-form-model]` i zdefiniowaniu dla niego nazwy. Dzięki niej będzie można poruszać się pomiędzy kodem komponentu a danymi wprowadzanymi przez użytkownika. Tworzenie kolejnych kontrolek sprowadza się do dodania `ng-controler` z nazwą oraz zdefiniowanie grupy w instancji `FormBuiler`a. 

```
import {bootstrap, Component, FormBuilder, Validators, FORM_DIRECTIVES, FORM_PROVIDERS} from 'angular2/angular2';

@Component({
  selector: 'login-form',
  template: `
    <form [ng-form-model]="loginForm" (ng-submit)="onSubmit()">
        <p>
            <label>First name:</label>
            <input type="text" ng-control="firstName">
        </p>
        <p>
            <label>Password:</label>
            <input type="password" ng-control="password">
        </p>
        <p>
            <button type="submit" [disabled]="!loginForm.valid">Submit</button>
        </p>
    </form>
  `,
  directives: [FORM_DIRECTIVES]
})
class LoginForm {
  constructor(formBuilder: FormBuilder) {
    this.loginForm = formBuilder.group({
      "firstName": ["", Validators.required],
      "password": ["", Validators.required]
    });
  }
  
  onSubmit() {
    console.log(this.loginForm.value);
  }
}

bootstrap(LoginForm, [FORM_PROVIDERS]);
```