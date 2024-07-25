#Exercice 2: serveur angular

#Journalisation d'un site web (avec deux routes : home et help sur un serveur angular), sur un fichier .txt, à chaque visite du client.7

# Resolution
#J'ai commencé par installer Angular CLI en utilisant npm. Vous devrez peut-être utiliser sudo pour les permissions administratives :
sudo npm install -g @angular/cli
#Après l'installation, vérifiez que Angular CLI est installé correctement 
ng version

#Créer un projet Angular:
mkdir my-angular-app
ng new my-angular-app
cd my-angular-app

#Générer deux composants pour les routes :
ng generate component home
ng generate component help

#Configurer les routes Angular dans app-routing.module.ts :
nano app-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { HelpComponent } from './help/help.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'help', component: HelpComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }

#Configurer les composants pour afficher un simple message :
nano home.component.html
<h1>Welcome to the Home Page!</h1>
nano help.component.html
<h1>Welcome to the Help Page!</h1>

#Créer un backend Node.js pour journaliser les visites
#Créer un nouveau répertoire pour le backend
mkdir backend
cd backend
npm init -y
npm install express body-parser

#Créer un fichier server.js :
nano angular-server.js
const express = require('express');
const fs = require('fs');
const path = require('path');
const bodyParser = require('body-parser');

const app = express();
const port = 3001;

app.use(bodyParser.json());

// Middleware pour journaliser les visites
app.use((req, res, next) => {
  const log = `${new Date().toISOString()} - ${req.method} ${req.url}\n`;
  fs.appendFile(path.join(__dirname, 'visits.log'), log, (err) => {
    if (err) {
      console.error('Failed to write log:', err);
    }
  });
  next();
});

// Endpoint pour recevoir les visites des routes Angular
app.post('/log', (req, res) => {
  const log = `${new Date().toISOString()} - ${req.body.route}\n`;
  fs.appendFile(path.join(__dirname, 'visits.log'), log, (err) => {
    if (err) {
      console.error('Failed to write log:', err);
      res.status(500).send('Failed to write log');
    } else {
      res.status(200).send('Log written');
    }
  });
});

app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}/`);
});

#Créer un service Angular pour envoyer les visites au backend :
ng generate service log
nano log.service.ts 
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { environment } from '../environments/environment';

@Injectable({
  providedIn: 'root'
})
export class LogService {
  private apiUrl = 'http://localhost:3001/log';

  constructor(private http: HttpClient) { }

  logRoute(route: string) {
    return this.http.post(this.apiUrl, { route }).subscribe();
  }
}

#Injecter le service dans les composants :
nano home.component.ts
import { Component, OnInit } from '@angular/core';
import { LogService } from '../log.service';

@Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
export class HomeComponent implements OnInit {
  constructor(private logService: LogService) { }

  ngOnInit(): void {
    this.logService.logRoute('Home Page');
  }
}

nano help.component.ts 
import { Component, OnInit } from '@angular/core';
import { LogService } from '../log.service';

@Component({
  selector: 'app-help',
  templateUrl: './help.component.html',
  styleUrls: ['./help.component.css']
})
export class HelpComponent implements OnInit {
  constructor(private logService: LogService) { }

  ngOnInit(): void {
    this.logService.logRoute('Help Page');
  }
}

#Configurer HttpClientModule dans app.module.ts :
nano app.module.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { HomeComponent } from './home/home.component';
import { HelpComponent } from './help/help.component';
import { HttpClientModule } from '@angular/common/http';

@NgModule({
  declarations: [
    AppComponent,
    HomeComponent,
    HelpComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    HttpClientModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }

#pour la journalisation:j'ai utilise une bibliothèque de journalisation winston :

npm install winston
# Lancer le backend et le frontend
#Lancer le backend :

node angular-server.js &

#Lancer le frontend :
ng serve



