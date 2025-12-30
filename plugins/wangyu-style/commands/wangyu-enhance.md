# Wang Yu Style Enhancement

Convert or enhance existing text to the Wang Yu style, injecting Wang Yu's characteristics while maintaining the story's logic.

## Usage

```
/wangyu-enhance [text or file path] [options]
```

## Enhancement Modes

### Light Enhancement (--light)
- Adjust the narrative tone.
- Standardize the wording of the power system.
- Add a small amount of environmental description.

### Medium Enhancement (--medium) - Default
- Rewrite dialogue and actions.
- Increase the description of resource scarcity.
- Insert characters' psychological activities.
- Moderately increase the suspense.

### Heavy Enhancement (--heavy)
- Completely restructure the paragraphs.
- Add world-building details.
- Reshape the characters' personalities.
- Set up foreshadowing and suspense.
- Adjust the plot's pacing.

## Enhancement Example

### Original Text
```
Zhang San walked into the cave and saw many treasures inside. He was very happy and collected them.
```

### Light Enhancement
```
Zhang San cautiously stepped into the cave dwelling. His spiritual sense swept across and he found several magical artifacts piled in a corner. He suppressed the joy in his heart, first setting up a warning formation, and only then did he begin to collect them.
```

### Medium Enhancement
```
Zhang San paused for a moment at the entrance of the cave. After confirming that no restrictions had been triggered, he slowly walked into the cave dwelling.

By the light of a night-luminescent pearl, he saw the situation inside the cave clearly—three magical artifacts of good quality lay quietly on a stone platform, with a few storage bags next to them.

"A senior's cave dwelling?" Zhang San's heart stirred, but his years of experience kept him vigilant. There was no such thing as a free lunch in this world; these things appeared too easily.

He first probed with his magical power to confirm that there were no hidden restrictions, and only then did he carefully collect them. Even so, he only took half, leaving the rest as a way out.
```

### Heavy Enhancement
```
At the entrance of the cave dwelling, Zhang San stopped.

The air was filled with a faint medicinal fragrance, mixed with a hint of a faint smell of blood. This was a smell he was very familiar with—a cultivator had fallen here, no more than three months ago.

He took out a detection talisman. With a flash of golden light, the talisman paper turned into dozens of light spots and entered the cave. A moment later, the light spots returned one by one, and the information they brought made his brow furrow slightly.

There were indeed many spiritual objects in the depths of the cave: three magical artifacts, seven storage bags, and a complete alchemy furnace. But there was an area in the deepest part that the detection talisman could not penetrate.

"Is it a feint, or is there something else going on?"

Zhang San pondered for a moment and took out a corpse puppet from his storage bag. This was a pathfinding tool that he had bought at a great price, which could save his life at a critical moment.

The corpse puppet walked stiffly into the cave dwelling, while he stood a hundred zhang away, controlling it with his spiritual sense.

Half an incense stick later, the corpse puppet safely returned with two storage bags. After Zhang San checked them, he found that they only contained some low-level pills and a thousand or so low-grade spirit stones.

"Bait," he concluded.

The real good stuff was probably in that undetectable area. But the level of danger there was probably far beyond his ability to handle.

Zhang San decisively turned and left. After so many years in the cultivation world, he knew one thing very well—greed was the main reason cultivators fell.
```

## Enhancement Points

### 1. Narrative Style
- Remove exclamation marks and use periods.
- Reduce adjectives and increase specific descriptions.
- Maintain the objectivity of the third-person perspective.

### 2. Detail Supplementation
- Add cultivation elements such as magical power and spiritual sense.
- Supplement with specific information such as time and distance.
- Increase the description of the environmental atmosphere.

### 3. Character Behavior
- Reflects caution and calculation.
- Shows experience and methods.
- Highlights the weighing of interests.

### 4. Plot Deepening
- Complicate simple plots.
- Add uncertain factors.
- Plant potential crises.

## Configuration Options

- `--mode <light|medium|heavy>` - Enhancement level.
- `--keep-plot` - Keep the original plot unchanged.
- `--add-suspense` - Specifically add suspense.
- `--focus <aspect>` - Focus on enhancing a certain aspect.

## Notes

- The enhancement will not change the core plot.
- It will retain the key information of the original text.
- It may moderately expand the length.
- It is recommended to first use `/wangyu-analyze` to analyze.

## Related Commands

- `/wangyu-style` - Activate the Wang Yu style.
- `/wangyu-write` - Create directly.
- `/wangyu-analyze` - Style analysis.
