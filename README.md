# CodeWeave

CodeWeave is a collaborative coding workspace that runs in the browser. It combines a React IDE frontend with a Node.js API and Socket.IO server for accounts, projects, collaboration, and AI-assisted coding.

## Features

- Edit project files with Monaco, a navigable file tree, syntax highlighting, and file operations.
- Run supported Node.js projects in the browser using WebContainers, with an integrated terminal and preview.
- Collaborate in shared projects with real-time messages and typing indicators.
- Ask the AI assistant to chat about code, build a project, or modify existing files using the current project file tree as context.
- Configure provider keys and models for Groq, Google Gemini, or Hugging Face in the IDE.
- Manage accounts, project invitations, and user preferences.

## Technology

| Area | Technologies |
| --- | --- |
| Frontend | React 18, Vite, React Router, Tailwind CSS, Monaco Editor |
| Backend | Node.js 18+, Express, Socket.IO |
| Persistence | MongoDB with Mongoose |
| Authentication | JWT and bcryptjs |
| AI providers | Groq, Google Gemini, Hugging Face Inference API |
| In-browser execution | WebContainer API |

## Requirements

- Node.js 18 or newer and npm
- MongoDB, either locally or through MongoDB Atlas
- An API key for an AI provider to use AI features

## Run locally

Clone the repository:

```sh
git clone https://github.com/heykunalkumar/code-weave.git
cd code-weave
```

### Start the backend

In a terminal, install the backend dependencies:

```sh
cd backend
npm install
```

Create `backend/.env` with your MongoDB connection string and a strong JWT secret:

```env
PORT=3000
MONGODB_URI=mongodb://127.0.0.1:27017/codeweave
JWT_SECRET=replace_with_a_long_random_secret
```

For MongoDB Atlas, use your Atlas connection string for `MONGODB_URI`. Start the API and Socket.IO server:

```sh
npm run dev
```

The backend listens on port `3000` by default. Set `PORT` to use a different port.

### Start the frontend

In a second terminal, install the frontend dependencies:

```sh
cd frontend
npm install
```

Create `frontend/.env` and point it at the backend:

```env
VITE_API_URL=http://localhost:3000
```

Start Vite:

```sh
npm run dev
```

Open the local URL printed by Vite, normally `http://localhost:5173`. Keep both development servers running while using the app.

### Configure AI

AI provider credentials are configured in the application and sent with AI requests. Choose a supported provider, enter its API key and model, then use the assistant from a project workspace. Do not put provider secrets in `VITE_` variables: frontend environment values are included in browser code.

## npm scripts

Backend scripts, run from `backend/`:

| Command | Description |
| --- | --- |
| `npm run dev` | Start the backend with nodemon. |
| `npm start` | Start the backend with Node.js. |

Frontend scripts, run from `frontend/`:

| Command | Description |
| --- | --- |
| `npm run dev` | Start the Vite development server. |
| `npm run build` | Build the frontend into `frontend/dist/`. |
| `npm run preview` | Preview a production build locally. |
| `npm run lint` | Run ESLint. |

## Application structure

```text
backend/
  controllers/  HTTP request handlers
  db/           MongoDB connection
  middleware/   Authentication and uploads
  models/       Mongoose models
  routes/       User, project, and AI API routes
  services/     AI, code execution, project, and user services
  server.js     HTTP and Socket.IO server entry point
frontend/
  src/
    auth/       Protected-route authentication
    components/ Shared UI components
    config/     API, Socket.IO, and WebContainer clients
    context/    User state
    routes/     Application routes
    screens/    Landing, account, dashboard, and IDE screens
```

## Notes

- The frontend API URL is controlled by `VITE_API_URL`; Vite loads environment variables from the frontend directory.
- Vite's development server sets the cross-origin isolation headers needed by WebContainers.
- Keep `.env` files and private keys out of version control. Use `backend/.env.example` as the backend variable reference.

