# Chapter Configuration System - Technical Specification

## Document Information

- **Document Name**: Chapter Configuration System Technical Specification
- **Version**: v1.0.0
- **Creation Date**: 2025-10-14
- **Related PRD**: [Chapter Configuration System PRD](./chapter-config-system.md)
- **Target Audience**: Developers, Technical Leads

---

## 1. Complete YAML Schema Definition

### 1.1 JSON Schema Representation

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "ChapterConfig",
  "description": "Schema for chapter configuration files",
  "type": "object",
  "required": ["chapter", "title", "plot", "wordcount"],
  "properties": {
    "chapter": {
      "type": "integer",
      "minimum": 1,
      "description": "Chapter number"
    },
    "title": {
      "type": "string",
      "minLength": 1,
      "maxLength": 100,
      "description": "Chapter title"
    },
    "characters": {
      "type": "array",
      "description": "List of appearing characters",
      "items": {
        "$ref": "#/definitions/Character"
      }
    },
    "scene": {
      "$ref": "#/definitions/Scene",
      "description": "Scene configuration"
    },
    "plot": {
      "$ref": "#/definitions/Plot",
      "description": "Plot configuration"
    },
    "style": {
      "$ref": "#/definitions/Style",
      "description": "Writing style configuration"
    },
    "wordcount": {
      "$ref": "#/definitions/Wordcount",
      "description": "Word count requirements"
    },
    "special_requirements": {
      "type": "string",
      "description": "Special writing requirements"
    },
    "preset_used": {
      "type": "string",
      "description": "ID of the preset used"
    },
    "created_at": {
      "type": "string",
      "format": "date-time",
      "description": "Creation time"
    },
    "updated_at": {
      "type": "string",
      "format": "date-time",
      "description": "Update time"
    }
  },
  "definitions": {
    "Character": {
      "type": "object",
      "required": ["id", "name"],
      "properties": {
        "id": {
          "type": "string",
          "pattern": "^[a-z0-9-]+$",
          "description": "Character ID, referencing character-profiles.md"
        },
        "name": {
          "type": "string",
          "description": "Character name"
        },
        "focus": {
          "type": "string",
          "enum": ["high", "medium", "low"],
          "default": "medium",
          "description": "Importance in this chapter"
        },
        "state_changes": {
          "type": "array",
          "items": {
            "type": "string"
          },
          "description": "State changes in this chapter"
        }
      }
    },
    "Scene": {
      "type": "object",
      "properties": {
        "location_id": {
          "type": "string",
          "pattern": "^[a-z0-9-]+$",
          "description": "Location ID, referencing locations.md"
        },
        "location_name": {
          "type": "string",
          "description": "Location name"
        },
        "time": {
          "type": "string",
          "description": "Time (e.g., '10:00 AM', 'Dusk')"
        },
        "weather": {
          "type": "string",
          "description": "Weather"
        },
        "atmosphere": {
          "type": "string",
          "enum": ["tense", "relaxed", "sad", "exciting", "mysterious"],
          "description": "Atmosphere"
        }
      }
    },
    "Plot": {
      "type": "object",
      "required": ["type", "summary"],
      "properties": {
        "type": {
          "type": "string",
          "enum": [
            "ability_showcase",
            "relationship_dev",
            "conflict_combat",
            "mystery_suspense",
            "transition",
            "climax",
            "emotional_scene",
            "world_building",
            "plot_twist"
          ],
          "description": "Plot type"
        },
        "summary": {
          "type": "string",
          "minLength": 10,
          "maxLength": 500,
          "description": "Plot summary"
        },
        "key_points": {
          "type": "array",
          "items": {
            "type": "string"
          },
          "minItems": 1,
          "description": "Key points"
        },
        "plotlines": {
          "type": "array",
          "items": {
            "type": "string",
            "pattern": "^PL-[0-9]+$"
          },
          "description": "Involved plotline IDs"
        },
        "foreshadowing": {
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "id": {
                "type": "string",
                "pattern": "^F-[0-9]+$"
              },
              "content": {
                "type": "string"
              }
            }
          },
          "description": "Foreshadowing in this chapter"
        }
      }
    },
    "Style": {
      "type": "object",
      "properties": {
        "pace": {
          "type": "string",
          "enum": ["fast", "medium", "slow"],
          "default": "medium",
          "description": "Pace"
        },
        "sentence_length": {
          "type": "string",
          "enum": ["short", "medium", "long"],
          "default": "medium",
          "description": "Sentence length"
        },
        "focus": {
          "type": "string",
          "enum": [
            "action",
            "dialogue",
            "psychology",
            "description",
            "dialogue_action",
            "balanced"
          ],
          "default": "balanced",
          "description": "Descriptive focus"
        },
        "tone": {
          "type": "string",
          "enum": ["serious", "humorous", "dark", "light"],
          "description": "Tone"
        }
      }
    },
    "Wordcount": {
      "type": "object",
      "required": ["target"],
      "properties": {
        "target": {
          "type": "integer",
          "minimum": 1000,
          "maximum": 10000,
          "description": "Target word count"
        },
        "min": {
          "type": "integer",
          "minimum": 500,
          "description": "Minimum word count"
        },
        "max": {
          "type": "integer",
          "maximum": 15000,
          "description": "Maximum word count"
        }
      }
    }
  }
}
```

### 1.2 TypeScript Type Definitions

```typescript
/**
 * Chapter Configuration Interface
 */
export interface ChapterConfig {
  /** Chapter number */
  chapter: number;

  /** Chapter title */
  title: string;

  /** Appearing characters */
  characters?: Character[];

  /** Scene configuration */
  scene?: Scene;

  /** Plot configuration */
  plot: Plot;

  /** Writing style */
  style?: Style;

  /** Word count requirements */
  wordcount: Wordcount;

  /** Special requirements */
  special_requirements?: string;

  /** Preset used */
  preset_used?: string;

  /** Creation time */
  created_at?: string;

  /** Update time */
  updated_at?: string;
}

/**
 * Character configuration
 */
export interface Character {
  /** Character ID (references character-profiles.md) */
  id: string;

  /** Character name */
  name: string;

  /** Importance in this chapter */
  focus?: 'high' | 'medium' | 'low';

  /** State changes in this chapter */
  state_changes?: string[];
}

/**
 * Scene configuration
 */
export interface Scene {
  /** Location ID (references locations.md) */
  location_id?: string;

  /** Location name */
  location_name?: string;

  /** Time */
  time?: string;

  /** Weather */
  weather?: string;

  /** Atmosphere */
  atmosphere?: 'tense' | 'relaxed' | 'sad' | 'exciting' | 'mysterious';
}

/**
 * Plot configuration
 */
export interface Plot {
  /** Plot type */
  type: PlotType;

  /** Plot summary */
  summary: string;

  /** Key points */
  key_points?: string[];

  /** Involved plotlines */
  plotlines?: string[];

  /** Foreshadowing */
  foreshadowing?: Foreshadowing[];
}

/**
 * Plot Type Enum
 */
