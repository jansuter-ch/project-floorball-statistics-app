# Floorball Statistics

Web application for browsing Swiss floorball league statistics, built with Java, Spring Boot, and Budibase as part of the FHNW Internet Technology module.

---

## Contents

- [Analysis](#analysis)
  - [Scenario](#scenario)
  - [User Stories](#user-stories)
  - [Use Cases](#use-cases)
- [Design](#design)
  - [Wireframe](#wireframe)
  - [Domain Design](#domain-design)
  - [Business Logic](#business-logic)
- [Implementation](#implementation)
  - [Backend Technology](#backend-technology)
  - [Frontend Technology](#frontend-technology)
- [Execution](#execution)
- [Project Management](#project-management)
  - [Roles](#roles)
  - [Milestones](#milestones)

---

## Analysis

### Scenario

The Floorball Statistics project displays key information about the Swiss floorball league. The main goal is to present relevant stats for teams and players, giving coaches and analysts the data they need to prepare for upcoming games. The application offers filtered and ranked views of player and team performance, with full data editing capabilities for administrators.

### User Stories

- As a user, I want to view player stats and see how players rank against each other.
- As a user, I want to see teams and their standings in the league.
- As a user, I want to filter and sort data to find what I need quickly.
- As an admin, I want to create, edit, and delete player and team data.
- As an admin, I want a structured and easy-to-follow layout.
- As an admin, I want the application to be primarily accessible via desktop.

### Use Cases

Three use case diagrams cover the main views of the system:

**Use Case 100: Home page**

![UC-100](UC-100.jpg)

**Use Case 200: Player stats page**

![UC-200](UC-200.jpg)

**Use Case 300: League rankings page**

![UC-300](UC-300.jpg)

---

## Design

### Wireframe

The wireframe defines the structure and layout of the three main views: Home, Player Stats, and League Rankings. Each view outlines the major UI elements including filters, search bar, data tables, and navigation bar.

![Wireframe](Wireframe.png)

### Domain Design

The domain model is implemented using JPA entities in the package `ch.fhnw.pizza.data.domain`.

Core entities: Player, Team, League, Ranking.

Note: The Ranking entity was removed during development as Budibase handles sorting and ranking automatically based on total points.

![Domain Design](Domain%20Design.png)

### Business Logic

Business logic is handled in the `ch.fhnw.pizza.business.service` package. Each service class manages CRUD operations for its respective entity.

**PlayerService endpoints:**

- `GET /api/players` - returns all players
- `GET /api/players/{id}` - returns one player by ID
- `POST /api/players` - creates a new player
- `PUT /api/players/{id}` - updates an existing player (only provided fields are updated)
- `DELETE /api/players/{id}` - deletes a player by ID

**TeamService endpoints:**

- `GET /api/teams` - returns all teams
- `GET /api/teams/{id}` - returns one team by ID
- `POST /api/teams` - creates a new team
- `PUT /api/teams/{id}` - updates an existing team
- `DELETE /api/teams/{id}` - deletes a team by ID

**Authentication:**

- `GET /user` - validates credentials via Basic Auth

Swagger UI is available at `/swagger-ui.html` once the backend is running.

---

## Implementation

### Backend Technology

Built with Java 17 and Spring Boot via Spring Initializr.

Key dependencies:

- `spring-boot-starter-web` - REST API via Spring MVC
- `spring-boot-starter-data-jpa` - database integration via JPA
- `spring-boot-starter-security` - Basic Auth security
- `spring-boot-starter-validation` - input validation
- `spring-boot-starter-actuator` - health checks and monitoring
- `h2` (runtime) - in-memory database for development
- `springdoc-openapi-starter-webmvc-ui` - auto-generated Swagger docs
- `spring-boot-starter-test` - JUnit and Mockito for testing

Database config in `application.properties`:

```
spring.datasource.url=jdbc:h2:mem:testdb
spring.h2.console.enabled=true
```

Note: The H2 database is in-memory. Data resets on every application restart. Placeholder data is seeded automatically on startup via `initPlaceholderData()` in `PizzaApplication.java`.

### Frontend Technology

Built with Budibase. The frontend connects to the Spring Boot backend via REST API queries defined in the Budibase datasource.

**Home Screen**
- Displays the top players ranked by total points
- Users can view and sort the table
- API used: `GET /api/players`

**Player Stats Screen**
- Displays all players with full statistics
- Users can sort by any attribute and search by name or last name
- API used: `GET /api/players`

**League Rankings Screen**
- Displays all teams with full standings
- Users can sort by any attribute
- API used: `GET /api/teams`

**Login Screen**
- Admin login using Basic Auth
- Credentials: Username `myadmin`, Password `1234567`
- API used: `GET /user`

**Dashboard Screen** (admin only)
- Full CRUD for players and teams
- APIs used: `POST`, `PUT`, `DELETE` for `/api/players` and `/api/teams`

The Budibase app export (`db.txt`) is included in this repository and can be imported to restore the full frontend including all screens, queries, navigation, and theme.

---

## Execution

### Prerequisites

- GitHub account with access to this repository
- A Budibase account. FHNW students can use [inttech.budibase.app](https://inttech.budibase.app). External users can create a free account at [budibase.com](https://budibase.com).

### Step 1: Start the Backend

1. Open this repository in a GitHub Codespace (Code > Codespaces > Create codespace on main)
2. In the terminal, navigate to the pizza folder: `cd pizza`
3. Run the application: `./mvnw spring-boot:run`
4. Wait for "Started PizzaApplication" in the terminal
5. Go to the Ports tab, find port 8080, right-click and set visibility to Public
6. Copy the public URL (format: `https://[codespace-name]-8080.app.github.dev`)

### Step 2: Import the Budibase App

The full frontend is included in this repository as `db.txt`. Import it to restore all screens, queries, navigation, and theme.

1. Log into your Budibase account
2. On the home portal, create a new workspace
3. Inside the workspace, go to Settings
4. Under General, click "Import workspace"
5. Upload the `db.txt` file from the root of this repository
6. The app appears in the workspace automatically with all screens and datasource configuration in place

Note: The folder `Brugg_4_Floorball_Statistics-export-1749570509041` in this repository is an alternative Budibase export in folder format. Use `db.txt` for the import as it is the standard single-file format.

### Step 3: Connect Budibase to the Backend

1. Inside the imported app, go to Data > API Explorer
2. Select the Webservice-FloorballStatistics datasource from the left panel
3. Click "Edit connection + auth"
4. Update the Base URL to the public Codespace URL copied in Step 1 (no trailing slash)
5. Save and test the following queries to confirm 200 responses:
   - 02 - List all Players
   - 01 - List all Teams
   - 03 - Login

Important: the Codespace URL changes every time a new Codespace is created. Repeat Step 3 whenever you restart with a new Codespace.

### Step 4: Publish and Open the App

1. Click Publish in the top right of Budibase
2. Open the published app via the portal or the link icon next to Publish
3. To test admin access, log in with Username `myadmin` and Password `1234567`

---

## Project Management

### Roles

- Jan Suter: Frontend development (Budibase), API endpoint design, security configuration, testing
- Alessandro Gianoli: Backend development
- Jetmir Sylejmani: Backend development
- Sascha Niederhauser: Backend development

### Milestones

1. Analysis: Scenario ideation, use case analysis, user story writing
2. Prototype Design: Wireframe creation
3. Domain Design: Entity-relationship model definition
4. Business Logic and API Design: Endpoint specification and service layer design
5. Data and API Implementation: Repository and service layer implementation
6. Security and Frontend Implementation: Spring Security integration and Budibase frontend
7. (optional) Deployment: Cloud deployment of the backend

---

## License

Apache 2.0
