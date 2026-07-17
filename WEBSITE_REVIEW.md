# Website Review — kozodoi.me repo (live flagship: kozodoi.com)

*Review of the Jekyll/fastpages site in this repo, July 2026. Goal: make the personal site a flagship destination for an AI scientist/engineer working on GenAI and agentic solutions. Note: the live site is at **kozodoi.com** (React, per the README migrated in October 2025); this repo is the previous Jekyll version, whose `CNAME` still deploys to **kozodoi.me**.*

---

## 0. The two-domain problem — resolve this first

The live flagship is kozodoi.com, but this repo (`CNAME: kozodoi.me`, 82 hardcoded `kozodoi.me` URLs, zero `kozodoi.com` references) still exists as the old site. Search engines currently index **both** domains with competing content: Google shows the new site's titles ("Nikita Kozodoi | AI Scientist", `/awards`, `kozodoi.com/portfolio`) alongside the old site's titles ("Blog on AI, ML and other cool acronyms" at `kozodoi.me/certifications/`). For someone whose goal is "this is what people find when they look me up," that's the single most impactful thing to fix:

1. **Pick kozodoi.com as the one canonical domain** and make every `kozodoi.me` URL 301-redirect to its kozodoi.com equivalent (page-for-page, not just to the homepage — years of blog backlinks, talk slides, and paper PDFs point at deep .me URLs like `/blog/...`, `/talks/*.pdf`, `/cv.pdf`).
2. **Make sure the old Jekyll site stops being served as live content on .me.** As long as both render, you're splitting link equity and showing many visitors the outdated 2021-era brand.
3. **Update off-site profiles** (LinkedIn, GitHub, Kaggle, Scholar, X, YouTube) to link kozodoi.com consistently.
4. **Then archive this repo** (or repurpose it purely as redirect infrastructure) and delete the "this repo is an old version" framing from the public README.

Everything below reviews the content and code in this repo. The **content, positioning, and structure** findings (§1–§3, §7) apply to the live kozodoi.com site just as much — carry them over there. The code-level fixes only matter if any of this repo keeps serving traffic.

---

## 1. Strategic: the site doesn't say who you are today

This is the biggest gap. Everything else is polish.

**The site's self-image is 2021: Kaggle competitions and credit-scoring research. Your actual profile in 2026 is GenAI and agentic AI.** A recruiter, conference organizer, or collaborator landing on kozodoi.me today would conclude you mostly do computer-vision Kaggle competitions and credit-risk ML:

- The homepage intro says only "I am Applied Scientist working on the frontier of AI research & business." The words *generative AI*, *LLM*, and *agent* appear nowhere on the homepage.
- The three featured portfolio projects (`_pages/p1_portfolio.md`) are from 2021: text readability (Kaggle 2021), molecule translation (Kaggle 2021), fair credit scoring (paper 2021). The two GenAI projects (medical content generation, RAG with human feedback) are buried inside a collapsed accordion below the fold.
- The blog's latest post is July 2024; talks end in 2024; the footer says "© 2020 – 2023"; the Kaggle ranks are a snapshot; the teaching page speaks in present tense about HU Berlin duties that ended with the PhD in 2022.

**Recommendations:**

1. **Rewrite the hero.** Name, current role, and a one-sentence value proposition that leads with GenAI/agents. Something like: *"I'm Nikita Kozodoi, PhD — Applied Scientist at AWS. I design and ship generative AI and agentic systems for enterprise customers, from RAG assistants to multi-agent workflows."* Follow with 3–4 proof points as a compact row: PhD in ML · AWS certified · Kaggle Competitions Master · N publications / N citations · speaker at ECML, AWS events.
2. **Re-anchor the portfolio around GenAI and agentic work.** Promote the two AWS GenAI projects to featured position, add cards for other public agentic work (AWS ML blog posts, aws-samples repos, talks-turned-projects), and demote the 2021 Kaggle projects into a "Classic ML & competitions" section. Add a year to every card — undated work reads as current until the visitor figures out it isn't, and then trust drops.
3. **Fix every freshness signal.** Footer year (make it dynamic: `{{ site.time | date: '%Y' }}`), Kaggle ranks (state "Competitions Master" as the durable fact, not a rank number that goes stale), teaching page (past tense: "I taught ML at HU Berlin during my PhD"), talks (add 2025/2026 talks if any exist).
4. **Write 2–3 posts about agents.** The blog is your best asset — real tutorials with code — but it stops before the agentic era. Even a small series ("Patterns for production agent systems", "Evaluating RAG beyond retrieval metrics", "What Kaggle taught me that transfers to LLM engineering") instantly repositions the whole site. Cross-post short summaries of your AWS ML blog articles with canonical links back.
5. **Consider a `/now` section or "Recent highlights" strip on the homepage** — the cheapest way to prove the site is alive.