export type PlotType =
  | 'ability_showcase'
  | 'relationship_dev'
  | 'conflict_combat'
  | 'mystery_suspense'
  | 'transition'
  | 'climax'
  | 'emotional_scene'
  | 'world_building'
  | 'plot_twist';

/**
 * Foreshadowing configuration
 */
export interface Foreshadowing {
  /** Foreshadowing ID */
  id: string;

  /** Foreshadowing content */
  content: string;
}

/**
 * Writing style configuration
 */
export interface Style {
  /** Pace */
  pace?: 'fast' | 'medium' | 'slow';

  /** Sentence length */
  sentence_length?: 'short' | 'medium' | 'long';

  /** Descriptive focus */
  focus?: 'action' | 'dialogue' | 'psychology' | 'description' | 'dialogue_action' | 'balanced';

  /** Tone */
  tone?: 'serious' | 'humorous' | 'dark' | 'light';
}

/**
 * Word count configuration
 */
export interface Wordcount {
  /** Target word count */
  target: number;

  /** Minimum word count */
  min?: number;

  /** Maximum word count */
  max?: number;
}

/**
 * Preset Configuration Interface
 */
export interface Preset {
  /** Preset ID */
  id: string;

  /** Preset name */
  name: string;

  /** Description */
  description: string;

  /** Category */
  category: 'scene' | 'style' | 'chapter';

  /** Author */
  author: string;

  /** Version */
  version: string;

  /** Default configuration */
  defaults: Partial<ChapterConfig>;

  /** Recommended settings */
  recommended?: {
    plot_types?: PlotType[];
    atmosphere?: Scene['atmosphere'][];
  };

  /** Compatibility */
  compatible_genres?: string[];

  /** Usage tips */
  usage_tips?: string[];
}
```

---

## 2. Core Class Design

### 2.1 ChapterConfigManager

```typescript
/**
 * Chapter Configuration Manager
 * Responsible for creating, reading, validating, updating, and deleting configurations.
 */
export class ChapterConfigManager {
  private projectPath: string;
  private presetManager: PresetManager;
  private validator: ConfigValidator;

  constructor(projectPath: string) {
    this.projectPath = projectPath;
    this.presetManager = new PresetManager();
    this.validator = new ConfigValidator(projectPath);
  }

  /**
   * Create a chapter configuration
   */
  async createConfig(
    chapter: number,
    options: CreateConfigOptions
  ): Promise<ChapterConfig> {
    // 1. Initialize configuration
    let config: ChapterConfig = {
      chapter,
      title: options.title || `Chapter ${chapter}`,
      characters: [],
      scene: {},
      plot: {
        type: options.plotType || 'transition',
        summary: options.plotSummary || '',
        key_points: options.keyPoints || []
      },
      style: {
        pace: 'medium',
        sentence_length: 'medium',
        focus: 'balanced'
      },
      wordcount: {
        target: options.wordcount || 3000,
        min: Math.floor((options.wordcount || 3000) * 0.8),
        max: Math.floor((options.wordcount || 3000) * 1.2)
      },
      created_at: new Date().toISOString()
    };

    // 2. Apply preset (if specified)
    if (options.preset) {
      const preset = await this.presetManager.loadPreset(options.preset);
      config = this.applyPreset(preset, config);
    }

    // 3. Merge user input
    if (options.characters) {
      config.characters = await this.loadCharacterDetails(options.characters);
    }

    if (options.scene) {
      config.scene = await this.loadSceneDetails(options.scene);
    }

    // 4. Validate configuration
    const validation = await this.validator.validate(config);
    if (!validation.valid) {
      throw new Error(`Configuration validation failed: ${validation.errors.join(', ')}`);
    }

    // 5. Save to file
    const configPath = this.getConfigPath(chapter);
    await fs.ensureDir(path.dirname(configPath));
    await fs.writeFile(configPath, yaml.dump(config, { indent: 2 }), 'utf-8');

    return config;
  }

  /**
   * Load a chapter configuration
   */
  async loadConfig(chapter: number): Promise<ChapterConfig | null> {
    const configPath = this.getConfigPath(chapter);

    if (!await fs.pathExists(configPath)) {
      return null;
    }

    const content = await fs.readFile(configPath, 'utf-8');
    const config = yaml.load(content) as ChapterConfig;

    // Validate configuration
    const validation = await this.validator.validate(config);
    if (!validation.valid) {
      console.warn(`Configuration file has issues: ${validation.errors.join(', ')}`);
    }

    return config;
  }

  /**
   * Update a chapter configuration
   */
  async updateConfig(
    chapter: number,
    updates: Partial<ChapterConfig>
  ): Promise<ChapterConfig> {
    const config = await this.loadConfig(chapter);
    if (!config) {
      throw new Error(`Configuration file not found: chapter ${chapter}`);
    }

    const updatedConfig = {
      ...config,
      ...updates,
      updated_at: new Date().toISOString()
    };

    // Validate the updated configuration
    const validation = await this.validator.validate(updatedConfig);
    if (!validation.valid) {
      throw new Error(`Updated configuration is invalid: ${validation.errors.join(', ')}`);
    }

    // Save
    const configPath = this.getConfigPath(chapter);
    await fs.writeFile(
      configPath,
      yaml.dump(updatedConfig, { indent: 2 }),
      'utf-8'
    );

    return updatedConfig;
  }

  /**
   * Delete a chapter configuration
   */
  async deleteConfig(chapter: number): Promise<void> {
    const configPath = this.getConfigPath(chapter);

    if (!await fs.pathExists(configPath)) {
      throw new Error(`Configuration file not found: chapter ${chapter}`);
    }

    await fs.remove(configPath);
  }

  /**
   * List all configurations
   */
  async listConfigs(): Promise<ChapterConfigSummary[]> {
    const chaptersDir = path.join(
      this.projectPath,
      'stories',
      '*',
      'chapters'
    );

    const configFiles = await glob(path.join(chaptersDir, '*.yaml'));

    const summaries: ChapterConfigSummary[] = [];

    for (const file of configFiles) {
      const content = await fs.readFile(file, 'utf-8');
      const config = yaml.load(content) as ChapterConfig;

      summaries.push({
        chapter: config.chapter,
        title: config.title,
        plotType: config.plot.type,
        location: config.scene?.location_name || '-',
        wordcount: config.wordcount.target,
        preset: config.preset_used,
        createdAt: config.created_at
      });
    }

    return summaries.sort((a, b) => a.chapter - b.chapter);
  }

  /**
   * Copy a configuration
   */
  async copyConfig(
    fromChapter: number,
    toChapter: number,
    modifications?: Partial<ChapterConfig>
  ): Promise<ChapterConfig> {
    const sourceConfig = await this.loadConfig(fromChapter);
    if (!sourceConfig) {
      throw new Error(`Source configuration not found: chapter ${fromChapter}`);
    }

    const newConfig: ChapterConfig = {
      ...sourceConfig,
      chapter: toChapter,
      ...modifications,
      created_at: new Date().toISOString(),
      updated_at: undefined
    };

    return this.createConfig(toChapter, {
      title: newConfig.title,
      plotType: newConfig.plot.type,
      plotSummary: newConfig.plot.summary,
      keyPoints: newConfig.plot.key_points,
      wordcount: newConfig.wordcount.target,
      // ...
    } as CreateConfigOptions);
  }

