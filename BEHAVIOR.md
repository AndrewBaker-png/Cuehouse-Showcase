# App behavior and professional talking points

## A short introduction

> I’m building Cuehouse to make browsing and playing a personal Plex library more focused. I define the intended behavior and viewing workflows, then direct AI-assisted implementation and troubleshooting. One example is the viewing-choice panel: it lets someone start a random queue of movies, episodes, or titles below a chosen runtime without having to pick a title first.

## Home and viewing choices

Home provides shortcuts to Downloads, Watch Later, and Functions. Functions opens the viewing-choice controls described below.

| Control | What happens |
| --- | --- |
| **Play Anything** | Builds a shuffled queue from movies and individual TV episodes in the current server’s available movie and TV libraries, then attempts to start playback. |
| **Random Movie** | Builds a shuffled queue of movies and attempts to start playback. |
| **Random Episode** | Builds a shuffled queue of individual TV episodes and attempts to start playback. Episodes are not kept in season or story order. |
| **Time to watch** | Filters movies and episodes by individual runtime, builds a shuffled queue of eligible titles, and attempts to start playback when the viewer taps **Shuffle**. Moving the slider alone does not start playback. |

### What the time setting means

The slider selects a maximum runtime from **5 to 180 minutes**, in five-minute increments. The app remembers the selection; its initial default is 30 minutes.

Selecting **30 minutes** means each eligible title must be **shorter than 30 minutes**. A title exactly 30 minutes long is excluded. The filter uses the full runtime, not the remaining time on a partially watched title. Titles without a reliable runtime are excluded.

The setting does not build a queue whose combined duration fits 30 minutes, and it does not stop playback after 30 minutes. It is a title-selection filter, separate from a sleep timer.

**Example to explain aloud:**

> If I have half an hour, I can ask Cuehouse for individual titles under 30 minutes. That gives me a queue of shorter options. It doesn’t promise that watching the entire queue will fit into half an hour.

### How random selection works

- Watched and unwatched titles can both appear.
- Selection does not use viewing-history recommendations, show grouping, or a remembered shuffle order.
- Duplicate item identifiers are removed from the generated queue.
- For a capped selection, libraries contribute according to the number of eligible titles, so a small library is not given the same overall weight as a much larger one.
- Each action has its own saved limit: **10, 25, 50, 100, 300, or Unlimited**. The default is **50**. These are upper limits; a queue can contain fewer titles when fewer qualify.
- Unlimited reads the complete eligible catalogs rather than requesting a capped sample.

### Preparing and starting playback

Cuehouse uses saved library information when available and requests information from Plex when needed. This is a way to reduce repeated library loading; it is not a promise that streaming works offline.

While preparing, the interface displays **Choosing a title…** followed by **Starting playback…**, with a **Cancel** action. Preparation disables the other selection controls to avoid overlapping requests.

If a candidate is missing or has certain title-specific playback problems, startup can try another candidate, examining up to five titles. Connection, authorization, or invalid-response failures do not trigger a retry across the entire queue. Preparation has a 60-second timeout.

Changing the server or account context, permissions, or a shuffle limit cancels pending preparation. Leaving the controls also cancels pending work. Empty libraries and unsuccessful startup produce an explanatory error rather than silently doing nothing.

## Design decisions to discuss

| Decision | Professional explanation |
| --- | --- |
| Separate mixed, movie, and episode actions | “I wanted the controls to reflect the kind of viewing decision someone is making.” |
| Start from a shuffled queue | “The goal is to reduce browsing effort and move directly toward playback.” |
| Keep time selection separate from the Shuffle button | “Adjusting a preference should not unexpectedly start a video.” |
| Offer independent queue limits | “Different actions can have different queue sizes, and a capped sample can reduce library-loading work.” |
| Provide progress, cancellation, and bounded retries | “I specify the waiting and failure paths as part of the experience, alongside the successful path.” |
| Revisit the design after using the implementation | “My process is iterative: implemented behavior helps me refine the requirements and interface.” |

## My contribution

My role is product direction, behavior specification, and iterative interface design. I rely heavily on AI for implementation and troubleshooting. The project uses Swift, SwiftUI, and Apple playback frameworks; that technology list should not be read as a claim that I manually authored all of the code or independently implemented every subsystem.

## Basis and scope

This guide reflects a November 1, 2026 review of the current iPhone/iPad Home controls, shared queue-selection logic, and shuffle service, including the layout used on the iPhone Duo simulator. It describes code behavior, not a new end-to-end playback test or a verified guarantee of parity with Mac and Apple TV.

Further workflows can be documented separately as they are reviewed. For recorded test evidence, see [Testing and validation](VALIDATION.md).

[Back to the showcase](README.md)
