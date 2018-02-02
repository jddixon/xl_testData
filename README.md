# xl_testData

## Contents

This project contains data used for testing software developed as part of
the
[XLattice](https://jddixon.github.io/xlattice)
project.  Project software has been developed in a number of programming
languages.
To test interoperability it is convenient to have a common set of test
data in a public place: this project.

Derivative data structures to be included are

* [NLHTrees](https://jddixon.github.io/nlhtree_py)
* [BuildLists](https://jddixon.github.io/buildlist)
* [MerkleTrees](https://jddixon.github.io/merkletree)

These will shortly be supplemented by

* SHA test vectors
* [Chunks](https://jddixon.github.io/xlattice/chunks.html)

Much of this data was derived using variants of the
[Secure Hash Algorithm](https://en.wikipedia.org/wiki/Secure Hash Algorithm),
a family of functions each of which can producing a cryptographically secure
number of predetermined length from a document of arbitrary length.
These hash functions are *secure* in the sense that they are irreversible:
it is for all practical purposes impossible to identify which document a
document corresponds to without doing a systematic serach of the candidates.

In other wrods, an SHA hash is a fixed-length identifier guaranteed to be
unique to a document.  We use three SHA functions: SHA1, which produces
160-bit values; SHA2 (aka sha-256), which produces 256-bit values; and
the 256-bit variant of SHA3 (Keccak).

## Directory Structure

The contents include

* an RSA key used for making digital signatures, `node/skPriv`.
* a directory tree `dataDir`
* and then a number of subdirectories containing data structures derived
    from `dataDir` using project software.

The data under `binExample.1/` is sufficient to verify the correctness of
the BuildList and the 1-to-1 relationship between the files under
`binExample.1/datadir/` and those under `binExample.1/nlhTree/{1,2,3}/uDir/`

The notation '{1,2,3}' is an abbreviation for "each of the sequence
of values 1, 2, and 3, taken in turn."

    xl_testData/
        README.md                   # this file
        treeData/
            binExample.1/
                node/
                    skPriv          # PEM serialization of private RSA key
                dataDir/
                    data1
                    subDir1/
                        data11
                        data12      # empty file
                    data2
                    subDir2/        # empty directory
                    subDir3/
                        data31
                    subDir4/
                        subDir41/
                            subDir411/
                                data4111
                derived/
                    sha{1,2,3}/         # subdirectories: sha1/,sha2/,sha3/
                        example.nlh     # serialization of NLHTree
                        example.bld     # serialized BuildList
                        buildlist.hex   # BuildList.hash()
                        example.merkle  # serialized merkletree
                        merkle.hex      # returned by merkleize -x
                        DIR{_FLAT,16x16,256x256}/
                            uDir/
                                ...
                                in/
                                tmp/


### node

`skPriv` is the RSA private key used to sign the `example.bld`
build list.

### dataDir

As shown in the listing above,
`dataDir` consists of a
number of data files, at least one of which is empty, and a number of
subdirectories, at least one of which is empty.  Where files are populated,
the file consists of a random number of random bytes.

The information under `dataDir/` is a small directory tree.
The `data*` are data files containing quasi-random data.  Both the
file length and the contents are random.  `data12` is an empty file.
Subdirectory `subDir2` is an empty subdirectory.

Files below `node/` and `dataDir` are immutable in the sense that the
utility used to generate this data, `bl_createtestdata`, will not 
overwrite any of these files unless the `-f/--force` option is specified.
Additional `dataDir` files may be added in future, however.

On the other hand, the files below `derived` are regenerated during each 
`bl_createtestdata1` run.

### sha subdirectories

The information in these subdirectories is derived from that under
**dataDir** using one of the three Secure Hash Algorithm (SHA) types
supported, SHA1, SHA2, and SHA3, where `SHA2` means SHA256
and `SHA3` means SHA3-256, the 256-bit version of **Keccak**.

#### example.nlh

This is the serialization of the NLHTree describing the data files
under `dataDir`.

#### example.bld and buildlist.hex

`example.bld` is a BuildList for `dataDir`.  The BuildList contains
the public part of the RSA key used to sign the list, its title,
and a UTC timestamp, the time at which the list was signed.  The
body of the list is an indented list of the files under `dataDir/`,
with a line for each file, each line containing the SHA content
hash of the document and its title.  The BuildList ends with a
digital signature over the earlier part of the document.  In this
example, the RSA private key used in signing the document in
contained in `node/skPriv`.

`buildlist.hex` is the BuildList hash, a 40-character hexadecimal
value unique to the BuildList.

#### example.merkle and merkle.hex

The MerkleTree is another data structure useful for describing
directory structures.  `example.merkle` is the serialization of
the MerkleTree for `dataDir/`.  `merkle.hex` is the single value
specifying the whole tree in hexadecimal form.

#### DIR_FLAT, DIR16x16, DIR256x256

These are three different ways of organizing content-keyed directories.

In the first, `DIR_FLAG`, all files are collected in one directory.
This is suitable for small collections of data.

In the second, `DIR16x16`, there are 16 subdirectories `0/` through `f/'
and below each of these 16 further subdirectories, each named with a
hexadecimal digit.  This gives us a total of 256 subdirectories, each
of which contains data files whose content keys begin with the
corresponding two hex digits.

Under each of the three structural headings there is a content keyed
data store, `uDir`.  Each of the nine different versions of `uDir`
contains the same files as `dataDir` named by the corresponding
content key.

## Project Status

Includes test data as described above.  It is organized as a Python 
project to ease project management.


## On-line Documentation

More information on the **xl_testData** project can be found
[here](https://jddixon.github.io/xl_testData)
