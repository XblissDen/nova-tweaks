# Firefox Nova UI Tweaks

A small `userChrome.css` setup for customizing Firefox's native tabs and sidebar.

The goal is to keep the modern Firefox/Nova look while improving usability:

- cleaner native vertical tabs
- larger click targets
- less rounded tab corners
- no close/mute controls in collapsed vertical mode
- auto-hidden vertical tabs that appear on hover
- wider pinned tabs in horizontal mode
- favicon-only pinned tabs in horizontal mode

## Preview of the behavior

### Vertical tabs

When the sidebar is expanded:

- tabs are taller and easier to click
- tab corners are less rounded
- favicons are slightly larger
- selected and hovered tabs are easier to distinguish

When the sidebar is collapsed:

- close buttons are hidden
- mute/audio overlays are hidden
- only the favicon remains visible

### Auto-hide sidebar

The vertical-tab sidebar can be hidden almost completely when not in use.

Moving the cursor to the left edge of the Firefox window reveals the sidebar. The sidebar opens as an overlay, so the webpage does not resize or reflow every time the tabs appear.

### Horizontal pinned tabs

Pinned tabs in horizontal mode are:

- wider than Firefox's default pinned tabs
- easier to click
- favicon-only
- centered
- displayed without a close button

Normal horizontal tabs are not affected.

---

## Installation

### 1. Enable `userChrome.css`

Open:

```text
about:config
```

Search for:

```text
toolkit.legacyUserProfileCustomizations.stylesheets
```

Set it to:

```text
true
```

### 2. Open your Firefox profile folder

Open:

```text
about:support
```

Find **Profile Folder** and click **Open Folder**.

### 3. Create the `chrome` folder

Inside the profile folder, create:

```text
chrome
```

Then create:

```text
chrome/userChrome.css
```

### 4. Add the CSS

Paste your CSS into `userChrome.css`.

Restart Firefox.

---

## Recommended Firefox sidebar settings

For the custom auto-hide behavior, use Firefox's native vertical tabs but disable Firefox's own hover expansion.

Recommended setup:

```text
Vertical tabs              ON
Expand sidebar on hover    OFF
Hide tabs and sidebar      OFF
Sidebar position           LEFT
```

The CSS handles hiding and revealing the sidebar itself.

If the sidebar disappears completely, Firefox's sidebar toggle can usually restore it:

```text
Ctrl + Alt + Z
```

---

## Main customization values

These are the values you will probably want to tweak most often.

### Vertical tab height

```css
--tab-min-height: 42px !important;
```

Suggested values:

```text
40px  compact
42px  balanced
46px  comfortable
48px  large
```

### Tab corner radius

```css
--tab-border-radius: 5px !important;
```

Suggested values:

```text
8px   still fairly rounded
5px   balanced
2px   almost square
0px   completely square
```

### Gap between vertical tabs

```css
--tab-block-margin: 2px !important;
```

### Sidebar width

```css
--uc-sidebar-width: 260px;
```

Suggested values:

```text
220px  compact
260px  balanced
300px  roomy
```

### Hidden sidebar hover zone

```css
--uc-sidebar-hover-zone: 3px;
```

This is the small invisible area at the left edge that activates the sidebar.

A larger value makes the sidebar easier to reveal accidentally and intentionally.

### Horizontal pinned-tab width

```css
min-width: 56px !important;
max-width: 56px !important;
```

Suggested values:

```text
48px  subtle
52px  medium
56px  comfortable
64px  large
```

---

## What each section does

### Bigger vertical tabs

```css
#tabbrowser-tabs[orient="vertical"] {
    --tab-min-height: 42px !important;
    --tab-block-margin: 2px !important;
    --tab-border-radius: 5px !important;
    --tab-inline-padding: 10px !important;
}
```

This increases the vertical tab hitbox, adds a little spacing, and reduces the default rounded appearance.

---

### Hide close button in collapsed vertical mode

```css
#tabbrowser-tabs[orient="vertical"]:not([expanded])
.tabbrowser-tab .tab-close-button {
    display: none !important;
}
```

This prevents the close button from taking over part of the small collapsed tab.

---

### Hide mute/audio controls in collapsed vertical mode

```css
#tabbrowser-tabs[orient="vertical"]:not([expanded])
.tabbrowser-tab .tab-icon-overlay:is(
    [soundplaying],
    [muted],
    [activemedia-blocked]
) {
    display: none !important;
}

#tabbrowser-tabs[orient="vertical"]:not([expanded])
.tabbrowser-tab .tab-audio-button {
    display: none !important;
}
```

This keeps collapsed vertical tabs favicon-only.

---

### Larger favicons

```css
#tabbrowser-tabs[orient="vertical"]
.tabbrowser-tab .tab-icon-image {
    width: 18px !important;
    height: 18px !important;
}
```

The slightly larger favicon fits better with the taller tabs.

---

### Auto-hide vertical sidebar

The sidebar is moved almost completely outside the window:

```css
#sidebar-main {
    left: calc(
        -1 * (var(--uc-sidebar-width) - var(--uc-sidebar-hover-zone))
    ) !important;
}
```

When the sidebar is hovered or focused, it moves back into view:

```css
#sidebar-main:is(:hover, :focus-within) {
    left: 0 !important;
    opacity: 1 !important;
}
```

Because the sidebar is positioned as an overlay, showing it does not resize the webpage.

