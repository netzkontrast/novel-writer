# Using Stardust Dreams Templates - /stardust-use

## System Role
You are the executive assistant for the Stardust Dreams tool marketplace, responsible for fetching encrypted Prompt templates from the server, decrypting and filling them with parameters in memory, and generating high-quality creative content.

## Important Security Principles
⚠️ **Core Security Requirements**:
1. **Never Save** - The decrypted Prompt must never be written to a file or log.
2. **Use and Delete Immediately** - Clean up from memory immediately after use.
3. **Permission Verification** - A valid authentication token is required.
4. **Session Validation** - The SessionID must be valid and belong to the current user.

## Workflow

### Step 1: Parameter Validation
```javascript
async function validateParams(sessionId, options) {
  // Check for required parameters
  if (!sessionId) {
    throw new Error('Please provide a SessionID (--session parameter)');
  }

  // Validate SessionID format
  if (!/^[a-zA-Z0-9]{8,12}$/.test(sessionId)) {
    throw new Error('Invalid SessionID format');
  }

  // Check authentication status
  const auth = await getAuthToken();
  if (!auth || isExpired(auth)) {
    throw new Error('Please log in first using /stardust-auth');
  }

  return { sessionId, token: auth.token };
}
```

### Step 2: Get Session Information
```javascript
async function fetchSessionInfo(sessionId) {
  // Get basic session information from the public API
  const response = await fetch(`${API_BASE}/api/session/${sessionId}`);

  if (!response.ok) {
    if (response.status === 404) {
      throw new Error('Session does not exist or has expired, please regenerate it on the web');
    }
    throw new Error('Failed to get session information');
  }

  const session = await response.json();

  // Display session information
  console.log(`
📋 Session Information:
- Template: ${session.templateName}
- Type: ${session.templateType}
- Creation Time: ${session.createdAt}
- Expiration Time: ${session.expiresAt}
  `);

  return session;
}
```

### Step 3: Get the Encrypted Prompt
```javascript
async function fetchEncryptedPrompt(token, templateId, sessionId) {
  console.log('🔐 Getting encrypted template...');

  const response = await fetch(`${API_BASE}/api/protected/prompt/get`, {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${token}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      templateId,
      sessionId
    })
  });

  if (!response.ok) {
    if (response.status === 401) {
      throw new Error('Authentication failed, please log in again');
    }
    if (response.status === 403) {
      throw new Error('You do not have permission to access this template, please check your subscription status');
    }
    if (response.status === 429) {
      throw new Error('Too many requests, please try again later');
    }
    throw new Error(`Failed to get template: ${response.statusText}`);
  }

  const data = await response.json();

  return {
    encryptedPrompt: data.encryptedPrompt,  // The encrypted Prompt
    sessionKey: data.sessionKey,            // The decryption key
    parameters: data.parameters,            // User parameters
    metadata: data.metadata                 // Metadata
  };
}
```

### Step 4: In-memory Decryption and Processing
```javascript
async function processPromptInMemory(encryptedData) {
  let decryptedPrompt = null;
  let finalPrompt = null;

  try {
    console.log('🔓 Decrypting template...');

    // 1. Decrypt the Prompt in memory
    decryptedPrompt = await decrypt(
      encryptedData.encryptedPrompt,
      encryptedData.sessionKey
    );

    // 2. Fill in user parameters
    console.log('📝 Filling in parameters...');
    finalPrompt = fillTemplate(decryptedPrompt, encryptedData.parameters);

    // 3. Use immediately (pass to the AI)
    console.log('🤖 Generating content...');
    const result = await executeWithAI(finalPrompt);

    return result;

  } finally {
    // 4. Force cleanup of memory (whether successful or not)
    if (decryptedPrompt) {
      clearSensitiveData(decryptedPrompt);
      decryptedPrompt = null;
    }
    if (finalPrompt) {
      clearSensitiveData(finalPrompt);
      finalPrompt = null;
    }

    // If Node.js supports it, trigger garbage collection
    if (global.gc) {
      global.gc();
    }
  }
}

// Clean up sensitive data
function clearSensitiveData(data) {
  if (typeof data === 'string') {
    // JavaScript cannot truly overwrite memory, but it can release the reference as soon as possible
    data = '';
    data = null;
  } else if (typeof data === 'object') {
    Object.keys(data).forEach(key => {
      data[key] = null;
      delete data[key];
    });
  }
}
```

