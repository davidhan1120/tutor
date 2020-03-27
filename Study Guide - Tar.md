## Study Guide: Archiving and Logging Data
 
#### `tar`: Create, extract, compress, and manage tar backup archives.

Creating tar archives is part of your daily job functions. Today, you have decided to expand upon your knowledge of tar by learning how to extract or exclude specific files and directories to help speed up your workflow.

1. Strip the files `investments1.txt` and `Assests_1.txt` into a directory named `~/MyFinancials` from `Tardocs.tar` in  the `/Projects` directory. Extract the files without the directory structure.

    **Solution:**
`Step 1`: We need to first create a folder called `MyFinancials` in order to have files extracted out into that folder.
    ```bash
    $ mkdir MyFinancials
    ```
    - `mk` - make
    - `dir` - directory
 
    `Step 2`: We need to now determine what files are located inside that tar archive. We want to do that without extracting the files so I am going to look inside that tar archive just like opening up the file but without extracting it.
    ```bash
    $ tar -tf TarDocs.tar
    ```
    - -`t` - tree -> This creates a tree like structure that displays all the contents within the zipped file. Lists all the directories and files.
    - `f` - file -> This refers to the file that we want to look at with tar.

    As you can see we run into a problem. It just lists the files for us but we need a way to search for the file that we want to extract. We do not want to extract the entire archive and only want specific files and for this we will use the `grep`. We will pipe `|` this.
    ```bash
    $ TarDocs/Programs/NetBeansProjects/Welcome_2/nbproject/Makefile-variables.mk
    TarDocs/Programs/NetBeansProjects/Welcome_2/nbproject/private/
    TarDocs/Programs/NetBeansProjects/Welcome_2/nbproject/private/Makefile-variables.mk
    TarDocs/Programs/NetBeansProjects/Welcome_2/nbproject/private/launcher.properties
    TarDocs/Programs/NetBeansProjects/Welcome_2/nbproject/private/configurations.xml
    TarDocs/Programs/NetBeansProjects/Welcome_2/nbproject/private/private.xml
    ```
- `grep` is a command-line utility for searching plain-text data sets for lines that match a regular expression <- got that from google. In simple terms, it's a search program kind of like google for your files.
    ```bash
    $ tar -tf TarDocs.tar | grep "whatever you want to search for, do not include quotes"
    ```
    ```bash
    $ tar -tf TarDocs.tar | grep investments
    ```
    We only need `investments1.txt` so we can see that in there.
    **Results**:
    ```bash
    $ TarDocs/Financials/investments1.txt
    TarDocs/Financials/investments3.txt
    TarDocs/Financials/investments2.txt
    ```
    Now that we found where our file is located, let's go ahead and extract this file.
    ```bash
    $ tar -xvf TarDocs.tar -C MyFinancials/ --strip-components=2 "TarDocs/Financials/investments1.txt"
    ```
    `--strip-compontents`- The `--strip` part means to remove the first components of the file name.
    `--components`- Refers to the directory
    
    - `-C` - Change to directory that you want to save files to (I was wrong earlier when this was going to create a directory, my bad.)
    - `dir` - directory
    
    Now do the same for `Assets_1.txt` -> Be careful of the spelling because I messed up on this part. :)
    
2. Uncompress TarDocs.tar into a directory called `TarDocs`. Next, create a `tar` archive called `MyFinancials.tar` that excludes the `Java` directory from the `TarDocs/Document/` directory. 

    `Step 1:` We need to first extract (uncompress) the TarDocs.tar (which I forgot to do during our session, my apologies)
    ```bash
    $ tar -xvf TarDocs.tar
    ```
    - `x` - extract -> extracts the tar archive
    - `v` - verbose -> this just displays the results
    - `f` - file -> this refers to the file that you want to refer to
    
    `Step 2:` We want to extract our all the files in TarDocs **EXCEPT** the files located in `TarDocs/Documents/Java`
    
    ```bash
    $ tar cvf MyFinancials.tar --exclude="TarDocs/Documents/Java" TarDocs/
    ```
    - `c` - creates -> creates the tar archive
    - `v` - verbose -> this just displays the results
    - `f` - file -> this refers to the file that you want to refer to
    - `--exclude` -> We do not want a specific directory to be extracted
    - `TarDocs/` -> I want to put all the files into this folder (Need to create that folder first or else you will an error like I did)

3. **Bonus:** Create an incremental archive called `logs_backup.tar.gz` that contains only changed files by examining the `snapshot.file` for the `/var/log` directory.

    **Solution:**

    ```bash
    $ sudo tar --listed-incremental=snapshot.file -cvzf logs_backup.tar.gz /var/log
    ```
    - `--listed-incremental` -> creates an incremental archive
    - The rest should be self explannatory.
