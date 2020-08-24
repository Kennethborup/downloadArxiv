# Download ArXiv paper with title
Download Arxiv paper directly from the abstract page and infer the title of the paper. Add the following script to your `~/.bash_profile` or `~/.bashrc` (depending on your setting), where `###` is replaced by the folder, where the papers should be downloaded to:

```bash
# Download paper with title into folder
function paperdownload {
    currentDIR=$(pwd)
    cd ###

    if [[ $1 =~ ".pdf" ]]; then
        filename=$2
        echo "Saves paper with filename: $filename"
        curl -o "$filename.pdf" $1
    else
        filename=$(curl --silent $1 | grep -E "<h1.*><span.*>Title:<\/span>(.*?)<\/h1>" | sed 's/.*<h1.*>\(.*\)<\/h1>.*/\1/')
        echo "Saves paper with filename: $filename"
        website="${1/abs/pdf}.pdf"
        curl -o "$filename.pdf" $website
    fi

    cd $currentDIR
}
```

Simply use the function as
```bash
paperdownload https://arxiv.org/abs/2004.01411
```
or directly on the pdf with custom filename
```bash
paperdownload https://arxiv.org/pdf/2004.01411.pdf "Targeting predictors in random forest regression"
```

Remember to `source` `~/.bash_profile` or `~/.bashrc` before first use.
