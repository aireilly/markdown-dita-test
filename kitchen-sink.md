---
author:
  - Author One
  - Author Two
source: Source
publisher: Publisher
permissions: Permissions
audience: Audience
category: Category
keyword:
  - Keyword1
  - Keyword2
resourceid:
  - Resourceid1
  - Resourceid2
workflow: review
---

# Markdown DITA kitchen sink

Markdown DITA supports [ventilated prose](https://asciidoctor.org/docs/asciidoc-recommended-practices/#one-sentence-per-line), for example:

First sentence.
Second sentence.
One paragraph.

Third sentence. Fourth sentence. One paragraph.

Discrete paragraph.

Second discrete paragraph.

## Inline

**bold**
_italic_
`code`
~~strike-through~~

<!-- Use HTML comments -->

## Pandoc header attributes

Use this:

`# Topic title {#carrot .juice audience=novice}`

Get this:

```xml
<topic id="carrot" outputclass="juice" audience="novice">
  <title>Topic title</title>
```

## Definition lists

Term
: Definition.

## Codeblocks

Codeblocks can pass the code format which is useful for syntax highlighting.

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: openshift-storage
  labels:
    workload.openshift.io/allowed: "management"
    openshift.io/cluster-monitoring: "true"
  annotations: {}
```

## Lists

ul can be `*` or `-`.

- one
- two
  - three
  - four

ol:

1. one
2. two
3. one

nested ol (three spaces):

1. Well
2. Hello
   1. There
   2. Little fellow

## Definition lists

Term
: Definition

## Images

An inline ![Alt](test.jpg).

![Alt](test.jpg)

![Alt](test.jpg "Title")

## Admonitions

!!! note

    Note content.

!!! info

    Info content.

!!! caution

    Caution content.

!!! warning

    Warning content.

# Links

* [Test](test.md)

* [External](http://www.example.com/test.html)

* See this [in page reference in the current document](#definition-lists).
Where no ID exists, the link ID is rendered in kebab-case.

* See this local xref: [Link destination](test2.md#link-destination).

## Tables

| First Header | Second Header | Third Header |
| ------------ | :-----------: | -----------: |
| Content      |  _Long Cell_  |              |
| Content      |   **Cell**    |         Cell |