### Step 5: Decryption Implementation (In-memory operations only)
```javascript
const crypto = require('crypto');

async function decrypt(encryptedData, sessionKey) {
  // Parse the encrypted data
  const parts = encryptedData.split(':');
  const iv = Buffer.from(parts[0], 'base64');
  const authTag = Buffer.from(parts[1], 'base64');
  const encrypted = Buffer.from(parts[2], 'base64');

  // Derive the decryption key
  const key = crypto.scryptSync(sessionKey, 'stardust-dreams', 32);

  // Create the decipher
  const decipher = crypto.createDecipheriv('aes-256-gcm', key, iv);
  decipher.setAuthTag(authTag);

  // Decrypt (the result is only in memory)
  let decrypted = decipher.update(encrypted, null, 'utf8');
  decrypted += decipher.final('utf8');

  return decrypted;
}
```

### Step 6: Template Filling
```javascript
function fillTemplate(template, parameters) {
  let filled = template;

  // Simple replacement of {{variable}}
  Object.keys(parameters).forEach(key => {
    const regex = new RegExp(`{{\\s*${key}\\s*}}`, 'g');
    filled = filled.replace(regex, parameters[key]);
  });

  // Process conditional blocks {{#if condition}}...{{/if}}
  filled = processConditionals(filled, parameters);

  // Process loops {{#each items}}...{{/each}}
  filled = processLoops(filled, parameters);

  return filled;
}
```

## Command Options

### Basic Usage
```bash
/stardust-use --session <sessionId>
```

### Advanced Options
- `--session <id>` - Specify the session ID (required)
- `--output <file>` - Save the generated result to a file (only saves the result, not the Prompt)
- `--format <type>` - Output format (text/json/markdown)
- `--stream` - Stream the output (display the generation progress in real time)
- `--retry` - Automatically retry on failure

## Usage Examples

### Example 1: Basic Usage
```
User: /stardust-use --session xyz789abc
Assistant: 📋 Session Information:
      - Template: Brainstorming Generator
      - Type: Creative Tool
      - Creation Time: 10:30:00
      - Expiration Time: 10:45:00

      🔐 Getting encrypted template...
      🔓 Decrypting template...
      📝 Filling in parameters...
      🤖 Generating content...

      ✨ Generation complete!

      【Idea 1】A programmer system in the cultivation world
      The protagonist is a modern programmer who travels to a cultivation world and discovers that he can use code
      to write spells and formations...

      【Idea 2】The Great Dao of Data Streams
      In this world, cultivation is about processing data streams, and breakthroughs are algorithm
      optimizations...
```

### Example 2: Saving the Result
```
User: /stardust-use --session xyz789abc --output ideas.md
Assistant: ✅ Content has been generated and saved to ideas.md
```

### Example 3: Streaming Output
```
User: /stardust-use --session xyz789abc --stream
Assistant: 🤖 Generating... [Displays the generated text in real time]
```

## Error Handling

| Error Type | Cause | Solution |
|---------|------|----------|
| SESSION_NOT_FOUND | Session does not exist | Regenerate on the web |
| SESSION_EXPIRED | Session has expired | Regenerate on the web |
| AUTH_REQUIRED | Not logged in | Log in using /stardust-auth |
| SUBSCRIPTION_REQUIRED | Paid subscription required | Upgrade subscription plan |
| RATE_LIMIT | Too many requests | Wait and retry |
| DECRYPT_FAILED | Decryption failed | Check session validity |

## Performance Optimization

### Memory Management
```javascript
// Use WeakMap to automatically manage memory
const promptCache = new WeakMap();

// Set a memory limit
const MAX_MEMORY = 50 * 1024 * 1024; // 50MB

// Monitor memory usage
if (process.memoryUsage().heapUsed > MAX_MEMORY) {
  console.warn('High memory usage, clearing cache...');
  global.gc && global.gc();
}
```

### Caching Strategy
- **Do not cache Prompts** - The decrypted Prompt is never cached.
- **Cache session information** - Basic session information is cached for 5 minutes.
- **Cache authentication token** - The token is encrypted and cached until it expires.

## Security Audit

All operations will be recorded in an audit log (without sensitive content):
```json
{
  "action": "use_template",
  "userId": "user123",
  "templateId": "brainstorm",
  "sessionId": "xyz789abc",
  "timestamp": "2024-01-20T10:35:00Z",
  "success": true,
  "duration": 3500
}
```

## Notes

1. **Do not attempt to save the Prompt** - This violates the terms of use.
2. **Do not share the SessionID** - Each session is tied to a specific user.
3. **Use it in a timely manner** - The session expires after 15 minutes.
4. **Use it reasonably** - Comply with the rate limits.
5. **Protect your account** - Do not share your authentication information.

## Next Steps

After generating the content, you can:
1. Continue editing and refining the generated content.
2. Use other templates to generate more ideas.
3. Use `/stardust-session` to manage your sessions.
4. Visit the web interface to view your usage history and statistics.
