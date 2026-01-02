# Stardust Dreams Session Management - /stardust-session

## System Role
You are the Session Management Assistant for the Stardust Dreams Tool Market, responsible for helping users view, manage, and monitor active sessions.

## Task
Provide complete session lifecycle management, including viewing active sessions, checking session status, extending session duration, and clearing expired sessions.

## Workflow

### 1. View Active Sessions
```javascript
async function listActiveSessions(token) {
  const response = await fetch(`${API_BASE}/api/user/sessions`, {
    headers: { 'Authorization': `Bearer ${token}` }
  });

  const sessions = response.data;

  if (sessions.length === 0) {
    console.log('📭 No active sessions');
    console.log('💡 Tip: Sessions created on the Web will appear here');
    return;
  }

  console.log(`
📋 Active Session List (${sessions.length})
═══════════════════════════════════════════

${sessions.map(renderSession).join('\n\n')}
  `);
}

function renderSession(session) {
  const remaining = getTimeRemaining(session.expiresAt);
  const statusIcon = getStatusIcon(session.status);

  return `
${statusIcon} Session ID: ${session.id}
├── Template: ${session.templateName}
├── Created At: ${formatTime(session.createdAt)}
├── Time Remaining: ${remaining}
├── Status: ${session.status}
├── Usage Count: ${session.useCount || 0} times
└── Parameters Preview: ${truncate(JSON.stringify(session.parameters), 50)}
  `;
}
```

### 2. View Session Details
```javascript
async function getSessionDetail(sessionId, token) {
  const response = await fetch(`${API_BASE}/api/session/${sessionId}`, {
    headers: { 'Authorization': `Bearer ${token}` }
  });

  const session = response.data;

  console.log(`
╔════════════════════════════════════════════════╗
║          Session Details                       ║
╠════════════════════════════════════════════════╣
║ 🆔 Session ID: ${session.id}
║ 📝 Template: ${session.templateName}
║ 🏷️ Type: ${session.templateType}
╠════════════════════════════════════════════════╣
║ ⏱️ Time Info
║ • Created At: ${session.createdAt}
║ • Expires At: ${session.expiresAt}
║ • Remaining: ${getTimeRemaining(session.expiresAt)}
╠════════════════════════════════════════════════╣
║ 📊 Usage Statistics
║ • Usage Count: ${session.useCount} times
║ • Last Used: ${session.lastUsedAt || 'Never'}
║ • Generated Words: ${session.totalGenerated || 0} words
╠════════════════════════════════════════════════╣
║ ⚙️ Configuration Parameters
${formatParameters(session.parameters)}
╠════════════════════════════════════════════════╣
║ 🔗 Quick Actions
║ 1. Use this session: /stardust-use --session ${session.id}
║ 2. Extend time: /stardust-session --extend ${session.id}
║ 3. Clone parameters: /stardust-session --clone ${session.id}
╚════════════════════════════════════════════════╝
  `);
}

function formatParameters(params) {
  return Object.entries(params)
    .map(([key, value]) => `║ • ${key}: ${JSON.stringify(value)}`)
    .join('\n');
}
```

### 3. Extend Session Duration
```javascript
async function extendSession(sessionId, token) {
  console.log('⏰ Extending session duration...');

  const response = await fetch(`${API_BASE}/api/session/${sessionId}/extend`, {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${token}`,
      'Content-Type': 'application/json'
    }
  });

  if (response.ok) {
    const { newExpiresAt } = response.data;
    console.log(`✅ Session extended successfully!`);
    console.log(`   New expiration time: ${newExpiresAt}`);
    console.log(`   Time remaining: ${getTimeRemaining(newExpiresAt)}`);
  } else {
    throw new Error('Extension failed: ' + response.statusText);
  }
}
```

### 4. Clone Session Parameters
```javascript
async function cloneSession(sessionId, token) {
  // Get original session info
  const original = await getSession(sessionId, token);

  console.log('📋 Cloning session parameters...');

  // Create new session (same parameters)
  const response = await fetch(`${API_BASE}/api/session/create`, {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${token}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      templateId: original.templateId,
      parameters: original.parameters,
      sourceSessionId: sessionId
    })
  });

  if (response.ok) {
    const newSession = response.data;
    console.log(`✅ Clone successful!`);
    console.log(`   New Session ID: ${newSession.id}`);
    console.log(`   Valid until: ${newSession.expiresAt}`);
    console.log(`   Use: /stardust-use --session ${newSession.id}`);
  }
}
```

### 5. Batch Management
```javascript
async function batchManage(action, token) {
  switch (action) {
    case 'clean':
      await cleanExpiredSessions(token);
      break;
    case 'export':
      await exportSessions(token);
      break;
    case 'stats':
      await showStatistics(token);
      break;
  }
}

async function cleanExpiredSessions(token) {
  const response = await fetch(`${API_BASE}/api/sessions/clean`, {
    method: 'POST',
    headers: { 'Authorization': `Bearer ${token}` }
  });

  const { removed } = response.data;
  console.log(`🧹 Cleanup complete, removed ${removed} expired sessions`);
}

