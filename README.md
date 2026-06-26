# Timeline AI Engine — Dragon Ball Legacy: Rewritten

## Project Overview

This is an **interactive narrative architecture tool** built entirely in HTML/CSS/JavaScript. It visualizes a branching story system for an alternate Dragon Ball Z timeline where Goku (Kakarot) never loses his Saiyan programming and becomes a ruthless conqueror.

The project demonstrates a **state-driven narrative engine** with:
- Multi-stage decision trees
- Automatic alignment derivation based on choices
- Visual connection lines showing narrative trajectories
- AI-powered lore chatbot integration

---

## Key Features

### 1. **Story Branching System**
The narrative unfolds across **7 interconnected stages**:

| Stage | Name | Purpose |
|-------|------|---------|
| 1 | Prologue Origins | Select Kakarot's origin (Mountain Feral, Red Ribbon Army, etc.) |
| 2 | Prologue Node Choices | Make origin-specific decisions |
| 3 | Mid-Game Bridge | Choose narrative direction |
| 4 | Mid-Game Alignments | Auto-calculated alignment based on choices |
| 5 | Late-Game Crisis | Respond to Raditz's arrival |
| 6 | Endgame Closures | Reach one of three endings |
| 7 | Cosmic Aftermath | View the galaxy-scale consequences |

### 2. **Three Narrative Paths**

Each choice contributes to one of three **alignment paths**:

- **Saiyan Duty** (High Ki) — Strategic, mission-focused, loyal to hierarchy
- **Feral Bloodlust** (Max Malice) — Primal, violent, instinct-driven
- **Calculated Tyranny** (High Infamy) — Control-focused, long-game strategist

### 3. **Dynamic State Engine**

The engine automatically:
1. **Locks completed stages** to prevent backtracking
2. **Derives alignments** from bridge choices
3. **Auto-selects endings** based on crisis responses
4. **Draws SVG connection lines** linking your committed choices
5. **Tracks stats** (Ki, Malice, Infamy) through visual styling

### 4. **Progress Rail**

A visual timeline at the top shows:
- Completed stages (✓ checkmarks)
- Current stage (orange dot with glow)
- Future stages (numbered)
- Clickable history to restart from earlier steps

### 5. **Lore AI Chatbot**

A fixed sidebar widget powered by **Groq API** that:
- Answers questions about the timeline
- Maintains conversation context
- Discusses Kakarot's origins and Earth's fate
- Provides lore-accurate responses

---

## Technical Architecture

### **HTML Structure**
- **Navigation bar** — Links to related projects (Capsule Corp Coder Z team)
- **Header** — Project title and description
- **Info banner** — Instructions for users
- **Controls panel** — Progress rail, hints, reset button
- **Workspace** — 7 scrollable columns with story nodes and choices

### **CSS System**

#### Color Scheme
```css
--bg-dark: #0a0b10              /* Main background */
--accent-orange: #ff5500        /* Saiyan color */
--accent-blue: #00d2ff          /* UI accent */
--accent-purple: #b800ff        /* Tyrant color */
--accent-green: #00ff66         /* Feral color */
--path-saiyan: #ffaa00
--path-feral: #ff3333
--path-tyrant: #aa33ff
```

#### Key Classes

| Class | Purpose |
|-------|---------|
| `.node-card` | Story milestone (origin, ending, aftermath) |
| `.choice-item` | Interactive decision point |
| `.connection-line` | SVG path linking committed choices |
| `.locked` | Disabled/grayed out element |
| `.selectable` | Clickable, breathing animation |
| `.committed` | Selected choice with "✓ Locked in" tag |
| `.stage-active` | Highlights current column |

### **JavaScript Engine**

#### State Object
```javascript
state = {
    stage: 0,           // Current decision point (0–6)
    origin: null,       // Selected origin (1–4)
    prologue: null,     // Prologue choice ID
    bridge: null,       // Bridge choice ID
    alignment: null,    // Auto-derived ('saiyan'|'feral'|'tyrant')
    crisis: null,       // Crisis choice ID
    ending: null,       // Ending ID (1–3)
    aftermath: null     // Aftermath ID (1–3)
}
```

#### Core Functions

| Function | Role |
|----------|------|
| `render()` | Applies state to DOM; called after every change |
| `choose(el)` | Handles choice clicks; advances stage and derives alignment |
| `gotoStage(target)` | Resets from history; lets users backtrack |
| `drawLines()` | Re-renders SVG connection lines |
| `renderRail()` | Updates progress rail UI |
| `renderSummary()` | Shows committed trajectory at end |

#### Selection Flow
```
Origin → Prologue → Bridge (auto-derive Alignment) 
      → Crisis (auto-derive Ending & Aftermath) → Complete
```

### **SVG Connection Lines**

The engine draws **Bézier curves** connecting selected choices:
```javascript
arc(x1, y1, x2, y2, colorClass)
// Creates curved path with control points
// Color class determines Saiyan/Feral/Tyrant styling
```

---

## UI Components

### **Navigation Bar**
- Capsule Corp branding (blue background, orange accent)
- Links to team projects and main site
- Sticky positioning (commented out but available)