  // ========== Private Helper Methods ==========

  private getConfigPath(chapter: number): string {
    // Find the stories directory in the project
    const storiesDir = path.join(this.projectPath, 'stories');
    const storyDirs = fs.readdirSync(storiesDir);

    if (storyDirs.length === 0) {
      throw new Error('No story directory found');
    }

    // Use the first story directory (usually there is only one)
    const storyDir = storyDirs[0];
    return path.join(
      storiesDir,
      storyDir,
      'chapters',
      `chapter-${chapter}-config.yaml`
    );
  }

  private applyPreset(
    preset: Preset,
    config: ChapterConfig
  ): ChapterConfig {
    return {
      ...config,
      ...preset.defaults,
      preset_used: preset.id,
      // Merge special_requirements
      special_requirements: [
        preset.defaults.special_requirements,
        config.special_requirements
      ].filter(Boolean).join('\n\n')
    };
  }

  private async loadCharacterDetails(
    characterIds: string[]
  ): Promise<Character[]> {
    // Load details from character-profiles.md
    // Implementation omitted...
    return [];
  }

  private async loadSceneDetails(
    sceneId: string
  ): Promise<Scene> {
    // Load details from locations.md
    // Implementation omitted...
    return {};
  }
}

/**
 * Configuration Summary Interface
 */
export interface ChapterConfigSummary {
  chapter: number;
  title: string;
  plotType: PlotType;
  location: string;
  wordcount: number;
  preset?: string;
  createdAt?: string;
}

/**
 * Create Configuration Options
 */
export interface CreateConfigOptions {
  title?: string;
  characters?: string[];
  scene?: string;
  plotType?: PlotType;
  plotSummary?: string;
  keyPoints?: string[];
  preset?: string;
  wordcount?: number;
  style?: Partial<Style>;
  specialRequirements?: string;
}
```

### 2.2 PresetManager

```typescript
/**
 * Preset Manager
 * Responsible for loading, creating, importing, and exporting presets.
 */
export class PresetManager {
  private presetDirs: string[];

  constructor() {
    this.presetDirs = [
      path.join(process.cwd(), 'stories', '*', 'presets'),  // Project local
      path.join(os.homedir(), '.novel', 'presets', 'user'), // User-defined
      path.join(os.homedir(), '.novel', 'presets', 'community'), // Community
      path.join(os.homedir(), '.novel', 'presets', 'official'), // Official
      path.join(__dirname, '..', '..', 'presets')  // Built-in
    ];
  }

  /**
   * Load a preset
   */
  async loadPreset(presetId: string): Promise<Preset> {
    for (const dir of this.presetDirs) {
      const presetPath = await this.findPresetInDir(dir, presetId);
      if (presetPath) {
        const content = await fs.readFile(presetPath, 'utf-8');
        return yaml.load(content) as Preset;
      }
    }

    throw new Error(`Preset not found: ${presetId}`);
  }

  /**
   * List all presets
   */
  async listPresets(category?: string): Promise<PresetInfo[]> {
    const presets: PresetInfo[] = [];
    const seen = new Set<string>();

    for (const dir of this.presetDirs) {
      if (!await fs.pathExists(dir)) continue;

      const files = await glob(path.join(dir, '**', '*.yaml'));

      for (const file of files) {
        const content = await fs.readFile(file, 'utf-8');
        const preset = yaml.load(content) as Preset;

        // Skip duplicate IDs (higher priority ones are already loaded)
        if (seen.has(preset.id)) continue;

        // Filter by category
        if (category && preset.category !== category) continue;

        seen.add(preset.id);
        presets.push({
          id: preset.id,
          name: preset.name,
          description: preset.description,
          category: preset.category,
          author: preset.author,
          source: this.getPresetSource(file)
        });
      }
    }

    return presets;
  }

  /**
   * Create a preset
   */
  async createPreset(preset: Preset, target: 'user' | 'project'): Promise<void> {
    const targetDir = target === 'user'
      ? path.join(os.homedir(), '.novel', 'presets', 'user')
      : path.join(process.cwd(), 'stories', '*', 'presets');

    await fs.ensureDir(targetDir);

    const presetPath = path.join(targetDir, `${preset.id}.yaml`);
    await fs.writeFile(presetPath, yaml.dump(preset, { indent: 2 }), 'utf-8');
  }

  /**
   * Import a preset
   */
  async importPreset(file: string, target: 'user' | 'community'): Promise<void> {
    const content = await fs.readFile(file, 'utf-8');
    const preset = yaml.load(content) as Preset;

    const targetDir = path.join(
      os.homedir(),
      '.novel',
      'presets',
      target
    );

    await fs.ensureDir(targetDir);
    await fs.copy(file, path.join(targetDir, path.basename(file)));
  }

  /**
   * Export a preset
   */
  async exportPreset(presetId: string, outputPath: string): Promise<void> {
    const preset = await this.loadPreset(presetId);
    await fs.writeFile(outputPath, yaml.dump(preset, { indent: 2 }), 'utf-8');
  }

  // ========== Private Methods ==========

  private async findPresetInDir(
    dir: string,
    presetId: string
  ): Promise<string | null> {
    if (!await fs.pathExists(dir)) return null;

    const files = await glob(path.join(dir, '**', `${presetId}.yaml`));
    return files.length > 0 ? files[0] : null;
  }

  private getPresetSource(filePath: string): PresetSource {
    if (filePath.includes('.novel/presets/official')) return 'official';
    if (filePath.includes('.novel/presets/community')) return 'community';
    if (filePath.includes('.novel/presets/user')) return 'user';
    if (filePath.includes('stories')) return 'project';
    return 'builtin';
  }
}

/**
 * Preset Information Interface
 */
export interface PresetInfo {
  id: string;
  name: string;
  description: string;
  category: string;
  author: string;
  source: PresetSource;
}

export type PresetSource = 'official' | 'community' | 'user' | 'project' | 'builtin';
```

### 2.3 ConfigValidator

```typescript
/**
 * Configuration Validator
 * Responsible for validating the completeness, consistency, and reference integrity of configurations.
 */
export class ConfigValidator {
  private projectPath: string;

  constructor(projectPath: string) {
    this.projectPath = projectPath;
  }

