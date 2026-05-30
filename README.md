# 🤖 Agentic Chatbot with LangGraph

An intelligent, multi-modal chatbot application built with **LangGraph**, **Streamlit**, and **Groq LLM**. Features three distinct use cases for conversational AI, web search integration, and AI news aggregation.

---

## ✨ Features

### 1. **Basic Chatbot** 💬
A simple conversational AI agent that engages in natural dialogue.
- Real-time streaming responses
- Context-aware conversations
- Powered by Groq LLM (fast and efficient)

### 2. **Chatbot With Web Search** 🌐
An advanced chatbot with internet search capabilities.
- Dynamic web search using Tavily Search
- Tool-integrated responses with real-time information
- Conditional routing to search tools based on user queries
- Retrieves current information beyond training data

### 3. **AI News Explorer** 📰
Automated news fetching, summarization, and storage.
- **Time-based filtering**: Daily, Weekly, or Monthly news summaries
- **Topic**: AI and technology news from India and globally
- **Smart summarization**: Markdown-formatted summaries with:
  - Dates in IST timezone (YYYY-MM-DD format)
  - Concise summaries from latest articles
  - Source URLs for reference
  - Chronologically sorted (latest first)
- **Automated storage**: Saves summaries to `./AINews/{timeframe}_summary.md`

---

## 🏗️ Architecture

### State Management
- **LangGraph StateGraph**: Manages complex workflow states across multiple nodes
- **TypedDict-based State**: Ensures type safety for state transitions
- **Message history**: Maintains conversation context with LangChain message types

### Components

```
src/langgraphagenticai/
├── LLMS/
│   └── groqllm.py              # Groq LLM configuration
├── nodes/
│   ├── basic_chatbot_node.py    # Basic chatbot processing node
│   ├── chatbot_with_tool_node.py # Web search chatbot node
│   └── ai_news_node.py          # News fetching & summarization pipeline
├── tools/
│   └── search_tool.py           # Tavily web search tool integration
├── graph/
│   └── graph_builder.py         # LangGraph workflow builder
├── state/
│   └── state.py                 # Global state schema
└── ui/
    └── streamlitui/
        ├── loadui.py            # UI configuration & controls
        └── display_result.py     # Result rendering logic
```

### Graph Workflows

#### Basic Chatbot Graph
```
START → Chatbot Node → END
```

#### Chatbot With Web Graph
```
START → Chatbot Node → [Conditional Routing] 
                          ├─ Tools Node (if search needed)
                          └─ Return Result
                              ↓
                          Chatbot Node → END
```

#### AI News Graph
```
START → Fetch News → Summarize News → Save Result → END
         (Tavily)    (Groq LLM)      (Markdown)
```

---

## 🚀 Quick Start

