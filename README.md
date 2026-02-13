# Markdown DITA demo

Experimenting with markdown DITA. To build output:

```cmd
dita -i example.mditamap -f html5 -o out
```

## TODO

Vale rules for borked md syntax

* Unclosed [TEXT](URL)
* Unclosed header attr blocks, for example:

    ```cmd
    # Generating a key pair for cluster node SSH access  {#this-is-the-id .this-is-an-output-class
    ```

* Broken relative image paths (e.g. `images/test.jpg` when the image is in a sibling directory, should be `../images/test.jpg`)
* Doubled relative link paths (e.g. `topics/concept-tool-calling.md` from a file already inside `topics/`, resolving to `topics/topics/concept-tool-calling.md`)
* Absolute paths that don't resolve in DITA-OT (e.g. `/docs/topics/concept-tool-calling.md`)
* Unknown code block language `terminal` (use `bash` or `console` instead for shell commands)