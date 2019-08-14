# Monreser App

<br>

## Descripción

Esta es una aplicacion orientada a la gestion de contenedores. Los clientes pueden solicitar un contenedor o bien su recojida al administrador, este lo gestiona y le encarga el trabajo a la empresa de transportes.

## User Stories

  ### All Users
-  **404:** Como usuario veo la pagina 404 si intento acceder a un sitio que no existe
-  **Login** Como usuario puedo entrar en la plataforma, independientemente del tipo
-  **Logout:** Como usuario puedo cerrar mi sesion en la plataforma

  ### Admin User
-  **Signup:** Como administrador puedo crear el perfil para un nuevo cliente
-  **Aceptar solicitud contenedor** Como administrador puedo aceptar una solicitud para nuevo contenedor y encargar el transporte a un usuario transportista.
-  **Ver los contenedores actuales de cada cliente** Como administrador puedo ver el historial de contenedores de cada cliente y los que tiene actualmente, y hacer en ellos cualquier tipo de modificacion
-  ****

  ### Client User
-  **Solicitar contenedor** Como cliente puedo solicitar un nuevo contenedor.
-  **Solicitar recojida**  Como cliente puedo solicitar la recojida de un contenedor.
-  **Ver mi lista de contenedores actuales** Como cliente puedo ver en mi pagina principal la lista de contenedores que tengo actualmente y poder solicitar des de alli su recojida y adjuntar la documentacion necesaria.
-  **Ver mi historial de contenedores** Como cliente puedo ver todos los servicios de contenedores que he contratado y ver sus detalles.
-  **Modificar mis datos** Como usuario puedo modificar mi contraseña y algun dato.

  ### Transporter User
-  **Ver los contenedores que tengo contratados** Como transportista puedo ver una lista de contenedores que tengo contratados.
-  **Lista de tareas** Como transportista puedo ver una lista de tareas a hacer (llevar o recojer contenedor).
-  **Contacto con el cliente** Como transportista puedo ver los detalles del contenedor y el cliente al cual se ha servido, y poder acceder a sus datos de conacto.

## Backlog

-Geolocalizacion al lugar solicitado.
-Formulario con PDF para la hoja de seguimiento.
-Enviar notificaciones de estado a clients(Solicitud aceptada, contenedor recojido, residuo gestionado(con datos del servicio), ...)

<br>


# Client / Frontend

## Routes (React App)
| Path                      | Component            | Permissions | Behavior                                                     |
| ------------------------- | -------------------- | ----------- | ------------------------------------------------------------ |
| `/`                       | SplashPage           | public      | Home page                                        |
| `/auth/signup`            | SignupPage           | anon only   | Signup form, link to login, navigate to homepage after signup |
| `/auth/login`             | LoginPage            | anon only   | Login form, link to signup, navigate to homepage after login |
| `/auth/logout`            | n/a                  | anon only   | Navigate to homepage after logout, expire session            |
| `/tournaments`            | TournamentListPage   | user only   | Shows all tournaments in a list                              |
| `/tournaments/add`        | TournamentListPage   | user only   | Edits a tournament                                           |
| `/tournaments/:id`        | TournamentDetailPage | user only   | Details of a tournament to edit                              |
| `/tournament/:id`         | n/a                   | user only   | Delete tournament                                            |
| `/tournament/players`     | PlayersListPage      | user only   | List of players of a tournament                              |
| `/tournament/players/add` | PlayersListPage      | user only   | Add a player to the tournament                               |
| `/tournament/players/:id` | PlayersDetailPage    | user only   | Edit player for tournament                                   |
| `/tournament/players/:id` | PlayersListPage      | user only   | Delete player from tournament                                |
| `/tournament/tableview`   | TableView            | user only   | Games view and brackets                                      |
| `/tournament/ranks`       | RanksPage            | user only   | Ranks list                                                   |
| `/tournament/game`        | GameDetailPage       | user only   | Game details                                                  |
| `/tournament/game`        | Game                 | user only   |                                                              |


## Components

- LoginPage

- SplashPage

- TournamentListPage

- Tournament Cell

- TournamentDetailPage

- TableViewPage

- PlayersListPage

- PlayerDetailPage

