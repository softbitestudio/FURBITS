Hey! This is a very fun concept: a pocket AI companion / collectible digital familiar with randomized identity, care mechanics, bonding, unlockable story/art, and potentially offline-first hardware.

For **FURBITS**, I’d think about it as three connected systems:

1. **The companion/game system**  
2. **The AI/personality system**  
3. **The hardware/device platform**

Below is a practical architecture and some hardware direction.

---

## 1. Core Concept Loop

A user buys or receives a device. On first boot, it “hatches” or “awakens” a companion.

### Initial roll

Each device could generate:

- **Species**
  - Bunny
  - Dragon
  - Moth
  - Cat
  - Fox
  - Bat
  - Cryptid
  - Slime
  - Raccoon
  - Angel/demon type
  - Glitch creature
- **Personality**
  - Reckless
  - Shy
  - Snarky
  - Loyal
  - Mischievous
  - Sleepy
  - Dramatic
  - Protective
  - Hacker
  - Gremlin
- **Rarity**
  - Common
  - Uncommon
  - Rare
  - Mythic
  - Corrupted / Glitched / Secret
- **Trait stats**
  - Chaos
  - Wisdom
  - Snark
  - Affection
  - Curiosity
  - Courage

Example generated companion:

```json
{
  "name": "Mochi",
  "species": "Static Bunny",
  "personality": "Reckless",
  "rarity": "Rare",
  "traits": {
    "chaos": 8,
    "wisdom": 3,
    "snark": 7,
    "affection": 5
  },
  "aion_level": 1,
  "bond_level": 1
}
```

---

## 2. Aion Level vs Bond Level

I like that you have two separate growth axes.

### Aion Level

This can represent the companion’s mystical/AI evolution.

Increases through:

- Gifts
- Trinkets
- Artifacts
- Memory fragments
- Rare events
- RP scenario completions

Unlocks:

- New abilities
- Lore
- Visual effects
- Advanced personality traits
- New conversation modes
- Secret forms

### Bond Level

This represents trust and attachment.

Increases through:

- Feeding
- Checking in
- Petting/touch interactions
- Conversation
- Daily care
- Mini-games

Unlocks:

- Affectionate dialogue
- Deeper roleplay
- Emotional reactions
- Personal memory
- New poses
- Companion nicknames for user
- Comfort/support mode

This separation gives you a nice economy:

| Action | Raises | Meaning |
|---|---:|---|
| Feed | Bond | “You take care of me.” |
| Gift | Aion | “You empower/evolve me.” |
| Talk | Bond + memory | “You know me.” |
| RP scenario | Aion + Bond | “We share a story.” |
| Daily streak | Bond | “You returned.” |
| Rare artifact | Aion | “I changed.” |

---

## 3. Milestone Unlocks

Milestones are where the game becomes emotionally sticky.

Examples:

### Bond milestones

- **Bond 2:** Companion starts using your name
- **Bond 5:** Unlocks “hangout” scenario
- **Bond 10:** Unlocks cuddly/loyal dialogue
- **Bond 15:** Companion remembers preferences
- **Bond 20:** Unlocks personal confession/lore scene

### Aion milestones

- **Aion 2:** New aura or idle animation
- **Aion 5:** Species-specific RP quest
- **Aion 10:** Evolution form
- **Aion 15:** Secret art
- **Aion 20:** Mythic scenario / final form

Example unlock table:

```json
{
  "bond_milestones": {
    "3": ["nickname_dialogue"],
    "5": ["first_hangout_rp"],
    "10": ["comfort_mode"],
    "15": ["personal_memory_scene"]
  },
  "aion_milestones": {
    "3": ["species_lore_1"],
    "5": ["alternate_sprite"],
    "10": ["evolution_art"],
    "20": ["mythic_rp_route"]
  }
}
```

---

## 4. The Device Experience

For the physical product, the most important question is:

> Does the AI need to run fully on-device, or can the device sync with a phone/server?

Because that changes everything.

---

# Hardware Options