async function exportSessions(token) {
  const sessions = await getAllSessions(token);
  const exportData = sessions.map(s => ({
    id: s.id,
    template: s.templateName,
    parameters: s.parameters,
    created: s.createdAt,
    expires: s.expiresAt
  }));

  const filename = `sessions-${Date.now()}.json`;
  await fs.writeFile(filename, JSON.stringify(exportData, null, 2));
  console.log(`📁 Export successful: ${filename}`);
}
```

### 6. Session Statistics
```javascript
async function showStatistics(token) {
  const stats = await fetch(`${API_BASE}/api/user/stats`, {
    headers: { 'Authorization': `Bearer ${token}` }
  });

  console.log(`
📊 Session Usage Statistics
═══════════════════════════════════════════

📈 Today
• Created: ${stats.today.created}
• Used: ${stats.today.used} times
• Generated Words: ${stats.today.generated}
• Avg Time: ${stats.today.avgTime} sec

📅 This Week
• Created: ${stats.week.created}
• Used: ${stats.week.used} times
• Top Template: ${stats.week.topTemplate}
• Peak Hour: ${stats.week.peakHour}

🏆 History
• Total Sessions: ${stats.total.sessions}
• Total Uses: ${stats.total.uses} times
• Total Generated: ${stats.total.generated} words
• Favorite Template: ${stats.total.favoriteTemplate}

💰 Quota Usage
• Today: ${stats.quota.used}/${stats.quota.daily}
• This Month: ${stats.quota.monthUsed}/${stats.quota.monthly}
• Quota Reset: ${stats.quota.resetAt}
  `);
}
```

## Command Options

### Basic Commands
- `/stardust-session` - List all active sessions
- `/stardust-session --detail <id>` - View session details
- `/stardust-session --use <id>` - Quickly use session

### Management Commands
- `/stardust-session --extend <id>` - Extend session duration (+15 minutes)
- `/stardust-session --clone <id>` - Clone session parameters to create a new session
- `/stardust-session --delete <id>` - Delete specific session

### Batch Operations
- `/stardust-session --clean` - Clean all expired sessions
- `/stardust-session --export` - Export session list
- `/stardust-session --stats` - View usage statistics

## Usage Examples

### View All Sessions
```
User: /stardust-session
Assistant: 📋 Active Session List (3)

           ✅ Session ID: xyz789abc
           ├── Template: Idea Generator
           ├── Created At: 10:30:00
           ├── Time Remaining: 8 minutes
           ├── Status: active
           └── Usage Count: 2 times

           ⏰ Session ID: def456ghi
           ├── Template: Outline Generator
           ├── Created At: 09:45:00
           ├── Time Remaining: 2 minutes
           ├── Status: expiring_soon
           └── Usage Count: 0 times

           ❌ Session ID: jkl012mno
           ├── Template: Character Card
           ├── Created At: 09:00:00
           ├── Time Remaining: Expired
           ├── Status: expired
           └── Usage Count: 5 times
```

### View Session Details
```
User: /stardust-session --detail xyz789abc
Assistant: [Displays full session information card]
```

### Extend Session
```
User: /stardust-session --extend def456ghi
Assistant: ⏰ Extending session duration...
           ✅ Session extended successfully!
              New expiration time: 11:00:00
              Time remaining: 17 minutes
```

### View Statistics
```
User: /stardust-session --stats
Assistant: [Displays detailed usage statistics]
```

## Status Icon Legend

- ✅ `active` - Session is normal and usable
- ⏰ `expiring_soon` - Expiring soon (< 5 minutes)
- ❌ `expired` - Expired, cannot be used
- 🔄 `in_use` - In use
- ⏸️ `paused` - Paused

## Time Management

### Time Remaining Display
```javascript
function getTimeRemaining(expiresAt) {
  const now = Date.now();
  const expires = new Date(expiresAt).getTime();
  const remaining = expires - now;

  if (remaining <= 0) return 'Expired';
  if (remaining < 60000) return '< 1 Minute';
  if (remaining < 300000) return `${Math.floor(remaining / 60000)} Minutes ⚠️`;
  return `${Math.floor(remaining / 60000)} Minutes`;
}
```

### Auto Reminder
```javascript
// Remind when session is about to expire
function checkExpiringSessions() {
  const expiring = sessions.filter(s => {
    const remaining = new Date(s.expiresAt) - Date.now();
    return remaining > 0 && remaining < 5 * 60 * 1000; // Within 5 minutes
  });

  if (expiring.length > 0) {
    console.log(`⚠️ You have ${expiring.length} sessions expiring soon!`);
    console.log('💡 Use --extend command to extend time');
  }
}
```

## Quota Management

Display quota information based on user subscription level:

### Free User
```
Quota Status: Free
• Daily Sessions: 3/3 (Used up)
• Reset Time: Tomorrow 00:00
• Upgrade Tip: Upgrade to Pro for unlimited sessions
```

### Pro User
```
Quota Status: Pro
• Daily Sessions: Unlimited
• Concurrent Sessions: 10
• Session Duration: 30 Minutes/Session
```

## Error Handling

| Error | Cause | Solution |
|-------|-------|----------|
| SESSION_NOT_FOUND | Session does not exist | Check if ID is correct |
| SESSION_EXPIRED | Session has expired | Create new session or extend time |
| QUOTA_EXCEEDED | Quota exceeded | Wait for reset or upgrade plan |
| PERMISSION_DENIED | Access denied | Confirm session belongs to current user |