- RanksPage

- TournamentDetailPageOutput

- Navbar


  

 

## Services

- Auth Service
  - auth.login(user)
  - auth.signup(user)
  - auth.logout()
  - auth.me()
  - auth.getUser() // synchronous
- Tournament Service
  - tournament.list()
  - tournament.detail(id)
  - tournament.add(id)
  - tournament.delete(id)
  
- Player Service 

  - player.detail(id)
  - player.add(id)
  - player.delete(id)

- Game Service

  - Game.put(id)



<br>


# Server / Backend


## Models

User model

```javascript
{
  username: {type: String, required: true, unique: true},
  email: {type: String, required: true, unique: true},
  password: {type: String, required: true},
  favorites: [Tournament]
}
```



Tournament model

```javascript
 {
   name: {type: String, required: true},
   img: {type: String},
   players: [{type: Schema.Types.ObjectId,ref:'Participant'}],
   games: [{type: Schema.Types.ObjectId,ref:'Game'}]
 }
```



Player model

```javascript
{
  name: {type: String, required: true},
  img: {type: String},
  score: []
}
```



Game model

```javascript
{
  player1: [{type: Schema.Types.ObjectId,ref:'Participant'}],
  player2: [{type: Schema.Types.ObjectId,ref:'Player'}],
  player2: [{type: Schema.Types.ObjectId,ref:'Player'}],
  winner: {type: String},
  img: {type: String}
}
```


<br>


## API Endpoints (backend routes)

| HTTP Method | URL                         | Request Body                 | Success status | Error Status | Description                                                  |
| ----------- | --------------------------- | ---------------------------- | -------------- | ------------ | ------------------------------------------------------------ |
| GET         | `/auth/profile    `           | Saved session                | 200            | 404          | Check if user is logged in and return profile page           |
| POST        | `/auth/signup`                | {name, email, password}      | 201            | 404          | Checks if fields not empty (422) and user not exists (409), then create user with encrypted password, and store user in session |
| POST        | `/auth/login`                 | {username, password}         | 200            | 401          | Checks if fields not empty (422), if user exists (404), and if password matches (404), then stores user in session |
| POST        | `/auth/logout`                | (empty)                      | 204            | 400          | Logs out the user                                            |
| GET         | `/tournaments`                |                              |                | 400          | Show all tournaments                                         |
| GET         | `/tournaments/:id`            | {id}                         |                |              | Show specific tournament                                     |
| POST        | `/tournaments/add-tournament` | {}                           | 201            | 400          | Create and save a new tournament                             |
| PUT         | `/tournaments/edit/:id`       | {name,img,players}           | 200            | 400          | edit tournament                                              |
| DELETE      | `/tournaments/delete/:id`     | {id}                         | 201            | 400          | delete tournament                                            |
| GET         | `/players`                    |                              |                | 400          | show players                                                 |
| GET         | `/players/:id`                | {id}                         |                |              | show specific player                                         |
| POST        | `/players/add-player`         | {name,img,tournamentId}      | 200            | 404          | add player                                                   |
| PUT         | `/players/edit/:id`           | {name,img}                   | 201            | 400          | edit player                                                  |
| DELETE      | `/players/delete/:id`         | {id}                         | 200            | 400          | delete player                                                |
| GET         | `/games`                      | {}                           | 201            | 400          | show games                                                   |
| GET         | `/games/:id`                  | {id,tournamentId}            |                |              | show specific game                                           |
| POST        | `/games/add-game`             | {player1,player2,winner,img} |                |              | add game                                                     |
| POST        | `/games/add-all-games`        |                              |                |              | add all games from a tournament. Gets a list of players and populates them via algorithm. |
| PUT         | `/games/edit/:id`             | {winner,score}               |                |              | edit game                                                    |


<br>


## Links

### Trello/Kanban

[Link to your trello board](https://trello.com/b/PBqtkUFX/curasan) 
or picture of your physical board

### Git

The url to your repository and to your deployed project

[Client repository Link](https://github.com/screeeen/project-client)

[Server repository Link](https://github.com/screeeen/project-server)

[Deployed App Link](http://heroku.com)

### Slides

The url to your presentation slides

[Slides Link](http://slides.com)