---

## 2. Structure: too many top-level pages, no hierarchy of importance

Current nav: Blog · About · Portfolio · Talks · Papers · Kaggle · Certifications · Teaching · Search. That's flat and long, and the homepage duplicates the About page's role.

**Proposed sitemap:**

| Page | Contents |
|---|---|
| **Home** | Hero + featured projects (3) + latest posts (3) + speaking strip + contact |
| **About** | Bio, experience timeline, certifications (folded in), teaching (folded in, past tense), contact |
| **Projects** | GenAI/agentic first, then classic ML; Kaggle achievements as a section here, not a top-level page |
| **Research** | Papers + citation stats + reviewing |
| **Talks** | As today, refreshed |
| **Blog** | As today |

Certifications and Teaching don't earn top-level slots for a senior industry profile — they're supporting evidence, not headline content. Search can live as an icon in the header. This halves the nav and makes the first click obvious.

Other structural points:

- The homepage (`index.html`) currently doubles as the blog index (paginated posts below the intro). Decide: either the homepage is a landing page (recommended for a flagship portfolio) and `/blog/` is the post index, or it's a blog-first site. Right now `/` and `/blog/` render nearly the same list.
- The intro text block on `index.html` and the About page opener are near-duplicates; after the split above, each page gets one distinct job.

---

## 3. Design and style

The site is a customized fastpages/Minima theme, which reads as "2020 ML blog," not "flagship portfolio." Full redesign aside, the highest-leverage fixes:

- **Buttons that are really links.** Every action on the site is `<button onclick="window.open(...)">` (portfolio, talks, papers, Kaggle pages). These break middle-click/new-tab, aren't crawlable, are invisible to screen readers as navigation, and can't be pre-fetched. Replace with styled `<a>` tags.
- **Emoji as UI icons** (💻 🗒 📊 in every button, emoji bullets in every list). Emoji render differently per OS and read as informal. FontAwesome is already loaded — use one consistent icon set, or none.
- **Duplicated "cover-desc" / "page-desc" text.** Every menu page repeats its description twice in the markup (once over the cover image, once below) with visibility toggled by CSS. It works, but the cover images themselves (full-width, 50% aspect, 0.8 opacity photos with text overlaid) are heavy and generic. A flagship site would drop the photo-banner pattern for a clean typographic header.
- **Spacing hacks throughout:** invisible `<hr>` elements for spacing, negative inline margins (`margin-top: -10px`), `style` attributes on nearly every element. Move to classes; this is also what makes a future redesign painful.
- **Dark mode:** implemented with `!important` on nearly every rule (`_sass/minima/dark-light-mode.scss`), and there's a typo — `color: var(--high-emph)h !important;` (site-title rule). The toggle is a bare checkbox hardcoded to `checked` in three layouts, so its visual state can disagree with the actual theme on load. Fine to keep the feature; worth rebuilding it as a proper `data-theme` toggle with one source of truth.
- **Typography:** default Minima with Primer CSS loaded on top (entire framework from unpkg, plus FontAwesome 5.0.7 from a 2018 CDN URL). Pick one system, self-host, and choose a distinctive but restrained font pairing — typography alone would modernize the site more than any other single design change.
- **The parrot GIFs** (animated parrot next to "Hi, I am Nikita!", Sherlock parrot on 404). Charming on a blog; on a flagship professional site I'd keep at most one as an easter egg (the 404 is the right place).
- **Accessibility:** `alt="Notebook"` on every portfolio image (wrong and repeated), "Click here" link text throughout (bad for a11y and SEO — make link text descriptive), no visible focus states on the custom buttons/accordions.

---

## 4. Concrete bugs found

1. **Broken button** — `_pages/p4_kaggle.md:42`: `window.open('https://www.kaggle.com/c/cassava-leaf-disease-classification/discussion/220751)` is missing the closing quote, so the onclick is a JS syntax error and the "Summary" button for your *gold-medal* competition does nothing.
2. **404 poster link** — `_pages/p2_talks.md:221` links `ECML_2019_poster.pdf` but the file is `talks/ECML_2019_Poster.pdf`; GitHub Pages is case-sensitive, so this 404s.
3. **Duplicate SEO/meta tags** — `_includes/head.html` emits `{% seo %}` and `{% feed_meta %}`, then includes `custom-head.html` which emits both *again*. Every page has two `<title>`s, duplicate OG tags, and duplicate feed links. Remove one set.
4. **Stray `<meta charset="UTF-8">` inside `<body>`** on index and several `_pages/*` files — invalid HTML; the charset is already set in `<head>`.
5. **Citations iframe** — `_pages/p3_papers.md:241` embeds `https://www.jung.ms/citations.php?...`, a third-party PHP widget that can die (or serve anything) at any time. Replace with static numbers updated occasionally, or a link to Google Scholar.
6. **Config/footer social drift** — Instagram was removed from the site but is still in `_config.yml` social links (renders in the footer). Twitter is branded/linked as Twitter; decide on X vs dropping it.

