# Lao document language uses Wiki.js locale `th`

Wiki.js has no Lao locale pack (`lo` is not in the host download list). Installed locales are `en` and `th`. We publish Lao bodies as the `th` locale pair of the English page (same GraphQL path, `locale: "th"`, URLs `/th/…`) rather than a second path or both languages in one English page. Working-copy filenames follow the locale (`.th.md`) so `/wiki-publish` can resolve them. The language switcher will label the pages Thai; that is the host constraint, not Thai content. Revisit if Wiki.js ships a Lao pack.
