`````md path="README.md"
# DeepSeek Chat Application

A full‑stack **Next.js** application that provides a simple chat interface powered by the **DeepSeek** language model.  
The project demonstrates how to integrate:

* **Next.js 15** (App Router)  
* **Clerk** for authentication  
* **MongoDB** (via Mongoose) for persistent chat storage  
* **OpenAI SDK** (pointed at DeepSeek's API) for AI completions  
* A clean UI built with **TailwindCSS**, **React‑Hot‑Toast**, and **PrismJS** for syntax highlighting.

---

## Table of Contents

- [Features](#features)  
- [Prerequisites](#prerequisites)  
- [Installation](#installation)  
- [Environment Variables](#environment-variables)  
- [Running the App](#running-the-app)  
- [API Endpoints](#api-endpoints)  
- [Project Structure](#project-structure)  
- [Deployment](#deployment)  
- [License](#license)

---

## Features

- **User authentication** via Clerk (sign‑in, sign‑up, and session management).  
- **Create / Read / Update / Delete** chats stored in MongoDB.  
- **Real‑time AI responses** from DeepSeek (`deepseek-chat` model).  
- **Chat renaming** and **message persistence** with timestamps.  
- **Responsive UI** with a collapsible sidebar, toast notifications, and syntax‑highlighted code blocks.  

---

## Prerequisites

| Tool | Minimum Version |
|------|-----------------|
| Node.js | v20.x |
| npm / yarn / pnpm / bun | any |
| MongoDB | URI to a reachable instance |
| Clerk account | for authentication keys |
| DeepSeek API key | for AI completions |

---

## Installation

```bash
# Clone the repository
git clone https://github.com/im-Vengeance0/deepseek.git
cd deepseek

# Install dependencies (choose your package manager)
npm install   # or yarn install, pnpm install, bun install
```

---

## Environment Variables

Create a `.env.local` file in the project root (it is ignored by git) and add the following keys:

```dotenv
# MongoDB connection string
MONGODB_URI=mongodb+srv://<username>:<password>@cluster0.mongodb.net/deepseek?retryWrites=true&w=majority

# Clerk keys (public & secret)
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
CLERK_SECRET_KEY=your_clerk_secret_key
SIGNING_SECRET=your_svix_signing_secret   # for webhook verification

# DeepSeek (OpenAI SDK) API key
DEEPSEEK_API_KEY=your_deepseek_api_key
```

> **Note:** `SIGNING_SECRET` is used by the Clerk webhook route (`/api/clerk/route.js`) to verify incoming events.

---

## Running the App

```bash
# Development mode (TurboPack enabled)
npm run dev   # or yarn dev / pnpm dev / bun dev

# Build for production
npm run build

# Start the production server
npm start
```

Visit **http://localhost:3000** in your browser. After signing in with Clerk, the app will automatically create a default chat if none exist.

---

## API Endpoints

All API routes live under `app/api/chat/*` and expect a **Bearer** token (Clerk JWT) in the `Authorization` header.

| Method | Path                     | Description                                   |
|--------|--------------------------|-----------------------------------------------|
| `POST` | `/api/chat/create`       | Creates a new chat (`name: "New Chat"`).      |
| `POST` | `/api/chat/get`          | Retrieves all chats for the authenticated user. |
| `POST` | `/api/chat/delete`       | Deletes a chat (`{ chatId }`).                |
| `POST` | `/api/chat/rename`       | Renames a chat (`{ chatId, name }`).           |
| `POST` | `/api/chat/ai`           | Sends a user prompt to DeepSeek and stores the AI reply. (`{ chatId, prompt }`). |
| `POST` | `/api/clerk/route`       | Clerk webhook handler (user create/update/delete). |

All responses follow the shape:

```json
{
  "success": true | false,
  "data": <payload> | null,
  "message": "<optional message>",
  "error": "<error message if any>"
}
```

---

## Project Structure

```
├── .gitignore
├── README.md               ← You are here
├── app
│   ├── api
│   │   ├── chat
│   │   │   ├── ai        → DeepSeek completion handler
│   │   │   ├── create    → New chat creator
│   │   │   ├── delete    → Chat remover
│   │   │   ├── get       → Fetch user chats
│   │   │   └── rename    → Update chat name
│   │   └── clerk          → Clerk webhook
│   ├── favicon.ico
│   ├── globals.css
│   ├── layout.tsx          → Root layout with ClerkProvider & context
│   ├── page.jsx            → Main entry page
│   └── prism.css
├── assets                  → SVG/PNG assets used by UI
├── components
│   ├── ChatLabel.jsx
│   ├── Message.jsx
│   ├── PromptBox.jsx
│   └── Sidebar.jsx
├── config
│   └── db.js              → Mongoose connection (caching)
├── context
│   └── AppContext.jsx     → Global state (chats, selectedChat, CRUD helpers)
├── middleware.ts          → Clerk middleware + route matcher
├── models
│   ├── Chat.js            → Chat schema (name, messages, userId)
│   └── User.js            → User schema (mirrors Clerk data)
├── next.config.ts
├── package.json
├── postcss.config.mjs
├── public                  → Static assets
└── tsconfig.json
```

---

## Deployment

The app is ready for deployment on **Vercel** (or any platform supporting Next.js).  
When deploying, ensure that all environment variables listed above are added to the platform’s environment configuration.

**Vercel CLI quick deploy:**

```bash
npm i -g vercel
vercel login
vercel
```

Vercel will automatically detect the Next.js project and set up the build step (`npm run build`).

---

## License

This project is released under the **HR**. Feel free to fork, modify, and use it in your own applications.

---

## Contributing

1. Fork the repository.  
2. Create a feature branch (`git checkout -b feature/awesome-feature`).  
3. Commit your changes and push to your fork.  
4. Open a Pull Request describing the changes.

Contributions are welcome! 🎉
`````
