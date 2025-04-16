# Markdown DITA demo

Experimenting with markdown DITA. To build output:

```cmd
dita -i example.ditamap -f html5 -o out
```
or

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