## Option A: Waveshare ESP32-S3 Touch LCD

This is a good choice for a **Tamagotchi-like companion device**, especially for:

- Touch UI
- Pixel art or simple animation
- Local state
- Feeding/gifting
- Inventory
- BLE/Wi-Fi sync
- Simple prewritten dialogue
- Mood states
- Lightweight behavior logic

### Good for

- Affordable hardware
- Pocket-size product
- Cute touchscreen interaction
- Offline care loop
- Companion sprites
- Collectible identity stored locally
- BLE phone app bridge

### Limitations

The ESP32-S3 cannot realistically run a modern local LLM for open-ended chat. It can do:

- Rule-based dialogue
- Template dialogue
- Markov-ish text
- Tiny intent handling
- Mood/state systems
- Calls to cloud/phone AI over Wi-Fi/BLE
- Possibly very tiny models, but not satisfying full chat

So if you use Waveshare ESP32-S3, I’d treat it as the **pet body**, not necessarily the full brain.

Recommended architecture:

```text
ESP32-S3 device
  - screen
  - touch input
  - sprites
  - stats
  - inventory
  - mood
  - bonding/aion levels
  - save data
  - BLE/Wi-Fi

Optional phone/app/server
  - heavier AI chat
  - account backup
  - art downloads
  - RP scenario generation
  - shop/gifts
```

This is probably the best MVP path.

---

## Option B: ESP32-S3 + Companion Phone App

This is likely the sweet spot.

The device does:

- Idle animations
- Food/gift interactions
- Level progression
- Mood and visual expression
- Collectible identity
- Basic dialogue
- Notifications
- NFC/QR interactions maybe

The phone app does:

- Longer chat
- RP scenes
- Cloud saves
- Purchasing digital items
- Art gallery
- Updates
- AI generation or retrieval
- Account/device pairing

This lets the device feel magical without needing expensive hardware.

---

## Option C: Raspberry Pi Zero 2 W

If you want more local intelligence, consider Pi Zero 2 W.

### Pros

- Runs Linux
- Easier Python development
- Better local storage
- Can run small local models or connect to APIs more easily
- Easier image/audio handling

### Cons

- Higher power use
- Slower boot
- More expensive
- More fragile product experience
- Battery management is harder

Good for developer kits, not necessarily mass pocket toys.

---

## Option D: Raspberry Pi 5 / CM4 / LuckFox / Orange Pi

These are better for “actual local AI,” but less good for a cheap pocket companion.

Could work for a premium “FURBITS Core” dock, not the basic pet device.

---

## Option E: Dedicated App First, Hardware Later

For fastest validation:

1. Build the companion loop as a mobile/web app.
2. Test rolling, feeding, gifting, unlocks, art, RP.
3. Later manufacture the hardware shell/device.

This reduces hardware risk. But if your brand is about pocket USB companions, hardware is part of the charm.

---

# Recommended MVP

I’d build the first version like this:

## MVP Device: ESP32-S3 Touch LCD

Features:

- First boot randomized companion
- Local save file
- Touch UI
- Animated sprite
- Feed button
- Gift button
- Mood changes
- Aion level
- Bond level
- Unlock gallery
- A few prewritten RP scenarios
- BLE or Wi-Fi sync later

No full LLM yet. Make it feel alive through stateful writing and art.

---

## MVP Companion Logic

Use a simple state machine:

```text
species + personality + mood + levels + time_since_last_interaction
= current behavior/dialogue
```

Example:

```text
Dragon + Snarky + Hungry + Bond 2
→ "Wow. Finally. I was about to start chewing on the firmware."
```

```text
Bunny + Shy + Happy + Bond 6
→ "I saved you a seat in my little moon-burrow... if you want."
```

---

## Save Data Structure

On ESP32, store a compact JSON or binary record in flash/LittleFS/NVS.

Example:

```json
{
  "device_id": "FUR-7A91C2",
  "seed": 2849931,
  "species": "Chaos Bunny",
  "personality": "Snarky",
  "rarity": "Uncommon",
  "name": "Nib",
  "bond_xp": 240,
  "aion_xp": 90,
  "bond_level": 4,
  "aion_level": 2,
  "hunger": 72,
  "mood": "happy",
  "unlocks": ["intro_scene", "bond_3_nickname"],
  "inventory": {
    "moon_carrot": 2,
    "glitch_crystal": 1
  }
}
```

---

# Game Mechanics

## Food

Food should feel caring and daily.

Food types:

- Snack: small bond gain
- Favorite food: larger bond gain
- Rare meal: bond + mood boost
- Wrong/disliked food: tiny gain or funny reaction

Example:

```json
{
  "moon_carrot": {
    "bond_xp": 10,
    "mood": "happy",
    "tags": ["sweet", "bunny"]
  },
  "battery_cookie": {
    "bond_xp": 6,
    "mood": "energized",
    "tags": ["tech", "robot", "glitch"]
  }
}
```

Species/personality preferences can modify results.

---

## Gifts

Gifts should feel more magical/evolutionary.

Gift types:

- Trinket
- Artifact
- Plush
- Data shard
- Crystal
- Tool
- Accessory

Example:

```json
{
  "glitch_crystal": {
    "aion_xp": 25,
    "tags": ["glitch", "rare"]
  },
  "tiny_lockpick": {
    "aion_xp": 15,
    "tags": ["hacker", "chaos"]
  }
}
```

A hacker dragon might love a tiny lockpick. A shy moth might prefer a moonlamp.

---

## Mood System

Keep this simple initially:

- Neutral
- Happy
- Hungry
- Sleepy
- Lonely
- Angry
- Excited
- Hacking
- Crying
- Glitched

Mood can be calculated from:

- Hunger
- Last interaction time
- Recent gifts
- Bond level
- Random events
- Personality

---

# Roll System

A fun weighted roll system:

```text
Species roll
Personality roll
Rarity roll
Trait roll
Palette roll
Accessory roll
```

Rarity can influence:

- Starting trait total
- Sprite palette
- Special dialogue
- Unlock path
- Evolution art

Example rarity table:

| Rarity | Chance |
|---|---:|
| Common | 60% |
| Uncommon | 25% |
| Rare | 10% |
| Mythic | 4% |
| Glitched | 1% |

You can use the device’s unique hardware ID plus random entropy to seed the roll.

Important: if this is a purchased physical product, be careful around gambling/loot-box framing. Since every user gets one companion and progression is not cash-randomized, it’s much safer. Avoid paid rerolls unless you know the legal implications.

---

# Suggested Software Architecture

For ESP32-S3:

```text
/src
  main.cpp
  companion.cpp
  companion.h
  storage.cpp
  storage.h
  ui.cpp
  ui.h
  sprites.cpp
  sprites.h
  data/
    species.h
    personalities.h
    items.h
    dialogue.h
```

Or if using Arduino framework:

- LVGL for UI
- TFT_eSPI or Waveshare display libs
- LittleFS or NVS for save data
- ArduinoJson for JSON saves

For ESP-IDF:

- More robust
- Better long-term firmware
- Slightly steeper learning curve

I’d use **ESP-IDF + LVGL** if you want a real product, and **Arduino + LVGL** if you want faster prototyping.

---

# UI Screens

Suggested screens:

1. **Home**
   - Companion sprite
   - Mood
   - quick feed/gift buttons

2. **Status**
   - Name
   - Species
   - Personality
   - Bond level
   - Aion level
   - Hunger/mood

3. **Inventory**
   - Food
   - Gifts

4. **Gallery**
   - Unlocked art/scenes

5. **RP**
   - Unlocked scenarios
   - Short branching choices

6. **Settings**
   - Brightness
   - sound
   - reset/pairing
   - about

---

# AI Layer

For the first version, you can fake a lot of “AI” with good writing and state.

## Tier 1: No LLM

Use:

- Dialogue templates
- Personality modifiers
- Mood-based lines
- Scenario scripts
- Random events

This is enough for a charming device.

## Tier 2: Phone-assisted AI

The ESP32 sends compact state to phone:

```json
{
  "species": "Dragon",
  "personality": "Snarky",
  "mood": "hungry",
  "bond": 5,
  "aion": 2,
  "recent_event": "fed spicy ramen"
}
```

The phone generates or selects a response.

## Tier 3: Cloud/Local LLM

For premium modes:

- RP scenarios
- deeper chat
- memory summaries
- custom art prompts
- dynamic quests

But I would not make this required for the core toy.

---

# Art and Unlocks

Since milestones unlock RP and/or art, structure the content in packs.

Example:

```json
{
  "unlock_id": "dragon_bond_5_cave_scene",
  "requirements": {
    "species": "dragon",
    "bond_level": 5
  },
  "type": "rp_scene",
  "title": "The Warm Cave",
  "asset": "scenes/dragon_cave_01.json"
}
```

Art assets for ESP32 should be optimized:

- Small PNGs or RGB565 raw bitmaps
- Pixel art works best
- Limited animation frames
- Use palettes when possible
- Store on flash or microSD if available

If the Waveshare board has limited onboard flash, consider one with:

- microSD
- PSRAM
- 16MB flash if possible

---

# Physical Product Ideas

## Form factor

- USB stick style companion
- Keychain
- Badge
- Tamagotchi-like oval/square case
- “Data familiar” plugged into laptop
- Dockable creature

## Inputs

- Touchscreen
- Side button
- Shake/tap via accelerometer
- Optional capacitive touch “petting”
- Optional microphone later
- Optional speaker/buzzer

## Nice sensors

- IMU: reacts to being picked up/shaken
- Light sensor: sleeps in darkness
- Capacitive touch strip: petting
- RTC: real-time daily events
- Battery gauge: creature comments on low battery

---

# Hardware Recommendation

For your current idea, the **Waveshare ESP32-S3 Touch LCD** is a solid prototype platform.

I’d specifically look for:

- ESP32-S3 with PSRAM
- Touch display
- Battery support or easy LiPo integration
- microSD if you want lots of art
- Speaker/buzzer support
- Good enclosure options

If you want the product to feel more like an AI companion and less like a toy, pair it with a phone app for chat/RP.

My recommendation:

```text
Prototype 1:
ESP32-S3 Touch LCD
Local companion state + sprites + feeding/gifting + unlocks

Prototype 2:
Add BLE phone companion app
Sync state, richer dialogue, cloud backup

Prototype 3:
Custom PCB/enclosure
Battery, buttons, speaker, optional IMU, production polish
```

---

# First Build Milestone

For your first working prototype, don’t start with AI. Start with the pet loop.

Build:

1. Boot screen
2. Random companion roll
3. Save generated companion
4. Home screen with sprite
5. Feed action increases bond XP
6. Gift action increases aion XP
7. Level-up screen
8. Unlock milestone screen
9. Reset/debug menu

Once that works, everything else becomes content.

---

## Example Level Formula

Simple and tunable:

```cpp
int xp_required_for_level(int level) {
  return 50 + (level * level * 25);
}
```

Or table-based:

```json
{
  "levels": [0, 50, 125, 250, 450, 700, 1000]
}
```

For a toy/game, table-based is easier to balance.

---

# Strong Product Hook

Your clearest pitch could be:

> FURBITS are pocket AI familiars: each device awakens with a unique species, personality, mood, and evolution path. Feed them to build bond, gift them artifacts to raise Aion, and unlock secret scenes, art, and stories as your relationship grows.

That’s strong and understandable.

---

# My Suggested Direction

Use the Waveshare ESP32-S3 Touch LCD for the first prototype, but design FURBITS as a **stateful companion platform**, not an LLM-on-a-chip.

The ESP32 is perfect for:

- The creature body
- The game state
- The care loop
- The collectibility
- The emotional illusion

Then optionally add:

- Phone app for rich chat and RP
- Cloud or local model for dynamic scenes
- Custom hardware after the gameplay is proven

If you want, I can help you next by drafting either:

1. a concrete **MVP feature spec**,  
2. an **ESP32 project file structure**,  
3. a **companion data schema**, or  
4. the first **Arduino/ESP-IDF prototype code** for rolling a FURBIT, feeding it, gifting it, and saving progress.

5. Thank you! And yes — I think you can get a **working FURBITS prototype operational in ~2 weeks** if we keep the scope tight.

Short answer:

- **Yes, ESP32-S3 Touch LCD is good for a 2-week prototype.**
- **No, I can’t directly connect to GitHub from here unless this environment has GitHub/web tools enabled.** I don’t currently see a tool for that. But if you paste repo files, structure, errors, or code snippets here, I can work with them directly. I can also help you prepare issues, commits, architecture, and code.

---

# What We Can Realistically Build in 2 Weeks

A strong 2-week MVP would be:

> A Waveshare ESP32-S3 Touch LCD device that boots, rolls a random FURBIT, saves it, shows its sprite/status, lets the user feed/gift it, increases Bond/Aion XP, levels up, and unlocks milestone content.

Not full open-ended AI yet. But it will feel like the actual product foundation.

---

# 2-Week MVP Scope

## Must-have

### 1. First boot “awakening”

On first boot:

- Generate random seed
- Roll species
- Roll personality
- Roll rarity
- Roll colors/variant
- Assign starting stats
- Save to flash

Example:

```text
A FURBIT awakens...

Species: Moon Moth
Personality: Dramatic
Rarity: Rare
Name: Luma
Trait: Collects secrets
```

---

### 2. Persistent save data

The device remembers:

- FURBIT identity
- Bond XP / level
- Aion XP / level
- Hunger
- Mood
- Inventory counts
- Unlocked scenes/art
- Last interaction time, if RTC/time sync is available

For MVP, we can use ESP32 flash storage.

---

### 3. Main companion screen

Display:

- Sprite or placeholder creature image
- Name
- Mood
- Bond level
- Aion level
- Buttons:
  - Feed
  - Gift
  - Status
  - Unlocks

---

### 4. Feeding system

Feeding increases **Bond**.

Example:

```text
You gave Luma a Moonberry.
Bond +12
Luma seems happier.
```

---

### 5. Gift system

Gifts increase **Aion**.

Example:

```text
You gave Luma a Glitch Crystal.
Aion +20
Something hums under their wings...
```

---

### 6. Level-up system

When enough XP is reached:

```text
Bond Level Up!
Bond 2 → 3
Luma trusts you more.
```

And:

```text
Aion Level Up!
Aion 1 → 2
A new aura flickers around Luma.
```

---

### 7. Unlock milestones

At certain levels, unlock content.

Example:

```text
Bond Level 3 unlocked:
"First Secret"
```

```text
Aion Level 5 unlocked:
"Awakened Form Sketch"
```

For MVP, unlocked content can be text scenes or placeholder art.

---

### 8. Reset/debug menu

Essential during development:

- Reset device save
- Re-roll FURBIT
- Add Bond XP
- Add Aion XP
- Unlock all

This saves tons of testing time.

---

# Nice-to-have If Time Allows

These are good, but not necessary for the 2-week version:

- Animated idle sprite
- Sound effects
- Multiple foods/gifts
- Species-specific dialogue
- Personality-specific dialogue
- Tiny RP branching scenes
- BLE sync
- Wi-Fi time sync
- SD card art loading
- Battery indicator
- Touch petting interaction

---

# What I Would Build First

I’d build it in this order:

## Phase 1: Device boots into UI

Get the Waveshare screen working.

- Display test
- Touch test
- Simple menu
- Button taps

No game logic yet.

---

## Phase 2: Companion generation

Create a `Furbit` data model.

