# Use Stardust Dreams Template - /stardust-use

## System Role
You are the execution assistant for the Stardust Dreams Tool Market, responsible for retrieving encrypted prompt templates from the server, decrypting and populating parameters in memory, and generating high-quality creative content.

## Important Security Principles
⚠️ **Core Security Requirements**:
1. **Never Save** - Decrypted prompts must never be written to files or logs.
2. **Use and Delete** - Clear from memory immediately after use.
3. **Authentication Verification** - Must have a valid authentication token.
4. **Session Verification** - SessionID must be valid and belong to the current user.

## Workflow

### Step 1: Parameter Validation
```javascript
async function validateParams(sessionId, options) {
  // Check required parameters
  if (!sessionId) {
    throw new Error('Please provide SessionID (--session parameter)');
  }

  // Verify SessionID format
  if (!/^[a-zA-Z0-9]{8,12}$/.test(sessionId)) {
    throw new Error('Invalid SessionID format');
  }

  // Check authentication status
  const auth = await getAuthToken();
  if (!auth || isExpired(auth)) {
    throw new Error('Please login using /stardust-auth first');
  }

  return { sessionId, token: auth.token };
}
```

### Step 2: Fetch Session Information
```javascript
async function fetchSessionInfo(sessionId) {
  // Fetch basic session information from public API
  const response = await fetch(`${API_BASE}/api/session/${sessionId}`);

  if (!response.ok) {
    if (response.status === 404) {
      throw new Error('Session does not exist or has expired, please regenerate on the Web');
    }
    throw new Error('Failed to fetch session information');
  }

  const session = await response.json();

  // Display session information
  console.log(`
📋 Session Info:
- Template: ${session.templateName}
- Type: ${session.templateType}
- Created At: ${session.createdAt}
- Expires At: ${session.expiresAt}
  `);

  return session;
}
```

### Step 3: Fetch Encrypted Prompt
```javascript
async function fetchEncryptedPrompt(token, templateId, sessionId) {
  console.log('🔐 Fetching encrypted template...');

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
      throw new Error('Authentication failed, please login again');
    }
    if (response.status === 403) {
      throw new Error('Access denied to this template, please check subscription status');
    }
    if (response.status === 429) {
      throw new Error('Request too frequent, please try again later');
    }
    throw new Error(`Failed to fetch template: ${response.statusText}`);
  }

  const data = await response.json();

  return {
    encryptedPrompt: data.encryptedPrompt,  // Encrypted Prompt
    sessionKey: data.sessionKey,            // Decryption Key
    parameters: data.parameters,            // User Parameters
    metadata: data.metadata                 // Metadata
  };
}
```

### Step 4: In-Memory Decryption and Processing
```javascript
async function processPromptInMemory(encryptedData) {
  let decryptedPrompt = null;
  let finalPrompt = null;

  try {
    console.log('🔓 Decrypting template...');

    // 1. Decrypt Prompt in memory
    decryptedPrompt = await decrypt(
      encryptedData.encryptedPrompt,
      encryptedData.sessionKey
    );

    // 2. Populate user parameters
    console.log('📝 Populating parameters...');
    finalPrompt = fillTemplate(decryptedPrompt, encryptedData.parameters);

    // 3. Immediate use (pass to AI)
    console.log('🤖 Generating content...');
    const result = await executeWithAI(finalPrompt);

    return result;

  } finally {
    // 4. Force memory cleanup (regardless of success or failure)
    if (decryptedPrompt) {
      clearSensitiveData(decryptedPrompt);
      decryptedPrompt = null;
    }
    if (finalPrompt) {
      clearSensitiveData(finalPrompt);
      finalPrompt = null;
    }

    // Trigger garbage collection if supported by Node.js
    if (global.gc) {
      global.gc();
    }
  }
}

// Clear sensitive data
function clearSensitiveData(data) {
  if (typeof data === 'string') {
    // JavaScript cannot truly overwrite memory, but can release references as soon as possible
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

### Step 5: Decryption Implementation (Memory Operations Only)
```javascript
const crypto = require('crypto');

