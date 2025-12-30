# Dreams Server Integration Plan

## Document Information

- **Version**: v1.0.0
- **Creation Date**: 2025-10-14
- **Author**: Novel Writer Team
- **Status**: Planning Phase

## Table of Contents

1. [Background](#1-background)
2. [Integration Goals](#2-integration-goals)
3. [Current State of the Dreams System](#3-current-state-of-the-dreams-system)
4. [Integration Architecture Design](#4-integration-architecture-design)
5. [Web UI Design](#5-web-ui-design)
6. [API Design](#6-api-design)
7. [Session Sync Mechanism](#7-session-sync-mechanism)
8. [Phased Implementation Plan](#8-phased-implementation-plan)
9. [Technical Challenges and Solutions](#9-technical-challenges-and-solutions)
10. [Success Metrics](#10-success-metrics)

---

## 1. Background

### 1.1 Current Situation

**novel-writer-cn CLI**:
- ✅ Complete specification-driven system (specification.md, constitution.md)
- ✅ Character, scene, and plot tracking system
- ✅ Slash Command writing flow (/write)
- 🚧 Chapter configuration system (new in this iteration)
- ❌ No web-based visual interface

**Dreams Server**:
- ✅ Next.js 14 full-stack platform
- ✅ YAML form system
- ✅ Session management mechanism
- ✅ tRPC type-safe API
- ✅ Project management, format conversion, tool marketplace
- ❌ No chapter configuration management

### 1.2 Integration Value

| Feature | Pure CLI Solution | Dreams Integration Solution | Value Added |
|---|---|---|---|
| Config Creation | Command-line interactive/manual YAML editing | Visual web form | ⭐⭐⭐⭐⭐ |
| Preset Browsing | `novel preset list` | Visual cards + preview | ⭐⭐⭐⭐ |
| Config Management | Local file system | Cloud storage + version control | ⭐⭐⭐⭐ |
| Team Collaboration | Git sharing | Real-time cloud sync | ⭐⭐⭐ |
| Mobile Access | Not supported | Responsive Web UI | ⭐⭐⭐ |

---

## 2. Integration Goals

### 2.1 Short-term Goals (v1.0)

1. **Web Config Form**: Provide a visual interface for creating chapter configurations.
2. **Preset Marketplace**: Display and search for preset templates.
3. **Session Sync**: Sync configurations from the web to the local CLI.
4. **Basic CRUD**: Create, read, update, and delete chapter configurations.

### 2.2 Long-term Goals (v2.0+)

1. **Cloud Storage**: Cloud backup for configuration files, multi-device sync.
2. **Version Control**: Version history and rollback for configuration files.
3. **Team Collaboration**: Shared configurations, commenting, and approval workflows.
4. **Intelligent Recommendations**: Recommend suitable presets and configurations based on the project type.
5. **Config Template Marketplace**: Users can share and sell custom configuration templates.

---

## 3. Current State of the Dreams System

### 3.1 YAML Form System

Dreams already has a complete YAML form infrastructure:

```yaml
# forms/intro.yaml example
fields:
  - id: genre
    type: select
    label: Genre
    options:
      - {value: xuanhuan, label: Xuanhuan}
      - {value: dushi, label: Urban}
    required: true

  - id: protagonist_name
    type: text
    label: Protagonist Name
    required: true

  - id: special_requirements
    type: textarea
    label: Special Requirements
    rows: 5
```

**Existing Capabilities**:
- ✅ Multiple field types (text, textarea, select, radio, checkbox)
- ✅ Form validation
- ✅ Session storage
- ✅ CLI integration interface

### 3.2 CLI Integration Mechanism

The integration flow between Dreams and the CLI:

```
Web form submission → Session storage → CLI pulls → Local prompt → AI executes
     ↓
  (session-id)
```

Existing CLI commands:
```bash
novel intro --session {session-id}   # Pull data from Dreams
novel write --session {session-id}    # Write using a Dreams configuration
```

### 3.3 Database Architecture

Dreams uses Prisma + MySQL:

```prisma
model User {
  id        String   @id @default(cuid())
  email     String   @unique
  books     Book[]
  sessions  Session[]
}

model Book {
  id          String    @id @default(cuid())
  title       String
  userId      String
  user        User      @relation(...)
  chapters    Chapter[]
}

model Session {
  id          String   @id @default(cuid())
  userId      String
  data        Json
  createdAt   DateTime
}
```

---

## 4. Integration Architecture Design

### 4.1 Overall Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     Dreams Web UI                            │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │ Chapter Config Form │  │ Preset Marketplace │  │ Config Management │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
└─────────────────────────────────────────────────────────────┘
                              ▲
                              │ tRPC API (Type-safe)
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                  Dreams Backend (Next.js API)                │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │ ChapterConfig │  │ PresetManager│  │  Session     │      │
│  │   Service     │  │   Service    │  │  Service     │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
│                    ┌──────────────┐                          │
│                    │   Database   │ (MySQL + Prisma)         │
│                    └──────────────┘                          │
└─────────────────────────────────────────────────────────────┘
                              ▲
                              │ Session ID / API Key
                              ▼
┌─────────────────────────────────────────────────────────────┐
│               novel-writer-cn CLI                            │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │ chapter-config│  │ write.md     │  │  Local YAML  │      │
│  │   commands    │  │  (slash cmd) │  │   Storage    │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
└─────────────────────────────────────────────────────────────┘
```

### 4.2 Data Flow

#### 4.2.1 Web Configuration Creation Flow

```
1. User fills out the chapter configuration form on the Dreams website.
   ↓
2. Frontend submits to the tRPC API: chapterConfig.create().
   ↓
3. Backend validates the data and stores it in the database.
   ↓
4. A session is created, and the session-id is returned to the frontend.
   ↓
5. The frontend displays a CLI command prompt:
   novel chapter-config pull --session {session-id}
   ↓
6. The user executes the command in the CLI to pull the configuration locally.
   ↓
7. The configuration is saved locally as .novel/chapters/chapter-X-config.yaml.
   ↓
8. The user executes /write, which automatically loads the local configuration file.
```

#### 4.2.2 CLI Push Configuration Flow (Bidirectional Sync)

```
1. User creates a configuration in the CLI:
   novel chapter-config create 5 --interactive
   ↓
2. The configuration is saved locally to chapter-5-config.yaml.
   ↓
3. The user executes the push command:
   novel chapter-config push 5
   ↓
4. The CLI reads the local YAML and calls the Dreams API.
   ↓
5. Dreams stores it in the database, linking it to the user's project.
   ↓
6. A config-id is returned, and the CLI updates the local metadata.
```

### 4.3 Storage Strategy

| Storage Location | Data Type | Purpose | Sync Method |
|---|---|---|---|
| Local CLI File | chapter-X-config.yaml | Read directly during writing | Primary storage |
| Dreams Database | ChapterConfig record | Cloud backup, cross-device sync | pull/push |
| Dreams Session | Temporary config data | Web→CLI transfer | session-id |
| CLI Metadata | .novel/meta/sync.json | Record sync status | Maintained locally |

**Sync Metadata Example**:
```json
{
  "chapters": {
    "5": {
      "local_path": ".novel/chapters/chapter-5-config.yaml",
      "remote_id": "cuid_abc123",
      "last_synced": "2025-10-14T10:30:00Z",
      "hash": "sha256_hash_value"
    }
  },
  "last_pull": "2025-10-14T08:00:00Z"
}
```

---

## 5. Web UI Design

### 5.1 Page Structure

#### 5.1.1 Chapter Configuration Creation Page

**Route**: `/app/(dashboard)/books/[bookId]/chapters/[chapterId]/config`

**Layout**:
```
┌────────────────────────────────────────────────────────────┐
│ 【Back】 Chapter 5 Config - First Glimmer of Talent    【Save】 │
├────────────────────────────────────────────────────────────┤
│                                                              │
│ ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│ │  Basic Info  │  │ Char & Scene │  │  Plot & Style│      │
│ └──────────────┘  └──────────────┘  └──────────────┘      │
│                                                              │
│ ╔════════════════ Basic Information ═══════════════╗         │
│ ║ Chapter No.: [  5  ]  Chapter Title: [First Glimmer of Talent] ║ │
│ ║                                           ║               │
│ ║ Use Preset: [Select Preset ▼]  or  [Start from Scratch]   ║ │
│ ╚═══════════════════════════════════════════╝               │
│                                                              │
│ ╔═════════════ Appearing Characters ══════════════╗         │
│ ║ [+ Add Character]                               ║         │
│ ║                                           ║               │
│ ║ ┌─ Char 1: Lin Chen (Protagonist)───────────┐ ║         │
│ ║ │ Focus: ● High  ○ Medium  ○ Low          │ ║         │
│ ║ │ State Changes:                         │ ║         │
│ ║ │ • Injured (minor)                     │ ║         │
│ ║ │ • Power increased                       │ ║         │
│ ║ └───────────────────────────────────┘     ║               │
│ ╚═══════════════════════════════════════════╝               │
│                                                              │
│ ╔═════════════════ Scene Setting ════════════════╗         │
│ ║ Location: [Abandoned Factory ▼] Time: [2:00 AM]      ║   │
│ ║ Weather: [Heavy Rain ▼]                           ║       │
│ ║ Atmosphere: [Tense ▼]                           ║       │
│ ╚═══════════════════════════════════════════╝               │
│                                                              │
│ ╔══════════════════ Plot Type ═══════════════════╗         │
│ ║ Type: [Conflict/Combat ▼]                     ║         │
│ ║                                           ║               │
│ ║ Plot Summary:                               ║         │
│ ║ ┌───────────────────────────────────┐     ║               │
│ ║ │ The protagonist's first direct confrontation... │ ║   │
│ ║ └───────────────────────────────────┘     ║               │
│ ║                                           ║               │
│ ║ Key Plot Points:                            ║         │
│ ║ • The villain launches a surprise attack      ║         │
│ ║ • An intense fight                        ║         │
│ ║ [+ Add Plot Point]                          ║         │
│ ╚═══════════════════════════════════════════╝               │
│                                                              │
│ ╔═════════════════ Writing Style ════════════════╗         │
│ ║ Pace: ● Fast  ○ Medium  ○ Slow                  ║         │
│ ║ Sentence Length: ● Short  ○ Medium  ○ Long      ║         │
│ ║ Focus: ☑ Action  ☐ Dialogue  ☐ Psychology  ☐ Environment │
│ ║ Tone: [Serious ▼]                           ║         │
│ ╚═══════════════════════════════════════════╝               │
│                                                              │
│ ╔══════════════ Word Count Requirement ════════════╗         │
│ ║ Target Word Count: [ 3500 ]                     ║         │
│ ║ Min Word Count: [ 3000 ]  Max Word Count: [ 4000 ] ║       │
│ ╚═══════════════════════════════════════════╝               │
│                                                              │
│ ╔══════════════ Special Requirements ═════════════╗         │
│ ║ ┌───────────────────────────────────┐     ║               │
│ ║ │ Action scene writing requirements:          │ ║         │
│ ║ │ - Mainly short sentences, 15-25 words each. │ ║         │
│ ║ │ ...                                 │ ║         │
│ ║ └───────────────────────────────────┘     ║               │
│ ╚═══════════════════════════════════════════╝               │
│                                                              │
│          ┌──────────┐  ┌────────┐  ┌──────────┐             │
│          │ Save Draft │  │ Preview  │  │ Generate CLI │             │
│          └──────────┘  └────────┘  └──────────┘             │
└────────────────────────────────────────────────────────────┘
```

#### 5.1.2 Preset Marketplace Page

**Route**: `/app/(dashboard)/presets`

**Layout**:
```
┌────────────────────────────────────────────────────────────┐
│ Preset Template Marketplace           [Search Box] [Filter ▼] │
├────────────────────────────────────────────────────────────┤
│                                                              │
│ Categories: [All] [Action] [Dialogue] [Emotion] [Suspense] [...] │
│                                                              │
│ ┌─────────────┐  ┌─────────────┐  ┌─────────────┐         │
│ │ Intense Action Scene │  │ Dialogue-Heavy Scene │  │ Emotional Confession │ │
│ │             │  │             │  │             │         │
│ │ Suitable for high- │  │ Suitable for negotiation, │ │ Suitable for confessions, │ │
│ │ intensity action │  │ debate, and other │ │ reconciliation, and other │ │
│ │ like fights and │  │ dialogue-heavy scenes. │ │ emotion-focused scenes. │ │
│ │ chases.       │  │             │  │             │         │
│ │             │  │             │  │             │         │
│ │ ⭐4.8 · 1.2k │  │ ⭐4.7 · 890 │  │ ⭐4.9 · 2.1k │         │
│ │ [Use Preset]  │  │ [Use Preset]  │  │ [Use Preset]  │         │
│ └─────────────┘  └─────────────┘  └─────────────┘         │
│                                                              │
│ ┌─────────────┐  ┌─────────────┐  ┌─────────────┐         │
│ │ Suspense Building Scene │ │ Relationship Dev Scene │ │ Ability Showcase Scene │ │
│ │ ...          │  │ ...          │  │ ...          │         │
│ └─────────────┘  └─────────────┘  └─────────────┘         │
│                                                              │
└────────────────────────────────────────────────────────────┘
```

Clicking on a preset card opens a details dialog:
```
┌────────────────────────────────────────────────────────────┐
│ Intense Action Scene                                   【Close ×】 │
├────────────────────────────────────────────────────────────┤
│ Version: v1.0.0                                             │
│ Author: Novel Writer Official                               │
│ Category: Scene Preset                                      │
│                                                              │
│ 📝 Description:                                                │
│ Suitable for high-intensity action descriptions like fights and chases, │
│ with a fast pace, short sentences, and dense action.           │
│                                                              │
│ ⚙️ Default Configuration:                                       │
│ • Pace: Fast                                                │
│ • Sentence Length: Short                                    │
│ • Focus: Action                                             │
│ • Target Word Count: 3000 words                             │
│                                                              │
│ 📚 Recommended Scenarios:                                      │
│ • Conflict/Combat                                           │
│ • Climax                                                    │
│ • Chase scenes                                              │
│                                                              │
│ 🎭 Compatible Genres:                                          │
│ Xuanhuan · Wuxia · Urban Fantasy · Sci-fi Mecha           │
│                                                              │
│ 💡 Usage Tips:                                                 │
│ • Suitable for the climax of a chapter; should not be overused. │
│ • Recommended for shorter chapters (2000-3500 words).         │
│ • Needs setup and follow-up chapters.                        │
│                                                              │
│          ┌────────────┐  ┌───────────┐                       │
│          │ Use This Preset │  │ View Example │                       │
│          └────────────┘  └───────────┘                       │
└────────────────────────────────────────────────────────────┘
```

#### 5.1.3 Configuration Management Page

**Route**: `/app/(dashboard)/books/[bookId]/configs`

**Layout**:
```
┌────────────────────────────────────────────────────────────┐
│ Chapter Config Management - "Rebirth of the Urban Immortal" [+ New Config] │
├────────────────────────────────────────────────────────────┤
│ Filter: [All] [Completed] [Draft] [Not Synced]   Sort by: [Chapter No. ▼] │
│                                                              │
│ ┌──────────────────────────────────────────────────────┐   │
│ │ Chapter 5: First Glimmer of Talent           [Edit] [Delete] │ │
│ │ Type: Ability Showcase | Word Count: 3000 | Status: ✓ Synced │ │
│ │ Created: 2025-10-14 | Last Modified: 2025-10-14 10:30      │ │
│ └──────────────────────────────────────────────────────┘   │
│                                                              │
│ ┌──────────────────────────────────────────────────────┐   │
│ │ Chapter 8: First Confrontation             [Edit] [Delete] │ │
│ │ Type: Conflict/Combat | Word Count: 3500 | Status: ⚠ Not Synced │ │
│ │ Created: 2025-10-14 | Last Modified: 2025-10-14 15:30      │ │
│ │ [Sync to CLI]                                            │   │
│ └──────────────────────────────────────────────────────┘   │
│                                                              │
│ ┌──────────────────────────────────────────────────────┐   │
│ │ Chapter 15: Hearts Aligned                 [Edit] [Delete] │ │
│ │ Type: Relationship Development | Word Count: 2500 | Status: 📝 Draft │ │
│ │ Created: 2025-10-14 | Last Modified: 2025-10-14 16:00      │ │
│ └──────────────────────────────────────────────────────┘   │
│                                                              │
└────────────────────────────────────────────────────────────┘
```

### 5.2 YAML Form Mapping

Dreams' YAML form system can directly map to the chapter configuration:

```yaml
# forms/chapter-config.yaml
form_id: chapter-config
title: Chapter Configuration
version: 1.0.0

fields:
  # Basic Information
  - id: chapter
    type: number
    label: Chapter Number
    required: true
    min: 1

  - id: title
    type: text
    label: Chapter Title
    required: true

  - id: preset_id
    type: select
    label: Use Preset (Optional)
    options_source: api  # Dynamically load preset list from API
    endpoint: /api/presets/list

  # Characters Section
  - id: characters
    type: array
    label: Appearing Characters
    min_items: 1
    item_schema:
      - id: character_id
        type: select
        label: Select Character
        options_source: api
        endpoint: /api/characters/list?bookId={bookId}

      - id: focus
        type: radio
        label: Focus
        options:
          - {value: high, label: High}
          - {value: medium, label: Medium}
          - {value: low, label: Low}
        default: medium

      - id: state_changes
        type: array
        label: State Changes
        item_type: text

  # Scene Section
  - id: scene.location_id
    type: select
    label: Location
    options_source: api
    endpoint: /api/locations/list?bookId={bookId}

  - id: scene.time
    type: text
    label: Time
    placeholder: e.g., 10:00 AM, Dusk, Dawn

  - id: scene.weather
    type: text
    label: Weather
    placeholder: e.g., Sunny, Heavy Rain, Overcast

  - id: scene.atmosphere
    type: select
    label: Atmosphere
    options:
      - {value: tense, label: Tense}
      - {value: relaxed, label: Relaxed}
      - {value: sad, label: Sad}
      - {value: exciting, label: Exciting}
      - {value: mysterious, label: Mysterious}

  # Plot Section
  - id: plot.type
    type: select
    label: Plot Type
    required: true
    options:
      - {value: ability_showcase, label: Ability Showcase}
      - {value: relationship_dev, label: Relationship Development}
      - {value: conflict_combat, label: Conflict/Combat}
      - {value: mystery_suspense, label: Suspense/Foreshadowing}
      - {value: plot_twist, label: Plot Twist}
      - {value: climax, label: Climax}

  - id: plot.summary
    type: textarea
    label: Plot Summary
    required: true
    rows: 3
    placeholder: Summarize the main plot of this chapter in one or two sentences.

  - id: plot.key_points
    type: array
    label: Key Plot Points
    item_type: text
    min_items: 1

  # Writing Style
  - id: style.pace
    type: radio
    label: Pace
    options:
      - {value: fast, label: Fast}
      - {value: medium, label: Medium}
      - {value: slow, label: Slow}
    default: medium

  - id: style.sentence_length
    type: radio
    label: Sentence Length
    options:
      - {value: short, label: Short}
      - {value: medium, label: Medium}
      - {value: long, label: Long}
    default: medium

  - id: style.focus
    type: checkbox
    label: Descriptive Focus (Multi-select)
    options:
      - {value: action, label: Action}
      - {value: dialogue, label: Dialogue}
      - {value: psychology, label: Psychology}
      - {value: environment, label: Environment}
    min_selections: 1

  - id: style.tone
    type: select
    label: Tone
    options:
      - {value: light, label: Light}
      - {value: serious, label: Serious}
      - {value: dark, label: Dark}
      - {value: humorous, label: Humorous}

  # Word Count Requirements
  - id: wordcount.target
    type: number
    label: Target Word Count
    required: true
    min: 1000
    max: 10000
    default: 3000

  - id: wordcount.min
    type: number
    label: Minimum Word Count
    required: true
    min: 500

  - id: wordcount.max
    type: number
    label: Maximum Word Count
    required: true
    max: 15000

  # Special Requirements
  - id: special_requirements
    type: textarea
    label: Special Writing Requirements
    rows: 8
    placeholder: Detailed writing guidance and notes.

validation:
  # Custom validation rules
  - rule: wordcount.min < wordcount.target < wordcount.max
    message: Target word count must be between the minimum and maximum word count.
```

### 5.3 UI Component Reuse

Dreams' existing shadcn/ui components can be used directly:

- `<Form>` / `<FormField>` - Base form components
- `<Input>` / `<Textarea>` - Input fields
- `<Select>` - Dropdown selector
- `<RadioGroup>` - Radio button group
- `<Checkbox>` - Checkbox
- `<Button>` - Button
- `<Card>` - Card container
- `<Dialog>` - Dialog modal
- `<Tabs>` - Tabs

**New Components**:
- `<ArrayFieldEditor>` - Array field editor (for character lists, plot point lists)
- `<PresetSelector>` - Preset selector (with preview functionality)
- `<ConfigPreview>` - Configuration preview component (displays in YAML format)

---

## 6. API Design

### 6.1 tRPC Router: chapterConfigRouter

```typescript
// server/api/routers/chapterConfig.ts

import { z } from 'zod';
import { router, protectedProcedure } from '../trpc';

// Zod Schema (corresponds to JSON Schema)
const ChapterConfigSchema = z.object({
  chapter: z.number().int().min(1),
  title: z.string().min(1),
  characters: z.array(z.object({
    id: z.string(),
    name: z.string(),
    focus: z.enum(['high', 'medium', 'low']),
    state_changes: z.array(z.string()).optional(),
  })).optional(),
  scene: z.object({
    location_id: z.string().optional(),
    location_name: z.string().optional(),
    time: z.string().optional(),
    weather: z.string().optional(),
    atmosphere: z.enum([
      'tense', 'relaxed', 'sad', 'exciting',
      'mysterious', 'romantic'
    ]).optional(),
  }).optional(),
  plot: z.object({
    type: z.enum([
      'ability_showcase', 'relationship_dev', 'conflict_combat',
      'mystery_suspense', 'plot_twist', 'climax', 'transition'
    ]),
    summary: z.string(),
    key_points: z.array(z.string()),
    plotlines: z.array(z.string()).optional(),
    foreshadowing: z.array(z.object({
      id: z.string(),
      content: z.string(),
    })).optional(),
  }),
  style: z.object({
    pace: z.enum(['fast', 'medium', 'slow']),
    sentence_length: z.enum(['short', 'medium', 'long']),
    focus: z.array(z.enum([
      'action', 'dialogue', 'psychology', 'environment'
    ])),
    tone: z.enum(['light', 'serious', 'dark', 'humorous']),
  }).optional(),
  wordcount: z.object({
    target: z.number().int().min(1000).max(10000),
    min: z.number().int().min(500),
    max: z.number().int().max(15000),
  }),
  special_requirements: z.string().optional(),
  preset_used: z.string().optional(),
});

export const chapterConfigRouter = router({
  // Create configuration
  create: protectedProcedure
    .input(z.object({
      bookId: z.string(),
      config: ChapterConfigSchema,
    }))
    .mutation(async ({ ctx, input }) => {
      const { bookId, config } = input;
      const userId = ctx.session.user.id;

      // 1. Verify book ownership
      const book = await ctx.db.book.findUnique({
        where: { id: bookId },
      });

      if (!book || book.userId !== userId) {
        throw new Error('Book not found or unauthorized');
      }

      // 2. Check for chapter number conflict
      const existing = await ctx.db.chapterConfig.findUnique({
        where: {
          bookId_chapter: {
            bookId,
            chapter: config.chapter,
          },
        },
      });

      if (existing) {
        throw new Error(`Config for chapter ${config.chapter} already exists`);
      }

      // 3. Create config record
      const chapterConfig = await ctx.db.chapterConfig.create({
        data: {
          bookId,
          chapter: config.chapter,
          title: config.title,
          configData: config, // Store full config in a JSON field
          createdAt: new Date(),
          updatedAt: new Date(),
        },
      });

      // 4. Create session (for CLI to pull)
      const session = await ctx.db.session.create({
        data: {
          userId,
          type: 'chapter-config',
          data: {
            configId: chapterConfig.id,
            bookId,
            chapter: config.chapter,
            config,
          },
          expiresAt: new Date(Date.now() + 24 * 60 * 60 * 1000), // 24 hours
        },
      });

      return {
        configId: chapterConfig.id,
        sessionId: session.id,
        cliCommand: `novel chapter-config pull --session ${session.id}`,
      };
    }),

  // Get config list
  list: protectedProcedure
    .input(z.object({
      bookId: z.string(),
    }))
    .query(async ({ ctx, input }) => {
      const { bookId } = input;
      const userId = ctx.session.user.id;

      // Verify permissions
      const book = await ctx.db.book.findUnique({
        where: { id: bookId },
      });

      if (!book || book.userId !== userId) {
        throw new Error('Unauthorized');
      }

      // Get config list
      const configs = await ctx.db.chapterConfig.findMany({
        where: { bookId },
        orderBy: { chapter: 'asc' },
        select: {
          id: true,
          chapter: true,
          title: true,
          configData: true,
          createdAt: true,
          updatedAt: true,
          syncStatus: true,
        },
      });

      return configs.map(config => ({
        id: config.id,
        chapter: config.chapter,
        title: config.title,
        plotType: (config.configData as any).plot?.type,
        wordcount: (config.configData as any).wordcount?.target,
        syncStatus: config.syncStatus,
        createdAt: config.createdAt,
        updatedAt: config.updatedAt,
      }));
    }),

  // Get a single config
  get: protectedProcedure
    .input(z.object({
      configId: z.string(),
    }))
    .query(async ({ ctx, input }) => {
      const { configId } = input;
      const userId = ctx.session.user.id;

      const config = await ctx.db.chapterConfig.findUnique({
        where: { id: configId },
        include: { book: true },
      });

      if (!config || config.book.userId !== userId) {
        throw new Error('Config not found or unauthorized');
      }

      return {
        id: config.id,
        bookId: config.bookId,
        chapter: config.chapter,
        title: config.title,
        config: config.configData,
        createdAt: config.createdAt,
        updatedAt: config.updatedAt,
      };
    }),

  // Update config
  update: protectedProcedure
    .input(z.object({
      configId: z.string(),
      config: ChapterConfigSchema.partial(),
    }))
    .mutation(async ({ ctx, input }) => {
      const { configId, config } = input;
      const userId = ctx.session.user.id;

      // Verify permissions
      const existing = await ctx.db.chapterConfig.findUnique({
        where: { id: configId },
        include: { book: true },
      });

      if (!existing || existing.book.userId !== userId) {
        throw new Error('Unauthorized');
      }

      // Merge config
      const mergedConfig = {
        ...(existing.configData as any),
        ...config,
      };

      // Update record
      const updated = await ctx.db.chapterConfig.update({
        where: { id: configId },
        data: {
          configData: mergedConfig,
          updatedAt: new Date(),
          syncStatus: 'pending', // Mark as needing sync
        },
      });

      return {
        configId: updated.id,
        config: updated.configData,
      };
    }),

  // Delete config
  delete: protectedProcedure
    .input(z.object({
      configId: z.string(),
    }))
    .mutation(async ({ ctx, input }) => {
      const { configId } = input;
      const userId = ctx.session.user.id;

      // Verify permissions
      const existing = await ctx.db.chapterConfig.findUnique({
        where: { id: configId },
        include: { book: true },
      });

      if (!existing || existing.book.userId !== userId) {
        throw new Error('Unauthorized');
      }

      // Delete record
      await ctx.db.chapterConfig.delete({
        where: { id: configId },
      });

      return { success: true };
    }),

  // Pull config from session (called by CLI)
  pullFromSession: protectedProcedure
    .input(z.object({
      sessionId: z.string(),
    }))
    .query(async ({ ctx, input }) => {
      const { sessionId } = input;
      const userId = ctx.session.user.id;

      // Get session
      const session = await ctx.db.session.findUnique({
        where: { id: sessionId },
      });

      if (!session || session.userId !== userId) {
        throw new Error('Session not found or unauthorized');
      }

      if (session.expiresAt < new Date()) {
        throw new Error('Session expired');
      }

      // Return config data
      const sessionData = session.data as any;
      return {
        bookId: sessionData.bookId,
        chapter: sessionData.chapter,
        config: sessionData.config,
      };
    }),

  // Push config to cloud (called by CLI)
  push: protectedProcedure
    .input(z.object({
      bookId: z.string(),
      chapter: z.number(),
      config: ChapterConfigSchema,
      localHash: z.string(), // Hash of the local file
    }))
    .mutation(async ({ ctx, input }) => {
      const { bookId, chapter, config, localHash } = input;
      const userId = ctx.session.user.id;

      // Verify permissions
      const book = await ctx.db.book.findUnique({
        where: { id: bookId },
      });

      if (!book || book.userId !== userId) {
        throw new Error('Unauthorized');
      }

      // Upsert config
      const chapterConfig = await ctx.db.chapterConfig.upsert({
        where: {
          bookId_chapter: { bookId, chapter },
        },
        create: {
          bookId,
          chapter,
          title: config.title,
          configData: config,
          localHash,
          syncStatus: 'synced',
          createdAt: new Date(),
          updatedAt: new Date(),
        },
        update: {
          configData: config,
          localHash,
          syncStatus: 'synced',
          updatedAt: new Date(),
        },
      });

      return {
        configId: chapterConfig.id,
        remoteHash: localHash,
        syncStatus: 'synced',
      };
    }),

  // Check sync status (called by CLI)
  checkSyncStatus: protectedProcedure
    .input(z.object({
      bookId: z.string(),
      chapters: z.array(z.object({
        chapter: z.number(),
        localHash: z.string(),
      })),
    }))
    .query(async ({ ctx, input }) => {
      const { bookId, chapters } = input;
      const userId = ctx.session.user.id;

      // Verify permissions
      const book = await ctx.db.book.findUnique({
        where: { id: bookId },
      });

      if (!book || book.userId !== userId) {
        throw new Error('Unauthorized');
      }

      // Batch query remote configs
      const remoteConfigs = await ctx.db.chapterConfig.findMany({
        where: {
          bookId,
          chapter: { in: chapters.map(c => c.chapter) },
        },
        select: {
          chapter: true,
          localHash: true,
          updatedAt: true,
        },
      });

      // Compare hashes
      const syncStatuses = chapters.map(local => {
        const remote = remoteConfigs.find(r => r.chapter === local.chapter);

        if (!remote) {
          return {
            chapter: local.chapter,
            status: 'not_synced',
            needsPush: true,
          };
        }

        if (remote.localHash !== local.localHash) {
          return {
            chapter: local.chapter,
            status: 'conflict',
            needsResolve: true,
            remoteUpdatedAt: remote.updatedAt,
          };
        }

        return {
          chapter: local.chapter,
          status: 'synced',
        };
      });

      return { syncStatuses };
    }),
});
```

### 6.2 Preset Router

```typescript
// server/api/routers/preset.ts

export const presetRouter = router({
  // Get preset list
  list: protectedProcedure
    .input(z.object({
      category: z.enum(['scene', 'style', 'genre']).optional(),
      search: z.string().optional(),
    }))
    .query(async ({ ctx, input }) => {
      const { category, search } = input;

      const presets = await ctx.db.preset.findMany({
        where: {
          ...(category && { category }),
          ...(search && {
            OR: [
              { name: { contains: search } },
              { description: { contains: search } },
            ],
          }),
        },
        orderBy: [
          { featured: 'desc' },
          { rating: 'desc' },
          { usageCount: 'desc' },
        ],
      });

      return presets;
    }),

  // Get preset details
  get: protectedProcedure
    .input(z.object({
      presetId: z.string(),
    }))
    .query(async ({ ctx, input }) => {
      const preset = await ctx.db.preset.findUnique({
        where: { id: input.presetId },
        include: {
          author: {
            select: {
              name: true,
              avatar: true,
            },
          },
        },
      });

      if (!preset) {
        throw new Error('Preset not found');
      }

      return preset;
    }),

  // Apply preset to config
  applyToConfig: protectedProcedure
    .input(z.object({
      presetId: z.string(),
      baseConfig: ChapterConfigSchema.partial(),
    }))
    .mutation(async ({ ctx, input }) => {
      const { presetId, baseConfig } = input;

      // Get preset
      const preset = await ctx.db.preset.findUnique({
        where: { id: presetId },
      });

      if (!preset) {
        throw new Error('Preset not found');
      }

      // Merge preset defaults
      const presetData = preset.configData as any;
      const mergedConfig = {
        ...baseConfig,
        style: {
          ...presetData.defaults?.style,
          ...baseConfig.style,
        },
        wordcount: {
          ...presetData.defaults?.wordcount,
          ...baseConfig.wordcount,
        },
        special_requirements: presetData.defaults?.special_requirements || '',
        preset_used: presetId,
      };

      // Increment usage count
      await ctx.db.preset.update({
        where: { id: presetId },
        data: {
          usageCount: { increment: 1 },
        },
      });

      return { config: mergedConfig };
    }),
});
```

### 6.3 Database Schema Update

```prisma
// prisma/schema.prisma

model ChapterConfig {
  id          String   @id @default(cuid())
  bookId      String
  chapter     Int
  title       String
  configData  Json     // Full chapter config JSON
  localHash   String?  // Hash of the local file
  syncStatus  String   @default("not_synced") // synced, pending, conflict

  book        Book     @relation(fields: [bookId], references: [id], onDelete: Cascade)

  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt

  @@unique([bookId, chapter])
  @@index([bookId])
  @@map("chapter_configs")
}

model Preset {
  id              String   @id @default(cuid())
  presetId        String   @unique // e.g., "action-intense"
  name            String
  description     String
  category        String   // scene, style, genre
  configData      Json     // Preset configuration data

  authorId        String
  author          User     @relation(fields: [authorId], references: [id])

  featured        Boolean  @default(false)
  rating          Float    @default(0)
  ratingCount     Int      @default(0)
  usageCount      Int      @default(0)

  createdAt       DateTime @default(now())
  updatedAt       DateTime @updatedAt

  @@index([category])
  @@index([featured, rating])
  @@map("presets")
}

model Session {
  id          String   @id @default(cuid())
  userId      String
  type        String   // intro, chapter-config, etc.
  data        Json
  expiresAt   DateTime

  user        User     @relation(fields: [userId], references: [id], onDelete: Cascade)

  createdAt   DateTime @default(now())

  @@index([userId])
  @@index([expiresAt])
  @@map("sessions")
}

// Extend existing Book model
model Book {
  // ... existing fields
  chapterConfigs  ChapterConfig[]
}
```

---

## 7. Session Sync Mechanism

### 7.1 Sync Flow

#### Option A: Session-based One-way Sync (v1.0 MVP)

**Use Case**: Web creation → CLI usage

```
┌──────────────┐
│  Dreams Web  │
│ Create Config│
└───────┬──────┘
        │
        │ 1. POST /api/chapterConfig/create
        ▼
┌──────────────┐
│   Backend    │
│ Store + Create │
│  Session     │
└───────┬──────┘
        │
        │ 2. Return session-id
        ▼
┌──────────────┐
│  User Copies │
│  CLI Command │
└───────┬──────┘
        │
        │ 3. novel chapter-config pull --session {id}
        ▼
┌──────────────┐
│   CLI        │
│ GET /api/    │
│ pullFromSession
└───────┬──────┘
        │
        │ 4. Download config data
        ▼
┌──────────────┐
│  Save Locally│
│  YAML File   │
└──────────────┘
```

**Pros**:
- Simple and reliable
- No complex sync logic
- Sessions have an expiration time for automatic cleanup

**Cons**:
- One-way sync, does not support CLI → Web
- Requires manual command copying

#### Option B: Hash-based Bidirectional Sync (v2.0+)

**Use Case**: CLI ↔ Web bidirectional sync

```
┌──────────────┐              ┌──────────────┐
│   Local CLI  │              │  Dreams Web  │
│              │              │              │
│ chapter-5-   │◀────┐        │              │
│ config.yaml  │     │        │              │
│              │     │        │              │
│ hash: abc123 │     │        │              │
└──────┬───────┘     │        └──────┬───────┘
       │             │               │
       │ 1. novel    │               │ 5. Web view
       │ chapter-    │               │ /configs
       │ config      │               │
       │ sync        │               │
       │             │               ▼
       │             │        ┌──────────────┐
       │             │        │   Backend    │
       │             │        │              │
       │             │        │ Remote DB:   │
       │             └────────│ hash: xyz789 │
       │                      │              │
       │ 2. checkSyncStatus   │              │
       ├─────────────────────▶│              │
       │                      │              │
       │ 3. Return status     │              │
       │    conflict!         │              │
       │◀─────────────────────┤              │
       │                      │              │
       │ 4. User chooses:     │              │
       │    - push (overwrite remote)│        │
       │    - pull (fetch remote)│          │
       │    - merge (merge)     │            │
       │                      │              │
       │ novel chapter-config │              │
       │ push 5 --force       │              │
       ├─────────────────────▶│              │
       │                      │ Update hash  │
       │                      │ hash: abc123 │
       │◀─────────────────────┤              │
       │  Sync successful     │              │
       │                      └──────────────┘
```

**Hash Calculation**:
```typescript
import crypto from 'crypto';

function calculateConfigHash(config: ChapterConfig): string {
  // Exclude metadata fields (created_at, updated_at, etc.)
  const stableConfig = {
    chapter: config.chapter,
    title: config.title,
    characters: config.characters,
    scene: config.scene,
    plot: config.plot,
    style: config.style,
    wordcount: config.wordcount,
    special_requirements: config.special_requirements,
  };

  // Sort keys to ensure consistency
  const canonical = JSON.stringify(stableConfig, Object.keys(stableConfig).sort());

  // Calculate SHA-256 hash
  return crypto.createHash('sha256').update(canonical).digest('hex');
}
```

**Conflict Resolution Strategy**:

1. **Automatic Resolution**: Timestamp priority
   ```bash
   novel chapter-config sync --auto
   # Automatically select the latest version
   ```

2. **Manual Resolution**: Three-way comparison
   ```bash
   novel chapter-config sync 5
   # Display conflict details:
   # Local:  modified 2025-10-14 15:30
   # Remote: modified 2025-10-14 16:00
   #
   # Select action:
   #   1. Use local version (overwrite remote)
   #   2. Use remote version (overwrite local)
   #   3. Manual merge (open editor)
   ```

3. **Merge Strategy**: Field-level merge
   ```typescript
   // Automatically merge non-conflicting fields
   // Prompt user to choose for conflicting fields
   function mergeConfigs(
     local: ChapterConfig,
     remote: ChapterConfig,
     base: ChapterConfig
   ): ChapterConfig {
     const merged = { ...base };

     for (const key of Object.keys(local)) {
       if (JSON.stringify(local[key]) === JSON.stringify(base[key])) {
         // Local not changed, use remote
         merged[key] = remote[key];
       } else if (JSON.stringify(remote[key]) === JSON.stringify(base[key])) {
         // Remote not changed, use local
         merged[key] = local[key];
       } else {
         // Both changed, record conflict
         conflicts.push(key);
       }
     }

     return merged;
   }
   ```

### 7.2 Sync Command Design

```bash
# Pull (Web → CLI)
novel chapter-config pull --session {session-id}
novel chapter-config pull 5 --remote  # Pull chapter 5 config from the cloud

# Push (CLI → Web)
novel chapter-config push 5
novel chapter-config push 5 --force   # Force overwrite remote

# Sync (Bidirectional intelligent sync)
novel chapter-config sync              # Sync all configs
novel chapter-config sync 5            # Sync chapter 5
novel chapter-config sync --auto       # Automatically resolve conflicts

# Check sync status
novel chapter-config status
# Example output:
# Chapter 5: ✓ Synced (last synced: 2025-10-14 10:30)
# Chapter 8: ⚠ Conflict (local: 15:30, remote: 16:00)
# Chapter 15: ↑ Not synced (local changes, need push)
```

### 7.3 CLI Implementation

```typescript
// src/commands/chapter-config.ts

import { Command } from 'commander';
import { ChapterConfigManager } from '../lib/chapter-config';
import { DreamsClient } from '../lib/dreams-client';

export function registerChapterConfigCommands(program: Command) {
  const chapterConfig = program
    .command('chapter-config')
    .description('Manage chapter configurations');

  // pull command
  chapterConfig
    .command('pull')
    .description('Pull configuration from Dreams')
    .option('--session <id>', 'Session ID')
    .option('--remote', 'Pull from the cloud')
    .argument('[chapter]', 'Chapter number')
    .action(async (chapter, options) => {
      const manager = new ChapterConfigManager();
      const client = new DreamsClient();

      if (options.session) {
        // Session mode
        const data = await client.pullFromSession(options.session);
        const config = data.config;

        await manager.saveConfig(config);
        console.log(`✓ Configuration saved to .novel/chapters/chapter-${config.chapter}-config.yaml`);
      } else if (options.remote && chapter) {
        // Remote pull mode
        const bookId = await manager.getCurrentBookId();
        const config = await client.getConfig(bookId, parseInt(chapter));

        await manager.saveConfig(config);
        console.log(`✓ Chapter ${chapter} configuration pulled from the cloud`);
      } else {
        console.error('Error: Must provide --session or --remote option');
        process.exit(1);
      }
    });

  // push command
  chapterConfig
    .command('push <chapter>')
    .description('Push configuration to Dreams')
    .option('--force', 'Force overwrite remote configuration')
    .action(async (chapter, options) => {
      const manager = new ChapterConfigManager();
      const client = new DreamsClient();

      const chapterNum = parseInt(chapter);
      const config = await manager.loadConfig(chapterNum);

      if (!config) {
        console.error(`Error: Configuration for chapter ${chapter} does not exist`);
        process.exit(1);
      }

      const bookId = await manager.getCurrentBookId();
      const localHash = manager.calculateHash(config);

      try {
        const result = await client.pushConfig(bookId, chapterNum, config, localHash, options.force);

        // Update local metadata
        await manager.updateSyncMetadata(chapterNum, {
          remote_id: result.configId,
          remote_hash: result.remoteHash,
          last_synced: new Date().toISOString(),
        });

        console.log(`✓ Chapter ${chapter} configuration has been pushed to the cloud`);
      } catch (error) {
        if (error.message.includes('conflict')) {
          console.error('⚠ Conflict detected, remote configuration has been modified');
          console.error('Use --force to overwrite, or run pull first to get the remote configuration');
        } else {
          throw error;
        }
      }
    });

  // sync command
  chapterConfig
    .command('sync [chapter]')
    .description('Bidirectional synchronization of configurations')
    .option('--auto', 'Automatically resolve conflicts')
    .action(async (chapter, options) => {
      const manager = new ChapterConfigManager();
      const client = new DreamsClient();

      const bookId = await manager.getCurrentBookId();

      // Get list of local configurations
      const localConfigs = await manager.listConfigs();

      // Check sync status
      const statusResult = await client.checkSyncStatus(
        bookId,
        localConfigs.map(c => ({
          chapter: c.chapter,
          localHash: manager.calculateHash(c.config),
        }))
      );

      for (const status of statusResult.syncStatuses) {
        if (chapter && status.chapter !== parseInt(chapter)) {
          continue;
        }

        if (status.status === 'synced') {
          console.log(`Chapter ${status.chapter}: ✓ Synced`);
        } else if (status.status === 'not_synced') {
          // Needs push
          console.log(`Chapter ${status.chapter}: ↑ Pushing to cloud...`);
          await client.pushConfig(bookId, status.chapter, ...);
        } else if (status.status === 'conflict') {
          // Conflict handling
          if (options.auto) {
            // Automatically choose the latest
            if (status.remoteUpdatedAt > localUpdatedAt) {
              console.log(`Chapter ${status.chapter}: ↓ Pulling remote config (remote is newer)...`);
              await client.getConfig(bookId, status.chapter);
            } else {
              console.log(`Chapter ${status.chapter}: ↑ Pushing local config (local is newer)...`);
              await client.pushConfig(bookId, status.chapter, ..., true);
            }
          } else {
            console.log(`Chapter ${status.chapter}: ⚠ Conflict detected`);
            console.log(`  Local modification time: ${localUpdatedAt}`);
            console.log(`  Remote modification time: ${status.remoteUpdatedAt}`);

            const answer = await inquirer.prompt([{
              type: 'list',
              name: 'action',
              message: 'Select action:',
              choices: [
                { name: '1. Use local version (overwrite remote)', value: 'push' },
                { name: '2. Use remote version (overwrite local)', value: 'pull' },
                { name: '3. Skip this chapter', value: 'skip' },
              ],
            }]);

            if (answer.action === 'push') {
              await client.pushConfig(bookId, status.chapter, ..., true);
            } else if (answer.action === 'pull') {
              await client.getConfig(bookId, status.chapter);
            }
          }
        }
      }

      console.log('\n✓ Sync complete');
    });

  // status command
  chapterConfig
    .command('status')
    .description('View synchronization status')
    .action(async () => {
      const manager = new ChapterConfigManager();
      const client = new DreamsClient();

      const bookId = await manager.getCurrentBookId();
      const localConfigs = await manager.listConfigs();

      const statusResult = await client.checkSyncStatus(bookId, localConfigs.map(...));

      console.log('\nChapter Configuration Sync Status:\n');

      for (const status of statusResult.syncStatuses) {
        const icon = status.status === 'synced' ? '✓' :
                     status.status === 'not_synced' ? '↑' :
                     '⚠';

        const statusText = status.status === 'synced' ? 'Synced' :
                          status.status === 'not_synced' ? 'To be pushed' :
                          'Conflict';

        console.log(`  Chapter ${status.chapter}: ${icon} ${statusText}`);

        if (status.status === 'conflict') {
          console.log(`    Remote modification time: ${status.remoteUpdatedAt}`);
        }
      }

      console.log('');
    });
}
```

---

## 8. Phased Implementation Plan

### Phase 1: Infrastructure (2 weeks)

**Goal**: Complete the Dreams backend foundation and one-way sync.

**Tasks**:
1. Database schema design and migration
   - Create `ChapterConfig` table
   - Create `Preset` table
   - Extend `Session` table

2. tRPC API development
   - `chapterConfigRouter`: create, list, get, update, delete, pullFromSession
   - `presetRouter`: list, get

3. CLI command development
   - `novel chapter-config pull --session`
   - Local YAML file saving

4. Testing
   - API unit tests
   - CLI integration tests

**Deliverables**:
- ✅ Database migration files
- ✅ tRPC Router implementation
- ✅ CLI pull command
- ✅ Test cases

---

### Phase 2: Web UI Development (3 weeks)

**Goal**: Complete the Dreams frontend configuration creation interface.

**Tasks**:
1. YAML form system extension
   - Create `forms/chapter-config.yaml`
   - Support for array fields (character list, plot point list)
   - Support for dynamic options (loading characters, locations from API)

2. Page development
   - Chapter config creation page (`/books/[id]/chapters/[chapter]/config`)
   - Config list page (`/books/[id]/configs`)
   - Session result page (displaying CLI command)

3. Component development
   - `<ArrayFieldEditor>` - Array field editor
   - `<ConfigPreview>` - YAML preview component
   - `<CharacterSelector>` - Character selector (with search)

4. Testing
   - Frontend unit tests
   - E2E tests

**Deliverables**:
- ✅ YAML form configuration
- ✅ Config creation page
- ✅ Config list page
- ✅ UI component library
- ✅ E2E tests

---

### Phase 3: Preset System (2 weeks)

**Goal**: Complete the preset marketplace and preset application functionality.

**Tasks**:
1. Preset data preparation
   - Create official presets (10)
   - Preset metadata (category, tags, examples)
   - Database seed script

2. Preset marketplace page
   - Preset list page (`/presets`)
   - Preset details dialog
   - Search and filter functionality

3. Preset application logic
   - Preset application API: `applyToConfig`
   - Frontend preset selector integration
   - Preset usage statistics

4. CLI preset support
   - `novel preset list`
   - `novel preset get <id>`
   - `novel chapter-config create --preset <id>`

**Deliverables**:
- ✅ 10 official presets
- ✅ Preset marketplace page
- ✅ Preset application API
- ✅ CLI preset commands

---

### Phase 4: Bidirectional Sync (3 weeks)

**Goal**: Complete CLI → Web push and conflict resolution.

**Tasks**:
1. Hash calculation and metadata management
   - Implement `calculateConfigHash()`
   - Local metadata file `.novel/meta/sync.json`
   - Sync status tracking

2. Push API development
   - `chapterConfig.push`
   - `chapterConfig.checkSyncStatus`
   - Conflict detection logic

3. CLI sync commands
   - `novel chapter-config push`
   - `novel chapter-config sync`
   - `novel chapter-config status`

4. Conflict resolution UI
   - CLI interactive conflict resolution
   - Web-based conflict comparison page (optional)

5. Testing
   - Sync scenario tests
   - Conflict handling tests

**Deliverables**:
- ✅ Hash calculation module
- ✅ Push API
- ✅ CLI sync commands
- ✅ Conflict resolution mechanism
- ✅ Test cases

---

### Phase 5: Optimization and Enhancement (2 weeks)

**Goal**: Performance optimization, user experience improvement.

**Tasks**:
1. Performance optimization
   - API response time optimization
   - Frontend loading optimization (code splitting)
   - Database query optimization (indexing)

2. User experience
   - Form auto-save (draft)
   - Form validation message optimization
   - Loading states and error handling
   - Operation success feedback

3. Documentation and tutorials
   - Dreams integration documentation
   - Video tutorial (config creation flow)
   - CLI command documentation

4. Monitoring and logging
   - API call monitoring
   - Error tracking (Sentry)
   - Sync failure alerts

**Deliverables**:
- ✅ Performance optimization report
- ✅ UX improvement checklist
- ✅ User documentation
- ✅ Monitoring dashboard

---

## 9. Technical Challenges and Solutions

### 9.1 Challenge 1: Performance of Large Forms

**Problem**: The chapter configuration form has many fields (20+), and array fields can have multiple elements, which may cause rendering and interaction lag.

**Solutions**:
1. **Form Segmentation**: Use Tabs or Accordions to display sections, rendering only the visible part.
2. **Virtual Scrolling**: Use virtual scrolling for array fields (character list) to optimize long lists.
3. **Debounced Input**: Use debounce for text inputs to reduce re-renders.
4. **React.memo Optimization**: Use `memo` for child components to avoid unnecessary re-renders.

```typescript
// Optimize array item component with React.memo
const CharacterItem = React.memo(({ character, onChange }) => {
  return (
    <div>
      <Select value={character.id} onChange={...} />
      <RadioGroup value={character.focus} onChange={...} />
      {/* ... */}
    </div>
  );
});
```

### 9.2 Challenge 2: Format Conversion Between YAML and JSON

**Problem**: The Dreams backend uses JSON for storage, while the CLI uses YAML format, requiring lossless conversion.

**Solutions**:
1. **Standardized Conversion**: Use the `js-yaml` library to ensure consistent bidirectional conversion.
2. **Preserve Comments**: YAML supports comments, which JSON does not, requiring special handling.

```typescript
import yaml from 'js-yaml';
import { preserveComments } from '../utils/yaml-comments';

export function yamlToJson(yamlString: string): ChapterConfig {
  return yaml.load(yamlString) as ChapterConfig;
}

export function jsonToYaml(config: ChapterConfig, includeComments = true): string {
  let yamlString = yaml.dump(config, {
    indent: 2,
    lineWidth: 80,
    noRefs: true,
  });

  if (includeComments) {
    yamlString = preserveComments(yamlString);
  }

  return yamlString;
}

// Function to preserve comments
function preserveComments(yamlString: string): string {
  // Add comments after key fields
  return yamlString
    .replace(/^chapter:/m, 'chapter: # Chapter number')
    .replace(/^title:/m, 'title: # Chapter title')
    .replace(/^characters:/m, 'characters: # Appearing characters')
    // ...
}
```

### 9.3 Challenge 3: Conflict Handling in Cross-Device Sync

**Problem**: Users editing on multiple devices simultaneously may create conflicts.

**Solutions**:
1. **Optimistic Locking**: Use `updatedAt` timestamps and version numbers.
2. **Three-Way Merge**: Record a base version for a three-way comparison.
3. **Field-Level Conflict Marking**: Mark only truly conflicting fields, and automatically merge non-conflicting ones.

```typescript
interface SyncMetadata {
  chapter: number;
  local_hash: string;
  remote_hash: string;
  base_hash: string;  // Hash at the last sync (for three-way merge)
  last_synced: string;
}

async function sync(chapter: number) {
  const local = await manager.loadConfig(chapter);
  const remote = await client.getConfig(bookId, chapter);
  const meta = await manager.getSyncMetadata(chapter);

  const localHash = calculateHash(local);
  const remoteHash = calculateHash(remote);

  if (localHash === remoteHash) {
    return { status: 'synced' };
  }

  if (localHash === meta.base_hash) {
    // Local unchanged, remote changed → pull remote
    await manager.saveConfig(remote);
    return { status: 'pulled' };
  }

  if (remoteHash === meta.base_hash) {
    // Remote unchanged, local changed → push local
    await client.pushConfig(bookId, chapter, local, localHash);
    return { status: 'pushed' };
  }

  // Both changed → conflict
  return { status: 'conflict', local, remote };
}
```

### 9.4 Challenge 4: Data Loading for Dynamic Form Options

**Problem**: Options for characters, locations, etc., need to be dynamically loaded from the project's knowledge base, which may have delays.

**Solutions**:
1. **Preloading**: Fetch all option data in parallel when the page loads.
2. **Caching**: Use React Query to cache option data.
3. **Skeleton Screens**: Display skeleton screens while data is loading to improve perceived performance.

```typescript
// Preload all options with React Query
function ChapterConfigForm({ bookId }) {
  const { data: characters } = useQuery({
    queryKey: ['characters', bookId],
    queryFn: () => api.characters.list({ bookId }),
    staleTime: 5 * 60 * 1000, // 5-minute cache
  });

  const { data: locations } = useQuery({
    queryKey: ['locations', bookId],
    queryFn: () => api.locations.list({ bookId }),
    staleTime: 5 * 60 * 1000,
  });

  if (!characters || !locations) {
    return <FormSkeleton />;
  }

  return <Form characters={characters} locations={locations} />;
}
```

### 9.5 Challenge 5: Extensibility of the Preset System

**Problem**: The number of presets may grow, how to manage and extend them?

**Solutions**:
1. **Preset Categories**: Categorize by scene, style, type.
2. **Tagging System**: Support multiple tags for easier searching.
3. **Version Management**: Support preset versions, allowing users to choose.
4. **User-Defined Presets**: Allow users to create and share presets.

```prisma
model Preset {
  // ...
  category    String     // scene, style, genre
  tags        String[]   // Array of tags
  version     String     @default("1.0.0")
  isOfficial  Boolean    @default(false)
  isPublic    Boolean    @default(false)

  // Support for forking and inheritance
  parentId    String?
  parent      Preset?    @relation("PresetFork", fields: [parentId], references: [id])
  forks       Preset[]   @relation("PresetFork")
}
```

---

## 10. Success Metrics

### 10.1 Functional Completeness Metrics

- [ ] Web form submission success rate > 95%
- [ ] Session sync success rate > 98%
- [ ] Correct YAML format for configuration files: 100%
- [ ] Preset application success rate > 99%

### 10.2 Performance Metrics

- [ ] Config creation page load time < 2s
- [ ] Form submission response time < 500ms
- [ ] CLI `pull` command execution time < 3s
- [ ] CLI `sync` command execution time < 5s (for a single chapter)

### 10.3 User Experience Metrics

- [ ] Config creation completion time < 5 minutes (for first-time use)
- [ ] Config creation completion time < 2 minutes (using a preset)
- [ ] User satisfaction rating > 4.5/5
- [ ] Feature adoption rate > 40% (percentage of users who create a config)

### 10.4 Stability Metrics

- [ ] API error rate < 0.5%
- [ ] CLI command failure rate < 1%
- [ ] Sync conflict rate < 5%
- [ ] Data loss incidents = 0

---

## Appendix A: References

### A.1 Related Documents

- [Chapter Configuration System PRD](./chapter-config-system.md)
- [Technical Specification](./tech-spec.md)
- [Dreams YAML Form System](../../../other/dreams/docs/form-system-architecture.md)
- [novel-writer-cn CLI Architecture](../../../README.md)

### A.2 Tech Stack Documentation

- [Next.js 14 Documentation](https://nextjs.org/docs)
- [tRPC Documentation](https://trpc.io/docs)
- [Prisma Documentation](https://www.prisma.io/docs)
- [shadcn/ui Components](https://ui.shadcn.com/)
- [React Hook Form](https://react-hook-form.com/)
- [js-yaml Library](https://github.com/nodeca/js-yaml)

---

## Update Log

- **v1.0.0** (2025-10-14): Initial version, complete Dreams integration plan.
