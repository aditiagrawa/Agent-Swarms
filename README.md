# ⚡ SWARM — AI Agent Orchestration Platform

A production-grade, full-stack AI Agent Swarm Orchestration Platform where multiple specialized AI agents collaborate in real time to solve complex multi-step problems.

## ✨ Features

- **9 Specialized Agents** — Planner, Researcher, Analyst, Developer, Designer, Writer, QA, Critic, Manager
- **5 Preset Swarm Configurations** — Startup Builder, Software Dev Team, Research Swarm, Sales & Marketing, Full Analysis
- **Real-time Streaming** — Watch agents think and respond word-by-word via WebSockets
- **Live Network Visualization** — Animated canvas showing agent collaboration in real time
- **Smart Result Rendering** — Beautiful structured output per agent type, plus raw JSON view
- **Swarm History** — Track all previous runs
- **Context Passing** — Each agent builds on all previous agents' outputs

## 🏗️ Architecture

```
swarm/
├── backend/                    # Node.js + Express + WebSocket
│   ├── server.js               # Main server (REST + WS)
│   ├── agents/
│   │   ├── definitions.js      # All agent personas & system prompts
│   │   └── orchestrator.js     # Swarm execution engine
│   ├── .env.example
│   └── package.json
│
└── frontend/                   # React + Vite
    ├── src/
    │   ├── App.jsx             # Main application (Configure / Run / History)
    │   ├── components/
    │   │   ├── AgentCard.jsx        # Agent display card
    │   │   ├── ResultPanel.jsx      # Smart result renderer
    │   │   └── SwarmVisualizer.jsx  # Canvas network graph
    │   ├── hooks/
    │   │   └── useSwarmWebSocket.js # Real-time WS hook
    │   ├── services/
    │   │   └── api.js               # REST API client
    │   └── constants/
    │       └── agents.js            # Agent metadata
    ├── index.html
    └── package.json
```

## 🚀 Quick Start

### Prerequisites
- Node.js 18+
- An Anthropic API key (get one at https://console.anthropic.com)

### 1. Backend Setup

```bash
cd swarm/backend

# Install dependencies
npm install

# Configure environment
cp .env.example .env
# Edit .env and add your ANTHROPIC_API_KEY

# Start backend
npm start
# or for development with auto-reload:
npm run dev
```

Backend will start at: http://localhost:3001

### 2. Frontend Setup

```bash
cd swarm/frontend

# Install dependencies
npm install

# Start frontend
npm run dev
```

Frontend will start at: http://localhost:5173

### 3. Open the app

Navigate to http://localhost:5173 in your browser.

## 🎯 Usage

1. **Configure Tab** — Enter your goal, choose a preset swarm (or build custom), hit Launch
2. **Live Run Tab** — Watch agents work in real time with streaming output and network visualization
3. **History Tab** — Review all past swarm runs

## 🤖 Agent Roles

| Agent | Role | Best For |
|-------|------|----------|
| 🧠 Planner | Strategic Orchestrator | Breaking down complex goals |
| 🔍 Researcher | Intelligence Gatherer | Finding & synthesizing information |
| 📊 Analyst | Data Intelligence | SWOT analysis, metrics, insights |
| 💻 Developer | Code Architect | Tech stack, architecture, code |
| 🎨 Designer | UX Architect | User flows, visual systems |
| ✍️ Writer | Content Strategist | Copy, pitch decks, documentation |
| ✅ QA Agent | Quality Guardian | Validation & gap analysis |
| 🎯 Critic | Devil's Advocate | Stress-testing assumptions |
| 🏆 Manager | Synthesis Engine | Final integrated deliverable |

## 📡 API Reference

### REST Endpoints

```
GET  /health                    # Backend health check
GET  /api/agents                # List all agent definitions
GET  /api/presets               # List all preset configurations
POST /api/swarm/start           # Start a new swarm
     Body: { goal, agents?, presetId? }
GET  /api/swarm/:swarmId        # Get swarm status + results
GET  /api/swarms                # List recent swarms
```

### WebSocket Events (ws://localhost:3001/ws)

**Client → Server:**
```json
{ "type": "register", "swarmId": "uuid" }
```

**Server → Client:**
```
swarm_start          → Swarm initialized
agent_start          → Agent begins thinking
agent_streaming      → Agent output begins
agent_stream_chunk   → Individual text chunk (live typing)
agent_complete       → Agent finished with full result
swarm_progress       → Progress update
swarm_complete       → All agents done
swarm_error          → Error occurred
```

## 🔧 Customization

### Adding a New Agent

In `backend/agents/definitions.js`:
```javascript
my_agent: {
  id: 'my_agent',
  name: 'My Agent',
  role: 'My Role',
  emoji: '🤖',
  color: '#your_color',
  description: 'What this agent does',
  systemPrompt: `You are... ALWAYS respond in this EXACT JSON format: {...}`,
  temperature: 0.4,
}
```

Then add to `frontend/src/constants/agents.js`:
```javascript
my_agent: { id: 'my_agent', name: 'My Agent', emoji: '🤖', color: '#your_color', role: 'My Role', desc: 'Short description' }
```

### Adding a New Preset

In `frontend/src/constants/agents.js`:
```javascript
{
  id: 'my_preset',
  name: 'My Preset',
  emoji: '🎯',
  description: 'What this swarm does',
  agents: ['planner', 'researcher', 'my_agent', 'manager'],
  category: 'Custom',
  example: 'Example goal for this swarm',
}
```

## 💡 Example Goals by Preset

**Startup Builder:**
- "Build a fitness tracking app for Gen Z"
- "Create a B2B SaaS for restaurant inventory management"

**Research Swarm:**
- "Research EV adoption trends in India 2025"
- "Analyze the competitive landscape of AI coding assistants"

**Software Dev Team:**
- "Create an expense tracking web app with team collaboration"
- "Build a URL shortener with analytics dashboard"

**Full Analysis:**
- "Evaluate entering the edtech market in India"
- "Assess the opportunity in AI-powered legal tech"

## 🛠️ Troubleshooting

**Backend offline:** Check that server is running on port 3001 and ANTHROPIC_API_KEY is set in .env

**WebSocket connection fails:** Ensure backend is running before launching a swarm; the frontend waits up to 3 seconds for WS registration

**Agent errors:** Check your API key has sufficient credits; streaming requires a valid claude-sonnet-4-20250514 access