### Prerequisites
- Python 3.11+
- Groq API Key ([Get one](https://console.groq.com/keys))
- Tavily API Key (for web search features) ([Get one](https://app.tavily.com/home))

### Installation

1. **Clone the repository**
   ```bash
   cd AI_Bootcamp/AI_Projects/Agentic_chatbot
   ```

2. **Create and activate virtual environment**
   ```bash
   python -m venv .venv
   .venv\Scripts\activate  # Windows
   source .venv/bin/activate  # macOS/Linux
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

### Running the Application

Start the Streamlit app:
```bash
streamlit run app.py
```

The app will open at `http://localhost:8501`

---

## 📖 Usage Guide

### 1. Basic Chatbot
1. Select **"Basic Chatbot"** from the sidebar
2. Enter your **Groq API Key**
3. Type your message in the chat input
4. Get streaming responses from the AI

### 2. Chatbot With Web Search
1. Select **"Chatbot With Web"** from the sidebar
2. Enter your **Groq API Key** and **Tavily API Key**
3. Ask questions that require web search (e.g., "What are the latest AI developments in 2025?")
4. The chatbot automatically decides when to search the web

### 3. AI News Explorer
1. Select **"AI News"** from the sidebar
2. Enter your **Groq API Key** and **Tavily API Key**
3. Choose a time frame: **Daily**, **Weekly**, or **Monthly**
4. Click **"🔍 Fetch Latest AI News"**
5. View the formatted markdown summary with:
   - Article dates (IST timezone)
   - Concise summaries
   - Direct source links
   - Latest news first

---

## 🔧 Configuration

### UI Configuration (`uiconfigfile.ini`)
Customize page title, LLM options, and use cases:
```ini
[page]
title=Agentic Chat Bot

[llm_models]
options=Groq

[groq_models]
options=mixtral-8x7b-32768,llama-2-70b-chat

[usecases]
options=Basic Chatbot,Chatbot With Web,AI News
```

### Groq Models Supported
- `mixtral-8x7b-32768` - Fast multilingual model
- `llama-2-70b-chat` - Powerful conversation model

---

## 📋 Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| **langchain** | ≥0.1.0 | LLM framework |
| **langgraph** | ≥1.2.2 | State machine & graph workflows |
| **langchain-groq** | ≥1.1.2 | Groq LLM integration |
| **langchain-tavily** | ≥0.2.18 | Web search tool |
| **tavily-python** | ≥0.7.24 | Tavily API client |
| **streamlit** | ≥1.57.0 | Web UI framework |
| **faiss-cpu** | ≥1.14.2 | Vector similarity search |

---

## 📁 Project Structure

```
Agentic_chatbot/
├── app.py                           # Entry point
├── README.md                        # This file
├── requirements.txt                 # Dependencies
├── pyproject.toml                   # Project metadata
├── AINews/                          # Generated news summaries
│   ├── daily_summary.md
│   ├── weekly_summary.md
│   └── monthly_summary.md
└── src/
    └── langgraphagenticai/
        ├── main.py                  # Core app logic
        ├── LLMS/
        ├── nodes/
        ├── tools/
        ├── graph/
        ├── state/
        └── ui/
```

---

## 🎯 Key Features

### ✅ Multi-Use Case Support
- Flexible architecture supports adding new chatbot types
- Modular node-based design using LangGraph

### ✅ Real-Time Streaming
- Streaming responses for improved UX
- Progressive message display in Streamlit

### ✅ Tool Integration
- Web search with Tavily
- Conditional tool execution based on query analysis
- Extensible tool framework

### ✅ AI News Automation
- Scheduled/on-demand news fetching
- Smart summarization with LLM
- Markdown export for sharing
- Time-zone aware date formatting

### ✅ Error Handling
- Comprehensive error messages
- API key validation
- Graceful degradation

### ✅ State Management
- TypedDict-based state for type safety
- Proper state propagation through graph nodes
- Message history tracking

---

## 🔐 Security

- API keys are passed as session state (not stored)
- Password input fields for sensitive credentials
- Environment variables for API keys
- Validation checks before processing

---

## 🛠️ Troubleshooting

### Issue: "File not found: ./AINews/weekly_summary.md"
- Ensure Tavily API Key is valid and has quota
- Check that news was successfully fetched
- Verify the graph executed all nodes (fetch → summarize → save)

### Issue: "LLM model could not be initialized"
- Verify Groq API Key is correct
- Check internet connectivity
- Ensure sufficient API quota

### Issue: No web search results
- Verify Tavily API Key is valid
- Ensure query is specific enough
- Check Tavily account for rate limits

---

## 📝 Example Queries

### Basic Chatbot
- "What is machine learning?"
- "Tell me a joke about AI"
- "Explain quantum computing"

### Chatbot With Web
- "What happened in tech news today?"
- "Latest cryptocurrency prices"
- "Recent AI breakthroughs in 2025"

### AI News
- Fetch Weekly AI news summaries
- Get Daily news from India tech scene
- Monthly digest of AI developments

---

## 🚧 Future Enhancements

- [ ] Multi-turn conversation memory
- [ ] Custom knowledge base integration
- [ ] PDF/document upload support
- [ ] News subscription/digest feature
- [ ] Analytics dashboard
- [ ] User preference persistence
- [ ] Support for additional LLM providers
- [ ] Voice input/output support

---

## 📄 License

This project is part of the AI Bootcamp curriculum.

---

## 👥 Support

For issues or questions:
1. Check the troubleshooting section
2. Verify API credentials and quotas
3. Review logs for detailed error messages
4. Ensure all dependencies are installed correctly

---

**Happy Chatting! 🚀**
