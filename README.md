# Kontrolle mit Hash?
## Deep Dive in git!

Neben den Slides meines Vortrags auf der enterJS 2016 sind hier auch die genuzten Befehle der Session nochmal aufgelistet.

Viele der Befehle gehören zu den sogenannten "Plumbing-Commands" die im täglichen Gebrauch weniger gebräuchlich sind. Aber das Verständnis von git erhöhen können.

    # start with a simple command
    $ mkdir .git
    
    # create the objects directory (all objects stored here)
    $ mkdir .git/objects
    
    # refs stores the tag and branches
    $ mkdir .git/refs
    
    # create the directory for branch information
    $ mkdir .git/refs/heads
    
    # tell git on which the branch the repos is
    $ echo "ref: refs/heads/MyMaster"

Ein `git status` erkennt nun bereits an der erstellten Struktur ein git Repository. Weiter geht es mit dem Befüllen des Repositories.

    # create a blob object
    $ echo "Hello enterJS!" | git hash-object -w --stdin
    9c549bff452686ca42e444962efccdc54c5c54df
    
    # a blob is now written to the object directory, we use the hash of blob to get some information.
    # start with the type of object
    $ git cat-file -t 9c549bff452686ca42e444962efccdc54c5c54df
    blob
    
    # get the size of the object
    $ git cat-file -s 9c549bff452686ca42e444962efccdc54c5c54df
    15
    
    # get the content of the blob in a 'pretty print' format
    $ git cat-file -p 9c549bff452686ca42e444962efccdc54c5c54df
    Hello enterJS!
    
    # now assing a filename to the blob via the staging area
    $ git update-index --add --cacheinfo 100644,9c549bff452686ca42e444962efccdc54c5c54df,readme.txt
    
    # write the tree object with the content of the staging area
    $ git write-tree
    f4fc37f5c58a930e446f692daa2adabb6c38f8b4
    
    # show the tree object
    $ git cat-file -p f4fc37f5c58a930e446f692daa2adabb6c38f8b4
    100644 blob 9c549bff452686ca42e444962efccdc54c5c54df    readme.txt
    
    # new create the commit with the reference to the tree object
    $ commit-tree f4fc37f5c58a930e446f692daa2adabb6c38f8b4 -m "My first commit"
    3a2b89cefb9082732af564435249c6fa613728f9
    
    # now let the branch point to this commit
    echo 3a2b89cefb9082732af564435249c6fa613728f9 > .git/refs/heads/MyMaster
