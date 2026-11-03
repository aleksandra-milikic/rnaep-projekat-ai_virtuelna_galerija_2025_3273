# AI Virtual Gallery

A web-based virtual art gallery built with **Next.js, React, TypeScript, Prisma, and PostgreSQL**. The application provides a digital space where users can explore artworks, search the collection, save favorite pieces, and receive personalized artwork recommendations. It also includes role-based content management, allowing curators and administrators to manage the gallery's artwork collection.

## Features

### User Authentication and Roles
- User registration and login.
- Password hashing using **bcryptjs**.
- Authentication using **JSON Web Tokens (JWT)**.
- Three user roles:
  - `USER` – browse artworks, manage favorites, and view personalized recommendations.
  - `CURATOR` – add and edit artworks.
  - `ADMIN` – manage the artwork collection, including deleting artworks.

### Virtual Art Gallery
- Browse artworks stored in the database.
- Paginated artwork loading, displaying 15 artworks per page.
- Search artworks by title and description.
- View artwork images, titles, descriptions, and available information.
- Open artwork images in an enlarged overlay.

### Favorites
- Add artworks to a personal favorites collection.
- Remove artworks from favorites.
- View all saved artworks on a dedicated Favorites page.
- Favorites are associated with individual user accounts.

### Personalized Recommendations
- Generate artwork recommendations based on the tags of a user's favorite artworks.
- Analyze the tags associated with favorited pieces and calculate scores for other artworks based on matching tags.
- Exclude artworks that the user has already favorited.
- Rank recommendations by their calculated scores and return up to 10 results.

This provides a simple content-based recommendation approach that helps users discover artworks related to their interests.

### Artwork Management
- Curators can add new artworks and edit existing entries.
- Administrators can delete artworks.
- Artwork records support the following information:
  - Title
  - Description
  - Image URL
  - Artist
  - Year
  - Category
  - Tags
  - Associated gallery

## Technology Stack

| Technology | Purpose |
|---|---|
| Next.js 16 | Full-stack React framework and application routing |
| React 19 | User interface |
| TypeScript | Static typing |
| Tailwind CSS 4 | Styling and responsive layouts |
| PostgreSQL | Relational database |
| Prisma ORM | Database schema and data access |
| JWT (jsonwebtoken) | Token-based authentication |
| bcryptjs | Password hashing |
| React Hot Toast | Application notifications |
| Docker Compose | Local PostgreSQL environment |

## Database Structure

The application uses **PostgreSQL** with **Prisma ORM** to manage its relational data.

The database consists of five main models:

- **User** – stores user information, credentials, assigned role, and account creation date.
- **Artwork** – stores artwork details, image URLs, metadata, tags, and relationships with users, galleries, and exhibitions.
- **Gallery** – represents a gallery and its associated artworks.
- **Exhibition** – stores exhibition information and its active status.
- **Favorite** – connects users with the artworks they have saved as favorites.

The database also defines a `Role` enum with three values: `USER`, `CURATOR`, and `ADMIN`.

Relationships between models allow artworks to be associated with their creators, galleries, and exhibitions, while favorites are linked to both users and artworks.

## API Overview

The application uses Next.js Route Handlers to provide server-side API functionality.

| Endpoint | Method | Description |
|---|---|---|
| `/api/auth/register` | POST | Registers a new user |
| `/api/auth/login` | POST | Authenticates a user and returns a JWT |
| `/api/auth/logout` | POST | Returns a logout confirmation |
| `/api/auth/me` | GET | Retrieves the authenticated user's basic information |
| `/api/artworks` | GET | Retrieves artworks with pagination |
| `/api/artworks` | POST | Creates a new artwork (Curator/Admin) |
| `/api/artworks/[id]` | GET | Retrieves an artwork by ID |
| `/api/artworks/[id]` | PUT | Updates an artwork (Curator/Admin) |
| `/api/artworks/[id]` | DELETE | Deletes an artwork (Admin only) |
| `/api/favorites` | GET | Retrieves the authenticated user's favorites |
| `/api/favorites` | POST | Adds or removes an artwork from favorites |
| `/api/recommendations` | GET | Retrieves personalized artwork recommendations |

Protected API operations use JWT authentication, with role checks applied to artwork management operations.

## Project Structure

```text
.
├── prisma/
│   ├── migrations/
│   └── schema.prisma
├── public/
├── src/
│   ├── app/
│   │   ├── api/
│   │   │   ├── auth/
│   │   │   ├── artworks/
│   │   │   ├── favorites/
│   │   │   └── recommendations/
│   │   ├── dashboard/
│   │   ├── favorites/
│   │   ├── gallery/
│   │   ├── login/
│   │   ├── recommendations/
│   │   ├── register/
│   │   ├── layout.tsx
│   │   ├── page.tsx
│   │   └── global.css
│   ├── components/
│   └── lib/
│       ├── auth.ts
│       └── prisma.ts
├── docker-compose.yml
├── package.json
├── prisma.config.ts
└── tsconfig.json
```

## Getting Started

Follow these steps to run the application locally.

### Prerequisites

Make sure the following tools are installed:

- [Node.js](https://nodejs.org/)
- [npm](https://www.npmjs.com/)
- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- [Git](https://git-scm.com/)

### 1. Clone the Repository

```bash
git clone <repository-url>
cd RNAEP-PROJEKAT-AI_VIRTUELNA_GALERIJA_2025_3273
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Configure Environment Variables

Create a `.env` file in the project root directory:

```env
DATABASE_URL="postgresql://postgres:postgres@localhost:5432/ivg_database?schema=public"
JWT_SECRET="your-secret-key"
```

Replace `your-secret-key` with a secure secret of your own.

### 4. Start the PostgreSQL Database

The project includes a Docker Compose configuration for PostgreSQL 15.

Start the database container:

```bash
docker compose up -d
```

### 5. Apply Database Migrations

Run the Prisma migrations to create the database tables:

```bash
npx prisma migrate deploy
```

Generate the Prisma Client:

```bash
npx prisma generate
```

### 6. Start the Development Server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser to access the application.

## Available Scripts

| Command | Description |
|---|---|
| `npm run dev` | Starts the development server |
| `npm run build` | Builds the application for production |
| `npm run start` | Starts the production server |
| `npm run lint` | Runs ESLint |

## Notes

- The application uses JWT-based authentication and role-based access checks.
- Artwork images are referenced through image URLs. Wikimedia Commons is configured as an allowed remote image source in the Next.js image configuration.
- The recommendation functionality uses artwork tags and favorite selections to calculate relevance scores.
- PostgreSQL data is persisted through a Docker volume configured in `docker-compose.yml`.

## License

No license has been specified for this project.
