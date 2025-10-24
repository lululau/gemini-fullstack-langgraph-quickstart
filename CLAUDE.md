# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Starting Development Environment
- `make dev` - Start both frontend and backend development servers concurrently
- `make dev-frontend` - Start only the frontend development server (Vite) at http://localhost:5173
- `make dev-backend` - Start only the backend development server (LangGraph) at http://127.0.0.1:2024

### Frontend Development
- `cd frontend && npm install` - Install frontend dependencies
- `cd frontend && npm run dev` - Start frontend development server
- `cd frontend && npm run build` - Build production frontend
- `cd frontend && npm run lint` - Run ESLint on frontend code
- `cd frontend && npm run preview` - Preview production build

### Backend Development
- `cd backend && pip install .` - Install backend dependencies
- `cd backend && langgraph dev` - Start backend development server with LangGraph UI
- `cd backend && python examples/cli_research.py "your query"` - Test agent from CLI
- `cd backend && ruff check .` - Run linting on backend code
- `cd backend && ruff format .` - Format backend code

## Architecture Overview

This is a fullstack application with a React frontend and LangGraph-powered backend that demonstrates research-augmented conversational AI using Google's Gemini models.

### Frontend Structure (`frontend/`)
- React application built with Vite and TypeScript
- Uses Tailwind CSS and Shadcn UI components
- Key files:
  - `src/App.tsx` - Main application component with API configuration
  - `src/components/` - UI components for the chat interface
  - `src/hooks/` - Custom React hooks for interacting with the backend

### Backend Structure (`backend/`)
- LangGraph application with FastAPI
- Research agent with iterative web search capabilities
- Key files:
  - `src/agent/graph.py` - Core LangGraph agent implementation with state machine
  - `src/agent/` - Agent modules (search, reflection, answer generation)
  - `src/api/` - API endpoints and server configuration
  - `examples/cli_research.py` - CLI tool for testing the agent

### Agent Workflow
The research agent follows a 5-step process defined in `backend/src/agent/graph.py`:
1. **Generate Initial Queries** - Creates search queries based on user input using Gemini
2. **Web Research** - Executes searches using Google Search API via Gemini
3. **Reflection & Analysis** - Identifies knowledge gaps and determines if more research is needed
4. **Iterative Refinement** - Generates follow-up queries and repeats research (with configurable max loops)
5. **Finalize Answer** - Synthesizes gathered information into a coherent answer with citations

### Configuration
- Backend requires `GEMINI_API_KEY` environment variable
- Create `backend/.env` from `backend/.env.example` and add your Gemini API key
- Frontend API URL is configured in `frontend/src/App.tsx` (defaults to localhost:8123 for production, localhost:2024 for development)

### Deployment
- Docker support with multi-stage builds
- Production setup requires Redis (pub-sub broker) and Postgres (state storage)
- Backend serves optimized frontend build in production
- Use `docker build -t gemini-fullstack-langgraph -f Dockerfile .` from project root
- Production runs on port 8123 (configurable)

### Documentation Guidelines
When generating documentation to help users understand the existing project architecture and code, save the generated documents to the `learnings/docs/` directory.