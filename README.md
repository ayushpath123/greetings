# A Little Magic — Birthday Surprise

An interactive birthday love letter built with Next.js (App Router), TypeScript,
Tailwind CSS v4, Framer Motion and Lucide icons.

## The experience

1. **Opening.** A sealed envelope. Opening it starts the music (browsers only allow audio after a click).
2. **Birthday reveal.** Glowing title, fireworks, rose petals, and a cake whose candles can be blown out one by one or all at once.
3. **Four experiences**, visited in any order:
   - **A. A Letter From My Heart.** A handwritten letter that writes itself line by line.
   - **B. Our Little Universe.** A constellation of memories; each star opens a photo card.
   - **C. A Garden of Wishes.** Flowers that bloom to reveal birthday wishes.
   - **D. Just You and Me.** A moonlit scene that ends with stars forming a heart, plus a replay button.
4. **Final surprise.** Unlocks once all four have been visited.

## Running it

```bash
npm install
npm run dev        # http://localhost:3000
npm run build      # production build
npm start          # serve the production build
npm run lint
```

## Making it yours

All the personal content lives in [`content/`](content):

| What                      | File                                         |
| ------------------------- | -------------------------------------------- |
| The love letter           | `content/letter.ts`                          |
| Memories (stars + photos) | `content/memories.ts`                        |
| Garden wishes             | `content/wishes.ts`                          |
| Her name, headlines & final message | `content/messages.ts` (`HER_NAME`)  |

**Photos:** put images in `public/memories/` and set `image: "/memories/your-photo.jpg"`
on an entry in `content/memories.ts`. Entries with `image: null` show a placeholder card.
You can also move a star by changing its `x` / `y` (percent of the sky).

**Music:** see [`public/audio/README.md`](public/audio/README.md). Put four royalty-free MP3s
in `public/audio/` with the listed names. Any mood without a file uses a gentle,
generated Web Audio arrangement, so the site always has sound and never loads music
from an external URL.

## How it's put together

```
app/                    layout (fonts, metadata), page, global styles & keyframes
components/
  experience/           scene state machine + experience metadata
  scenes/               one component per scene
  effects/              particles, starfield, canvas fireworks, glowing heart
  music/                MusicProvider (context) + floating MusicPlayer
  ui/                   buttons, cake, animated titles
content/                all editable text
lib/music/              track config, audio engine, generative synth fallback
lib/random.ts           seeded PRNG (keeps decorative particles hydration-safe)
```

- **Audio.** One `AudioContext` is created on the first click. When switching tracks, the old one fully fades out before the new one fades in, so two tracks never overlap. Volume and mute changes ramp smoothly. Each scene suggests a mood until the listener picks a track, and after that their choice is respected.
- **Reduced motion.** `MotionConfig reducedMotion="user"` covers Framer Motion. CSS particles are hidden, canvas fireworks are skipped, and timed sequences (the letter, the moonlit scene, the finale) show their content right away.
- **Performance.** Particles animate only `transform` and `opacity`. Fireworks run on a single DPR-capped canvas. There are no runtime network calls and no paid APIs.