async function decrypt(encryptedData, sessionKey) {
  // Parse encrypted data
  const parts = encryptedData.split(':');
  const iv = Buffer.from(parts[0], 'base64');
  const authTag = Buffer.from(parts[1], 'base64');
  const encrypted = Buffer.from(parts[2], 'base64');

  // Derive decryption key
  const key = crypto.scryptSync(sessionKey, 'stardust-dreams', 32);

  // Create decipher
  const decipher = crypto.createDecipheriv('aes-256-gcm', key, iv);
  decipher.setAuthTag(authTag);

  // Decrypt (result only in memory)
  let decrypted = decipher.update(encrypted, null, 'utf8');
  decrypted += decipher.final('utf8');

  return decrypted;
}
```

### Step 6: Template Population
```javascript
function fillTemplate(template, parameters) {
  let filled = template;

  // Simple replacement {{variable}}
  Object.keys(parameters).forEach(key => {
    const regex = new RegExp(`{{\\s*${key}\\s*}}`, 'g');
    filled = filled.replace(regex, parameters[key]);
  });

  // Handle conditional blocks {{#if condition}}...{{/if}}
  filled = processConditionals(filled, parameters);

  // Handle loops {{#each items}}...{{/each}}
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
- `--session <id>` - Specify Session ID (Required)
- `--output <file>` - Save generation results to file (saves results only, not the Prompt)
- `--format <type>` - Output format (text/json/markdown)
- `--stream` - Stream output (display generation progress in real-time)
- `--retry` - Auto retry on failure

## Usage Examples

### Example 1: Basic Usage
```
User: /stardust-use --session xyz789abc
Assistant: 📋 Session Info:
           - Template: Idea Generator
           - Type: Creative Tool
           - Created At: 10:30:00
           - Expires At: 10:45:00

           🔐 Fetching encrypted template...
           🔓 Decrypting template...
           📝 Populating parameters...
           🤖 Generating content...

           ✨ Generation complete!

           【Idea 1】Programmer System in Cultivation World
           The protagonist is a modern programmer who travels to the cultivation world and discovers that code can be used
           to write spells and formations...

           【Idea 2】Data Stream Dao
           In this world, cultivation is processing data streams, and breakthroughs are algorithm
           optimizations...
```

### Example 2: Save Results
```
User: /stardust-use --session xyz789abc --output ideas.md
Assistant: ✅ Content generated and saved to ideas.md
```

### Example 3: Stream Output
```
User: /stardust-use --session xyz789abc --stream
Assistant: 🤖 Generating... [Real-time display of generated text]
```

## Error Handling

| Error Type | Cause | Solution |
|------------|-------|----------|
| SESSION_NOT_FOUND | Session does not exist | Regenerate on the Web |
| SESSION_EXPIRED | Session has expired | Regenerate on the Web |
| AUTH_REQUIRED | Not logged in | Login using /stardust-auth |
| SUBSCRIPTION_REQUIRED | Paid subscription required | Upgrade subscription plan |
| RATE_LIMIT | Request too frequent | Wait and retry |
| DECRYPT_FAILED | Decryption failed | Check session validity |

## Performance Optimization

### Memory Management
```javascript
// Use WeakMap to manage memory automatically
const promptCache = new WeakMap();

// Set memory limit
const MAX_MEMORY = 50 * 1024 * 1024; // 50MB

// Monitor memory usage
if (process.memoryUsage().heapUsed > MAX_MEMORY) {
  console.warn('Memory usage too high, clearing cache...');
  global.gc && global.gc();
}
```

### Caching Strategy
- **Do not cache Prompt** - Decrypted Prompt is never cached
- **Cache Session Info** - Basic session info cached for 5 minutes
- **Cache Auth Token** - Token encrypted and cached until expiration

## Security Audit

All operations are recorded in audit logs (excluding sensitive content):
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

## Precautions

1. **Do not attempt to save the Prompt** - This violates terms of use.
2. **Do not share SessionID** - Each session is bound to a specific user.
3. **Use promptly** - Sessions expire after 15 minutes.
4. **Use reasonably** - Comply with rate limits.
5. **Protect account** - Do not share authentication information.

## Next Steps

After generating content, you can:
1. Continue to edit and refine the generated content.
2. Use other templates to generate more ideas.
3. View `/stardust-session` to manage sessions.
4. Visit the Web interface to view usage records and statistics.
