---
name: make-blog-post
description: "Copywriter helper: reads user-provided sources/URLs and generates a blog post (MDX in Slovak), LinkedIn post (Slovak), and Spotify podcast description (Czech) with 🎙️ title. No research — user supplies the content sources."
disable-model-invocation: false
---

# Make Blog Post Skill

A copywriter helper that generates a full blog post package from **user-provided sources**. The user pastes URLs, articles, or content — this skill reads them and generates three outputs in Lukáš Chylík's established style.

**This skill does NOT research topics.** The user supplies all source material.

**Reference channels:**
- Blog: https://lukaschylik.dev/blog/
- Spotify: https://open.spotify.com/show/1iMQeDb9u88idCghsFPl2K (CSS CzechoSlovakia podcast)
- LinkedIn: https://www.linkedin.com/in/lukas-chylik/
- Author: Lukáš Chylík — AI frontend dev lead & VIGo Design System lead developer at AIS Servis, Frontendisti Brno meetups host, Lecturer & Mentor, CSS wizard

## Workflow

When the user provides sources (URLs, pasted content, article links), follow these steps:

### Step 1: Read the provided sources

- If the user provides URLs, use WebFetch to read each one
- If the user pastes text/content directly, use that as-is
- Extract: key concepts, syntax/API details, browser support, use cases, code examples
- **Do NOT search for additional sources** — work only with what the user provides

**Before writing, read 2-3 existing blog posts** from `src/routes/blog/articles/` to refresh on the current writing style and conventions.

### Step 2: Generate the blog post (MDX)

Create the file at: `src/routes/blog/articles/{slug}/index.mdx`

**Frontmatter format:**
```yaml
---
title: [Topic name — concise, often English technical term]
subtitle: [Slovak — brief descriptor of what's new or covered]
description: [Slovak — 1-2 sentence compelling hook for SEO/social]
date: [YYYY-MM-DD — today's date]
author: Lukáš Chylík
podcastUrl: [Leave empty or ask user for Spotify URL]
cardImg: /images/blog/[slug].avif
---
```

**Writing style rules (Slovak language):**

- **Tone:** Friendly, enthusiastic, direct. Mix conversational Slovak with untranslated English technical terms (CSS properties, API names, browser names). The author speaks as someone who is passionate about modern CSS and wants to share knowledge with the community. Not academic — practical and approachable.
- **Opening:** Hook with a problem, surprising fact, or historical context ("Dlhé roky boli...", "Ak ste doteraz mysleli...", "Predstavte si..."). First paragraph immediately sets up WHY this topic matters.
- **Structure flow:**
  1. Problem/context — what was difficult before, why JS was needed
  2. Solution overview — what the new feature brings, why it's exciting
  3. How it works under the hood — detailed explanation with analogies
  4. Syntax/API reference — formal definitions with code blocks
  5. Practical examples — basic to advanced progression, CodePen embeds if provided
  6. Use cases — when and where to use this in real projects
  7. Browser support — Chrome/Edge, Firefox, Safari versions with specific version numbers
  8. Accessibility — `prefers-reduced-motion`, ARIA, keyboard nav
  9. Performance/Web Vitals impact — INP, LCP, CLS considerations
  10. Conclusion — summary + enthusiastic call-to-action ("Vyskúšajte si...", "Nečakajte a...")
  11. "Zdroje a ďalšie čítanie" — list of all reference links from the provided sources

