---
name: track-init
description: Initialize the tracking system, setting up tracking data based on the story outline.
allowed-tools: Read(//stories/**/specification.md), Read(stories/**/specification.md), Read(//stories/**/outline.md), Read(stories/**/outline.md), Read(//stories/**/creative-plan.md), Read(stories/**/creative-plan.md), Write(//spec/tracking/**), Write(spec/tracking/**), Bash(find:*), Bash(grep:*), Bash(wc:*), Bash(*)
model: claude-sonnet-4-5-20250929
scripts:
  sh: .specify/scripts/bash/init-tracking.sh
  ps: .specify/scripts/powershell/init-tracking.ps1
---

# Initialize Tracking System

Initializes all tracking data files based on the created story outline and chapter plan.

## When to Use

Execute this command after completing `/story` and `/outline`, but before starting to write.

## Initialization Process

1.  **Read Base Data**
    -   Read `stories/*/story.md` to get the story settings.
    -   Read `stories/*/outline.md` to get the chapter plan.
    -   Read `.specify/config.json` to get the writing methodology.

2.  **Initialize Tracking Files**

    **Important**: Prioritize reading the plotline management specifications from Chapter 5 of `specification.md` to populate the tracking files.

    Create or update `spec/tracking/plot-tracker.json`:
    -   Read all plotline definitions from `specification.md 5.1`.
    -   Read all intersection points from `specification.md 5.3`.
    -   Read all foreshadowing from `specification.md 5.4`.
    -   Read the plotline distribution for chapter sections from `creative-plan.md`.
    -   Set the current status (assuming writing has not yet begun).

    **`plot-tracker.json` Structure**:
    ```json
    {
      "novel": "[Read story name from specification.md]",
      "lastUpdated": "[YYYY-MM-DD]",
      "currentState": {
        "chapter": 0,
        "volume": 1,
        "mainPlotStage": "[Initial Stage]"
      },
      "plotlines": {
        "main": {
          "name": "[Main Plotline Name]",
          "status": "active",
          "currentNode": "[Starting Point]",
          "completedNodes": [],
          "upcomingNodes": "[Read from intersection points and chapter plan]"
        },
        "subplots": [
          {
            "id": "[Read from 5.1, e.g., PL-01]",
            "name": "[Plotline Name]",
            "type": "[Main/Subplot/Main Support]",
            "priority": "[P0/P1/P2]",
            "status": "[active/dormant]",
            "plannedStart": "[Starting Chapter]",
            "plannedEnd": "[Ending Chapter]",
            "currentNode": "[Current Node]",
            "completedNodes": [],
            "upcomingNodes": "[Read from intersection points table]",
            "intersectionsWith": "[Read related plotlines from 5.3 intersection points table]",
            "activeChapters": "[Read from 5.2 pacing plan]"
          }
        ]
      },
      "foreshadowing": [
        {
          "id": "[Read from 5.4, e.g., F-001]",
          "content": "[Foreshadowing Content]",
          "planted": {"chapter": null, "description": "[Setup Description]"},
          "hints": [],
          "plannedReveal": {"chapter": "[Reveal Chapter]", "description": "[Reveal Method]"},
          "status": "planned",
          "importance": "[high/medium/low]",
          "relatedPlotlines": "[List of related plotline IDs]"
        }
      ],
      "intersections": [
        {
          "id": "[Read from 5.3, e.g., X-001]",
          "chapter": "[Intersection Chapter]",
          "plotlines": "[List of involved plotline IDs]",
          "content": "[Intersection Content]",
          "status": "upcoming",
          "impact": "[Expected Effect]"
        }
      ]
    }
    ```

    Create or update `spec/tracking/timeline.json`:
    -   Set time nodes based on the chapter plan.
    -   Mark important time events.

    Create or update `spec/tracking/relationships.json`:
    -   Extract initial relationships from character settings.
    -   Set up faction groups.

    Create or update `spec/tracking/character-state.json`:
    -   Initialize character statuses.
    -   Set starting locations.

3.  **Generate Tracking Report**
    Display the initialization results to confirm that the tracking system is ready.

## Smart Association

-   Automatically set checkpoints based on the writing methodology.
-   **Hero's Journey**: 12 tracking points for the 12 stages.
-   **Three-Act Structure**: Turning points for the three acts.
-   **Seven-Point Structure**: 7 key nodes.

After the tracking system is initialized, subsequent writing will automatically update this data.