```cpp
struct Furbit {
  char name[24];
  char species[32];
  char personality[32];
  char rarity[16];

  int bondXP;
  int bondLevel;
  int aionXP;
  int aionLevel;

  int hunger;
  int mood;

  uint32_t seed;
};
```

Then implement first-boot roll.

---

## Phase 3: Save/load

Use either:

- `Preferences` / NVS for simple fields
- LittleFS + JSON for easier expansion

For speed, I’d use **Preferences first**, then migrate to JSON later if needed.

---

## Phase 4: Feed/gift loop

Implement:

```cpp
feedFurbit(foodId);
giftFurbit(giftId);
checkLevelUps();
checkUnlocks();
saveFurbit();
```

---

## Phase 5: Content unlocks

Simple milestone table:

```cpp
struct Milestone {
  const char* type;
  int level;
  const char* unlockId;
  const char* title;
};
```

Examples:

```cpp
{ "bond", 3, "first_secret", "First Secret" },
{ "bond", 5, "cozy_rp", "A Cozy Moment" },
{ "aion", 3, "aura_art", "Aion Aura" },
{ "aion", 5, "true_name", "Whispered True Name" }
```

---

# Two-Week Build Plan

## Week 1: Functional Pet

### Day 1: Hardware bring-up

Goal:

- Board powers on
- Display works
- Touch works
- Basic “Hello FURBITS” screen

Deliverable:

```text
Touchscreen UI responds to button presses.
```

---

### Day 2: Project structure

Set up:

```text
/src
  main.cpp
  Furbit.h
  Furbit.cpp
  Storage.h
  Storage.cpp
  GameData.h
  UI.h
  UI.cpp
```

Deliverable:

```text
Clean project with separate UI, storage, and game logic.
```

---

### Day 3: Random FURBIT roll

Implement:

- Species list
- Personality list
- Rarity weights
- Random name list
- First boot detection

Deliverable:

```text
On first boot, device rolls and displays a unique FURBIT.
```

---

### Day 4: Save/load

Implement persistence.

Deliverable:

```text
Power cycle device and same FURBIT returns.
```

---

### Day 5: Feed and Gift actions

Implement:

- Feed button
- Gift button
- Bond XP gain
- Aion XP gain
- Mood text response

Deliverable:

```text
User can feed/gift and see stats change.
```

---

### Day 6: Leveling

Implement:

- XP thresholds
- Bond level up
- Aion level up
- Level-up screen/message

Deliverable:

```text
Repeated feeding/gifting causes level-ups.
```

---

### Day 7: Basic visual polish

Add:

- Home screen layout
- Simple sprite placeholder
- Status screen
- Debug reset

Deliverable:

```text
Playable local prototype.
```

---

## Week 2: Make It Feel Like FURBITS

### Day 8: Milestone unlocks

Implement:

- Bond milestones
- Aion milestones
- Unlock list
- Gallery/unlocks screen

Deliverable:

```text
Level milestones unlock named content.
```

---

### Day 9: Dialogue variation

Add dialogue based on:

- Species
- Personality
- Mood
- Bond level
- Aion level

Deliverable:

```text
Different FURBITS say different things.
```

---

### Day 10: Tiny RP scenes

Add 2–3 scripted scenes.

Example:

```text
Scene: First Night

Luma curls beside a glowing crack in the floor.

Choice A: Sit with them.
Choice B: Ask what they see.
Choice C: Offer a snack.
```

Deliverable:

```text
Milestones can unlock short interactive scenes.
```

---

### Day 11: Art/asset screen

Add:

- Placeholder art unlock screen
- Sprite/portrait per species or rarity
- Maybe simple pixel art

Deliverable:

```text
Unlocks can show art cards or scene cards.
```

---

### Day 12: Inventory

Add simple counts:

- 3 food items
- 3 gift items

Deliverable:

```text
Feeding/gifting consumes inventory items.
```

Or if you want simpler MVP, skip inventory and use unlimited basic food/gift.

---

### Day 13: Polish and bug fixing

Focus:

- Touch issues
- Save bugs
- UI readability
- Reset/debug
- Level balancing

