# 🏥 SahAI — WhatsApp Healthcare Assistant

> **"Sah"** (Tamil: சகா) means "companion" — SahAI is your AI health companion on WhatsApp.

## 🎯 What is SahAI?

SahAI is a multi-agent WhatsApp bot that provides:

1. **Health Triage** — Symptom assessment, doctor recommendations, appointment booking
2. **Government Scheme Assistant** — Discover & apply for health/welfare schemes
3. **Educational Content** — On-demand learning, quizzes, homework help
4. **Emergency Services** — One-tap ambulance/police/fire dispatch with location sharing

Built for a 24-hour hackathon as an **educational reference implementation** demonstrating
production-grade patterns: agent orchestration, multi-layer memory, RAG, MCP tools, and
WhatsApp Business API integration.

---

## 🏗️ Architecture Overview

```
┌─────────────────────────────────────────────────────────┐
│                    WhatsApp Business API                 │
│                  (Twilio / Meta Cloud API)               │
└─────────────────┬───────────────────────────────────────┘
                  │  Webhook
                  ▼
┌─────────────────────────────────────────────────────────┐
│              Message Handler & Router                    │
│  • Language detection (Tamil/Hindi/English)              │
│  • Intent classification                                │
│  • Session management                                   │
│  • Voice-to-text (Whisper API)                          │
└─────────┬───────┬───────┬───────┬───────────────────────┘
          │       │       │       │
          ▼       ▼       ▼       ▼
┌────────┐┌──────┐┌─────┐┌──────────────┐
│ Health ││Scheme││Edu  ││  Emergency   │
│ Agent  ││Agent ││Agent││  Agent       │
└───┬────┘└──┬───┘└──┬──┘└──────┬───────┘
    │        │       │          │
    ▼        ▼       ▼          ▼
┌─────────────────────────────────────────────────────────┐
│              Context Assembly Engine                     │
│  Memory(MEMORY.md) + RAG + History + Tools + USER.md    │
└─────────────────────┬───────────────────────────────────┘
                      │
          ┌───────────┼───────────┐
          ▼           ▼           ▼
    ┌──────────┐┌──────────┐┌──────────┐
    │  Memory  ││   RAG    ││   MCP    │
    │  System  ││  Engine  ││  Tools   │
    └──────────┘└──────────┘└──────────┘
```

## 📁 Project Structure

```
sahai/
├── src/
│   ├── agents/              # Agent orchestration layer
│   │   ├── orchestrator.ts  # Main routing agent
│   │   ├── health.agent.ts  # Health triage agent
│   │   ├── scheme.agent.ts  # Government scheme agent
│   │   ├── edu.agent.ts     # Education agent
│   │   ├── emergency.agent.ts # Emergency agent
│   │   └── base.agent.ts    # Base agent class with shared logic
│   ├── memory/              # Multi-layer memory system
│   │   ├── manager.ts       # Unified memory manager
│   │   ├── short-term.ts    # In-memory conversation buffer
│   │   ├── working.ts       # Session scratch notes
│   │   ├── long-term.ts     # File-backed MEMORY.md system
│   │   ├── heartbeat.ts     # Periodic check/reminder system
│   │   └── profile.ts       # USER.md / SOUL.md / TOOLS.md
│   ├── rag/                 # Retrieval-Augmented Generation
│   │   ├── vector-store.ts  # Vector database operations
│   │   ├── indexer.ts       # Background message indexer
│   │   ├── retriever.ts     # Semantic search & retrieval
│   │   └── embeddings.ts    # OpenAI embedding integration
│   ├── mcp-tools/           # Model Context Protocol tools
│   │   ├── registry.ts      # Tool registration & dispatch
│   │   ├── healthcare/      # Health-specific tools
│   │   ├── government/      # Scheme tools
│   │   ├── education/       # Learning tools
│   │   └── emergency/       # Emergency tools
│   ├── whatsapp/            # WhatsApp integration layer
│   │   ├── client.ts        # WhatsApp API client
│   │   ├── webhook.ts       # Incoming message handler
│   │   ├── session.ts       # Session management
│   │   └── media.ts         # Voice/image/document handling
│   ├── utils/               # Shared utilities
│   │   ├── language.ts      # Language detection & translation
│   │   ├── logger.ts        # Structured logging
│   │   └── errors.ts        # Error types & recovery
│   ├── config/              # Configuration
│   │   └── index.ts         # Environment & settings
│   └── db/                  # Database layer
│       ├── schema.ts        # TypeScript interfaces
│       └── client.ts        # Database client (SQLite/Postgres)
├── memory/                  # File-backed memory store (per-user)
│   ├── MEMORY.md            # Curated long-term memory
│   ├── USER.md              # User preferences & profile
│   ├── SOUL.md              # Bot personality definition
│   ├── TOOLS.md             # Integration notes
│   └── YYYY-MM-DD.md        # Daily conversation logs
├── tests/                   # Test suite
├── docs/                    # Additional documentation
├── .env.example             # Environment variable template
├── package.json
├── tsconfig.json
└── README.md
```

## 🚀 Quick Start

```bash
# 1. Clone & install
git clone https://github.com/novarohan8-cyber/Mod_bot.git
cd Mod_bot && npm install

# 2. Configure environment
cp .env.example .env
# Fill in: OPENAI_API_KEY, TWILIO_*, DATABASE_URL

# 3. Start development server
npm run dev

# 4. Expose webhook (for local dev)
ngrok http 3000

# 5. Configure Twilio webhook URL
# Set: https://your-ngrok-url.ngrok.io/api/webhook
```

## 📚 Educational Notes

Every file in this project is heavily commented to explain **why** each design
decision was made. Look for:

- `// WHY:` — Explains architectural decisions
- `// PATTERN:` — Names the design pattern being used
- `// TRADEOFF:` — Documents conscious tradeoffs
- `// HACKATHON NOTE:` — Shortcuts taken for time, with production alternatives

## License

MIT — Built for learning. Use freely.
