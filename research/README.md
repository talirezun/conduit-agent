# Research

A home for articles and research on the ideas behind Conduit: prompt-driven agents, agent governance and accountability, working memory and the compounding context layer, local-first privacy, and the practice of building assistant agents that run on your own computer.

This section is open to contributions — from the maintainers, from students, and from the wider community.

---

## How it's organized

```
research/
├── README.md          ← this index
└── articles/          ← one markdown file per article (kebab-case filename)
```

Articles live in [`articles/`](articles/) as individual markdown files named in kebab-case after their title, e.g. `prompt-driven-agents-for-non-technical-users.md`.

---

## Articles

*The first articles are on their way. This list will grow.*

<!-- Add new entries to the table below, one row per article. -->
<!-- | [Article Title](articles/article-filename) | One-line summary | Author | Month Year | -->

| Title | Summary | Author | Date |
|---|---|---|---|
| *(coming soon)* | The first articles will appear here. | | |

---

## Contributing an article

1. Write your piece as a markdown file in [`articles/`](articles/), named in kebab-case after the title.
2. Start with a title (`#`), then a byline line: *Author · Month Year*.
3. Keep it readable and cite your sources.
4. Add a row to the **Articles** table above.
5. Open a pull request. See [CONTRIBUTING.md](../CONTRIBUTING.md).

Topics we'd love to see: real use-case write-ups, comparisons of runtimes/models for non-technical users, memory and knowledge-graph techniques, agent governance and the EU AI Act, privacy and local-first patterns, and teaching notes from running labs with Conduit.

---

## Citing this work

When referencing an article from this collection, please use a format like:

```bibtex
@misc{conduit_research,
  author       = {Author Name},
  title        = {Article Title},
  year         = {2026},
  month        = {June},
  howpublished = {Conduit research collection},
  url          = {https://github.com/talirezun/conduit-agent/tree/main/research/articles}
}
```

---

## Related reading

- **[The Curator research collection](https://github.com/talirezun/the-curator/tree/main/research)** — articles on the second-brain architecture that powers Conduit's context layer.
- **[Conduit docs](../docs/)** — the framework itself, explained.