---

### Favicon-only horizontal pinned tabs

```css
#tabbrowser-tabs[orient="horizontal"]
.tabbrowser-tab[pinned] .tab-label-container {
    display: none !important;
}
```

Pinned tabs keep only their favicon.

Their width is then fixed:

```css
#tabbrowser-tabs[orient="horizontal"]
.tabbrowser-tab[pinned] {
    min-width: 56px !important;
    max-width: 56px !important;
}
```

And the favicon is centered:

```css
#tabbrowser-tabs[orient="horizontal"]
.tabbrowser-tab[pinned] .tab-content {
    justify-content: center !important;
    padding-inline: 0 !important;
}
```

---

## Example combined CSS

```css
/* ============================================================
   Firefox Nova UI Tweaks
   ============================================================ */

:root {
    --uc-sidebar-width: 260px;
    --uc-sidebar-hover-zone: 3px;

    --uc-sidebar-show-speed: 180ms;
    --uc-sidebar-hide-speed: 250ms;
}


/* ============================================================
   VERTICAL TABS
   ============================================================ */

#tabbrowser-tabs[orient="vertical"] {
    --tab-min-height: 42px !important;
    --tab-block-margin: 2px !important;
    --tab-border-radius: 5px !important;
    --tab-inline-padding: 10px !important;
}


/* Hide close button while collapsed */

#tabbrowser-tabs[orient="vertical"]:not([expanded])
.tabbrowser-tab .tab-close-button {
    display: none !important;
}


/* Hide mute/audio controls while collapsed */

#tabbrowser-tabs[orient="vertical"]:not([expanded])
.tabbrowser-tab .tab-icon-overlay:is(
    [soundplaying],
    [muted],
    [activemedia-blocked]
) {
    display: none !important;
}

#tabbrowser-tabs[orient="vertical"]:not([expanded])
.tabbrowser-tab .tab-audio-button {
    display: none !important;
}


/* Slightly larger favicons */

#tabbrowser-tabs[orient="vertical"]
.tabbrowser-tab .tab-icon-image {
    width: 18px !important;
    height: 18px !important;
}


/* Hover */

#tabbrowser-tabs[orient="vertical"]
.tabbrowser-tab:hover .tab-background {
    background-color:
        color-mix(in srgb, currentColor 9%, transparent) !important;
}


/* Selected tab */

#tabbrowser-tabs[orient="vertical"]
.tabbrowser-tab[selected] .tab-background {
    outline: 1px solid
        color-mix(in srgb, currentColor 12%, transparent) !important;

    outline-offset: -1px !important;
}


/* ============================================================
   AUTO-HIDE VERTICAL SIDEBAR
   ============================================================ */

#sidebar-launcher-splitter {
    display: none !important;
}

#sidebar-main {
    position: absolute !important;

    top: 0 !important;
    bottom: 0 !important;

    left: calc(
        -1 * (var(--uc-sidebar-width) - var(--uc-sidebar-hover-zone))
    ) !important;

    width: var(--uc-sidebar-width) !important;
    min-width: var(--uc-sidebar-width) !important;
    max-width: var(--uc-sidebar-width) !important;

    z-index: 9999 !important;

    opacity: 0 !important;

    background-color: var(--sidebar-background-color) !important;

    transition:
        left var(--uc-sidebar-hide-speed)
            cubic-bezier(.16, 1, .3, 1),
        opacity 150ms ease !important;
}

#sidebar-main:is(:hover, :focus-within) {
    left: 0 !important;
    opacity: 1 !important;

    transition:
        left var(--uc-sidebar-show-speed)
            cubic-bezier(.16, 1, .3, 1),
        opacity 120ms ease !important;

    box-shadow: 6px 0 20px rgba(0, 0, 0, 0.18) !important;
}


/* ============================================================
   HORIZONTAL PINNED TABS
   ============================================================ */

/* Hide title */

#tabbrowser-tabs[orient="horizontal"]
.tabbrowser-tab[pinned] .tab-label-container {
    display: none !important;
}


/* Wider pinned tabs */

#tabbrowser-tabs[orient="horizontal"]
.tabbrowser-tab[pinned] {
    min-width: 56px !important;
    max-width: 56px !important;
}


/* Center favicon */

#tabbrowser-tabs[orient="horizontal"]
.tabbrowser-tab[pinned] .tab-content {
    justify-content: center !important;
    padding-inline: 0 !important;
}


/* Hide pinned-tab close button */

#tabbrowser-tabs[orient="horizontal"]
.tabbrowser-tab[pinned] .tab-close-button {
    display: none !important;
}
```

---

## Notes

Firefox does not officially guarantee compatibility for `userChrome.css`.

Internal browser UI selectors can change between Firefox releases, so some rules may need adjustments after major UI updates.

If a tweak suddenly stops working after a Firefox update:

1. temporarily comment out the affected section
2. restart Firefox
3. inspect whether Firefox changed the relevant UI selector
4. update the selector in `userChrome.css`

It is a good idea to keep the CSS in version control so changes are easy to track and revert.

---

## Scope

This configuration intentionally avoids turning Firefox into a completely different theme.

It focuses only on small usability improvements around:

- native vertical tabs
- sidebar visibility
- tab hitboxes
- pinned tabs
- favicon presentation

Colors and most of Firefox's visual styling are still controlled by the active Firefox theme.
