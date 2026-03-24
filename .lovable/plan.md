
Goal: remove the ambiguity in the story and stop the font instability by simplifying the typography system and tightening the technical wording on delivery/failover.

1. Stabilize fonts by making one source of truth
- Right now fonts are defined in 3 places: `@fontsource` imports in `src/main.tsx`, Tailwind font families in `tailwind.config.ts`, and global element selectors in `src/index.css`.
- That is why this feels harder than it should: some text is driven by semantic tags (`h1`-`h6`), some by `font-display`, and some by default body inheritance. During rebuilds/HMR, that mix can look inconsistent even if the files are “correct”.
- I will simplify this to:
  - keep local `@fontsource` imports
  - keep Tailwind `font-sans` / `font-display`
  - remove reliance on global heading selectors as the primary mechanism
  - explicitly apply typography classes where needed so slide text is deterministic
- Files likely involved: `src/index.css`, `tailwind.config.ts`, `src/components/slides/SlideLayout.tsx`, plus any slide components using display/body text inconsistently.

2. Redesign Slide 2 so it clearly answers “Why are we here?”
Current problem:
- The slide mixes customer facts, transformation journey, acquisitions, and threats, but it does not clearly state:
  - current state
  - future state
  - program goal

New Slide 2 structure:
- Title: `AT Retailers: Current State → Target State`
- Left column: Current State
  - legacy monolith
  - on-prem + AWS
  - acquired platforms on different providers
  - inconsistent security posture
  - teams not yet trained on Akamai
- Right column: Target State
  - Akamai in front of all customer traffic
  - standardized hostname onboarding + certificate process
  - delivery optimized for 5x peaks and image performance
  - modern security controls for WAF / bots / APIs
  - repeatable operating model across geographies and teams
- Bottom banner: Program Objective
  - `Onboard 5,000 hostnames in 30 days while improving resiliency, performance, and security during the migration.`
This makes Slide 2 a context-setting slide, not a solution slide.

3. Remove the unsupported “10% canary” assumption unless we frame it correctly
You are right to question it.
- The current wording implies a generic old/new split, which is only valid if there are parallel paths available.
- I will change the messaging so it is precise:
  - if legacy provider and Akamai are both live, a controlled traffic split can send 10% to Akamai and 90% to the legacy path
  - if this is an Akamai config change in front of the same origin, the safer story is phased hostname cohorts / pilot wave / instant rollback, not arbitrary traffic split between “old” and “new system”
- I will update the roadmap and delivery solution copy to present this as:
  - pilot cohort
  - low-risk hostname wave
  - batch cutover
  - rollback path
This will make the delivery plan more credible.

4. Correct the 5xx / origin-load wording
You are also right here.
- Akamai does not magically “fix” all 5xx errors.
- 5xx responses usually originate from the application or origin infrastructure.
- The accurate story is:
  - Akamai reduces origin load through caching, edge offload, image optimization, and tiered delivery
  - GTM / failover can route away from a degraded origin
  - for cacheable content, Akamai may reduce blast radius via stale/error handling
  - for truly dynamic uncached requests, a 5xx is still an origin/app problem
I will revise the delivery language so it says:
- “reduce origin load and lower the chance of overload-related 5xx”
instead of
- “Akamai reduces 5xx errors” in a blanket way

5. Concrete content changes
- `src/components/slides/CompanyOverviewSlide.tsx`
  - rebuild into Current State / Target State / Program Objective
- `src/components/slides/RoadmapSlide.tsx`
  - replace hard-coded “10% canary” language with controlled cutover wording
- `src/components/slides/SolutionDesignSlide.tsx`
  - update “No testing” solution to cohort-based rollout / pilot wave
  - tighten the 5x traffic and 5xx language so it is technically accurate
- `src/components/slides/ArchitectureSlide.tsx`
  - align any origin/failover wording with the corrected explanation
- `src/index.css` / `tailwind.config.ts` / possibly `SlideLayout.tsx`
  - unify typography behavior

Technical details
- Root cause of font pain: mixed typography strategy, not just a missing import.
- Better typography approach for this deck:
  - body copy = `font-sans`
  - section titles / slide titles = `font-display`
  - avoid depending on `h1-h6` selectors for deck-wide styling
- Better delivery wording:
  - GTM can steer between available endpoints/providers/origins
  - DNS/GTM split is valid only when both paths exist
  - Akamai reduces origin stress; it does not eliminate all origin-generated 5xx

Expected outcome
- Fonts stop “flipping” because styling becomes deterministic.
- Slide 2 clearly explains current state, target state, and business goal.
- The rollout story becomes believable and technically defensible.
- The deck stops overclaiming what Akamai does for 5xx behavior.
