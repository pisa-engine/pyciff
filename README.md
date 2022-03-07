PyCIFF
======

This package provides a python interface to [PISA's Common Index File Format Import/Export toolset](https://github.com/pisa-engine/ciff), which is written in Rust.

Usage
-----

Converting CIFF to PISA:
```
pyciff.ciff_to_pisa(input_file, output, generate_lexicons)
```
- `input_file` is the input CIFF file.
- `output` is the *basename* of the output PISA canonical files.
- `generate_lexicons` is a Boolean flag; if True, the `.termlex` and `.doclex` files will be created.

Example (using the toy CIFF file stored in this repo):
```
$> cd tests

$> python -c "import pyciff; pyciff.ciff_to_pisa('toy-complete-20200309.ciff', 'my-pisa-files', False)"

----- CIFF HEADER -----
Version: 1
No. Postings Lists: 9
Total Postings Lists: 9
No. Documents: 3
Total Documents: 3
Total Terms in Collection 16
Average Document Length: 5.333333333333333
Description: Export of toy 3-document collection from Anserini's io.anserini.integration.TrecEndToEndTest test case
-----------------------
Processing postings
  [00:00:00] [========================================] / (0s)
Processing document lengths
  [00:00:00] [========================================] / (0s)

$> ls my-pisa-files.*
my-pisa-files.docs  my-pisa-files.documents  my-pisa-files.freqs  my-pisa-files.sizes  my-pisa-files.terms
```

Converting PISA to CIFF:
```
 pyciff.pisa_to_ciff(collection_input, terms_input, titles_input, output, description)
```
 - `collection_input` is the *basename* of the (canonical PISA) collection.
 - `terms_input` is a newline delimited file containing a single term per line (the first line is the 0-th postings list).
 - `titles_input` is a newline delimited file containing a single document identifier per line (the first line is the 0-th document identifier).
 - `output` is the name of the CIFF file to output.
 - `description` is stored inside the CIFF blob, and can be used to describe the collection/parsing/etc.

Example using the example files created previously:
```
# Still working in `tests` directory

$> python3 -c "import pyciff; pyciff.pisa_to_ciff('my-pisa-files', 'my-pisa-files.terms', 'my-pisa-files.documents', 'my-new.ciff', 'My example description')"

Collecting posting lists statistics
  [00:00:00] [========================================] / (0s)
Computing average document length
  [00:00:00] [========================================] / (0s)
Writing postings
  [00:00:00] [========================================] / (0s)

$> ls my-new.ciff
my-new.ciff

```


Deployment
-----------

To upload to Pypi:

```shell
docker run --rm -v (pwd):/io konstin2/maturin publish -r https://test.pypi.org/legacy/ -u USER -p PASSWORD
```

To upload to Test Pypi:

```shell
docker run --rm -v (pwd):/io konstin2/maturin publish -u USER -p PASSWORD
```
