# Slides project — Claude guidance

## Slide titles
Always use `layout: default` with the `heading:` frontmatter prop instead of `# Title` markdown. This uses the correct `headline` typescale and renders the accent underline. Example:
```yaml
---
layout: default
heading: My Slide Title
---
```

## Hard rules
- **Never use emojis.** Use icons instead via UnoCSS class syntax: `<span class="i-ph-coffee w-5 h-5" />`. Size icons with `w-*`/`h-*`, not `text-*`. Color with `text-current` to inherit. Available icon sets: `carbon` (`i-carbon-*`), `ph` Phosphor (`i-ph-*`), `svg-spinners` (`i-svg-spinners-*`). No Lucide — it is not installed.
- **Never add borders** unless explicitly asked. Do not use `border`, `border-*`, or `outline` utility classes or CSS properties unprompted.

## Theme components
Always use existing components from the installed theme (`slidev-theme-inders`) instead of writing raw HTML/CSS.

Available components (auto-imported by Slidev):
- `Card` — variants: `filled` | `outlined` | `elevated` | `tonal`; props: `shape`, `padding`
- `Grid` — props: `cols` (number or raw template string), `gap`, `rowGap`, `align`
- `Flex`, `Row`, `Stack` — flex-based layout primitives
- `Callout`, `SplitCallout` — highlighted callout blocks
- `Chip` — small label/tag
- `Avatar` — speaker/user avatar
- `CTAButton` — call-to-action button
- `Stepper`, `StepperItem` — step-by-step flows
- `Timeline`, `TimelineItem` — timeline layout
- `Stats` — stat/metric display
- `IconGrid` — icon + label grid
- `Divider`, `Spacer`, `Center` — utility layout

Available layouts (use in slide frontmatter):
- `cover` — title slide with `event`, `subheading`, `speakers` props
- `intro` — single speaker bio with `name`, `role`, `avatar`, `bio`, `social`, `chips` props
- `closing` — closing slide with default slot and `footer` named slot
- `section` — section divider
- `two-col` — two-column layout
- `statement`, `quote`, `center`, `default`
