# Valentine's Coupon Booklet — Claude Code Handoff

## What this is
A mobile-friendly Valentine's Day coupon booklet web app. The gift-giver (Erin) gives it to her fiancé, and he can tap to redeem coupons for things like "watch a movie together" or "listen to a podcast." Some coupons have multiple uses (tracked with pip dots), and all redemptions are saved to localStorage so progress persists.

## Your first task
Scaffold a new React app and convert this project into it:

```bash
npm create vite@latest valentine-coupons -- --template react
cd valentine-coupons
npm install
```

Then recreate the existing design as a proper React app with clean component structure.

---

## Current design (already built as a single HTML file)

The existing HTML prototype has all the visual design finished. Your job is to faithfully port it to React, then we'll iterate from there.

### Fonts (load in index.html or via @import)
```
https://fonts.googleapis.com/css2?family=Fredoka+One&family=Nunito:wght@400;600;700;800&display=swap
```

### Color palette
```css
--pink: #FF6B9D
--coral: #FF8C69
--yellow: #FFE566
--mint: #7FDBCA
--purple: #C084FC
--bg: #FFF5F8
--dark: #2D2D2D
```

---

## Coupon data

```js
export const COUPONS = [
  { id: 'book',      emoji: '📖', color: 'c1', title: 'Read a Book',          desc: "You pick it, I read it. Even if it's 600 pages of fantasy.",         total: 1 },
  { id: 'album',     emoji: '🎵', color: 'c2', title: 'Listen to an Album',   desc: "Give me your worst.",                                                 total: 1 },
  { id: 'show',      emoji: '📺', color: 'c3', title: 'Watch a Full Series',  desc: "An entire show. Start to finish. No 'can we watch something else?'",  total: 1 },
  { id: 'movie',     emoji: '🎬', color: 'c4', title: 'Watch a Movie',        desc: "Your pick, my full attention. Three whole movies!",                   total: 3 },
  { id: 'videogame', emoji: '🎮', color: 'c5', title: 'Play a Video Game',    desc: "Even Elden Ring.",                                                    total: 1 },
  { id: 'audiobook', emoji: '🎧', color: 'c6', title: 'Audiobook Together',   desc: "We listen together. Yes, even Dungeon Crawler Carl.",                 total: 1 },
  { id: 'rpg',       emoji: '🎲', color: 'c7', title: 'DM a One-Shot RPG',    desc: "I'll DM a whole adventure for you + friends.",                        total: 1 },
  { id: 'boardgame', emoji: '🃏', color: 'c8', title: 'Play a Board Game',    desc: "This one's hard, but I won't even say \"this should be a video game\".", total: 1 },
  { id: 'podcast',   emoji: '🎙️', color: 'c9', title: 'Listen to a Podcast', desc: "Three episodes of Dwarkesh or other AI dorks.",                       total: 3 },
];
```

### Coupon color themes (9 themes, one per coupon)
Each coupon has a gradient background and an accent color for labels/tear line:

| ID  | Background gradient                        | Accent color |
|-----|--------------------------------------------|--------------|
| c1  | `#FFD6E7 → #FFADD2`                        | `#C2185B`    |
| c2  | `#E8F5E9 → #A5D6A7`                        | `#2E7D32`    |
| c3  | `#E3F2FD → #90CAF9`                        | `#1565C0`    |
| c4  | `#FFF9C4 → #FFF176`                        | `#F57F17`    |
| c5  | `#F3E5F5 → #CE93D8`                        | `#6A1B9A`    |
| c6  | `#E0F7FA → #80DEEA`                        | `#00695C`    |
| c7  | `#FBE9E7 → #FFAB91`                        | `#BF360C`    |
| c8  | `#F9FBE7 → #DCE775`                        | `#558B2F`    |
| c9  | `#FCE4EC → #F48FB1`                        | `#880E4F`    |

---

## Component structure to build

```
src/
  App.jsx               # Root, holds state, renders Cover + Booklet + Modal
  data/coupons.js       # The COUPONS array above
  hooks/useRedemptions.js  # localStorage read/write logic
  components/
    Cover.jsx           # Full-screen hero/cover section
    Booklet.jsx         # Scrollable list of coupons
    Coupon.jsx          # Individual coupon card
    RedeemModal.jsx     # Confirmation modal
    Confetti.jsx        # Confetti burst effect on redeem
```

---

## Feature specs

### State / persistence
- Use `localStorage` with key `valentine-coupons-2026`
- Shape: `{ [couponId]: number }` — tracks how many times each has been redeemed
- A custom hook `useRedemptions()` should expose: `{ usedCount, redeem, reset }`

### Cover section
- Full viewport height
- Background: `linear-gradient(135deg, #FF6B9D 0%, #FF8C69 50%, #FFE566 100%)`
- Animated scrolling emoji ticker across the top (CSS animation)
- Pulsing 💌 emoji (CSS keyframe)
- Title: "Coupon Booklet" in Fredoka One
- Subtitle: *"Each coupon is a promise to do something you love, even if it makes me mad."*
- Bouncing "Open the booklet ↓" button that smooth-scrolls to the booklet section

### Coupon card
- Rounded card (border-radius: 22px) with drop shadow
- Top section: big emoji + title + description + pip dots
- **Pip dots**: one circle per `total` use; filled/checked when redeemed
- Dashed tear line with ✂️ in the middle (decorative)
- Footer: status label on left, "Redeem 🎉" button on right
- When fully used: dim the card + show "ALL USED ✓" stamp overlay (rotated ~-11deg)
- Disabled redeem button when all uses are exhausted

### Redeem modal
- Triggered by tapping "Redeem 🎉"
- Shows the coupon's emoji, title, and a message:
  - If uses remain > 1: *"You'll use 1 of your X remaining uses. Ready?"*
  - If last use: *"This is your last use — make it count! 🎉"*
- Two buttons: "Not yet" (cancel) and "Yes, redeem! 🎉" (confirm)
- Backdrop blur overlay, pop-in animation on the modal card
- Close on backdrop click

### Confetti
- On successful redeem, burst 40–50 colored pieces from random x positions
- Colors: `['#FF6B9D','#FFE566','#7FDBCA','#FF8C69','#C084FC','#90CAF9']`
- CSS animation: fall from top to bottom + rotate, then remove from DOM
- Can use a library like `canvas-confetti` or implement with DOM elements

---

## Things to improve / iterate on next
These are ideas Erin wants to explore once the React port is done:

- [ ] Add a "who is this from" personalization screen on first open (enter your name)
- [ ] Redeem history log — show timestamps of when each coupon was used
- [ ] Ability to add a note when redeeming ("used for Interstellar, Feb 14")
- [ ] Shareable link / deploy to Vercel or Netlify so it's accessible from any device
- [ ] Possible: password-protect so only he can redeem (not just view)

---

## Notes
- Keep it mobile-first — this will primarily be used on a phone
- The design is intentionally fun and playful (not serious/minimal)
- Fredoka One is the display font for all titles and buttons
- Nunito is the body font
- The current HTML prototype is saved as `coupon-booklet.html` in this folder if you want to reference the exact CSS
