# Markdown DITA demo

A demo repository for authoring DITA content in Markdown (MDITA) instead of XML.
MDITA is part of the Lightweight DITA (LwDITA) specification and allows technical writers to use familiar Markdown syntax while producing structured DITA output via the DITA Open Toolkit.

## What's in this repo

* `docs/example.mditamap` -- an MDITA map that references 4 example topics
* `mkdocs.yml` -- MkDocs Material configuration for previewing the same Markdown source as a static site

## MDITA conventions used

* YAML front matter with `$schema` to declare topic type (concept, task, reference) per the [DITA-OT Markdown schemas](https://www.dita-ot.org/dev/reference/markdown/markdown-schemas) spec
* Standard MDITA metadata fields: `author`, `source`, `publisher`, `permissions`, `audience`, `category`, `keyword`, `resourceid`
* Short description as the first paragraph after the H1 heading
* Pandoc header attributes for setting topic IDs and output classes, e.g. `{#topic-id .concept}`

## Notes

* The DITA OT [org.lwdita](https://github.com/jelovirt/org.lwdita) plugin has a bug related to transforming markdown into task DITA topics. See https://github.com/jelovirt/org.lwdita/pull/242. This repo includes a hotfix build of the plugin that fixes this issue.

* Mkdocs style admonitions are supported.

    ```markdown
    !!! pied-piper "Pied Piper"
    
        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et
        euismod nulla. Curabitur feugiat, tortor non consequat finibus, justo
        purus auctor massa, nec semper lorem quam in massa.
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