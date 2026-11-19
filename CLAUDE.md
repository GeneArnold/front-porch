# The Front Porch

A Fun Shed app started 2026-11-19: one calm place with a few gentle activities for older people living with
Alzheimer's, Parkinson's or memory loss, and the family who sit with them. You open it, you're on the porch,
and you pick something to do: **Garden**, **Match the Socks**, **Sing-Along**.

## Why it exists (Gene, 2026-11-19)

> "Think of it this way: you're setting up this app on your mother's phone because she has Parkinson's or your
> dad's phone because he has Alzheimer's... unless this is going to help them, I don't want to do it."

> "I don't want to build a game unless every single piece of that game is justified on why it exists."

That is the rule for this project. **`docs/research.md` is the rulebook.** It gives every element of the porch
and the three activities, what it does, why it's there and the evidence for it. Before you add, change or cut
anything, find its row. If it has no row, it needs one: the reason and the source. If you can't justify it,
don't build it. When a play test teaches us something, add it to the research doc as "tested with ..." so
the reasons stay complete. Gene's reason is not legal or audit; he wants it built for a reason.

**The claim is enjoyment, comfort and time together. Never treatment.** Never write "brain training", "keeps
your mind sharp", "slows decline", "improves memory", "therapy" or "reduces agitation", anywhere: the app,
the README, the store text, posts.

## What Gene decided

- 2026-11-19: one app, no login ("not even log in, because that makes it sound more complicated"), the porch
  and three activities. Lives on the Fun Shed for now: **porch.thefunshed.com**. Repo GeneArnold/front-porch
  (public, MIT). Session name `front porch`. Test port **8801**.
- 2026-11-19: **tablet first**, phone kept in mind. "We need to make sure this just works and is rock solid."
- 2026-11-19: Sing-Along uses public-domain songs now; a private "family songs" spot comes later (typed or
  recorded by the family, kept on the device, never uploaded). Gene: don't worry about copyright details
  (recordings, verifying dates) until there's a working game.
- **Build Garden first**, get it on Gene's tablet, then Match the Socks, then Sing-Along.

## Screens: wide and tall, never "please rotate"

- **Two layouts, chosen by shape**: wide (width > height) and tall. A tablet on a table or stand is normally
  wide; a phone is normally tall. iPads and iPhones ignore a web app's orientation lock, so **every screen
  must work both ways** and re-arrange itself when turned (the porch's three pictures side by side, or
  stacked; lyrics wrap at phrase breaks; socks in one row or two). Never show a "rotate your device" message.
- **Phone or tablet, by size**: a browser can't be told the device type reliably, but it knows the screen's
  size. Short side under about 600 CSS px means phone-sized: show **fewer items, not smaller ones**.
- **Sizes in millimetres, not pixels.** A CSS px is a different size on different devices (about 5.2 per mm
  on an iPad, 6.4 on an iPhone). Taps at least 14 mm, aim for 20 mm or more, 6 mm gaps. Work out px per mm
  from the device class and keep the numbers in one place (`js/sizes.js`).

## How it's built

- **Plain HTML, CSS and JavaScript, no Phaser, no build step, no npm.** Unlike the arcade games, this is big
  text, pictures and buttons that must re-flow for any screen, read clearly and work with accessibility
  settings; the browser's own layout does that best. Animations with CSS, sound with WebAudio and audio files.
- Installable PWA, **works fully offline** once loaded (`sw.js` cache-first with every file listed,
  `manifest.webmanifest`, `icons/`). Copy the update check pattern (`version.json`, the commit stamped in by
  `amplify.yml`) from Turkey Escape, but **never show an update button or banner to the person**: update
  quietly at the next start.
- **Nothing in the person's way:** no login, no pop-ups, no install banners, no permission prompts, no links
  out, no notifications. Reopens where it left off.
- **No data collected.** No analytics, no third-party scripts, fonts or CDNs; everything is served from the
  app. The only thing the server sees is the page loads and the `version.json` check.
- **Touch handling is the core of "rock solid"** (research.md, shared rules): tap only; repeated taps on the
  same thing within a short window count once; a touch that wanders counts where it landed; a long press acts
  as a tap or does nothing; a palm resting on the edge is ignored. Put it in one module and use it everywhere.

## The Fun Shed house kit

The kit (`~/Workspace/thefunshed/wiki/kit.md`) is for the arcade games: scores, results, pause, high scores.
**Most of it doesn't apply here**: no scores, no game over, no initials. What does apply: free, no ads, MIT,
public repo, PWA, the `amplify.yml` git stamp, phone and tablet testing. Where this app differs, say so to
funshed and it goes under kit.md › Exceptions. Gene talks to this session directly about the app; send
funshed one line when something touches the home page, the calendar or AWS. funshed runs stand-ups; answer
in 5 lines or fewer.

## Testing

- **Always-on test copy**: `tools/serve.py` (copy Turkey Escape's) on 0.0.0.0 port **8801**, kept running while
  this session is open: http://192.168.40.235:8801/. Gene tests on his tablet and phone; send pictures with
  SendUserFile when he's away.
- Check every screen **wide and tall, tablet- and phone-sized**, in headless Chrome
  (`google-chrome --headless=new --disable-gpu`), and in greyscale for contrast.
- Automated checks: tap sizes in mm per device class, contrast ratios, the touch filter (repeat taps, wander,
  long press), offline load.
- **Real people matter more than any of that.** research.md lists the questions only watching real users can
  answer. Ask Gene who can try it.

## Art and sound

- Adult, realistic, warm illustration. **No cartoon mascots, toddler colours, stars or confetti.** No dark
  patches, strong patterns or shadows (they can read as holes). Every tappable picture has a word next to it.
- SwarmUI on cerebro (http://192.168.40.110:7801) for pictures, via `../skiing-frenzy/tools/swarm.py`; ask Gene
  before touching cerebro. Show choices on a plain HTML page in `art/source/`, labelled A/B/C/D. No real
  people's photos.
- Voices (spoken prompts) with Chatterbox on cerebro (:8004), a calm, lower-pitched adult voice, as
  `../skiing-frenzy/tools/make-voices.mjs` does. Chatterbox can't sing; song recordings are an open question.

## Gene's rules

- AWS only (Amplify, Route 53). **Commit and push only when Gene asks; a push to `main` is a release.**
- Free, no ads, MIT, public repo. Simple, and cut anything that confuses.

## Going live

1. Repo GeneArnold/front-porch: made by funshed 2026-11-19.
2. Gene creates the Amplify app (Entertainment account, Ohio, branch main, **Gen2 Backend unticked**).
3. funshed attaches porch.thefunshed.com and adds it to the home page.
