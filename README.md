Hi folks.

This tree has some branches with simple, well-known edit conflicts in it. To
experiment with a given conflict, run the commands outlined in that section.

# Baseline state

This repository's baseline state contains a simple grocery list. This is a
text file with one line per item and a relatively small number of items - but
sufficient to establish context around diffs.

# Simple edit conflict

The `edit-conflict/left` and `edit-conflict/right` branches contain edits that
both change the same line of the grocery list. The left branch changes the
line "- Ham" for "- Bacon" and the right branch changes the line "- Ham" for
"- Beef".

As both branches alter the same line, you can't merge both without generating
a conflict for that line:

    $ git checkout master
    $ git merge edit-conflict/left
    $ git merge edit-conflict/right
    Auto-merging groceries.txt
    CONFLICT (content): Merge conflict in groceries.txt
    Automatic merge failed; fix conflicts and then commit the result.

This is a good way to experiment with manual resolution, `rerere`, merge
tools, and other approaches to resolving simple conflicts.

If you get really stuck, run `git reset --hard origin/master` to throw away
your work and start back from a clean version of the master branch.

# Append conflict

The `append-conflict/left` and `append-conflict/right` branches contain a
common variation on edit conflicts, where the changes on both branches are at
the end of the file (and there's no trailing context). The left branch appends
the line '- Toys for Bob' and the right branch appends the line '- Cookies'.

The process for testing this conflict is as above.

# Edit-move conflict

Edit-move conflicts are a kind of "tree" conflict, where branches make
incompatible changes to the structure of the project rather than to the
contents of files. The `edit-move/left` branch changes the line "- Ham" for "-
Bacon" as above, while the right branch changes the name of the file from
"groceries.txt" to "grocery-list.txt".

Git will automatically resolve this kind of conflict:

    $ git checkout master -b t
    $ git merge edit-move/left
    $ git merge edit-move/right
    Merge made by recursive.
     groceries.txt => grocery-list.txt |    0
     1 files changed, 0 insertions(+), 0 deletions(-)
     rename groceries.txt => grocery-list.txt (100%)

Note that after this merge, grocery-list.txt still contains the line "- Bacon"
from the left branch.

# Move-move conflict

Move-move conflicts are a second kind of tree conflict. In this case, both
branches make changes to the structure of the project in conflicting ways. The
`move-move/left` branch renames "groceries.txt" to "shopping.txt", while the
`move-move/right` branch renames "groceries.txt" to "grocery-list.txt". When
you merge them, Git does something interesting:

    $ git checkout master
    $ git merge move-move/left
    Fast-forward
     groceries.txt => shopping.txt |    0
     1 files changed, 0 insertions(+), 0 deletions(-)
     rename groceries.txt => shopping.txt (100%)
    $ git merge move-move/right
    error: refusing to lose untracked file at 'grocery-list.txt'
    error: refusing to lose untracked file at 'grocery-list.txt'
    CONFLICT (rename/rename): Rename "groceries.txt"->"shopping.txt" in branch "HEAD" rename "groceries.txt"->"grocery-list.txt" in "move-move/right"
    CONFLICT (rename/rename): Rename "groceries.txt"->"shopping.txt" in branch "HEAD" rename "groceries.txt"->"grocery-list.txt" in "move-move/right"
    Automatic merge failed; fix conflicts and then commit the result.
    
    $ git status
    # On branch t
    # Unmerged paths:
    #   (use "git add/rm <file>..." as appropriate to mark resolution)
    #
    #	both deleted:       groceries.txt
    #	added by them:      grocery-list.txt
    #	added by us:        shopping.txt
    #
    no changes added to commit (use "git add" and/or "git commit -a")

Unlike the edit-conflict case, there are no conflict markers in the individual
files. You'll need to decide which of the two result files to keep, `git add`
that one, and `git rm` the other one (or opt to keep both using `git add`)
before you can `git commit` the merge.

# A note about branch stability

To keep the history of this repository simple and easy-to-follow, I regularly
push history-altering changes. Do not rely on being able to pull and merge
from this repository; if you're starting a new set of experiments, clone a new
copy of this repository. I do try to keep the history of `master` intact, so
that pull requests make sense, but the conflict branches get reset and rebased
as needed.
