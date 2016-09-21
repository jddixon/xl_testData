# xl_test_data

## Contents

This project contains data used for testing software developed as part of
the
[XLattice](https://jddixon.github.io/xlattice)
project.  Project software has been developed in a number of programming
languages.
To test interoperability it is convenient to have a common set of test
data in a public place: this project.

Derivative data structures to be included are

*   [NLHTrees](https://jddixon.github.io/nlhtree_py)
*   [BuildLists](https://jddixon.github.io/buildList)
*   [MerkleTrees](https://jddixon.github.io/merkletree)

These will shortly be supplemented by
    [SHA test vectors]()
    [Chunks](https://jddixon.github.io/xlattice/chunks.html)


** Directory Structure

The key material here is

* a directory tree `dataDir`
* an RSA key used for making digital signatures
* and then a number of subdirectories containing data structures derived
    from `dataDir` using project software.

`dataDir` consists of a
number of data files, at least one of which is empty, and a number of
subdirectories, at least one of which is empty.  Where files are populated,
the file consists of a random number of random bytes.

    xl_test_data/
        README.md                   # this file
        treeData
            binExample.1
                node
                    skPriv          # PEM serialization of private RSA key
                dataDir/
                    data1
                    subDir1/
                        data11
                        data12      # empty
                    data2
                    subDir2/
                    subDir3/
                        data31
                    subDir4/
                        subDir41
                            subDir411
                                data4111
                nlhTree
                    {1,2,3}         # three subdirectories: 1/, 2/, 3/
                        example.nlh # serialization of NLHTree
                        uDir/
                            00/
                            ...
                            ff/
                            in/
                            tmp/
                buildList
                    {1,2,3}
                        example.bld     # serialized BuildList
                        hex             # BuildList.hash()
                merkleTree
                    {1,2,3}
                        example.tree    # serialized
                        hex             # returned by merkleize -x


`skPriv` is the RSA private key used to sign the `example.bld`
build list.

The information under `dataDir/` is a small directory tree.
The `data*` are data files containing quasi-random data.  Both the
file length and the contents are random.  `data12` is an empty file.
Subdirectory `subDir2` is an empty subdirectory.

`example.bld` is a build list for `dataDir`.  The build list contains
the public part of the RSA key used to sign the list, its title,
and a UTC timestamp, the time at which the list was signed.  The
body of the list is an indented list of the files under `dataDir/`,
with a line for each file, each line containing the SHA content
hash of the document and its title.  The build list ends with a
digital signature over the earlier part of the document.  In this
example, the RSA private key used in signing the document in
contained in `node/skPriv`.

`nlhTree/{1,2,3}/uDir/` contains the same set of files as under `dataDir/`,
key, by the `SHA{1,2,3}` hash of the file, where `SHA2` means SHA256
and `SHA3` means SHA3-256, the 256-bit version of **Keccak**.

The notation '{1,2,3}' is an abbreviation for "each of the sequence
of values 1, 2, and 3, taken in turn."

The data under `binExample.1/` is sufficient to verify the correctness of
the build list and the 1-to-1 relationship between the files under
`binExample.1/datadir/` and those under `binExample.1/nlhTree/{1,2,3}/uDir/`

For MerkleTrees, there are three serializations, indented lists
created using `SHA1`, `SHA2`, and `SHA3` under `1/`, `2/`, and `3/`
respectively; and also the value returned by `merkleize -x`, the hash value
for the entire MerkleTree.

## Project Status

A skeletal repository as yet containing no test data.  It is organized as
a Python project to ease project management.

## On-line Documentation

More information on the **xl_test_data** project can be found
[here](https://jddixon.github.io/xl_test_data)
