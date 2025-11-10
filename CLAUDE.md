# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

NOFX is an AI-powered cryptocurrency trading system built with Go backend and React frontend. It supports multiple exchanges (Binance, Hyperliquid, Aster DEX) and AI models (DeepSeek, Qwen) for automated trading with risk management and real-time monitoring.

## Common Development Commands

### Backend Development

```bash
# Build the backend
go build -o nofx

# Run the backend (requires config.json)
./nofx

# Run with custom database path
./nofx custom.db

# Install Go dependencies
go mod download

# Format Go code
go fmt ./...
```

### Frontend Development

```bash
# Navigate to frontend directory
cd web

# Install dependencies
npm install

# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

### Docker Deployment (Recommended)

```bash
# Start all services
./start.sh start

# Build and start
./start.sh start --build

# View logs
./start.sh logs

# Check status
./start.sh status

# Stop services
./start.sh stop

# Restart services
./start.sh restart
```

Manual Docker commands:
```bash
# Start with docker compose
docker compose up -d --build

# Stop services
docker compose down

# View logs
docker compose logs -f
```

## System Architecture

### Core Components

- **Backend (Go)**: Trading engine, API server, database management
- **Frontend (React/TypeScript)**: Web dashboard for monitoring and configuration
- **Database (SQLite)**: Configuration storage, trader management, decision logs
- **AI Integration**: DeepSeek and Qwen API integration for trading decisions
- **Multi-Exchange Support**: Unified interface for Binance, Hyperliquid, and Aster DEX

### Backend Modules

- `main.go`: Application entry point, database initialization, trader loading
- `api/`: REST API server with authentication and configuration endpoints
- `trader/`: Exchange-specific implementations (Binance, Hyperliquid, Aster)
- `manager/`: Trader lifecycle management and coordination
- `market/`: Real-time market data monitoring via WebSocket
- `decision/`: AI decision engine with prompt management
- `config/`: Database operations and configuration management
- `auth/`: JWT-based authentication with 2FA support
- `logger/`: Decision logging and performance tracking

### Frontend Structure

- `src/App.tsx`: Main application with routing and state management
- `src/components/`: Reusable UI components (charts, forms, tables)
- `src/pages/`: Page components (dashboard, competition, trader details)
- `src/types/`: TypeScript type definitions
- `src/hooks/`: Custom React hooks for API integration
- `src/lib/`: Utility functions and API clients

## Key Configuration Files

### Backend Configuration
- `config.json`: Main configuration (leverage, trading pairs, API settings)
- `config.db`: SQLite database for trader configurations and system state
- `beta_codes.txt`: Beta access codes for user registration

### AI Prompts
- `prompts/default.txt`: Default AI trading prompt template
- `prompts/adaptive.txt`: Adaptive prompt with market condition awareness
- `prompts/nof1.txt`: Specialized trading strategy prompt
- `prompts/taro_long_prompts.txt`: Long-form trading analysis prompts

### Docker Configuration
- `docker-compose.yml`: Multi-service deployment configuration
- `docker/Dockerfile.backend`: Go backend container definition
- `docker/Dockerfile.frontend`: React frontend container definition

## Database Schema

The SQLite database manages:
- **Users**: Authentication and beta code management
- **AI Models**: DeepSeek/Qwen API configurations
- **Exchanges**: Binance/Hyperliquid/Aster API credentials
- **Traders**: AI model + exchange combinations with specific settings
- **Decisions**: AI decision logs with full Chain of Thought reasoning
- **Performance Metrics**: Win rates, profit/loss tracking, equity curves

## API Architecture

### Authentication Endpoints
- `POST /api/login`: User authentication with optional 2FA
- `POST /api/register`: New user registration with beta code
- `POST /api/verify-otp`: 2FA verification

### Configuration Management
- `GET/PUT /api/models`: AI model configuration
- `GET/PUT /api/exchanges`: Exchange API settings
- `POST /api/traders`: Create new trader combinations
- `POST /api/traders/:id/start|stop`: Control trader execution

### Monitoring & Data
- `GET /api/status`: System health and trader status
- `GET /api/account`: Account balance and positions
- `GET /api/positions`: Current trading positions
- `GET /api/decisions/latest`: Recent AI decisions
- `GET /api/equity-history`: Performance chart data

## Development Workflow

### Adding New Exchange Support
1. Implement `trader.Trader` interface in new file under `trader/`
2. Add exchange configuration to database schema
3. Update API endpoints to support new exchange type
4. Add frontend configuration UI components

### Adding New AI Model Support
1. Add model API client in `decision/` package
2. Update prompt templates for new model format
3. Add model configuration fields to database
4. Test API integration and response parsing

### Modifying AI Prompts
1. Update prompt templates in `prompts/` directory
2. Test with different market conditions
3. Monitor decision quality and performance metrics
4. Adjust prompt structure based on AI response patterns

## Risk Management Features

- **Position Limits**: Per-asset limits based on account equity
- **Leverage Controls**: Configurable maximum leverage by asset type
- **Margin Management**: Total usage limited to 90% of available margin
- **Stop Loss/Take Profit**: Mandatory 1:2 risk-reward ratio enforcement
- **Drawdown Protection**: Daily loss limits and maximum drawdown controls

## Testing & Validation

### Current Testing Approach
- No automated test suite currently exists
- Manual testing through web interface
- Paper trading recommended before live deployment
- Performance monitoring through decision logs and equity curves

### Testing New Features
1. Create test trader with small capital allocation
2. Monitor AI decision quality through web interface
3. Review decision logs for Chain of Thought reasoning
4. Validate risk management rules are enforced
5. Check API integration and error handling

## Important Notes

- **Security**: Never commit API keys or sensitive configuration
- **Risk**: Always test with small amounts before deploying significant capital
- **Monitoring**: Regularly check system logs and trader performance
- **Backups**: Backup `config.db` and `decision_logs/` regularly
- **Dependencies**: Requires TA-Lib library for technical indicator calculations

## Environment Setup

### Prerequisites
- Go 1.25+
- Node.js 18+
- TA-Lib library
- Docker & Docker Compose (for containerized deployment)

### Quick Development Setup
1. Copy `config.json.example` to `config.json` and configure
2. Install TA-Lib: `brew install ta-lib` (macOS) or `sudo apt-get install libta-lib0-dev` (Ubuntu)
3. Start backend: `go build && ./nofx`
4. Start frontend: `cd web && npm install && npm run dev`
5. Access web interface at `http://localhost:3000`

### Production Deployment
1. Configure production settings in `config.json`
2. Use Docker deployment: `./start.sh start --build`
3. Monitor system through web interface and logs
4. Set up regular backups of database and decision logs