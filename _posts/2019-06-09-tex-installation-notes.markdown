---
layout: post
title: "TeX Installation Notes"
date: 2019-06-09
tags: [latex, tex, xelatex, linux]
---

This post outlines the steps I took to install TeX Live 2019 on my Arch-based PC. For my TeX editor, I use Visual Studio Code. My configuration for Visual Studio code will be covered in a later post. 

#### Preliminary Steps
Install perl, tk, and perl-tk packages in Arch. The perl-tk package is needed for the GUI-based installation.
```
sudo pacman -S perl tk perl-tk
```

---

#### [TeX Live 2019](https://www.tug.org/texlive/) Installation Steps
1. Create directory for holding installation files:

    ```bash
    mkdir texlive-2019
    cd texlive-2019/
    wget http://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz
    tar -xzvf install-tl-unx.tar.gz
    cd install-tl-20190608/
    ````
2. Start the installer. The install options and collections.

    ```bash
    sudo ./install-tl -gui
    ```
    <!-- Install options and collections: 
    ![alt text](/static/img/tex-live-1.png "Install options")
    ![alt text](/static/img/tex-live-2.png "Collections") -->

3. After installation, add the right paths (ensure these paths exist) to your PATH, MANPATH, and INFOPATH. 

    ```bash
    export PATH="/usr/local/texlive/2019/bin/x86_64-linux:$PATH"
    export MANPATH="/usr/local/texlive/2019/texmf-dist/doc/man:$MANPATH"
    export INFOPATH="/usr/local/texlive/2019/texmf-dist/doc/info:$INFOPATH"
    ```

4. The tlmgr GUI can be started with:
    ```bash
    sudo tlmgr -gui
    ```

---

#### TEXMFHOME

TeX programs will search through subdirectories in the `TEXMFHOME` directory during compilation. Check the location of the`TEXMFHOME` directory: 
```bash
kpsewhich -var-value=TEXMFHOME
```

Subdirectories in `TEXMFHOME` must follow the same subdirectory structure as seen in other `TEXMF` directories. A sample subdirectory structure can be seen by listing the contents of the `TEXMFLOCAL` directory (found via `kpsewhich -var-value=TEXMFLOCAL`) : 
```bash
ls /usr/local/texlive/texmf-local
```

To use `*.tex, *.sty` files and local packages from `TEXMFHOME`, create a `tex/latex` subfolder in `TEXMFHOME`. You can create separate directories to organize your files here as well.
```bash
mkdir -p /home/siam/texmf/tex/latex
```

To use custom BibTex style files (`*.bst`), create a `bibtex/bst` subfolder.
```bash
mkdir -p /home/siam/texmf/bibtex/bst/
```
Recent TeX Live distributions no longer require you to run `texhash` or `mktexlsr` after adding and/or modifying files in `TEXMFHOME`. In the past, a database (flat text file) of `TEXMFHOME` files, named `ls-R`, in `TEXMFHOME` would be updated will a listing of all files in `TEXMFHOME`. 

To verify if a file is being correct read from `TEXMFHOME`, you can use:

```bash
kpsewhich filename.sty
```
