# Codelab: Enhancing Obsidian Logistics with AI

Welcome to the Obsidian Logistics AI Codelab! In this guide, you'll learn how to take a modern Angular logistics dashboard and supercharge it with AI capabilities using the Gemini CLI, specialized skills, and the Model Context Protocol (MCP).

---

## Prerequisites

Before you begin, ensure you have the following installed:
- [Node.js](https://nodejs.org/) (v20 or higher)
- [Angular CLI](https://angular.dev/tools/cli) (`npm install -g @angular/cli`)
- [Gemini CLI](https://github.com/google-gemini/gemini-cli)

---

## Step 0: Environment Setup

First, let's prepare your workspace and tools.

### 1. Project Initialization
Navigate to the project root and install the dependencies:
```bash
cd logistics-manager-app
npm install
```

### 2. Install Gemini CLI Skills
Skills provide the agent with specialized knowledge. We'll add the `angular-developer` and `gemini-sdk` skills to help with code generation and API integration.
```bash
# Add Angular expertise
gemini skills install https://github.com/google-gemini/angular-developer --scope project

# Add Gemini SDK expertise
gemini skills install https://github.com/google-gemini/gemini-sdk --scope project
```

### 3. Configure MCP Servers
The Model Context Protocol (MCP) allows your AI tools to interact with your running application and the browser.

#### Angular MCP Server
This server allows the AI to understand your Angular project structure, components, and services.
```bash
gemini mcp add angular npx -y @modelcontextprotocol/server-angular --scope project
```

#### Chrome DevTools MCP Server
This allows the AI to inspect the running application in your browser for debugging and UI analysis.
```bash
gemini mcp add chromedevtools npx -y @modelcontextprotocol/server-chromedevtools --scope project
```

---

## Enhancement 1: AI-Powered Fleet Chat

The `app-chat` component is currently a UI shell. Your task is to implement the backend and frontend logic to allow users to ask natural language questions about the fleet.

### The Goal
A user should be able to type: *"Which units are currently in critical status and have less than 10% battery?"* and receive a concise list.

### Implementation Steps
1. **Service Integration**: Update `FleetService` to include a `queryFleet(prompt: string)` method.
2. **AI Logic**: Use the `gemini-sdk` skill to generate a prompt that sends the current `units()` state to Gemini and asks it to filter the data based on the user's input.
3. **UI Binding**: Connect the chat input and message list in `chat.ts` to the `FleetService`.

---

## Enhancement 2: Intelligent Service Prioritization

Currently, users manually select a priority when creating a service ticket. Let's automate this using AI analysis of the issue description.

### The Goal
When a user describes an issue like *"The vehicle is emitting smoke and the engine has stopped,"* the AI should automatically set the priority to `CRITICAL`.

### Implementation Steps
1. **Form Hook**: In `service-queue.ts`, add a listener to the `issue` field in the `serviceForm`.
2. **AI Analysis**: Create a small utility that sends the issue text to Gemini with a system prompt: *"Analyze this logistics vehicle issue and return one of: LOW, MEDIUM, HIGH, CRITICAL."*
3. **Auto-update**: Use the result to update the `priority` field in the form automatically as the user types or on blur.

---

## Enhancement 3: Predictive Battery Health Diagnostics

Add a "Run Diagnostic" feature to the vehicle detail view that predicts potential failures before they happen.

### The Goal
In the `FleetDetailModal`, provide a "Run AI Diagnostic" button. When clicked, it analyzes the unit's `speed`, `battery`, and `status` history to provide a "Health Score" and a "Recommended Action."

### Implementation Steps
1. **Diagnostic Action**: Add a button to the `FleetDetailModal` template.
2. **Telemetry Payload**: Gather the `FleetUnit` data and any historical trends (if available in `fleet-db.ts`).
3. **AI Insight**: Send this telemetry to Gemini to generate a report.
   - Example prompt: *"Analyze this unit: Speed 102km/h, Battery 14% (Declining rapidly), Status: Transit. Predict potential failure points."*
4. **Result Display**: Show the AI's response in a new "Diagnostics" section within the modal.

---

## Running the App

To see your changes in action, start the development server:
```bash
npm run start
```
The app will be available at `http://localhost:4200`.

---

## Summary
By completing these enhancements, you've transformed a static dashboard into an intelligent command center. You've leveraged:
- **Angular Signals** for reactive state management.
- **Gemini CLI Skills** for expert-level coding assistance.
- **MCP Servers** for deep integration between your AI and your workspace.
