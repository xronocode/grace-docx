# GRACE-DOCX

> LLMs lose track of large documents. GRACE-DOCX embeds a navigation map, editing contracts, and verification rules directly inside the .docx — so any agent knows exactly where to go and what not to touch.

---

## The Problem

Give an LLM a 50-page Word document and ask it to edit section 4.2. It reads all 4,000+ paragraphs, guesses where the section is, inserts something nearby, and has no idea that section 4.2 must stay in sync with Appendix B. By the fifth iteration it starts rewriting things you never asked it to touch.

This is **context rot** — the measurable degradation in output quality as input length grows. Research shows models effectively use only 10–20% of their stated context window [[1]](#references), and accuracy drops 30%+ when relevant content sits in the middle of a long context [[2]](#references). Larger context windows don't fix this; they just delay it.

Developers have solved this for codebases: semantic markup, knowledge graphs, explicit module boundaries. GRACE-DOCX applies the same approach to Word documents.

---

## The Idea

Instead of explaining the document structure in every prompt, embed that knowledge directly inside the `.docx` file — once. So any agent that opens the file immediately knows:

- which sections exist and where they are
- what can be edited and what cannot
- what needs to stay in sync when something changes
- how to verify nothing broke

The agent doesn't guess. It follows a protocol embedded in the file itself.

---

## Origin

GRACE-DOCX is a port of the **GRACE methodology** (Graph-RAG Anchored Code Engineering) to Word documents.

GRACE was designed and battle-tested by **Vladimir Ivanov** ([@turboplanner](https://t.me/turboplanner)) for AI-driven code generation: every module gets a contract before code is written, every code block gets semantic markers for navigation, a knowledge graph keeps the entire project map current.

Original GRACE plugin for Claude Code: [osovv/grace-marketplace](https://github.com/osovv/grace-marketplace)

GRACE-DOCX takes the same principles and applies them to `.docx` files.

---

## How It Works

Drop one file into your chat along with the document:

```
[grace-docx-bootstrap.md]
[your-document.docx]

Run bootstrap.
```

The agent analyzes the internal XML structure of the `.docx`, maps all H1/H2 headings, counts paragraphs and tables per section, detects cross-references — and **creates the markup itself, in a format that works for it**.

It embeds five XML metadata files directly inside the archive and injects navigation bookmarks into every section. The document becomes self-describing.

After bootstrap, you work like this:

```
[document_GRACE.docx]

Add a new clause to the "Approval Process" section:
purchases over $50,000 require CFO sign-off.
```

The agent reads the embedded map, finds the right section in O(1), checks the contract for that module, makes the edit surgically, checks must-sync dependencies, verifies structure, returns the file.

---

## What Gets Embedded

GRACE-DOCX adds five XML files to `word/` inside the archive:

| File | Purpose |
|------|---------|
| `grace-manifest.xml` | Entry point, protocol, output policy |
| `grace-graph.xml` | Module map with paragraph ranges and bookmarks |
| `grace-contracts.xml` | Per-module editing rules (can/cannot/must-sync) |
| `grace-instructions.xml` | Agent behavioral principles |
| `grace-verification.xml` | Structural invariants and post-edit checks |

Each H1 section gets a `w:bookmarkStart/End` pair in `document.xml` — standard Word mechanism, invisible to users, precise navigation anchor for agents.

Word ignores these additions entirely. The document renders identically before and after bootstrap.

---

## Repository Structure

```
grace-docx/
├── grace-docx-bootstrap.md   # The init prompt — drop into any chat with your .docx
├── README.md                 # This document
└── grace/                    # additinal framework data for development purposes
    ├── manifest.xml          # XML templates integrated and not required to run bootstrap 
    ├── graph.xml
    ├── contracts.xml
    ├── instructions.xml
    └── verification.xml
```

---

## Quick Start

1. Download `grace-docx-bootstrap.md`
2. Open Claude (or any capable LLM)
3. Attach the prompt file and your `.docx`
4. Say: `Run bootstrap`
5. Download the returned `.docx` — it's now GRACE-enabled

---

## References

<a name="references"></a>

[1] Bulatov et al. (AIRI / MIPT) — **BABILong: Testing the Limits of LLMs with Long Context Reasoning Tasks**, NeurIPS 2024
https://arxiv.org/abs/2406.10149

[2] Liu et al. (Stanford) — **Lost in the Middle: How Language Models Use Long Contexts**, TACL 2024
https://arxiv.org/abs/2307.03172

[3] Hong, Troynikov, Huber (Chroma) — **Context Rot: How Increasing Input Tokens Impacts LLM Performance**, 2025
https://research.trychroma.com/context-rot

---

## License

MIT

---

## Contributing

Pull requests welcome. Especially interested in:
- Edge cases with non-standard document structures
- Implementations for other formats (`.xlsx`, `.pptx`)
- Integrations with agent frameworks (LangChain, Claude Code, Cursor)

If you run bootstrap on an interesting document and hit a problem — open an issue with the bootstrap report output.
