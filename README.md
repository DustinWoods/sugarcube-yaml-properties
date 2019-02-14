# Sugarcube 2 (Twine) YAML Properties

## Background
Building Twine stories, I found myself needing to set state variables based on the passage name. I didn't want to organize these state variables at the beginning of each passage. My solution was this plugin that sets state variables using YAML format in passages.

## Example
Let's say you want each passage that starts with "Chapter 1" to set the variable `$passageProperties.chapterName` to "The Beginning". Here's how this would look:

**Twine Passages:**
```
::Chapter 1
!<<=$passageProperties.chapterName>>
Once upon a Twine... [[next->Chapter 1-2]]

::Chapter 1-2
!<<=$passageProperties.chapterName>>
The story continued... [[next->Chapter 2]]

::Chapter 2
!<<=$passageProperties.chapterName>>
Now we're on Chapter 2 (and the variable says so too)
```

**Proeprtie Passages (YAML):**
```
::properties: Chapter 1
chapterName: The Beginning

::properties: Chapter 2
chapterName: The Plot Thickens
```

**What's Happening**
For every passage name that begins with "Chapter 1", the property passageProperties.chapterName will be set to "The Beginning", and for every passage that begins with "Chapter 2", the property passageProperties.chapterName will be set to "The Plot Thickens".

## Other Examples
You can even use arrays:
```
::properties: Dungeon 1
possibleEnemies:
 - Ghost
 - Dragon
 - Goblin

::Dungeon 1-east
<<set _enemy to either($passageProperties.possibleEnemies)>>
Watch out! A <<=_enemy>> appeared!
```

## Events
This plugin fires and sets variables on `:passageinit`. When it's finished, it triggers `:passageinitproperties` with `passageProperties` passed as the first argument. This can be used for extending behavior.

## Installing and Loading
If using npm with Tweego/Twee, you can simply install with `npm i -S sugarcube-yaml-properties`.
For Twine Desktop/Web... I'm not sure yet best approach for installing.

After installed, it needs to be loaded somewhere in your global js:
```
  import yamlProperties from 'sugarcube-yaml-properties';

  yamlProperties({ State, Story, $document: $(document) });
```

## Further Improvements
**Passage Name Matching**
I don't entirely like the chapter-name matching. I want to either make this more flexible, give full regex support, or take advatage of passage tags.

**State Object**
I don't like the state variables be stored in passageProperties. I would instead like this to be flatter. The reason it's not is because the state object passageProperties is empty by default. This intended behavior ensures that variables from prior passages don't carry over. If we want the same behavior with a flat structure, we need to avoid wiping other state variables.

**Loading**
There may be a better standard for Twine/SugarCube modules.