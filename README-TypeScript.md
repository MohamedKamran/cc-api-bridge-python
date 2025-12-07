# Claude Code API Gateway - TypeScript Implementation

This is the TypeScript/Node.js implementation of the Claude Code API Gateway, providing the same OpenAI-compatible API as the Python version.

## Features

- **TypeScript** - Fully type-safe implementation with strict mode
- **Express.js** - Fast, minimalist web framework
- **Zod** - Runtime type validation for API requests
- **Better-SQLite3** - Synchronous SQLite database access
- **Pino** - High-performance JSON logging
- **Server-Sent Events** - Streaming support for real-time responses
- **OpenAI Compatible** - Drop-in replacement for OpenAI API endpoints

## Quick Start

### Prerequisites

- Node.js 18+ 
- npm or yarn
- Claude Code CLI installed and accessible

### Installation

```bash
# Install dependencies
make install-ts
# or
npm install
```

### Build

```bash
# Build TypeScript to JavaScript
make build-ts
# or
npm run build
```

### Run

```bash
# Production mode
make start-ts
# or
npm start

# Development mode with auto-reload
make start-ts-dev
# or
npm run dev
```

The API will be available at:
- **API**: http://localhost:8000
- **Health**: http://localhost:8000/health

## Project Structure

```
src/
├── api/                    # API route handlers
│   ├── chat.ts            # Chat completions endpoint
│   └── models.ts          # Models listing endpoint
├── core/                   # Core functionality
│   ├── config.ts          # Configuration management
│   ├── database.ts        # SQLite database layer
│   └── claude-manager.ts  # Claude process management
├── models/                 # Data models and schemas
│   ├── openai.ts          # OpenAI-compatible models (Zod)
│   └── claude.ts          # Claude-specific models
├── utils/                  # Utility functions
│   ├── logger.ts          # Pino logging configuration
│   ├── parser.ts          # JSONL output parser
│   └── streaming.ts       # SSE streaming utilities
└── main.ts                # Main application entry point
```

## Configuration

Configuration is loaded from environment variables. Create a `.env` file:

```bash
# Server Configuration
HOST=0.0.0.0
PORT=8000
DEBUG=false

# Claude Configuration
CLAUDE_BINARY_PATH=/path/to/claude
DEFAULT_MODEL=claude-3-5-haiku-20241022
MAX_CONCURRENT_SESSIONS=10
SESSION_TIMEOUT_MINUTES=30

# Project Configuration
PROJECT_ROOT=/tmp/claude_projects
MAX_PROJECT_SIZE_MB=1000

# Database Configuration
DATABASE_URL=./claude_api.db

# Logging Configuration
LOG_LEVEL=INFO
LOG_FORMAT=json

# CORS Configuration
ALLOWED_ORIGINS=*
ALLOWED_METHODS=*
ALLOWED_HEADERS=*

# Rate Limiting
RATE_LIMIT_REQUESTS_PER_MINUTE=100
RATE_LIMIT_BURST=10

# Streaming Configuration
STREAMING_CHUNK_SIZE=1024
STREAMING_TIMEOUT_SECONDS=300
```

## API Usage

### Chat Completions

```bash
curl -X POST http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "claude-3-5-haiku-20241022",
    "messages": [
      {"role": "user", "content": "Hello!"}
    ]
  }'
```

### Streaming Chat

```bash
curl -X POST http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "claude-3-5-haiku-20241022",
    "messages": [
      {"role": "user", "content": "Tell me a joke"}
    ],
    "stream": true
  }'
```

### List Models

```bash
curl http://localhost:8000/v1/models
```

## Development

### Linting

```bash
make lint-ts
# or
npm run lint
```

### Formatting

```bash
make format-ts
# or
npm run format
```

### Clean Build Artifacts

```bash
make clean-ts
# or
rm -rf dist node_modules
```

## Supported Models

- `claude-opus-4-20250514` - Claude Opus 4 (Most powerful)
- `claude-sonnet-4-20250514` - Claude Sonnet 4 (Latest Sonnet)
- `claude-3-7-sonnet-20250219` - Claude Sonnet 3.7 (Advanced)
- `claude-3-5-haiku-20241022` - Claude Haiku 3.5 (Fast & cost-effective)

## Differences from Python Implementation

While the TypeScript implementation provides the same API and functionality as the Python version, there are some technical differences:

- **Type System**: Uses Zod for runtime validation instead of Pydantic
- **Database**: Uses better-sqlite3 (synchronous) instead of aiosqlite (async)
- **Web Framework**: Uses Express instead of FastAPI
- **Logging**: Uses Pino instead of structlog

## Troubleshooting

### Claude binary not found

If you see "Claude binary not found", ensure:
1. Claude Code CLI is installed: `npm install -g @anthropic-ai/claude-code`
2. Claude is in your PATH or set `CLAUDE_BINARY_PATH` environment variable

### Port already in use

Kill the process using the port:
```bash
make kill PORT=8000
```

### Database locked

If you encounter database lock issues:
1. Ensure only one instance is running
2. Delete the database file and restart: `rm claude_api.db`

## License

This project is licensed under the GNU General Public License v3.0 - see the LICENSE file for details.
