# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

AI Digital Twin application with conversational memory - a full-stack chatbot that maintains conversation history across sessions. The backend serves as a personality-driven AI assistant using OpenAI's API, while the frontend provides a clean chat interface.

## Architecture

**Backend (FastAPI + Python)**
- `backend/server.py`: Main FastAPI application with chat endpoint, session management, and memory persistence
- `backend/me.txt`: Personality configuration file that defines the AI's role and behavior
- `backend/.env`: Environment variables (CORS_ORIGINS, OPENAI_API_KEY)
- Conversation history stored as JSON files in `memory/` directory (one file per session_id)

**Frontend (Next.js + React + TypeScript)**
- `frontend/app/page.tsx`: Main page that renders the Twin component
- `frontend/components/twin.tsx`: Chat UI component with message handling, session management, and loading states
- Uses Tailwind CSS v4 for styling, lucide-react for icons
- Client-side React component that maintains local message state and communicates with backend API

**Memory System**
- Each conversation session generates a UUID and stores messages in `../memory/{session_id}.json`
- Messages include role (user/assistant) and content
- Backend loads full conversation history on each request to provide context to OpenAI

## Development Commands

**Backend**
```bash
cd backend
# Install dependencies (uses uv)
uv sync
# Run development server
uv run uvicorn server:app --reload --host 0.0.0.0 --port 8000
# Or using Python directly
python server.py
```

**Frontend**
```bash
cd frontend
# Install dependencies
npm install
# Run development server (http://localhost:3000)
npm run dev
# Build for production
npm run build
# Start production server
npm start
# Run linter
npm run lint
```

## Key Implementation Details

- Backend expects `OPENAI_API_KEY` in environment variables
- CORS configured to allow requests from `http://localhost:3000` by default (override with CORS_ORIGINS env var)
- Chat API endpoint: `POST /chat` with JSON body `{message: string, session_id?: string}`
- Frontend hardcoded to connect to `http://localhost:8000/chat`
- Uses `gpt-4o-mini` model for chat completions
- System prompt loaded from `me.txt` defines the AI personality as a Digital Twin representing "Yl"

## Environment Setup

Backend requires:
- Python >=3.13
- `OPENAI_API_KEY` environment variable
- Optional: `CORS_ORIGINS` (comma-separated URLs)

Frontend requires:
- Node.js (with npm/pnpm)
- No environment variables needed (API URL hardcoded)
