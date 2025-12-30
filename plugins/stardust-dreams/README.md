# Stardust Dreams Tool Marketplace Plugin

> 🌟 Connect to the Stardust Dreams tool marketplace, use advanced AI creation templates, and make your creation more efficient and professional.

## 📋 Feature Overview

The Stardust Dreams tool marketplace plugin is a secure and efficient AI creation tool integration solution. Through a unique **server-side encryption + client-side decryption** architecture, it both protects core Prompt assets and provides an ultimate user experience.

### Core Features

- 🔐 **Secure Architecture** - Prompt templates are stored encrypted on the server, and the client only decrypts and uses them in memory.
- 🎯 **Rich Templates** - 35+ professional creation templates covering various creation scenarios.
- 📊 **Dynamic Forms** - Visual parameter configuration on the web, supporting complex form design.
- 🚀 **High-speed Generation** - Optimized decryption and template engine for millisecond-level response.
- 💎 **Business Model** - Supports free trials, paid subscriptions, and enterprise customization.

## 🏗️ System Architecture

```
┌─────────────────────────────────────────┐
│         Server-side (Not open source)     │
│  • Stores encrypted Prompt templates       │
│  • Manages user permissions and subscriptions│
│  • Provides a web form designer            │
│  • Generates sessions and decryption keys     │
└─────────────────────────────────────────┘
                    ↓
           Encrypted Prompt + SessionKey
                    ↓
┌─────────────────────────────────────────┐
│         Client-side Plugin (Open source)  │
│  • Gets the encrypted Prompt              │
│  • Decrypts in memory (not persistent)   │
│  • Fills in parameters to generate final content │
│  • Cleans up memory immediately after use   │
└─────────────────────────────────────────┘
```

## 🚀 Quick Start

### 1. Install the Plugin

```bash
# In the novel-writer project
novel plugins add stardust-dreams
```

### 2. Login and Authentication

```bash
# Use the command in your AI assistant
/stardust-auth
```

### 3. Use a Template

1. Visit the web interface: https://stardust-dreams.com
2. Select a template and fill out the form
3. Get the SessionID
4. Use it in your AI assistant:

```bash
/stardust-use --session xyz789abc
```

## 📚 Available Commands

| Command | Function | Description |
|------|------|------|
| `/stardust-auth` | Login and Authentication | Log in to your account to get access permissions |
| `/stardust-use` | Use a Template | Use a template via SessionID |
| `/stardust-list` | View Templates | List available creation templates |
| `/stardust-session` | Session Management | View and manage active sessions |

## 🎯 Use Cases

### Web Novel Creation
- **Hit Idea Generator** - A creative tool based on the analysis of 100,000+ hit works.
- **Tomato爽文 (爽文) Template** - A template optimized for the Tomato platform.
- **Golden Finger Designer** - A library of 1000+ Golden Finger templates.

### Traditional Literature
- **Writing Style Polisher** - Enhance literary and artistic quality.
- **Imagery Generator** - Create profound literary imagery.
- **Theme Sublimation Tool** - Deepen the theme of your work.

### Special Features
- **Novel Diagnoser** - Analyze problems in your work and provide improvement suggestions.
- **Pacing Optimizer** - Adjust the story's pacing and the distribution of "thrill points."
- **Character Relationship Map** - Generate a complex network of character relationships.

## 🔒 Security Mechanisms

### Prompt Protection
- ✅ Stored encrypted on the server, never transmitted in plaintext.
- ✅ Decrypted only in the client's memory, not written to disk.
- ✅ Memory is cleared immediately after use.
- ✅ Sessions automatically expire after 15 minutes.

### Authentication Security
- ✅ JWT Token authentication.
- ✅ Device-bound keys.
- ✅ Automatic token renewal.
- ✅ Encrypted storage of authentication information.

## 💰 Subscription Plans

### Free Version
- 8 basic templates
- 10 uses per day
- Community support

### Professional Version (Recommended)
- 35+ advanced templates
- Unlimited uses
- Priority customer support
- Monthly fee: ¥99

### Enterprise Version
- All professional version features
- Private deployment option
- Custom template development
- SLA guarantee
- Contact sales for pricing

## 🛠️ Technical Implementation

### Core Libraries

- **api-client.js** - API communication client
- **prompt-manager.js** - Prompt manager (in-memory operations)
- **decryptor.js** - AES-256-GCM decryptor
- **template-engine.js** - Template filling engine
- **secure-storage.js** - Secure storage (authentication information only)

### Template Syntax

Supports Handlebars-style template syntax:

```handlebars
{{genre}}                    # Variable substitution
{{#if condition}}...{{/if}}  # Conditional rendering
{{#each items}}...{{/each}}  # Loop rendering
{{#with object}}...{{/with}} # Context switching
```

## 📊 Usage Statistics

The plugin records anonymous usage statistics (without sensitive content):
- Template usage frequency
- Average generation time
- Success rate statistics

## 🤝 Contributing

Although the core Prompts are closed-source, the client plugin is open-source, and contributions are welcome:

1. Fork this repository
2. Create a feature branch
3. Submit your improvements
4. Open a Pull Request

## 📝 License

- **Client Plugin**: MIT License (Open source)
- **Server-side System**: Proprietary software (Closed source)
- **Prompt Templates**: Copyright protected, reverse engineering is prohibited.

## 🔗 Related Links

- Official Website: https://stardust-dreams.com
- API Documentation: https://docs.stardust-dreams.com
- User Community: https://community.stardust-dreams.com
- Technical Support: support@stardust-dreams.com

## ⚠️ Important Notes

1. **Do not attempt to save or export Prompts** - This violates the terms of use.
2. **Do not share SessionIDs** - Each session is tied to a specific user.
3. **Use sessions in a timely manner** - Sessions automatically expire after 15 minutes.
4. **Protect your account security** - Do not share your authentication information.

## 🎓 Learning Resources

- [Quick Start Tutorial](https://tutorial.stardust-dreams.com/quickstart)
- [Template Usage Guide](https://tutorial.stardust-dreams.com/templates)
- [Advanced Tips and Tricks](https://tutorial.stardust-dreams.com/advanced)
- [Video Tutorial Series](https://video.stardust-dreams.com)

## 💬 Getting Help

If you encounter any problems, you can get help in the following ways:

1. Use `/expert stardust-guide` to activate the expert guide.
2. Check the [FAQ](https://docs.stardust-dreams.com/faq).
3. Join the [user group](https://t.me/stardust_dreams).
4. Send an email to support@stardust-dreams.com.

---

**Stardust Dreams** - Making AI creation more professional, efficient, and secure.
