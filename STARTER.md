Create New Page
ng2-admin uses Angular 2 Component Router for navigation.

We strongly recommend to follow this page structure in your application. If it does not fit your needs please create a GitHub issue and tell us why. We would be glad to discuss.

Basically any page is just a common Angular 2 Component with a route defined for it.

Let’s create a blank page in 6 easy steps
1) Create a new directory for our new page inside of src/app/pages. We can call the directory new. 


2) Then let’s create a blank angular 2 component for our page called ‘new.component.ts’ inside of src/app/pages/new:

import {Component} from '@angular/core';

@Component({
  selector: 'new',
  template: `<strong>My page content here</strong>`
})
export class NewComponent {
  constructor() {}
}
This will create a simple Angular 2 component. For more detail please check out official Angular 2 documentation. 


3) After that we should create our component routing called new.routing.ts in the new directory.

import { Routes, RouterModule }  from '@angular/router';
import { NewComponent } from './new.component';

const routes: Routes = [
  {
    path: '',
    component: NewComponent
  }
];

export const routing = RouterModule.forChild(routes);


4) And now we should create new.module.ts in src/app/pages/new directory and import new.component.ts and new.routing.ts in it.

import { NgModule }      from '@angular/core';
import { CommonModule }  from '@angular/common';
import { NewComponent } from './new.component';
import { routing } from './new.routing';

@NgModule({
  imports: [
    CommonModule,
    routing
  ],
  declarations: [
    NewComponent
  ]
})
export default class NewModule {}


5) The penultimate thing we need to do is to declare a route in src/app/pages/pages.menu.ts. Typically all pages are children of the /pages route and defined under the children property of the root pages route like this:

export const PAGES_MENU = [
  {
    path: 'pages',
    children: [
      {
        path: 'new',  // path for our page
        data: { // custom menu declaration
          menu: {
            title: 'New Page', // menu title
            icon: 'ion-android-home', // menu icon
            pathMatch: 'prefix', // use it if item children not displayed in menu
            selected: false,
            expanded: false,
            order: 0
          }
        }
      },
      {
        path: 'dashboard',
        data: {
          menu: {
            title: 'Dashboard',
            icon: 'ion-android-home',
            selected: false,
            expanded: false,
            order: 0
          }
        }
      }
    }
  }
]
If you’d like to highlight menu item when current URL path partially match the menu item path - use pathMatch: ‘prefix’. In this case if the menu item has no children in the menu and you navigated to some child route - the item will be highlighted. 


6) And in the end let’s import our component in src/app/pages/pages.routing.ts like this:

const routes: Routes = [
  {
    path: 'pages',
    component: Pages,
    children: [
      { path: '', redirectTo: 'dashboard', pathMatch: 'full' },
      { path: 'dashboard', loadChildren: () => System.import('./dashboard/dashboard.module') },
      { path: 'new',  loadChildren: () => System.import('./new/new.module') }
    ]
  }
];


And that’s it! Now your page is available by the following this url http://localhost:3000/#/pages/new. Plus, your page is registered inside the sidebar menu. If you don’t want to have a link in the menu - just remove the menu declaration from the pages.menu.ts file.
