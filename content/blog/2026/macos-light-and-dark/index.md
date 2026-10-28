---
title: "Change macOS light and dark mode via command line"
date: "2026-10-28T15:37:15Z"
tags: ["macos", "dark-mode", "light-mode", "CLI", "terminal", "accessibility"]
featureAlt: "An atmospheric photo of a backlit basketball basket above heaps of snow."
---

I normally have my macOS set to dark mode because developer, but sometimes I need to
take screenshots for talk slides. For readability and accessibility, it's
[important to have enough contrast](https://www.w3.org/WAI/teach-advocate/accessible-presentations/),
and often dark text on a light background is best.

## Aliases

Rather than having to click 🍎 > System Settings… > Appearance > Light, here's a couple
of aliases to do it from the command line:

```sh
alias light-mode='osascript -e "tell app \"System Events\" to tell appearance preferences to set dark mode to false"'
alias  dark-mode='osascript -e "tell app \"System Events\" to tell appearance preferences to set dark mode to true"'
```

Then I can type `light-mode`, take the screenshot, and then go back with `dark-mode`.

## See also

Some info on accessible presentations:

- [GOV.WALES: How to create accessible PowerPoint presentations](https://www.gov.wales/how-create-accessible-powerpoint-presentations)

- [W3C Web Accessibility Initiative: Making Events Accessible](https://www.w3.org/WAI/teach-advocate/accessible-presentations/)

---

<small>Header photo: Snow basketball today
(<a target="_blank" rel="noopener noreferrer" href="https://creativecommons.org/licenses/by-nc-sa/2.0/">CC
BY-NC-SA 2.0</a>
[Hugo van Kemenade](https://www.flickr.com/photos/hugovk/3115549385/)).</small>
