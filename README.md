# Firefox Nova UI Tweaks

A small `userChrome.css` setup for customizing Firefox's native tabs and sidebar.

The goal is to keep the modern Firefox/Nova look while improving usability:

* cleaner native vertical tabs
* larger click targets
* less rounded tab corners
* no close/mute controls in collapsed vertical mode
* wider pinned tabs in horizontal mode
* favicon-only pinned tabs in horizontal mode

## Preview of the behavior

### Vertical tabs

When the sidebar is expanded:

* tabs are taller and easier to click
* tab corners are less rounded
* favicons are slightly larger
* selected and hovered tabs are easier to distinguish

When the sidebar is collapsed:

* close buttons are hidden
* mute/audio overlays are hidden
* only the favicon remains visible

### Horizontal pinned tabs

Pinned tabs in horizontal mode are:

* wider than Firefox's default pinned tabs
* easier to click
* favicon-only
* centered
* displayed without a close button

\---

## Installation

### 1\. Enable `userChrome.css`

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

### 2\. Open your Firefox profile folder

Open:

```text
about:support
```

Find **Profile Folder** and click **Open Folder**.

### 3\. Create the `chrome` folder

Inside the profile folder, create:

```text
chrome
```

Then create:

```text
chrome/userChrome.css
```

### 4\. Add the CSS

Paste your CSS into `userChrome.css`.

Restart Firefox.

## Scope

This configuration intentionally avoids turning Firefox into a completely different theme.

It focuses only on small usability improvements around:

* native vertical tabs
* sidebar visibility
* tab hitboxes
* pinned tabs
* favicon presentation

Colors and most of Firefox's visual styling are still controlled by the active Firefox theme.

