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

- Geolocalizacion al lugar solicitado.
- Formulario con PDF para la hoja de seguimiento.
- Enviar notificaciones de estado a clients(Solicitud aceptada, contenedor recojido, residuo gestionado(con datos del servicio), ...)

<br>


# Client / Frontend

## Routes (React App)
| Path                      | Component            | Permissions | Behavior                                                     |
| ------------------------- | -------------------- | ----------- | ------------------------------------------------------------ |
| `/auth/login`                       | LoginPage           | public      | In this page the users can login     |
|    Admin                       |           |       |                                        |
| `/admin`             | Admin HomePage    | admin only   | Is the admin home page, here are the list of rquests |
| `/admin/auth/signup`            | SignupPage           | admin only | Admin can add a new Client User |
| `/admin/clientslist`             | Clients List Page | admin only   | Admin can see all the clients |
| Client            |    |   |                               |
| `/client`        | HomePage   | client only   |  Can see all current containers  |
| `/client/request`        | Request Container Page | client only   | Request a new container |
| `/container/:id`         | Container page  | client only   | Can edit container features and request pick up |
| `/client/profile`     | Client Profile Page      | client only   | Hisotry of containers and options to edit|
| `/client/profiles/edit` | Edit Client Page      | client only   | Modify data  |
| Transporter |     |       |                                           |
| `/transporter` | Transportes Home Page      | transporter only   |  See all the pending tasks |
| `/transporter/profile`   | Transportes Profile Page            | transporter only   | All the containers served and active  |


## Components

- LoginPage

- Admin Homepage

- Admin List Clients

- Admin Signup client

- Client Homepage

- Client New Rquest

- Container Details

- Client Profile

- Client Edit

- Transporter Homepage

- Transporter Profile

- Navbar
 

## Services

- Auth
  - auth.login
  - auth.signup
  - auth.logout
  - auth.me

- Admin
  - admin.acceptRequest
  - admin.list
  - admin.detailContainer
  - admin.detailClient
  - admin.detailTransporter

- Client
  - client.newContainer
  - client.requestPickUp
  - client.edirProfile

- Transporter
  - transporter.taskList
  - transporter.deliveredContainer
  - transporter.takenContainer

- Container
  - container.edit
  - container.archived

<br>


# Server / Backend


## Models

Admin model

```javascript
{
  username: {type: String, required: true, unique: true},
  password: {type: String, required: true},
  clients: [{type: ObjectId}],
  containersRequest: [{type: ObjectId}],
  archivedContainers: [{type: ObjectId}],
}
```



Client model

```javascript
 {
   name: {type: String, required: true},
   password: {type: String, required: true},
   img: {type: String},
   email: {type: String, required: true},
   telef: {type: String, required: true},
   adress: {type: String, required: true},...
   activeContainers: [{type: ObjectId, required: true}],
   archivedContainers: [{type: ObjectId, required: true}],
   activeContainers: [{type: ObjectId, required: true}],
 }
```



Transporter model

```javascript
{
   name: {type: String, required: true},
   password: {type: String, required: true},
   img: {type: String},
   email: {type: String, required: true},
   telef: {type: String, required: true},
   adress: {type: String, required: true},...
   requestedServices: [{type: ObjectId, required: true}],
   activeContainers: [{type: ObjectId, required: true}],
   archivedContainers: [{type: ObjectId, required: true}],
}
```



Container model

```javascript
{
  name: {type: String, required: true},
  place: {type: String, required: true},
  content: {type: String, required: true},
  capacity: {type: String, required: true},
  img: {type: String, required: true},
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
