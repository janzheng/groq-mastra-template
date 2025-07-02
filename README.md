# Groq + Mastra Agent Framework Template

[Mastra](https://mastra.ai) is an open-source TypeScript agent framework designed to give you the primitives you need to build AI applications and features.

Mastra provides a comprehensive toolkit for building AI agents with memory, function execution capabilities, and deterministic workflow orchestration. You can chat with your agents in Mastra's local development environment, enhance them with application-specific knowledge through RAG, and evaluate their outputs with built-in assessment tools.

The Mastra integration with Groq includes the following features:

- **Agent Development Environment**: Interactive local playground for testing agents, workflows, and tools during development
- **Model Routing**: Unified interface to interact with Groq models through the Vercel AI SDK
- **Agent Memory and Tool Calling**: Persistent agent memory with semantic similarity retrieval and function execution capabilities
- **Workflow Graphs**: Deterministic LLM call execution with graph-based workflow engine and control flow syntax
- **Retrieval-Augmented Generation (RAG)**: Document processing, embedding creation, and vector database integration
- **Deployment Support**: Bundle agents and workflows for React, Next.js, Node.js, or serverless platforms
- **Automated Evaluations**: Model-graded, rule-based, and statistical assessment of LLM outputs

## Overview

This template demonstrates how to build AI agents and workflows using Mastra with Groq API for fast LLM inference. Built as a complete, end-to-end starter that you can fork, customize, and deploy for production AI applications.

**Key Features:**
- 🤖 Weather agent with memory and tool calling capabilities
- 🔄 Deterministic weather workflow with multi-step processing
- 🛠️ Custom tools for external API integration (Open-Meteo weather API)
- 🎮 Local development playground for interactive testing
- 📊 Built-in logging and telemetry with LibSQL storage
- Sub-second response times, efficient concurrent request handling, and production-grade performance powered by Groq

## Architecture

**Tech Stack:**
- **AI Infrastructure:** Groq API for fast LLM inference
- **Agent Framework:** Mastra for agents, workflows, and tools
- **Backend:** TypeScript with Hono server
- **Storage:** LibSQL for agent memory and telemetry
- **Development:** Local playground environment

## Quick Start

### Prerequisites
- Node.js 20.9.0 or higher
- Groq API key ([Create a free GroqCloud account and generate an API key here](https://console.groq.com/keys))

### Setup

1. **Clone the repository**
   ```bash
   gh repo clone http://github.com/janzheng/groq-mastra-template
   cd groq-mastra
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Configure your API key**
   Create a `.env` file in the root directory:
   ```bash
   GROQ_API_KEY=your_groq_api_key_here
   ```

4. **Start the development server**
   ```bash
   npm run dev
   ```

5. **Access the development interfaces**
   - **Playground**: http://localhost:4111/
   - **Mastra API**: http://localhost:4111/api
   - **OpenAPI Spec**: http://localhost:4111/openapi.json
   - **Swagger UI**: http://localhost:4111/swagger-ui

## Development Environment

Mastra provides a local development environment where you can test your agents, workflows, and tools during development. The development server provides access to multiple interfaces:

### Playground Interface
The Playground lets you interact with your agents, workflows, and tools. It provides dedicated interfaces for testing each component of your Mastra application during development and is available at http://localhost:4111/.

### API Explorer
Use the Swagger UI at http://localhost:4111/swagger-ui to explore and test your Mastra API endpoints directly.

## Project Structure

```
src/mastra/
├── index.ts                 # Main Mastra configuration
├── agents/
│   └── weather-agent.ts     # Weather assistant agent with memory
├── tools/
│   └── weather-tool.ts      # Weather data fetching tool
└── workflows/
    └── weather-workflow.ts  # Multi-step weather workflow
```

## Features

### Weather Agent
The included weather agent demonstrates:
- **Memory Persistence**: Conversations are stored and retrieved across sessions
- **Tool Integration**: Uses the weather tool to fetch real-time data
- **Natural Language Processing**: Handles location queries and activity planning
- **Groq Model Integration**: Powered by `llama-3.3-70b-versatile` for fast responses

### Weather Tool
Custom tool that:
- Fetches real-time weather data from Open-Meteo API
- Handles geocoding for location resolution
- Returns structured weather information (temperature, humidity, wind, conditions)
- Includes comprehensive weather condition mapping

### Weather Workflow
Deterministic workflow that:
- Fetches weather forecast for a specified city
- Plans activities based on weather conditions
- Uses the weather agent for intelligent activity suggestions
- Demonstrates multi-step processing with data flow between steps

## Usage Examples

### Testing the Weather Agent
1. Navigate to the Playground at http://localhost:4111/
2. Select the "Weather Agent" from the agents list
3. Ask questions like:
   - "What's the weather like in San Francisco?"
   - "Plan some outdoor activities for tomorrow in New York"
   - "Should I bring an umbrella in London today?"

### Running the Weather Workflow
1. Go to the Workflows section in the Playground
2. Select "Weather Workflow"
3. Provide a city name as input
4. Watch the multi-step process execute:
   - Step 1: Fetch weather data
   - Step 2: Generate activity recommendations

### API Usage
You can also interact with your agents and workflows programmatically:

```typescript
import { mastra } from './src/mastra';

// Get weather agent
const agent = mastra.getAgent('weatherAgent');

// Chat with the agent
const response = await agent.stream([
  { role: 'user', content: 'What\'s the weather in Tokyo?' }
]);

// Run weather workflow
const workflow = mastra.getWorkflow('weather-workflow');
const result = await workflow.execute({ city: 'Tokyo' });
```

## Customization

This template is designed to be a foundation for you to get started with. Key areas for customization:

### Model Selection
Update Groq model configuration in `src/mastra/agents/weather-agent.ts`:
```typescript
model: groq('llama-3.3-70b-versatile'), // Change to your preferred model
```

### Adding New Tools
Create new tools in `src/mastra/tools/`:
```typescript
import { createTool } from '@mastra/core/tools';
import { z } from 'zod';

export const myTool = createTool({
  id: 'my-tool',
  description: 'Description of what this tool does',
  inputSchema: z.object({
    input: z.string(),
  }),
  outputSchema: z.object({
    output: z.string(),
  }),
  execute: async ({ context }) => {
    // Tool implementation
    return { output: 'result' };
  },
});
```

### Creating New Agents
Add agents in `src/mastra/agents/`:
```typescript
import { groq } from '@ai-sdk/groq';
import { Agent } from '@mastra/core/agent';

export const myAgent = new Agent({
  name: 'My Agent',
  instructions: 'Agent instructions here',
  model: groq('llama-3.3-70b-versatile'),
  tools: { myTool },
});
```

### Building Workflows
Create workflows in `src/mastra/workflows/`:
```typescript
import { createStep, createWorkflow } from '@mastra/core/workflows';

const myStep = createStep({
  id: 'my-step',
  description: 'Step description',
  inputSchema: z.object({ input: z.string() }),
  outputSchema: z.object({ output: z.string() }),
  execute: async ({ inputData }) => {
    // Step implementation
    return { output: 'result' };
  },
});

const myWorkflow = createWorkflow({
  id: 'my-workflow',
  inputSchema: z.object({ input: z.string() }),
  outputSchema: z.object({ output: z.string() }),
}).then(myStep);
```

## Deployment

### Build for Production
```bash
npm run build
```

### Start Production Server
```bash
npm start
```

### Deployment Options
Mastra supports deployment to various platforms:
- **Vercel**: Serverless deployment
- **Cloudflare Workers**: Edge computing
- **Netlify**: JAMstack deployment
- **Node.js**: Traditional server deployment
- **Docker**: Containerized deployment

## Configuration

### Environment Variables
- `GROQ_API_KEY`: Your Groq API key (required)
- `NODE_ENV`: Environment mode (development/production)

### Storage Configuration
The template uses LibSQL for storage. You can modify the storage configuration in `src/mastra/index.ts`:

```typescript
storage: new LibSQLStore({
  url: ":memory:", // In-memory for development
  // url: "file:./mastra.db", // File-based for persistence
  // url: "libsql://your-turso-db.turso.io", // Remote Turso database
}),
```

### Supported Models
The template uses Groq models through the AI SDK. You can use any Groq-supported model:
- `llama-3.1-8b-instant`
- `llama-3.3-70b-versatile`
- `meta-llama/llama-4-scout-17b-16e-instruct`
- `meta-llama/llama-4-maverick-17b-128e-instruct`
- Full list of [Groq models here](https://console.groq.com/docs/models) 

## Next Steps

### For Developers
- **Create your free GroqCloud account**: Access official API docs, the playground for experimentation, and more resources via [Groq Console](https://console.groq.com).
- **Build and customize**: Fork this repo and start customizing to build out your own application.
- **Get support**: Connect with other developers building on Groq, chat with our team, and submit feature requests on our [Groq Developer Forum](community.groq.com).

### For Founders and Business Leaders
- **See enterprise capabilities**: This template showcases production-ready AI agents that can handle realtime business workloads with comprehensive memory and workflow management.
- **Discuss Your needs**: [Contact our team](https://groq.com/enterprise-access/) to explore how Groq can accelerate your AI initiatives.

## License
This project is licensed under the MIT License - see the LICENSE file for details.

## Credits
Created with Groq API and Mastra framework for production-ready AI agent development. 