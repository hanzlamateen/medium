# Qwen Local Code Assistant

A powerful, privacy-first local coding assistant powered by Qwen3-Coder-Next and Ollama. Get Claude Code-like capabilities running entirely on your machine!

## 🌟 Features

- 📂 **Full Codebase Understanding** - 256K token context window
- 💻 **Multi-File Editing** - Modify multiple files in a single conversation
- 🔍 **Smart Code Search** - Find and navigate complex projects
- 🐛 **Debugging Assistant** - Analyze and fix bugs
- 🧪 **Test Generation** - Create comprehensive test suites
- 📝 **Documentation** - Generate docstrings and READMEs
- 🔐 **100% Private** - Your code never leaves your machine
- 💰 **Zero Cost** - No subscriptions or API fees

## 🚀 Quick Start

### Prerequisites

- **RAM:** 46GB minimum (64GB recommended)
- **Disk Space:** 60GB free
- **OS:** macOS, Linux, or Windows
- **Python:** 3.8+

### Installation

```bash
# 1. Install Ollama
curl -fsSL https://ollama.com/install.sh | sh

# 2. Pull the model (takes 10-30 minutes)
ollama pull qwen3-coder-next:q4_K_M

# 3. Clone this repository
git clone https://github.com/yourusername/qwen-code-assistant
cd qwen-code-assistant

# 4. Set up Python environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# 5. Install dependencies
pip install -r requirements.txt

# 6. Run the assistant!
python assistant.py
```

## 📖 Usage Examples

### Create a New Project

```
You: Create a FastAPI project with user authentication
```

The assistant will:
- Create proper directory structure
- Write all necessary files
- Include best practices
- Add documentation

### Debug Code

```
You: This function is throwing an IndexError. Can you help?

def get_item(items, index):
    return items[index]
```

The assistant will:
- Identify the issue
- Explain the problem
- Provide a fixed version
- Suggest improvements

### Refactor Code

```
You: Refactor this file to follow SOLID principles
[paste code or provide filepath]
```

The assistant will:
- Analyze current structure
- Suggest improvements
- Implement changes
- Explain the refactoring

## 🛠️ Available Tools

The assistant has access to:

| Tool | Description |
|------|-------------|
| `read_file` | Read file contents |
| `write_file` | Create or update files |
| `execute_command` | Run shell commands |
| `list_files` | Browse directories |

## 💡 Tips for Best Results

1. **Be Specific**
   ```
   ❌ "Make a website"
   ✅ "Create a React component for a user profile card with avatar, name, and bio"
   ```

2. **Provide Context**
   ```
   ✅ "Using the FastAPI project in ./backend, add a new endpoint for user registration"
   ```

3. **Iterate**
   ```
   You: Create a user model
   [Review the output]
   You: Add email validation
   [Review again]
   You: Add password hashing
   ```

4. **Use Project Instructions**
   Create a `QWEN.md` file in your project root with:
   - Tech stack
   - Code style preferences
   - Testing requirements
   - File structure

## ⚙️ Configuration

### Change Model

Edit `assistant.py`:
```python
# Use different model variant
assistant = QwenCodeAssistant(model="qwen3-coder-next:q8_0")  # Higher quality
# or
assistant = QwenCodeAssistant(model="qwen3-coder:7b")  # Faster, less memory
```

### Adjust Memory Usage

```python
# Limit conversation history
def __init__(self, model: str = "qwen3-coder-next:q4_K_M"):
    self.model = model
    self.conversation_history: List[Dict] = []
    self.max_history = 10  # Add this
```

### Custom Tools

Add your own tools:
```python
def my_custom_tool(self, param: str) -> str:
    """Your custom functionality."""
    # Implementation
    return result

# Add to get_available_tools()
{
    'type': 'function',
    'function': {
        'name': 'my_custom_tool',
        'description': 'What it does',
        'parameters': { ... }
    }
}
```

## 📊 Performance

| Model Variant | Size | RAM Needed | Speed | Quality |
|---------------|------|------------|-------|---------|
| q4_K_M | 48GB | 46GB | ⚡⚡⚡ | ⭐⭐⭐ |
| latest | 52GB | 52GB | ⚡⚡ | ⭐⭐⭐⭐ |
| q8_0 | 85GB | 85GB | ⚡ | ⭐⭐⭐⭐⭐ |

## 🔧 Troubleshooting

### Model Not Found
```bash
ollama list  # Check installed models
ollama pull qwen3-coder-next:q4_K_M  # Pull if missing
```

### Out of Memory
```bash
# Use smaller model
ollama pull qwen3-coder:7b
```

### Slow Performance
```bash
# Check if model is loaded
ollama ps

# Preload model
ollama run qwen3-coder-next:q4_K_M "Ready" --keepalive 24h
```

### Connection Issues
```bash
# Restart Ollama
pkill ollama
ollama serve
```

## 📁 Project Structure

```
qwen-code-assistant/
├── assistant.py           # Main assistant code
├── requirements.txt       # Python dependencies
├── README.md             # This file
├── examples/             # Usage examples
│   ├── create_project.md
│   ├── debugging.md
│   └── refactoring.md
└── docs/                 # Additional documentation
    ├── architecture.md
    └── advanced-usage.md
```

## 🆚 Comparison with Other Tools

| Feature | This Assistant | Claude Code | GitHub Copilot |
|---------|---------------|-------------|----------------|
| Cost | Free | $20-30/mo | $10/mo |
| Privacy | 100% Local | Cloud | Cloud |
| Offline | ✅ Yes | ❌ No | ❌ No |
| Context Size | 256K | ~200K | Limited |
| Tool Calling | ✅ Yes | ✅ Yes | ❌ No |
| Customizable | ✅ Yes | Limited | ❌ No |

## 🤝 Contributing

Contributions welcome! Feel free to:
- Report bugs
- Suggest features
- Submit pull requests
- Improve documentation

## 📄 License

MIT License - feel free to use this however you want!

## 🙏 Acknowledgments

- **Alibaba Qwen Team** - For the amazing Qwen3-Coder-Next model
- **Ollama** - For making local LLMs accessible
- **The Open Source Community** - For continuous improvements

## 📚 Resources

- [Full Tutorial Article](https://medium.com/your-article-link)
- [Ollama Documentation](https://ollama.com)
- [Qwen3-Coder-Next on Hugging Face](https://huggingface.co/Qwen/Qwen3-Coder-Next)
- [Join the Discussion](https://discord.gg/ollama)

## 💬 Support

- **Issues:** Open an issue on GitHub
- **Discussions:** Use GitHub Discussions
- **Community:** Join the Ollama Discord

---

**Built with ❤️ for developers who value privacy and control**

Star ⭐ this repo if you find it useful!