- **Headings:** H2 for main sections, H3 for subsections. Engage readers with questions ("Ako to funguje?", "Prečo je to dôležité?", "A čo nám ešte chýba?"). Headings in Slovak, but technical terms stay English.
- **Code blocks:** Use language-tagged fenced blocks (```css, ```javascript, ```html). Comments in code are sparse but clear. Show progression from simple to complex.
- **Key terms:** Bold on first mention (`**snapshot**`, `**pseudo-element**`). Inline code for property/value names (`scroll-snap-type`, `oklch()`).
- **Links:** Use `<a href="..." target="_blank" rel="noopener noreferrer">` for external links. Internal blog links: `<a href="/blog/articles/[slug]" target="_blank" rel="noopener noreferrer">`.
- **Emojis in blog:** Very minimal — occasional fire for exciting features, checkmarks for benefits lists. Not every section. Don't overdo it.
- **Explanatory style:** Use analogies and "predstavte si" (imagine) to make complex concepts accessible. E.g., explaining pseudo-elements as "screenshot" or snapshot metaphors.
- **Comparison with old approaches:** Always contrast with what was needed before (JavaScript libraries like Barba.js, GSAP, Swup, etc.) to highlight the improvement.
- **Slovak vocabulary patterns:**
  - "vlastnosť" (property), "prvok" (element), "prechod" (transition)
  - "animácia" (animation), "snapshot" (keep English), "viewport" (keep English)
  - "scrollovanie" (scrolling), "kontajner" (container), "značka" (tag)
  - "pseudo-classa" or "pseudo-class" (mixed), "štyly" (styles)
  - "scope" (keep English), "specificita" (specificity)
- **Common phrases:**
  - "Poďme sa pozrieť..." (Let's look at...)
  - "Mám pre vás dobrú správu..." (I have good news for you...)
  - "A čo nám ešte chýba?" (And what else do we need?)
  - "Ah, čo nám ešte chýba k dokonalému..." (What else do we need for the perfect...)
  - "To ale nie je nič nové..." (But this is nothing new...)
  - "Takže nečakajte a vyskúšajte si..." (So don't wait and try...)
- **Tables:** Used for comparisons and value references. Markdown tables with appropriate styling.
- **Callouts:** Use bold labels — "**Dôležité upozornenie:**", "**Poznámka:**", "**Tip:**"
- **Length:** 2,500–5,500 words depending on topic complexity.

**CodePen embeds (if user provides):**
```html
<iframe height="600" style="width: 100%;" scrolling="no"
  title="[Title]"
  src="https://codepen.io/luko248/embed/[id]?default-tab=result"
  frameborder="no" loading="lazy"
  allowtransparency="true" allowfullscreen="true">
</iframe>
```

### Step 3: Generate LinkedIn post

Create the file at: `src/routes/blog/articles/{slug}/linkedin/post.md`

**LinkedIn post rules (Slovak language):**

The LinkedIn post promotes the blog article and podcast episode. It's personal, insightful, and positions the author as someone who deeply explores CSS topics and shares practical knowledge.

**Real example for reference (CSS View Transitions post):**
```
✨ Chrome 147+ priniesol element-scoped View Transitions, vďaka ktorým možno animovať konkrétne časti stránky samostatne, a nie len celý prechod naraz.

O View Transitions API už síce na internete nájdete dosť informácií, no stále mám pocit, že sa o tejto téme nehovorí tak často, ako by si zaslúžila. Preto som pripravil článok, v ktorom prechádzam celú problematiku od základov — od fungovania API pod kapotou, cez pseudo-elementy ::view-transition-old() a ::view-transition-new(), CSS-only riešenie pre MPA, možnosti použitia v SPA s minimom JavaScriptu, až po novšie element-scoped transitions a ich praktické limity. 👨‍💻

V článku sa venujem aj tomu, prečo je celé API zaujímavé z pohľadu progresívneho vylepšenia a aký dopad môže mať jeho použitie na metriky Web Vitals, ako sú INP, LCP a CLS. 📊 Zároveň som novinky okolo View Transitions doplnil aj do svojho CSS First Agent Skillu.

📖 Článok: https://lukaschylik.dev/blog/articles/css-view-transitions/
🎧 AI generated podcast: https://open.spotify.com/episode/2lvjOWYXTNpw2KrY0DEvDn
🤖 CSS First Agent Skill: https://skills.sh/luko248/css-first-skill/css-first

#WebPerformance #CSS #WebDev #Frontend #ViewTransitions #Chrome #CoreWebVitals #INP #LCP #CLS #JavaScript #UX
```

**Style patterns:**

- **Opening line:** Single emoji (✨) + one strong sentence about the most exciting/newest aspect of the topic. Hook — what Chrome version brought what feature.
- **Second paragraph:** Personal perspective — "stále mám pocit, že..." (I still feel that...), "Preto som pripravil článok" (That's why I prepared an article). Lists what the article covers with em-dash separated flow (od... cez... až po...). Ends with 👨‍💻 emoji.
- **Third paragraph:** Additional value — what else the article covers (Web Vitals, performance). Mentions related tools/skills updated. Uses 📊 emoji for metrics context.
- **Links block:** Each on its own line with emoji prefix:
  - 📖 Článok: https://lukaschylik.dev/blog/articles/{slug}/
  - 🎧 AI generated podcast: [Spotify episode URL]
  - 🤖 [Related tool/skill link — only if applicable]
- **Hashtags:** 10-15 on the last line, no emoji. Start with broad (#WebPerformance #CSS #WebDev #Frontend) then topic-specific then related metrics/tech.
- **Emoji usage:** Very restrained — only 3-4 total: ✨ opening, 👨‍💻 after technical listing, 📊 for metrics, then emoji prefixes on links. NO emoji in hashtags.
- **Tone:** Author shares personal motivation. Not just announcing — providing context about WHY the article exists and what unique value it brings.
- **Structure:** Hook (1 sentence) → Personal context + article overview (1 paragraph) → Additional value (1 paragraph) → Links → Hashtags
- **Length:** ~120-200 words (excluding links and hashtags).

### Step 4: Generate Spotify podcast description

Create the file at: `src/routes/blog/articles/{slug}/spotify/description.md`

**Spotify description rules (Czech language):**

The podcast is "CSS CzechoSlovakia podcast" — "Podcast pro českou a slovenskou komunitu, kde objevujeme sílu moderního CSS!"

**IMPORTANT: The podcast title MUST start with 🎙️ emoji.**

Format: `🎙️ [Topic Name in Czech] – [short hook phrase]`

**Real example for reference:**

Title: `🎙️ CSS View Transitions – animace přechodů stránek`

Description:
```
V nejnovější epizodě se věnujeme CSS View Transitions API, které otevírá nové možnosti pro přechody mezi stránkami i jednotlivými stavy rozhraní. Mluvíme o tom, jak tahle technologie funguje, kde dává v praxi smysl a proč může výrazně zlepšit výsledný uživatelský zážitek bez zbytečně složité implementace.
Podíváme se také na to, jaký dopad mohou mít na Web Vitals metriky, konkrétně CLS, INP či LCP, a kde je potřeba být při implementaci opravdu opatrní.
```

**Style patterns:**

- **Title:** Always starts with 🎙️ emoji, then topic name in Czech, dash, short hook phrase.
- **First paragraph:** "V nejnovější epizodě se věnujeme..." or "V této epizodě se podíváme na...". Describes what the episode covers and WHY it matters. Mentions practical benefits.
- **Second paragraph:** "Podíváme se také na..." or "Mluvíme o tom...". Additional dimension — typically performance impact, gotchas, or related technologies. Specific metrics mentioned by name.
- **Czech language patterns:**
  - "se věnujeme" (we focus on), "mluvíme o tom" (we talk about)
  - "podíváme se" (we'll look at), "objevíme" (we'll discover)
  - "v praxi smysl" (makes sense in practice)
  - "uživatelský zážitek" (user experience)
  - "přechody" (transitions), "animace" (animations)
  - "rozhraní" (interface), "implementace" (implementation)
  - "kde je potřeba být opravdu opatrní" (where you need to be careful)
- **Tone:** Informative, slightly more formal than LinkedIn. "We" perspective — as if two people are discussing/presenting. Practical focus.
- **Length:** ~60-100 words for description. Two concise paragraphs.
- **No links, no hashtags** — podcast platform description only.
- **No emojis in description body** — only in the title.

## Output format

The Spotify output file should contain both title and description clearly separated:

```
Title: 🎙️ [Topic] – [hook]

Description:
[description text]
```

## Output checklist

After generating all three files, confirm to the user:
- [ ] MDX file created at `src/routes/blog/articles/{slug}/index.mdx` with correct frontmatter
- [ ] Blog post follows the established structure and style (Slovak)
- [ ] All external links use `target="_blank" rel="noopener noreferrer"`
- [ ] Browser support section included with specific version numbers
- [ ] Accessibility section included
- [ ] LinkedIn post created at `src/routes/blog/articles/{slug}/linkedin/post.md` (Slovak)
- [ ] Spotify description created at `src/routes/blog/articles/{slug}/spotify/description.md` (Czech)
- [ ] Spotify title starts with 🎙️ emoji
- [ ] Ask user for any missing info (Spotify episode URL, CodePen IDs, card image, related skill links)

## Important notes

- This skill is a **copywriter helper** — the user will review, edit, and refine all outputs
- **Do NOT research or search for additional sources** — only use what the user provides
- If the user provides URLs, use WebFetch to read them
- If the topic has related existing blog posts, link to them internally within the new article
- Check existing articles in `src/routes/blog/articles/` to find cross-linking opportunities
