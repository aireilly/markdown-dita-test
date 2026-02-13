# Markdown DITA demo

A demo repository for authoring DITA content in Markdown (MDITA) instead of XML.
MDITA is part of the Lightweight DITA (LwDITA) specification and allows technical writers to use familiar Markdown syntax while producing structured DITA output via the DITA Open Toolkit.

## What's in this repo

* `docs/example.mditamap` -- an MDITA map that references topic files
* `docs/topics/` -- example topics using concept, task, reference, and generic topic schemas
* `mkdocs.yml` -- MkDocs Material configuration for previewing the same Markdown source as a static site

## MDITA conventions used

* YAML front matter with `$schema` to declare topic type (concept, task, reference) per the [DITA-OT Markdown schemas](https://www.dita-ot.org/dev/reference/markdown/markdown-schemas) spec
* Standard MDITA metadata fields: `author`, `source`, `publisher`, `permissions`, `audience`, `category`, `keyword`, `resourceid`
* Short description as the first paragraph after the H1 heading
* Pandoc header attributes for setting topic IDs and output classes, e.g. `{#topic-id .concept}`

## Core vs Extended Profile

MDITA defines two profiles that determine which Markdown features map to DITA elements. The `$schema` value in front matter selects the profile.

### Core Profile

Core Profile covers the basic structural elements that every MDITA document can use:

| Feature | Markdown syntax | DITA output |
|---------|----------------|-------------|
| Headings | `# H1` through `###### H6` | `<topic>`, `<section>` |
| Paragraphs | Plain text blocks | `<p>` |
| Unordered lists | `- item` or `* item` | `<ul><li>` |
| Ordered lists | `1. step` | `<ol><li>` (or `<steps>` in task topics) |
| Bold / Italic | `**bold**`, `_italic_` | `<b>`, `<i>` |
| Inline code | `` `code` `` | `<codeph>` |
| Links | `[text](url)` | `<xref>` |
| Images | `![alt](path)` | `<image>` |
| Code blocks | ` ```lang ` | `<codeblock>` |
| Tables | Pipe tables | `<simpletable>` |

Core Profile schemas:

```yaml
$schema: urn:oasis:names:tc:mdita:core:xsd:topic.xsd
```

### Extended Profile

Extended Profile adds features that map to richer DITA elements. These features are **not valid in Core Profile documents** and will produce linter warnings if used there.

| Feature | Markdown syntax | DITA output |
|---------|----------------|-------------|
| Definition lists | `Term` / `: Definition` | `<dl><dlentry>` |
| Footnotes | `[^label]` / `[^label]: text` | `<fn>` |
| Strikethrough | `~~text~~` | `<line-through>` |
| Generic attributes | `{#id .class key=val}` | `@id`, `@outputclass`, custom attributes |
| Admonitions | `!!! note` | `<note type="note">` |

Extended Profile schemas:

```yaml
$schema: urn:oasis:names:tc:mdita:extended:xsd:topic.xsd
# or the default MDITA topic schema (treated as Extended):
$schema: urn:oasis:names:tc:mdita:xsd:topic.xsd
```

### DITA schemas

Standard DITA schemas (`topic.xsd`, `concept.xsd`, `task.xsd`, `reference.xsd`, `map.xsd`) support all Extended Profile features and add schema-specific structural validation. For example, task topics are expected to contain ordered lists (procedure steps), and reference topics are expected to contain tables or definition lists.

### Admonition types

Admonitions (`!!!`) map to DITA `<note>` elements. The type keyword must be a valid DITA note type:

`note`, `tip`, `warning`, `caution`, `danger`, `attention`, `important`, `notice`, `fastpath`, `remember`, `restriction`, `trouble`

MkDocs types like `info`, `success`, `example`, and `bug` are not valid DITA note types and will produce linter warnings.

### Topics in this repo by profile

| File | Schema | Profile | Extended features used |
|------|--------|---------|----------------------|
| `concept-metrics-prereqs.md` | concept | Core | -- |
| `concept-tool-calling.md` | concept | Core | -- |
| `concept-validating-metrics.md` | concept | Core | -- |
| `task-validate-metrics.md` | task | Core | -- |
| `reference-server-arguments.md` | reference | Extended | Tables |
| `kitchen-sink.md` | topic | Extended | Definition lists, strikethrough, admonitions, tables, header attributes |

## Notes

* The DITA OT [org.lwdita](https://github.com/jelovirt/org.lwdita) plugin has a bug related to transforming markdown into task DITA topics. See https://github.com/jelovirt/org.lwdita/pull/242. This repo includes a hotfix build of the plugin that fixes this issue.

* Admonitions use MkDocs syntax. When building with DITA-OT, use DITA-compatible types (`note`, `caution`, `warning`, etc.) rather than MkDocs-only types (`info`, `success`).

    ```markdown
    !!! warning "Connection timeout"

        Ensure the server is reachable before running the validation.
    ```

* The mkdocs site in this demo is built from the markdown source in the `docs/` folder.

* The GitHub release zip contains a direct normalized DITA build of the md source.

## Building

Build HTML5 output with the DITA Open Toolkit:

```cmd
dita -i docs/example.mditamap -f html5 -o out
```

Preview with MkDocs Material:

```cmd
mkdocs serve
```

## TODO

Create Vale rules to lint broken md syntax that will break the DITA build.

* Unclosed [TEXT](URL)
* Unclosed header attr blocks, for example:

    ```cmd
    # Generating a key pair for cluster node SSH access  {#this-is-the-id .this-is-an-output-class
    ```

* Broken relative image paths (e.g. `images/test.jpg` when the image is in a sibling directory, should be `../images/test.jpg`)
* Doubled relative link paths (e.g. `topics/concept-tool-calling.md` from a file already inside `topics/`, resolving to `topics/topics/concept-tool-calling.md`)
* Ensure indents are correct for task step elements
* Absolute paths that don't resolve in DITA-OT (e.g. `/docs/topics/concept-tool-calling.md`)
* Unknown code block language `terminal` (use `bash` or `console` instead for shell commands)