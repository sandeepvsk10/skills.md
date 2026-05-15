---
name: text-scramble
description: "Use this skill when adding Anime.js scrambleText, decode, glitch, or character-cycling text animations to a frontend UI. Covers the Anime.js text API, React usage, accessibility, motion preferences, and visual restraint."
---

# Text Scramble Animation

Use this skill when a UI needs a short text scramble or decode effect for headings, labels, command output, status transitions, portfolio details, or terminal-inspired interfaces.

Primary reference: https://animejs.com/documentation/text/scrambletext/

Before implementing, check the latest Anime.js `scrambleText` docs for current imports, parameters, React guidance, and version requirements.

## When To Use

- Animate a word or short phrase during a state transition.
- Reveal hero text, section labels, tags, or command-like UI.
- Add subtle motion to portfolio, developer-tool, AI, data, or terminal-inspired interfaces.
- Reuse an existing `TextScramble` component if the app already has one.

Avoid this effect for long paragraphs, critical error messages, dense dashboards, form inputs, legal copy, or any text the user needs to read immediately.

## Anime.js API

Prefer Anime.js `scrambleText()` when Anime.js is already present or acceptable for the project. The docs describe it as a function-based tween value used directly inside `animate()`.

```ts
import { animate, scrambleText } from "animejs";

animate(".scramble", {
  innerHTML: scrambleText({
    text: "Hello World",
    chars: "uppercase",
    from: "center",
    cursor: "_",
    settleDuration: 500,
    revealRate: 60,
    settleRate: 30,
  }),
});
```

You can also import from the text subpath:

```ts
import { scrambleText } from "animejs/text";
```

Use `innerHTML` rather than `textContent` for `scrambleText()` because the Anime.js docs note that the effect uses `&nbsp;` to preserve spaces.

Common parameters to consider:

- `text`: final text to reveal.
- `chars`: character set, such as uppercase-style scrambling.
- `from`: reveal origin, such as start, center, or end depending on current docs.
- `cursor`: cursor glyph displayed during the effect.
- `revealRate`, `revealDelay`: reveal pacing.
- `settleRate`, `settleDuration`: how quickly random characters settle.
- `duration`, `delay`, `ease`: animation timing.
- `reversed`, `seed`, `perturbation`: use only when the design needs that behavior.

## Implementation Rules

- Keep the final text present in the DOM or provide it through an accessible label.
- Respect `prefers-reduced-motion`; render the final text immediately when reduced motion is requested.
- Limit scramble duration to roughly 300-900ms for normal UI and 1200ms only for prominent hero moments.
- Use stable layout dimensions so the animated characters do not shift nearby content.
- Do not animate every rerender. Trigger only on intentional text changes, hover/focus where appropriate, or first reveal.
- Use a fixed character set that matches the product tone; avoid noisy symbols in professional UI.
- Cleanup Anime.js animations on unmount.

## React Pattern

In React, use `useEffect()` with Anime.js `createScope()` when scoping animations to a component. Revert the scope during cleanup.

```tsx
import { animate, createScope, scrambleText } from "animejs";
import { useEffect, useRef } from "react";

type ScrambleTextProps = {
  text: string;
  className?: string;
  characters?: string;
};

export function ScrambleText({
  text,
  className,
  characters = "uppercase",
}: ScrambleTextProps) {
  const root = useRef<HTMLSpanElement>(null);
  const reduceMotion =
    typeof window !== "undefined" &&
    window.matchMedia("(prefers-reduced-motion: reduce)").matches;

  useEffect(() => {
    if (!root.current || reduceMotion) return;

    const scope = createScope({ root }).add(() => {
      animate(root.current, {
        innerHTML: scrambleText({
          text,
          chars: characters,
          from: "center",
          revealRate: 60,
          settleRate: 30,
          settleDuration: 500,
        }),
      });
    });

    return () => scope.revert();
  }, [characters, reduceMotion, text]);

  return (
    <span ref={root} className={className} aria-label={text}>
      {text}
    </span>
  );
}
```

When the app already has a local animation wrapper, adapt that wrapper instead of creating a competing abstraction.

## Implementation Checklist

1. Confirm Anime.js is already installed or acceptable to add.
2. Check the latest Anime.js `scrambleText` docs.
3. Prefer `animate(target, { innerHTML: scrambleText(...) })`.
4. In React, scope animations with `createScope()` and call `scope.revert()` on cleanup.
5. Render final readable text for reduced-motion users.
6. Verify layout remains stable before, during, and after the animation.

## Styling Guidance

- Use tabular or mono fonts only when the surrounding design supports it.
- Keep letter spacing normal unless the existing design system already uses tracking for labels.
- Use theme tokens for color, not hardcoded neon values.
- Pair the effect with restrained surrounding UI; the animation should be the accent, not the whole interface.

## Accessibility Checklist

- Final text is readable by screen readers.
- Motion can be skipped through `prefers-reduced-motion`.
- Scrambled intermediate characters are not announced repeatedly.
- Color contrast remains valid during and after animation.
- The animation never blocks access to controls or status feedback.
