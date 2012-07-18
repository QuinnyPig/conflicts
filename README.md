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

# A note about branch stability

To keep the history of this repository simple and easy-to-follow, I regularly
push history-altering changes. Do not rely on being able to pull and merge
from this repository; if you're starting a new set of experiments, clone a new
copy of this repository. I do try to keep the history of `master` intact, so
that pull requests make sense, but the conflict branches get reset and rebased
as needed.