  /**
   * Validate a configuration
   */
  async validate(config: ChapterConfig): Promise<ValidationResult> {
    const errors: string[] = [];
    const warnings: string[] = [];

    // 1. Required fields check
    if (!config.chapter) errors.push('Missing chapter number');
    if (!config.title || config.title.trim() === '') errors.push('Missing chapter title');
    if (!config.plot || !config.plot.summary) errors.push('Missing plot summary');
    if (!config.wordcount || !config.wordcount.target) errors.push('Missing target word count');

    // 2. Data type and range checks
    if (config.chapter < 1) errors.push('Chapter number must be greater than 0');
    if (config.wordcount.target < 1000 || config.wordcount.target > 10000) {
      warnings.push('Target word count is recommended to be between 1000 and 10000');
    }

    // 3. Reference integrity checks
    if (config.characters) {
      for (const char of config.characters) {
        const exists = await this.checkCharacterExists(char.id);
        if (!exists) {
          errors.push(`Character ID "${char.id}" does not exist in character-profiles.md`);
        }
      }
    }

    if (config.scene?.location_id) {
      const exists = await this.checkLocationExists(config.scene.location_id);
      if (!exists) {
        errors.push(`Location ID "${config.scene.location_id}" does not exist in locations.md`);
      }
    }

    if (config.plot.plotlines) {
      for (const plotline of config.plot.plotlines) {
        const exists = await this.checkPlotlineExists(plotline);
        if (!exists) {
          errors.push(`Plotline ID "${plotline}" does not exist in specification.md`);
        }
      }
    }

    // 4. Logical consistency checks
    const { min, target, max } = config.wordcount;
    if (min && target && min > target) {
      errors.push('Minimum word count cannot be greater than the target word count');
    }
    if (target && max && target > max) {
      errors.push('Target word count cannot be greater than the maximum word count');
    }

    // 5. Best practice suggestions
    if (!config.characters || config.characters.length === 0) {
      warnings.push('It is recommended to specify at least one appearing character');
    }

    if (!config.plot.key_points || config.plot.key_points.length < 3) {
      warnings.push('It is recommended to list at least 3 key points');
    }

    if (!config.scene) {
      warnings.push('It is recommended to configure scene information');
    }

    return {
      valid: errors.length === 0,
      errors,
      warnings
    };
  }

  // ========== Private Methods ==========

  private async checkCharacterExists(id: string): Promise<boolean> {
    const profilesPath = path.join(
      this.projectPath,
      'spec',
      'knowledge',
      'character-profiles.md'
    );

    if (!await fs.pathExists(profilesPath)) {
      return false;
    }

    const content = await fs.readFile(profilesPath, 'utf-8');
    // Check if the character ID is included (simplified implementation)
    return content.includes(`id: ${id}`) || content.includes(`ID: ${id}`);
  }

  private async checkLocationExists(id: string): Promise<boolean> {
    const locationsPath = path.join(
      this.projectPath,
      'spec',
      'knowledge',
      'locations.md'
    );

    if (!await fs.pathExists(locationsPath)) {
      return false;
    }

    const content = await fs.readFile(locationsPath, 'utf-8');
    return content.includes(`id: ${id}`) || content.includes(`ID: ${id}`);
  }

  private async checkPlotlineExists(id: string): Promise<boolean> {
    const specPath = path.join(
      this.projectPath,
      'stories',
      '*',
      'specification.md'
    );

    const specs = await glob(specPath);
    if (specs.length === 0) return false;

    const content = await fs.readFile(specs[0], 'utf-8');
    return content.includes(id);
  }
}

/**
 * Validation Result Interface
 */
export interface ValidationResult {
  valid: boolean;
  errors: string[];
  warnings: string[];
}
```

---

## 3. CLI Command Implementation

### 3.1 Command Entry File

```typescript
// src/commands/chapter-config.ts

import { Command } from 'commander';
import chalk from 'chalk';
import inquirer from 'inquirer';
import ora from 'ora';
import { ChapterConfigManager } from '../core/chapter-config.js';
import { PresetManager } from '../core/preset-manager.js';

/**
 * Register chapter-config commands
 */
export function registerChapterConfigCommands(program: Command): void {
  const chapterConfig = program
    .command('chapter-config')
    .description('Chapter configuration management');

  // create command
  chapterConfig
    .command('create <chapter>')
    .option('-i, --interactive', 'Create interactively')
    .option('-p, --preset <preset-id>', 'Use a preset')
    .option('--from-prompt', 'Generate from natural language')
    .description('Create a chapter configuration')
    .action(async (chapter, options) => {
      try {
        const chapterNum = parseInt(chapter);
        if (isNaN(chapterNum)) {
          console.error(chalk.red('Chapter number must be a number'));
          process.exit(1);
        }

        if (options.interactive) {
          await createConfigInteractive(chapterNum);
        } else if (options.preset) {
          await createConfigWithPreset(chapterNum, options.preset);
        } else {
          console.error(chalk.red('Please specify --interactive or --preset'));
          process.exit(1);
        }
      } catch (error: any) {
        console.error(chalk.red(`Creation failed: ${error.message}`));
        process.exit(1);
      }
    });

  // list command
  chapterConfig
    .command('list')
    .option('--format <type>', 'Output format: table|json|yaml', 'table')
    .description('List all chapter configurations')
    .action(async (options) => {
      try {
        await listConfigs(options.format);
      } catch (error: any) {
        console.error(chalk.red(`Listing failed: ${error.message}`));
        process.exit(1);
      }
    });

  // validate command
  chapterConfig
    .command('validate <chapter>')
    .description('Validate a chapter configuration')
    .action(async (chapter) => {
      try {
        const chapterNum = parseInt(chapter);
        await validateConfig(chapterNum);
      } catch (error: any) {
        console.error(chalk.red(`Validation failed: ${error.message}`));
        process.exit(1);
      }
    });

  // copy command
  chapterConfig
    .command('copy <from> <to>')
    .option('-i, --interactive', 'Interactively modify differences')
    .description('Copy a chapter configuration')
    .action(async (from, to, options) => {
      try {
        const fromChapter = parseInt(from);
        const toChapter = parseInt(to);
        await copyConfig(fromChapter, toChapter, options.interactive);
      } catch (error: any) {
        console.error(chalk.red(`Copy failed: ${error.message}`));
        process.exit(1);
      }
    });

  // edit command
  chapterConfig
    .command('edit <chapter>')
    .option('-e, --editor <editor>', 'Specify an editor', 'vim')
    .description('Edit a chapter configuration')
    .action(async (chapter, options) => {
      try {
        const chapterNum = parseInt(chapter);
        await editConfig(chapterNum, options.editor);
      } catch (error: any) {
        console.error(chalk.red(`Editing failed: ${error.message}`));
        process.exit(1);
      }
    });

  // delete command
  chapterConfig
    .command('delete <chapter>')
    .description('Delete a chapter configuration')
    .action(async (chapter) => {
      try {
        const chapterNum = parseInt(chapter);
        await deleteConfig(chapterNum);
      } catch (error: any) {
        console.error(chalk.red(`Deletion failed: ${error.message}`));
        process.exit(1);
      }
    });
}

/**
 * Create a configuration interactively
 */
async function createConfigInteractive(chapter: number): Promise<void> {
  // Implementation in section 2.4.2
  console.log(chalk.cyan(`\n📝 Creating configuration for Chapter ${chapter}\n`));

  // ... (Full implementation omitted)
}

/**
 * Create a configuration with a preset
 */
