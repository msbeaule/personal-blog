## cloning into new folder / device

Clone it.

Make a new testing.sh with the current IP address.

Pull down the theme with:

`git submodule update --init --recursive`

The `update` gets the most recent version of the theme, I think it can be excluded?

## run dev server

```
hugo server -D
```

Or run the `.bat` file to test it on other devices on the network.

## test for broken links

```
linkcheck localhost:1313 --skip-file linkcheck-skip.txt
```


## Shortcodes

accordion:

```
{{< details "Summary text here" >}}
text inside the accordion
{{< /details >}}
```

notices:

```
{{< notice info >}} This is a info notice, grey.{{< /notice >}}

{{< notice note >}} This is a note notice, blue.{{< /notice >}}

{{< notice tip >}} This is a tip notice, green.{{< /notice >}}

{{< notice warning >}} This is a warning notice, yellow.{{< /notice >}}

{{< notice danger >}} This is a danger notice, red.{{< /notice >}}
```