### **Column Structure**
Six scrollable columns display story content:
1. **Prologue Origins** — 4 cards with origin narratives
2. **Prologue Choices** — 4 choice groups (2 options each)
3. **Mid-Game Bridge** — 4 choice groups (origin-specific)
4. **Alignment Cards** — 3 auto-revealed paths
5. **Crisis Choices** — 3 choice groups (path-specific)
6. **Endings** — 3 closure cards
7. **Aftermath** — 3 cascading consequences

### **Choice Item Styling**
- **to-saiyan** — Yellow/orange borders (Saiyan path)
- **to-feral** — Red borders (Feral path)
- **to-tyrant** — Purple borders (Tyrant path)
- Hover effects; animation on unlocked items

---

## AI Chatbot Integration

### **Groq API Integration**
```javascript
const API_KEY = "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";
const SYSTEM_PROMPT = "You are an AI Scouter analyzing an alternate Dragon Ball timeline...";
```

### **Chatbot Features**
- **Persistent history** — Maintains conversation context
- **Status indicator** — Shows model loading state
- **Message threading** — Alternates bot/user messages
- **Fixed sidebar** — Always visible during navigation

---

## Interaction Design

### **User Flow**

1. **Select Origin** (Stage 0)
   - All 4 origin cards unlock
   - Click one to commit

2. **Choose Prologue Response** (Stage 1)
   - Choices for selected origin unlock
   - Other origin groups lock
   - Pick one choice

3. **Select Bridge Path** (Stage 2)
   - Bridge choices for your origin unlock
   - **Auto-derives alignment** based on choice type
   - Jumps to Stage 4 (skips Stage 3)

4. **Resolve Crisis** (Stage 4)
   - Only crisis choices matching your alignment unlock
   - **Auto-derives ending** (1=Saiyan, 2=Feral, 3=Tyrant)
   - **Auto-derives aftermath** based on ending
   - Jumps to Stage 6 (skips Stage 5)

5. **View Aftermath** (Stage 6)
   - Aftermath card reveals with breathing animation
   - Committed trajectory summary displays
   - SVG lines connect all chosen nodes
   - User can click rail to restart from any stage

### **State Transitions**

```javascript
switch (state.stage) {
    case 0: origin selection
    case 1: prologue choice
    case 2: bridge choice → auto-derive alignment
    case 3: SKIPPED (auto-stage)
    case 4: crisis choice → auto-derive ending/aftermath
    case 5: SKIPPED (auto-stage)
    case 6: aftermath display
}
```

---

## Visual Feedback

### **Animations**
- **Breathe** (2.4s loop) — Selectable items pulse with cyan glow
- **LineIn** (0.4s) — Connection lines fade in when committed
- **Hover effects** — Cards lift and brighten on mouseover

### **Color Coding**
- **Cyan (--accent-blue)** — Completed stages, info, UI
- **Orange (--accent-orange)** — Current stage, primary CTA
- **Green (--accent-green)** — Final closure
- **Purple (--accent-purple)** — Cosmic aftermath

---

## Browser Compatibility

- **Modern ES6 JavaScript** (uses arrow functions, template literals)
- **SVG support** for connection lines
- **CSS Grid/Flexbox** for layout
- **Responsive design** — Media query for mobile (max-width: 768px)

---

## Customization

### **Add New Choices**
1. Add `<div class="choice-item to-[path]" id="c[group]-[letter]">` to column
2. Update `allPrologue` or `allBridge` selectors if needed
3. Implement case in `choose()` function

### **Modify Alignments**
1. Update `PATHS` array
2. Add alignment cards to "Mid-Game Alignments" column
3. Add matching crisis groups to "Late-Game Crises"

### **Customize AI Prompt**
```javascript
const SYSTEM_PROMPT = "Your custom lore instructions here...";
```

---

## Notable Code Patterns

### **Element Caching**
```javascript
const byId = id => document.getElementById(id);
const origC = n => byId('origin-' + n);
```

### **Declarative State**
All UI updates flow from the `state` object—changes render atomically.

### **SVG Coordinate Mapping**
```javascript
function cx(el, wr) {
    const r = el.getBoundingClientRect();
    return { left: r.left - wr.left, right: r.right - wr.left, mid: ... };
}
```
Converts DOM element positions to SVG canvas coordinates.

---

## File Size & Performance

- **HTML+CSS+JS**: ~1000 lines (single file)
- **No external libraries** — Vanilla JS, no jQuery/React/Vue
- **Minimal DOM manipulation** — Uses class toggling and event delegation
- **requestAnimationFrame** for line drawing — Smooth 60fps updates

---

## Future Enhancements

- [ ] Persistent state (localStorage)
- [ ] Export trajectory as image/PDF
- [ ] Branching tree diagram generator
- [ ] Character stat tracker and inventory
- [ ] Multi-ending comparison view
- [ ] Keyboard navigation (arrow keys)
- [ ] Mobile touch swipe through stages

---

## Credits

**Project**: Dragon Ball Legacy: Rewritten — Lead Design Map  
**Team**: Capsule Corp Coder Z  
**Technology**: HTML5, CSS3, Vanilla JavaScript, Groq AI API  
**Inspiration**: Dragon Ball Z alternate timeline narrative design

---

## License

This project is part of the **Capsule Corp Coder Z** educational initiative. Usage subject to team guidelines.

---

**Last Updated**: 2026  
**Repository**: [Kidkudos77/timeline_ai_engine](https://github.com/Kidkudos77/timeline_ai_engine)
