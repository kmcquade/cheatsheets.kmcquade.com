# Heredoc

## Heredoc <a href="heredoc" id="heredoc"></a>

From here: [https://stackoverflow.com/questions/2953081/how-can-i-write-a-heredoc-to-a-file-in-bash-script/25903579#answer-25903579](https://stackoverflow.com/questions/2953081/how-can-i-write-a-heredoc-to-a-file-in-bash-script/25903579#answer-25903579)

Note:

* the following condenses and organizes other answers in this thread, esp the excellent work of [Stefan Lasiewski](https://stackoverflow.com/users/110223/stefan-lasiewski) and [Serge Stroobandt](https://stackoverflow.com/users/2192488/serge-stroobandt)
* Lasiewski and I recommend [Ch 19 (Here Documents) in the Advanced Bash-Scripting Guide](http://tldp.org/LDP/abs/html/here-docs.html)

The question (how to write a here document (aka heredoc) to a file in a bash script?) has (at least) 3 main independent dimensions or subquestions:

1. Do you want to overwrite an existing file, append to an existing file, or write to a new file?
2. Does your user or another user (e.g., root) own the file?
3. Do you want to write the contents of your heredoc literally, or to have bash interpret variable references inside your heredoc?

(There are other dimensions/subquestions which I don’t consider important. Consider editing this answer to add them!) Here are some of the more important combinations of the dimensions of the question listed above, with various different delimiting identifiers–there’s nothing sacred about EOF, just make sure that the string you use as your delimiting identifier does not occur inside your heredoc:

1.  To overwrite an existing file (or write to a new file) that you own, substituting variable references inside the heredoc:

    ```
    cat << EOF > /path/to/your/file
    This line will write to the file.
    ${THIS} will also write to the file, with the variable contents substituted.
    EOF
    ```
2.  To append an existing file (or write to a new file) that you own, substituting variable references inside the heredoc:

    ```
    cat << FOE >> /path/to/your/file
    This line will write to the file.
    ${THIS} will also write to the file, with the variable contents substituted.
    FOE
    ```
3.  To overwrite an existing file (or write to a new file) that you own, with the literal contents of the heredoc:

    ```
    cat << 'END_OF_FILE' > /path/to/your/file
    This line will write to the file.
    ${THIS} will also write to the file, without the variable contents substituted.
    END_OF_FILE
    ```
4.  To append an existing file (or write to a new file) that you own, with the literal contents of the heredoc:

    ```
    cat << 'eof' >> /path/to/your/file
    This line will write to the file.
    ${THIS} will also write to the file, without the variable contents substituted.
    eof
    ```
5.  To overwrite an existing file (or write to a new file) owned by root, substituting variable references inside the heredoc:

    ```
    cat << until_it_ends | sudo tee /path/to/your/file
    This line will write to the file.
    ${THIS} will also write to the file, with the variable contents substituted.
    until_it_ends
    ```
6.  To append an existing file (or write to a new file) owned by user=foo, with the literal contents of the heredoc:

    ```
    cat << 'Screw_you_Foo' | sudo -u foo tee -a /path/to/your/file
    This line will write to the file.
    ${THIS} will also write to the file, without the variable contents substituted.
    Screw_you_Foo
    ```

###  <a href="install-python-version-with-pyenv-on-mac" id="install-python-version-with-pyenv-on-mac"></a>
