# AngularRay

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 1.7.4.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory. Use the `-prod` flag for a production build.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via [Protractor](http://www.protractortest.org/).

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI README](https://github.com/angular/angular-cli/blob/master/README.md).

## My Tutorial
Learning Angular with Ray Villalobos

https://github.com/planetoftheweb/learnangular5


Create new angular project
ng new angular-ray
Run the project and open the browser
ng serve -o
Create dist folder with deployable application files
ng build
.angular-cli.json  : place where we can change dist folder, css style or index page

Install bootstrap and jquery and check node_modules for their presence
npm install jquery --save-dev
npm install bootstrap --save-dev
Add CSSs and JavaScripts to the project in angular-cli.json
"styles": [
  "../node_modules/bootstrap/dist/css/bootstrap.css",
  "styles.css"
],
"scripts": [
  "../node_modules/jquery/dist/jquery.js",
  "../node_modules/bootstrap/dist/js/bootstrap.bundle.js"
],

In index.html wrap the app-root component inside the container tag. Container lets app to live in its own grid. 

In app-component.html put some bootstrap code
<div class="row justify-content-center">
  <div class="card mt-sm-3 col-md-8">
    <div class="card-body">
      <h3 class="mb-0">Artist Directory</h3>
      <div class="form-group">
        <div class="form-label"></div>
        <input class="form-control mt-2" type="text">
      </div><!-- form-group -->
    </div><!-- card-body -->
  </div><!-- card -->
</div><!-- row -->

Change the background in index.html, use bootstrap class
<body class="bg-light">

To display some variable do it in app.component.ts
export class AppComponent {
  query: string;

  constructor() {
    this.query = 'Barot';
  }
}

The name of the variable is query, so we need to define in the template:
<div class="form-label"><strong>For:</strong> {{query}}</div>

 

Add the list of artists in app.component.ts:

artists: object;

constructor() {
  this.query = 'Barot';
  this.artists =  this.artists = [
    {
      "name":'Barot Bellingham',
      "shortname":"Barot_Bellingham",

loop through the list of artists and pick values

<a href="#" class="list-group-item list-group-item-action flex-column align-items-start"
*ngFor="let artist of artists">
  <div class="media">
    <div class="media-body align-self-center">
      <h5 class="m-0">{{artist.name}}</h5>
      {{artist.reknown}}
    </div><!-- media body -->


Events

Add click events and change the background
(click)="showArtist(artist); query=''"
[style.backgroundColor]="artist.highlight ? '#EEE' : '#FFF'">



showArtist(item) {
  this.query = item.name;
  item.highlight = !item.highlight;
}


Pouzitie properties ak by sa stavalo, ze sa stranka zobrazi predtym ako sa dotiahnu obrazky

[src]="'./assets/images/' + artist.shortname + '_tn.jpg'" alt="{{ 'Photo of ' + artist.name }}">


2 way data binding

<input class="form-control mt-2" type="text"
  [(ngModel)]="query">

Add imports app.module.ts
import { FormsModule } from '@angular/forms';

imports: [
  BrowserModule, FormsModule, HttpClientModule
],

Now what I write is displayed immediately
 

Lifecycle
Add import in app.module.ts

import { HttpClientModule } from '@angular/common/http';

imports: [
  BrowserModule, FormsModule, HttpClientModule
],

Add import in app.component.ts
import { Component, OnInit } from '@angular/core';
import { HttpClient } from '@angular/common/http';


export class AppComponent implements OnInit {
  query: string;
  artists: object;
  currentArtist: object;

  showArtist(item) {
    this.query = item.name;
    item.highlight = !item.highlight;
    this.currentArtist = item;
  }

  constructor( private http: HttpClient ) {
    this.query = '';
  }

  ngOnInit(): void {
    this.http.get<Object>('../assets/data.json').subscribe( data => {
      this.artists = data;
    })
  }


Subcomponents
Generate component
ng generate component artist-items
Extract some html part from 1 html and put in into another. Then we need to pass the variable from the original so we can loop through the items
<app-artist-items [artist]="artist" ></app-artist-items>


Pipes
Generate pipes to transform the output, search, etc..
ng generate pipe search-artist
it create search-artists.pipe.ts and add some imports.
pipeData : data which I want to search
pipeModifier :  the text I am searching, it’s the input text

import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'searchArtists'
})
export class SearchArtistsPipe implements PipeTransform {

  transform(pipeData, pipeModifier): any {
    return pipeData.filter(eachItem => {
      return (
        eachItem['name'].toLowerCase().includes(pipeModifier.toLowerCase()) ||
        eachItem['reknown'].toLowerCase().includes(pipeModifier.toLowerCase())
      )
    });
  }
}

This is how you use the pipe
*ngFor="let artist of (artists | searchArtists: query)"


<app-artist-details  *ngIf="currentArtist" 
  [artist]="currentArtist"></app-artist-details>

*ngIf check if there is a value in variable and only in that case displays it. 