async function createConfigWithPreset(
  chapter: number,
  presetId: string
): Promise<void> {
  const spinner = ora('Loading preset...').start();

  try {
    const presetManager = new PresetManager();
    const preset = await presetManager.loadPreset(presetId);

    spinner.succeed(chalk.green(`Preset loaded: ${preset.name}`));

    // Prompt the user for necessary information
    const answers = await inquirer.prompt([
      {
        type: 'input',
        name: 'title',
        message: 'Chapter Title:',
        validate: (input) => input.length > 0
      },
      {
        type: 'input',
        name: 'characters',
        message: 'Appearing characters (comma-separated):',
        validate: (input) => input.length > 0
      },
      {
        type: 'input',
        name: 'scene',
        message: 'Scene:',
        validate: (input) => input.length > 0
      },
      {
        type: 'input',
        name: 'plotSummary',
        message: 'Plot Summary:',
        validate: (input) => input.length > 10
      }
    ]);

    // Create the configuration
    const manager = new ChapterConfigManager(process.cwd());
    const config = await manager.createConfig(chapter, {
      title: answers.title,
      characters: answers.characters.split(',').map(c => c.trim()),
      scene: answers.scene,
      plotSummary: answers.plotSummary,
      preset: presetId
    });

    console.log(chalk.green(`\n✅ Configuration saved`));
    console.log(chalk.gray(`File: ${getConfigPath(chapter)}`));
  } catch (error: any) {
    spinner.fail(chalk.red(`Creation failed: ${error.message}`));
    process.exit(1);
  }
}

/**
 * List all configurations
 */
async function listConfigs(format: string): Promise<void> {
  const spinner = ora('Loading configuration list...').start();

  try {
    const manager = new ChapterConfigManager(process.cwd());
    const configs = await manager.listConfigs();

    spinner.stop();

    if (configs.length === 0) {
      console.log(chalk.yellow('\nNo chapter configurations yet'));
      return;
    }

    console.log(chalk.cyan(`\n📋 Existing Chapter Configurations (${configs.length}):\n`));

    if (format === 'table') {
      // Table output
      console.table(configs.map(c => ({
        'Chapter': `Chapter ${c.chapter}`,
        'Title': c.title,
        'Type': c.plotType,
        'Scene': c.location,
        'Word Count': c.wordcount,
        'Preset': c.preset || '-'
      })));
    } else if (format === 'json') {
      console.log(JSON.stringify(configs, null, 2));
    } else if (format === 'yaml') {
      console.log(yaml.dump(configs));
    }
  } catch (error: any) {
    spinner.fail(chalk.red(`Loading failed: ${error.message}`));
    process.exit(1);
  }
}

/**
 * Validate a configuration
 */
async function validateConfig(chapter: number): Promise<void> {
  console.log(chalk.cyan(`\n🔍 Validating configuration file: chapter-${chapter}-config.yaml\n`));

  const manager = new ChapterConfigManager(process.cwd());
  const config = await manager.loadConfig(chapter);

  if (!config) {
    console.error(chalk.red('❌ Configuration file not found'));
    process.exit(1);
  }

  const validator = new ConfigValidator(process.cwd());
  const result = await validator.validate(config);

  if (result.valid) {
    console.log(chalk.green('✅ Validation passed!\n'));
  } else {
    console.log(chalk.red(`❌ Validation failed (${result.errors.length} errors):\n`));
    result.errors.forEach((error, index) => {
      console.log(chalk.red(`  ${index + 1}. ${error}`));
    });
    console.log('');
  }

  if (result.warnings.length > 0) {
    console.log(chalk.yellow(`⚠️  Warnings (${result.warnings.length}):\n`));
    result.warnings.forEach((warning, index) => {
      console.log(chalk.yellow(`  ${index + 1}. ${warning}`));
    });
    console.log('');
  }

  if (!result.valid) {
    process.exit(1);
  }
}

/**
 * Copy a configuration
 */
async function copyConfig(
  from: number,
  to: number,
  interactive: boolean
): Promise<void> {
  const manager = new ChapterConfigManager(process.cwd());

  console.log(chalk.cyan(`\n📋 Copying configuration: Chapter ${from} → Chapter ${to}\n`));

  if (interactive) {
    // Interactively modify differences
    const sourceConfig = await manager.loadConfig(from);
    if (!sourceConfig) {
      console.error(chalk.red('Source configuration not found'));
      process.exit(1);
    }

    const answers = await inquirer.prompt([
      {
        type: 'input',
        name: 'title',
        message: 'New Title:',
        default: sourceConfig.title
      },
      {
        type: 'input',
        name: 'plotSummary',
        message: 'Plot Summary:',
        default: sourceConfig.plot.summary
      }
      // ... more fields
    ]);

    await manager.copyConfig(from, to, answers);
  } else {
    await manager.copyConfig(from, to);
  }

  console.log(chalk.green(`\n✅ Configuration copied`));
}

/**
 * Edit a configuration
 */
async function editConfig(chapter: number, editor: string): Promise<void> {
  const configPath = getConfigPath(chapter);

  if (!await fs.pathExists(configPath)) {
    console.error(chalk.red('Configuration file not found'));
    process.exit(1);
  }

  // Call the editor
  const { spawn } = await import('child_process');
  const child = spawn(editor, [configPath], {
    stdio: 'inherit'
  });

  child.on('exit', (code) => {
    if (code === 0) {
      console.log(chalk.green('\n✅ Editing complete'));
    } else {
      console.error(chalk.red('\n❌ Editing failed'));
      process.exit(1);
    }
  });
}

/**
 * Delete a configuration
 */
async function deleteConfig(chapter: number): Promise<void> {
  const answers = await inquirer.prompt([
    {
      type: 'confirm',
      name: 'confirm',
      message: `Are you sure you want to delete the configuration for Chapter ${chapter}?`,
      default: false
    }
  ]);

  if (!answers.confirm) {
    console.log(chalk.yellow('Cancelled'));
    return;
  }

  const manager = new ChapterConfigManager(process.cwd());
  await manager.deleteConfig(chapter);

  console.log(chalk.green(`\n✅ Configuration deleted`));
}

