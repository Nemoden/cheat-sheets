find (Need help here! Feel free to contribute)
===

Common use-cases
---

Find all files that are older than a week ago

    todo

Find all files that are newer than a week ago

    todo

Find all `.xml` files in `/path/to/dir` and `grep` for a term and output found line only

    find /path/to/dir -name "*.xml" -exec grep -i term {} \;

Find all `.xml` files in `/path/to/dir` and `grep` for a term and output file name, line number and line itself

    find /path/to/dir -name "*.xml" -exec grep -in term {} +

Find all `.xml` files in `/path/to/dir` and `grep` for a term and output files in which `term` occures

    find /path/to/dir -name "*.xml" -exec grep -ils term {} +