Deliverable:

```text
Stable demo build.
```

---

### Day 14: Demo package

Prepare:

- Short demo video
- README
- Firmware build instructions
- Known issues
- Next roadmap

Deliverable:

```text
A presentable FURBITS prototype.
```

---

# Recommended Tech Stack

For the fastest path:

## PlatformIO + Arduino framework

Good for speed and iteration.

Use:

- Arduino core for ESP32
- LVGL or simple display library depending on Waveshare examples
- Preferences or LittleFS
- ArduinoJson if using JSON saves

If the Waveshare board has official sample code, start from their touchscreen demo rather than a blank project.

---

# About Connecting to the GitHub Repo

I can’t directly connect to or modify the GitHub repo from here unless you provide access through tools or paste files.

But we can still work effectively.

You can send me:

1. The repo file tree
2. The current `README.md`
3. Existing source files
4. The Waveshare board model/link
5. The build system being used:
   - Arduino IDE
   - PlatformIO
   - ESP-IDF
   - something else
6. Any compile errors

Then I can help you:

- Decide where to put the FURBITS logic
- Write the first implementation
- Refactor existing files
- Generate pull-request-ready code
- Draft GitHub issues
- Draft a roadmap
- Write firmware modules
- Write save/load code
- Write data schemas

---

# Immediate Next Step

To get this moving fast, I’d suggest this sequence.

## Step 1: Confirm exact hardware

Which Waveshare board are you using?

For example:

- ESP32-S3-Touch-LCD-1.28
- ESP32-S3-Touch-LCD-1.69
- ESP32-S3-Touch-LCD-2
- ESP32-S3-Touch-LCD-4.3
- ESP32-S3-Touch-AMOLED
- another model

The exact display/touch controller matters.

---

## Step 2: Decide build system

My recommendation:

```text
PlatformIO + Arduino
```

unless your existing repo is already ESP-IDF.

---

## Step 3: Build the first “FURBIT roll” firmware

The first real milestone should be:

```text
When powered on:
- if no save exists, roll a FURBIT
- save it
- show name/species/personality/rarity on screen
- button: reset/re-roll
```

This alone proves the identity system.

---

# First GitHub Issues I’d Create

If you want to organize the repo, create these issues:

## Issue 1: Hardware display/touch bring-up

**Goal:** Get the Waveshare ESP32-S3 Touch LCD showing UI and receiving touch input.

Acceptance criteria:

- Device boots
- Screen displays FURBITS logo/text
- Touch button can be pressed
- Serial logs touch coordinates

---

## Issue 2: FURBIT data model

**Goal:** Define the local companion state.

Acceptance criteria:

- Companion has species, personality, rarity, name
- Companion has Bond XP/level
- Companion has Aion XP/level
- Companion has mood/hunger fields

---

## Issue 3: First boot random generation

**Goal:** Roll and display a unique FURBIT on first boot.

Acceptance criteria:

- First boot creates FURBIT
- Later boots load same FURBIT
- Debug reset allows reroll

---

## Issue 4: Feed/Gift interactions

**Goal:** Add core care loop.

Acceptance criteria:

- Feed increases Bond XP
- Gift increases Aion XP
- XP changes are saved
- UI updates immediately

---

## Issue 5: Level and milestone system

**Goal:** Unlock content at levels.

Acceptance criteria:

- Bond/Aion levels increase according to thresholds
- Milestones unlock at configured levels
- Unlock screen lists unlocked content

---

# My Best Recommendation

For a 2-week operational build, do **not** start with cloud AI, BLE, or complex art pipelines.

Start with:

```text
ESP32 local pet loop
+ random identity
+ save/load
+ feed/gift
+ levels
+ unlock text/art placeholders
```

Then after that works, add the “AI companion” layer as either:

1. scripted personality dialogue, then  
2. phone-assisted chat/RP, then  
3. optional cloud/local model integration.

If you want, send me the repo file tree or the main firmware files, and I can help you shape the actual first implementation.