---

## 5. Assets, performance, repo hygiene

- **~6 MB of hero JPGs**: `images/menu/photo_about.jpg` (1.1 MB), `photo_publications.jpg` (1.1 MB), `photo_kaggle.jpg` (1 MB), `photo_black.jpg` (870 KB — the homepage avatar!). Resize to display dimensions, convert to WebP/AVIF, add `loading="lazy"` and `srcset`. The homepage avatar alone should be <50 KB.
- **8+ portfolio images hotlinked from postimg.cc** (a free image host) plus more inside blog posts. If postimg purges them, your portfolio silently loses its images. Move them into `images/` in the repo.
- **Own images referenced by absolute URL** (`https://kozodoi.me/images/...` in `index.html`, 404 page, portfolio). Breaks local preview and any staging domain; use relative URLs.
- **`GA 20210101-20230210.xlsx` at the repo root** — a Google Analytics export sitting in a public repo and deployed to the live site. Delete it (and note it's already in the git history).
- **Stale generated cruft**: `python/` (old category-permalink HTML redirects), `images/copied_from_nb/` (5 MB), `_action_files/` (fastpages machinery), `.well-known/pki-validation/` (old cert validation). Prune whatever the current build doesn't use.
- **`talks/` is 41 MB of PDFs** — works, but consider whether all 12 need to live in the repo/deployment vs. linking a few to external hosting.

---

## 6. Platform: fastpages is dead — decide the endgame

The build pipeline is [fastpages](https://github.com/fastai/fastpages), archived by fast.ai in 2022. The GitHub Actions in `.github/workflows/` (`setup.yaml`, `upgrade.yaml`, `ci.yaml`) reference that dead upstream, the Ruby/Jekyll toolchain is pinned to old versions, and notebook-to-post conversion depends on unmaintained Docker images. This will keep getting harder to build.

Given the README already mentions a React migration, I'd commit to one of two paths rather than patching this one:

- **Path A — modern static stack (recommended):** Astro or Next.js with MDX. You keep markdown/notebook-derived content, get a component model for the portfolio cards/talk lists (which are currently hand-copied HTML blocks with manually numbered `dots1…dots11` IDs — adding one talk means renumbering), first-class image optimization, and a design you fully own. Content collections (projects, talks, papers as YAML/MDX with a schema) would replace ~1,500 lines of repeated HTML.
- **Path B — stay Jekyll, minimally:** drop fastpages, use plain Jekyll or al-folio (actively maintained academic theme), move posts to plain markdown, delete the action machinery.

Either way, the data-not-markup move is the big win: talks, papers, projects, and certifications are *lists of records* and should be YAML data files rendered by one template, not hand-maintained HTML. That alone fixes the numbering fragility, the inconsistent buttons, and makes adding your next talk a 5-line diff.

---

## 7. SEO and discoverability

- Site description is "Blog on AI, ML and other cool acronyms" (`_config.yml`) — fun, but it's what Google shows. Use something like "Nikita Kozodoi — AI scientist and engineer building generative AI and agentic systems. Blog, publications, talks, and projects."
- Menu pages have no per-page `description` front matter, so the SEO tag falls back to the site description everywhere.
- No default Open Graph image — set a proper social card (your name + role + face) so shared links look intentional on LinkedIn/X/Slack.
- Add JSON-LD `Person` structured data (name, jobTitle, affiliation, sameAs → LinkedIn/Scholar/GitHub/Kaggle) — this is what feeds Google's knowledge panel when people look you up, which is exactly your stated goal.
- Papers page: add `citation_*` meta tags or at least ensure Scholar links; consider an `llms.txt` — on-brand for a GenAI person.

---

## 8. Suggested order of attack

**Week 1 — stop the bleeding (few hours, do in this repo or live one):**
fix the four bugs (§4), rewrite hero + site description, dynamic footer year, promote GenAI projects to featured, past-tense the teaching page, compress the six hero images, delete the GA xlsx.

**Weeks 2–3 — content:**
new bio + experience timeline, portfolio re-org with dates (GenAI → agentic → classic ML → Kaggle), refresh talks/papers with anything from 2025–26, first agent-focused blog post, social card + JSON-LD.

**Month 2 — platform:**
pick Path A or B (§6), move talks/papers/projects into data files, rebuild the design with a proper type system, real links instead of button-onclicks, one icon set, rebuilt dark mode.

The strategic reframing (§1) matters more than everything else combined: fix the story first, then the pixels.
