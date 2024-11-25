### **Angular Forms și Router**

---

### **1. Angular Forms**
Angular Forms este un modul care facilitează lucrul cu formularele HTML, oferind două abordări principale pentru gestionarea datelor:
1. **Template-Driven Forms** (Formulare bazate pe template-uri)
2. **Reactive Forms** (Formulare reactive)

---

#### **Template-Driven Forms**
Această abordare este mai simplă și folosește directive pentru a lega elementele unui formular de modelul de date.

##### **Cum funcționează?**
1. Se bazează pe directive precum `ngModel` pentru **legarea bidirecțională** a datelor.
2. Codul pentru validare și logică se află în fișierul HTML (template-ul).

##### **Exemplu:**
1. **Componenta TypeScript (`form.component.ts`)**:
   ```typescript
   import { Component } from '@angular/core';

   @Component({
     selector: 'app-form',
     templateUrl: './form.component.html',
   })
   export class FormComponent {
     user = {
       name: '',
       email: '',
     };

     onSubmit() {
       console.log(this.user);
     }
   }
   ```

2. **Template-ul HTML (`form.component.html`)**:
   ```html
   <form #userForm="ngForm" (ngSubmit)="onSubmit()">
     <div>
       <label for="name">Name:</label>
       <input
         id="name"
         name="name"
         [(ngModel)]="user.name"
         required
         #name="ngModel"
       />
       <div *ngIf="name.invalid && name.touched">Name is required.</div>
     </div>
     <div>
       <label for="email">Email:</label>
       <input
         id="email"
         name="email"
         type="email"
         [(ngModel)]="user.email"
         required
         #email="ngModel"
       />
       <div *ngIf="email.invalid && email.touched">Valid email is required.</div>
     </div>
     <button type="submit" [disabled]="userForm.invalid">Submit</button>
   </form>
   ```

##### **Chei importante:**
- `[(ngModel)]`: Realizează legătura bidirecțională între model și interfață.
- `#userForm="ngForm"`: Atribuie un obiect de tip formular pentru validare.
- `ngSubmit`: Gestionează evenimentul de trimitere a formularului.

---

#### **Reactive Forms**
Această abordare este mai robustă și permite gestionarea formularului printr-un model declarativ, definit în TypeScript.

##### **Cum funcționează?**
1. Modelul formularului este definit în componenta TypeScript.
2. Este folosit pentru validări complexe și control asupra stării formularului.

##### **Exemplu:**
1. **Componenta TypeScript (`form.component.ts`)**:
   ```typescript
   import { Component } from '@angular/core';
   import { FormGroup, FormBuilder, Validators } from '@angular/forms';

   @Component({
     selector: 'app-form',
     templateUrl: './form.component.html',
   })
   export class FormComponent {
     userForm: FormGroup;

     constructor(private fb: FormBuilder) {
       this.userForm = this.fb.group({
         name: ['', Validators.required],
         email: ['', [Validators.required, Validators.email]],
       });
     }

     onSubmit() {
       console.log(this.userForm.value);
     }
   }
   ```

2. **Template-ul HTML (`form.component.html`)**:
   ```html
   <form [formGroup]="userForm" (ngSubmit)="onSubmit()">
     <div>
       <label for="name">Name:</label>
       <input id="name" formControlName="name" />
       <div *ngIf="userForm.get('name')?.invalid && userForm.get('name')?.touched">
         Name is required.
       </div>
     </div>
     <div>
       <label for="email">Email:</label>
       <input id="email" formControlName="email" />
       <div *ngIf="userForm.get('email')?.invalid && userForm.get('email')?.touched">
         Valid email is required.
       </div>
     </div>
     <button type="submit" [disabled]="userForm.invalid">Submit</button>
   </form>
   ```

##### **Chei importante:**
- `FormGroup` și `FormControl`: Controlează structura și validarea formularului.
- `formControlName`: Leagă un câmp HTML de un control din modelul formularului.

---

### **2. Angular Router**
Angular Router permite **navigarea între diferite pagini** sau **componente** ale aplicației fără reîncărcarea browserului.

---

#### **Cum funcționează?**
1. Definești un **sistem de rute** în aplicație.
2. Utilizezi directive precum `[routerLink]` pentru a naviga între rute.
3. Componentele se încarcă în `<router-outlet>`.

##### **Configurarea Routing-ului:**
1. **Definirea rutelor în `app-routing.module.ts`**:
   ```typescript
   import { NgModule } from '@angular/core';
   import { RouterModule, Routes } from '@angular/router';
   import { HomeComponent } from './home/home.component';
   import { AboutComponent } from './about/about.component';

   const routes: Routes = [
     { path: '', component: HomeComponent },
     { path: 'about', component: AboutComponent },
   ];

   @NgModule({
     imports: [RouterModule.forRoot(routes)],
     exports: [RouterModule],
   })
   export class AppRoutingModule {}
   ```

2. **Adăugarea `router-outlet` în componenta principală (`app.component.html`)**:
   ```html
   <nav>
     <a routerLink="/">Home</a>
     <a routerLink="/about">About</a>
   </nav>
   <router-outlet></router-outlet>
   ```

3. **Navigarea între pagini:**
   - Utilizatorul face clic pe un link (`routerLink`).
   - Angular încarcă componenta corespunzătoare în `<router-outlet>`.

##### **Cum se prelucrează parametrii în URL?**
1. Definirea unei rute cu parametru:
   ```typescript
   { path: 'details/:id', component: DetailsComponent }
   ```

2. Preluarea parametrului în componentă:
   ```typescript
   import { ActivatedRoute } from '@angular/router';

   constructor(private route: ActivatedRoute) {}

   ngOnInit(): void {
     const id = this.route.snapshot.paramMap.get('id');
     console.log(id);
   }
   ```

---

### **Diferențe și utilizări**
| Caracteristică        | Angular Forms                     | Angular Router                       |
|-----------------------|------------------------------------|--------------------------------------|
| **Funcționalitate**   | Gestionarea formularelor și datelor | Navigarea între paginile aplicației  |
| **Elemente cheie**    | `ngModel`, `FormGroup`, Validators | `routerLink`, `ActivatedRoute`, Routes |
| **Utilizare**         | Validare date, colectare informații | Gestionarea navigării și URL-urilor  |