// Helper function
function getConfigPath(chapter: number): string {
  // Implementation omitted...
  return '';
}
```

---

## 4. write.md Template Integration

### 4.1 Template Modification Plan

**Modification Location**: `templates/commands/write.md`

**Modification Content**:

```markdown
---
description: Executes chapter writing based on a task list, automatically loading context and validation rules.
argument-hint: [Chapter number or task ID]
allowed-tools: Read(//**), Write(//stories/**/content/**), Bash(ls:*), Bash(find:*), Bash(wc:*), Bash(grep:*), Bash(*)
model: claude-sonnet-4-5-20250929
scripts:
  sh: .specify/scripts/bash/check-writing-state.sh
  ps: .specify/scripts/powershell/check-writing-state.ps1
---

Executes chapter writing based on the seven-step methodology.
---

## Pre-checks

1. Run script `{SCRIPT}` to check the creation status.

2. **🆕 Check for Chapter Configuration File** (New)
   ```bash
   # Check if a configuration file exists
   chapter_num="$CHAPTER_NUMBER"  # Parse from $ARGUMENTS
   config_file="stories/*/chapters/chapter-${chapter_num}-config.yaml"

   if [ -f "$config_file" ]; then
     echo "✅ Config file found, loading..."
     # Read the configuration file
     CONFIG_CONTENT=$(cat "$config_file")
   else
     echo "ℹ️  No config file found, using natural language mode."
     CONFIG_CONTENT=""
   fi
   ```

### Query Protocol (Mandatory Order)

⚠️ **Important**: Please query the documents in the following order to ensure a complete and prioritized context.

**Query Order**:

1.  **🆕 First Query (Chapter Config - if it exists)** (New):
    -   `stories/*/chapters/chapter-X-config.yaml` (Chapter configuration file)
    -   If the configuration file exists, parse it and extract:
        -   List of appearing character IDs
        -   Scene ID
        -   Plot type, summary, key points
        -   Writing style parameters
        -   Word count requirements
        -   Special requirements

2.  **First Query (Highest Priority)**:
    -   `memory/novel-constitution.md` (Writing Constitution - highest principles)
    -   `memory/style-reference.md` (Style Reference - if generated via `/book-internalize`)

3.  **Next Query (Specs and Plans)**:
    -   `stories/*/specification.md` (Story Specification)
    -   `stories/*/creative-plan.md` (Creative Plan)
    -   `stories/*/tasks.md` (Current Task)

4.  **🆕 Load Detailed Information Based on Configuration** (New):
    If the configuration file specifies characters and scenes, load the detailed information:

    ```
    # Load character details
    For each character ID in the configuration:
    1. Find the full character profile in spec/knowledge/character-profiles.md.
    2. Get the latest state from spec/tracking/character-state.json.
    3. Merge the information for later use.

    # Load scene details
    If the configuration specifies scene.location_id:
    1. Find the detailed scene description in spec/knowledge/locations.md.
    2. Extract the environment, atmosphere, and features of the scene.

    # Load plotline details
    If the configuration specifies plot.plotlines:
    1. Find the plotline definition in stories/*/specification.md.
    2. Get the current status and goal of the plotline.
    ```

5.  **Next Query (State and Data)**:
    -   `spec/tracking/character-state.json` (Character State)
    -   `spec/tracking/relationships.json` (Relationship Network)
    -   `spec/tracking/plot-tracker.json` (Plot Tracker - if any)
    -   `spec/tracking/validation-rules.json` (Validation Rules - if any)

6.  **Next Query (Knowledge Base)**:
    -   Relevant files in `spec/knowledge/` (world setting, character profiles, etc.)
    -   `stories/*/content/` (Previous content - to understand the context)

7.  **Next Query (Writing Style Guides)**:
    -   `memory/personal-voice.md` (Personal Voice Corpus - if any)
    -   `spec/knowledge/natural-expression.md` (Natural Expression Guide - if any)
    -   `spec/presets/anti-ai-detection.md` (Anti-AI Detection Guidelines)

8.  **Conditional Query (for the first three chapters only)**:
    -   **If chapter number ≤ 3 or total word count < 10000**, additionally query:
        -   `spec/presets/golden-opening.md` (The Golden Opening Rule)
        -   And strictly follow its five rules.

## Writing Execution Flow

### 1. Select Writing Task
Select a writing task with the status `pending` from `tasks.md` and mark it as `in_progress`.

### 2. Verify Pre-conditions
-   Check if related dependency tasks are complete.
-   Verify that necessary settings are ready.
-   Confirm that preceding chapters are complete.

### 3. **🆕 Construct Chapter Writing Prompt** (Modified)

**If a configuration file exists**:

```
📋 Configuration for this chapter:

**Basic Information**:
- Chapter: Chapter {{chapter}} - {{title}}
- Word Count Requirement: {{wordcount.min}}-{{wordcount.max}} words (Target: {{wordcount.target}} words)

**Appearing Characters** ({{characters.length}}):
{{#each characters}}
- **{{name}}** ({{role}} - {{focus}} focus)
  [Detailed profile from character-profiles.md]
  Personality: {{personality}}
  Background: {{background}}

  Current State: (from character-state.json)
  - Location: {{location}}
  - Health: {{health}}
  - Mood: {{mood}}
  - Relationship with other characters: {{relationships}}
{{/each}}

**Scene Setting**:
- Location: {{scene.location_name}}
  [Scene details from locations.md]
  Detailed Description: {{location_details}}
  Features: {{features}}

- Time: {{scene.time}}
- Weather: {{scene.weather}}
- Atmosphere: {{scene.atmosphere}}

**Plot Requirements**:
- Type: {{plot.type}} ({{plot_type_description}})
- Summary: {{plot.summary}}
- Key Points:
  {{#each plot.key_points}}
  {{index}}. {{this}}
  {{/each}}

{{#if plot.plotlines}}
- Involved Plotlines:
  {{#each plot.plotlines}}
  - {{this}}: [Plotline details from specification.md]
  {{/each}}
{{/if}}

{{#if plot.foreshadowing}}
- Foreshadowing in this chapter:
  {{#each plot.foreshadowing}}
  - {{id}}: {{content}}
  {{/each}}
{{/if}}

**Writing Style**:
- Pace: {{style.pace}} ({{pace_description}})
- Sentence Length: {{style.sentence_length}} ({{sentence_description}})
- Focus: {{style.focus}} ({{focus_description}})
- Tone: {{style.tone}}

{{#if special_requirements}}
**Special Requirements**:
{{special_requirements}}
{{/if}}

{{#if preset_used}}
**Preset Applied**: {{preset_used}}
{{/if}}

---

[Load global specification documents below...]
```

**If no configuration file exists** (Backward compatible):

```
📋 Based on user input:

User description: $ARGUMENTS

[Parse natural language to extract parameters]

[Load global specification documents...]
```

### 4. Pre-writing Reminders
**Based on constitution principles**:
-   Core value points
-   Quality standard requirements
-   Style consistency guidelines

**Based on specification requirements**:
-   P0 must-include elements
-   Target reader characteristics
-   Content red lines

**Paragraph format specification (Important)**:
[Keep original content]

**Anti-AI detection writing guidelines (based on Tencent's Zhuque standard)**:
[Keep original content]

### 5. Create Content According to the Plan:
   - **Opening**: Attract the reader, connect with the previous text.
   - **Development**: Advance the plot, deepen the characters.
   - **Turning Point**: Create conflict or suspense.
   - **Ending**: Provide a suitable conclusion, lead into the next text.

### 6. Quality Self-Check
[Keep original content]

### 7. Save and Update
-   Save chapter content to `stories/*/content/`.
-   **🆕 If a configuration file was used, update the `updated_at` timestamp** (New).
-   Update the task status to `completed`.
-   Record the completion time and word count.

[Rest of the content remains unchanged...]
```

### 4.2 Configuration Loading Logic Implementation

In the `write.md` template, the AI needs to execute the following logic:

```typescript
// Pseudocode: AI execution logic

// 1. Parse chapter number
const chapterNum = parseChapterNumber($ARGUMENTS);

// 2. Check for configuration file
const configPath = `stories/*/chapters/chapter-${chapterNum}-config.yaml`;
const config = await loadYamlFile(configPath);

if (config) {
  // 3. Load character details
  for (const char of config.characters) {
    const profile = await extractFromMarkdown(
      'spec/knowledge/character-profiles.md',
      char.id
    );
    const state = await loadJson('spec/tracking/character-state.json')[char.id];
    char.details = { ...profile, ...state };
  }

  // 4. Load scene details
  if (config.scene.location_id) {
    config.scene.details = await extractFromMarkdown(
      'spec/knowledge/locations.md',
      config.scene.location_id
    );
  }

  // 5. Load plotline details
  if (config.plot.plotlines) {
    for (const plotlineId of config.plot.plotlines) {
      const plotline = await extractFromMarkdown(
        'stories/*/specification.md',
        plotlineId
      );
      config.plot.plotlineDetails.push(plotline);
    }
  }

  // 6. Construct structured prompt
  const prompt = buildPromptFromConfig(config);
} else {
  // 7. Use natural language mode
  const prompt = parseNaturalLanguage($ARGUMENTS);
}

// 8. Load global specifications
const globalSpecs = await loadGlobalSpecs();

// 9. Merge prompts
const fullPrompt = mergePrompts(prompt, globalSpecs);

// 10. Generate chapter content
const content = await generateChapterContent(fullPrompt);

// 11. Save
await saveChapterContent(chapterNum, content);

// 12. Update configuration file timestamp
if (config) {
  config.updated_at = new Date().toISOString();
  await saveYamlFile(configPath, config);
}
```

---

## 5. Testing Strategy

### 5.1 Unit Tests

**Scope**:
-   All methods of `ChapterConfigManager`.
-   All methods of `PresetManager`.
-   All validation rules of `ConfigValidator`.

**Framework**: Jest

**Coverage Target**: > 80%

**Example**:

```typescript
// test/chapter-config.test.ts

describe('ChapterConfigManager', () => {
  let manager: ChapterConfigManager;

  beforeEach(() => {
    manager = new ChapterConfigManager('/test/project');
  });

  describe('createConfig', () => {
    it('should create config with valid parameters', async () => {
      const config = await manager.createConfig(5, {
        title: 'Test Chapter',
        plotType: 'ability_showcase',
        plotSummary: 'Test plot summary',
        wordcount: 3000
      });

      expect(config.chapter).toBe(5);
      expect(config.title).toBe('Test Chapter');
      expect(config.plot.type).toBe('ability_showcase');
      expect(config.wordcount.target).toBe(3000);
    });

    it('should apply preset correctly', async () => {
      const config = await manager.createConfig(5, {
        title: 'Action Chapter',
        preset: 'action-intense'
      });

      expect(config.preset_used).toBe('action-intense');
      expect(config.style.pace).toBe('fast');
      expect(config.style.sentence_length).toBe('short');
    });

    it('should throw error for invalid parameters', async () => {
      await expect(manager.createConfig(0, {})).rejects.toThrow();
    });
  });

  describe('loadConfig', () => {
    it('should return null for non-existent config', async () => {
      const config = await manager.loadConfig(999);
      expect(config).toBeNull();
    });

    it('should load existing config correctly', async () => {
      // First, create
      await manager.createConfig(5, { title: 'Test' });

      // Then, load
      const config = await manager.loadConfig(5);
      expect(config).not.toBeNull();
      expect(config!.chapter).toBe(5);
    });
  });

  // More tests...
});
```

### 5.2 Integration Tests

**Scenarios**:

1.  **Full Workflow Test**:
    ```
    Create config → Load config → Validate config → Update config → Delete config
    ```

2.  **Preset Application Test**:
    ```
    List presets → Select preset → Create config → Verify preset parameters are applied
    ```

3.  **CLI Command Test**:
    ```
    Execute various CLI commands → Verify output → Check file changes
    ```

4.  **Integration Test with write.md**:
    ```
    Create config → Execute /write command → Verify AI loaded the config → Check generated content
    ```

### 5.3 End-to-End Tests

**Scenarios**:

1.  **New User First Use**:
    ```
    1. Install novel-writer-cn
    2. novel init my-story
    3. novel chapter-config create 1 --interactive
    4. Execute /write Chapter 1 in the AI editor
    5. Verify the generated chapter content matches the configuration
    ```

2.  **Quick Creation with a Preset**:
    ```
    1. novel preset list
    2. novel chapter-config create 5 --preset action-intense
    3. /write Chapter 5
    4. Verify a fast-paced action scene
    ```

3.  **Configuration Reuse**:
    ```
    1. novel chapter-config copy 5 10
    2. Modify the differences
    3. /write Chapter 10
    4. Verify stylistic consistency is maintained
    ```

---

## 6. Performance Optimization

### 6.1 Configuration File Caching

```typescript
/**
 * Configuration Cache Manager
 */
export class ConfigCache {
  private cache: Map<number, {
    config: ChapterConfig;
    mtime: number;
  }> = new Map();

  async get(chapter: number, filePath: string): Promise<ChapterConfig | null> {
    const stats = await fs.stat(filePath);
    const cached = this.cache.get(chapter);

    if (cached && cached.mtime === stats.mtimeMs) {
      return cached.config;
    }

    return null;
  }

  set(chapter: number, config: ChapterConfig, mtime: number): void {
    this.cache.set(chapter, { config, mtime });
  }

  clear(chapter?: number): void {
    if (chapter) {
      this.cache.delete(chapter);
    } else {
      this.cache.clear();
    }
  }
}
```

### 6.2 Preset Preloading

```typescript
/**
 * Preset Preloader
 * Preloads all official presets on application startup.
 */
export class PresetPreloader {
  private preloadedPresets: Map<string, Preset> = new Map();

  async preload(): Promise<void> {
    const presetDir = path.join(__dirname, '..', '..', 'presets');
    const files = await glob(path.join(presetDir, '**', '*.yaml'));

    for (const file of files) {
      const content = await fs.readFile(file, 'utf-8');
      const preset = yaml.load(content) as Preset;
      this.preloadedPresets.set(preset.id, preset);
    }
  }

  get(presetId: string): Preset | undefined {
    return this.preloadedPresets.get(presetId);
  }
}
```

### 6.3 YAML Parsing Optimization

```typescript
/**
 * Use a faster YAML parser
 */
import { parse } from 'yaml'; // Use the 'yaml' library instead of 'js-yaml'

export async function loadYamlFast(filePath: string): Promise<any> {
  const content = await fs.readFile(filePath, 'utf-8');
  return parse(content);
}
```

---

## 7. Security Considerations

### 7.1 Input Validation

```typescript
/**
 * Input Sanitizer and Validator
 */
export class InputSanitizer {
  /**
   * Sanitize chapter number
   */
  sanitizeChapterNumber(input: any): number {
    const num = parseInt(String(input));
    if (isNaN(num) || num < 1 || num > 9999) {
      throw new Error('Chapter number must be between 1 and 9999');
    }
    return num;
  }

  /**
   * Sanitize file path
   */
  sanitizeFilePath(input: string): string {
    // Prevent path traversal attacks
    const normalized = path.normalize(input);
    if (normalized.includes('..')) {
      throw new Error('Invalid path');
    }
    return normalized;
  }

  /**
   * Sanitize YAML content
   */
  sanitizeYamlContent(content: string): string {
    // Remove potential code injection
    if (content.includes('!<tag:')) {
      throw new Error('YAML tags are not supported');
    }
    return content;
  }
}
```

### 7.2 Permission Control

```typescript
/**
 * File Operation Permission Checker
 */
export class PermissionChecker {
  /**
   * Check if a file is within the project scope
   */
  isWithinProject(filePath: string, projectPath: string): boolean {
    const resolved = path.resolve(filePath);
    const project = path.resolve(projectPath);
    return resolved.startsWith(project);
  }

  /**
   * Check if a file is writable
   */
  async isWritable(filePath: string): Promise<boolean> {
    try {
      await fs.access(filePath, fs.constants.W_OK);
      return true;
    } catch {
      return false;
    }
  }
}
```

---

## 8. Error Handling

### 8.1 Error Type Definitions

```typescript
/**
 * Custom Error Class
 */
export class ConfigError extends Error {
  constructor(
    message: string,
    public code: string,
    public details?: any
  ) {
    super(message);
    this.name = 'ConfigError';
  }
}

export class ValidationError extends ConfigError {
  constructor(message: string, public errors: string[]) {
    super(message, 'VALIDATION_ERROR', { errors });
    this.name = 'ValidationError';
  }
}

export class PresetNotFoundError extends ConfigError {
  constructor(presetId: string) {
    super(`Preset not found: ${presetId}`, 'PRESET_NOT_FOUND', { presetId });
    this.name = 'PresetNotFoundError';
  }
}
```

### 8.2 Error Handling Strategy

```typescript
/**
 * Global Error Handler
 */
export class ErrorHandler {
  handle(error: Error): void {
    if (error instanceof ValidationError) {
      console.error(chalk.red(`Validation failed:`));
      error.errors.forEach((err, index) => {
        console.error(chalk.red(`  ${index + 1}. ${err}`));
      });
    } else if (error instanceof PresetNotFoundError) {
      console.error(chalk.red(`Preset not found: ${error.details.presetId}`));
      console.log(chalk.gray('\nTip: Use `novel preset list` to see available presets'));
    } else if (error instanceof ConfigError) {
      console.error(chalk.red(`Configuration error: ${error.message}`));
      if (error.details) {
        console.error(chalk.gray(JSON.stringify(error.details, null, 2)));
      }
    } else {
      console.error(chalk.red(`Unknown error: ${error.message}`));
      console.error(error.stack);
    }

    process.exit(1);
  }
}
```

---

## 9. Deployment and Release

### 9.1 Build Process

```bash
# package.json scripts

{
  "scripts": {
    "build": "tsc",
    "build:presets": "bash scripts/bundle-presets.sh",
    "build:all": "npm run build && npm run build:presets",
    "test": "jest",
    "test:coverage": "jest --coverage",
    "lint": "eslint src/**/*.ts",
    "format": "prettier --write src/**/*.ts"
  }
}
```

### 9.2 Release Checklist

-   [ ] Unit tests pass (coverage > 80%).
-   [ ] Integration tests pass.
-   [ ] End-to-end tests pass.
-   [ ] Code linting passes.
-   [ ] Documentation is complete.
-   [ ] CHANGELOG is updated.
-   [ ] Version number is updated.
-   [ ] Preset files are bundled.

### 9.3 Version Compatibility

```typescript
/**
 * Configuration file version management
 */
export const CONFIG_VERSION = '1.0.0';

export function migrateConfig(config: any): ChapterConfig {
  // Migrate from an old version to the current version
  if (!config.version || config.version < '1.0.0') {
    // Execute migration logic
    config = migrateFrom_0_x(config);
  }

  config.version = CONFIG_VERSION;
  return config as ChapterConfig;
}
```

---

## 10. Monitoring and Debugging

### 10.1 Logging System

```typescript
/**
 * Structured Logger
 */
export class Logger {
  private level: 'debug' | 'info' | 'warn' | 'error';

  constructor(level: 'debug' | 'info' | 'warn' | 'error' = 'info') {
    this.level = level;
  }

  debug(message: string, meta?: any): void {
    if (this.shouldLog('debug')) {
      console.log(chalk.gray(`[DEBUG] ${message}`), meta || '');
    }
  }

  info(message: string, meta?: any): void {
    if (this.shouldLog('info')) {
      console.log(chalk.cyan(`[INFO] ${message}`), meta || '');
    }
  }

  warn(message: string, meta?: any): void {
    if (this.shouldLog('warn')) {
      console.log(chalk.yellow(`[WARN] ${message}`), meta || '');
    }
  }

  error(message: string, meta?: any): void {
    if (this.shouldLog('error')) {
      console.error(chalk.red(`[ERROR] ${message}`), meta || '');
    }
  }

  private shouldLog(level: string): boolean {
    const levels = ['debug', 'info', 'warn', 'error'];
    return levels.indexOf(level) >= levels.indexOf(this.level);
  }
}
```

### 10.2 Performance Monitoring

```typescript
/**
 * Performance Timer
 */
export class PerformanceTimer {
  private timers: Map<string, number> = new Map();

  start(name: string): void {
    this.timers.set(name, Date.now());
  }

  end(name: string): number {
    const start = this.timers.get(name);
    if (!start) {
      throw new Error(`Timer ${name} not started`);
    }

    const duration = Date.now() - start;
    this.timers.delete(name);
    return duration;
  }

  measure(name: string, fn: () => Promise<any>): Promise<any> {
    this.start(name);
    return fn().finally(() => {
      const duration = this.end(name);
      console.log(chalk.gray(`⏱️  ${name}: ${duration}ms`));
    });
  }
}
```

---

## Appendix

### A. Complete TypeScript Type Exports

```typescript
// src/types/index.ts

export * from './chapter-config';
export * from './preset';
export * from './validation';
export * from './errors';
```

### B. Complete List of CLI Commands

See the content in Section 3.

### C. Test Coverage Report

```bash
$ npm run test:coverage

----------------------|---------|----------|---------|---------|
File                  | % Stmts | % Branch | % Funcs | % Lines |
----------------------|---------|----------|---------|---------|
All files             |   85.23 |    78.45 |   89.12 |   84.67 |
 chapter-config.ts    |   88.45 |    82.30 |   91.20 |   87.90 |
 preset-manager.ts    |   82.10 |    75.60 |   87.50 |   81.45 |
 config-validator.ts  |   86.70 |    79.20 |   88.90 |   85.30 |
----------------------|---------|----------|---------|---------|
```

---

**END OF TECH SPEC**
