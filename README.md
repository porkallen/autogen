# autogen

[![Build Status](https://travis-ci.org/mbrukman/autogen.svg?branch=master)](https://travis-ci.org/mbrukman/autogen)

Automatically generate boilerplate comments and code for new files with a single
command.

## Install

```bash
cd ~/bin
git clone git@github.com:mbrukman/autogen.git
echo 'alias autogen=~/bin/autogen/autogen' >> ~/.bash_profile
source ~/.bash_profile
```

## Editor support

For details on adding Autogen support to your editor, please see
the [`editors`](editors) directory.

## Usage

```bash
autogen -c [copyright holder] -l [license] [filename]
```

Modify an existing file in-place:

```bash
autogen -i [...other params as above...]
```

Bulk update many existing files in-place:

```bash
find . -type f \( -name "*.cpp" -o -name "*.c" -o -name "*.h" \) -exec autogen -i --no-top-level-comment [options...] {} \;
```

The flag `--no-top-level-comment` avoids adding a comment such as:

```css
/* TODO: High-level file comment */
```

because either some or all of the files have such a top-level comment, or if
they don't, you probably wouldn't want to have to manually add or delete at this
time, if you're just trying to add the license header in bulk.

To get a list of supported licenses, or to see the full set of flags, run
`autogen` with no parameters.

File type or language is determined based on the full filename or extension, as
appropriate. See [`autogen`](autogen) for a list of recognized file types.

The flag `--no-code` skips adding boilerplate code to source files.

The flag `--no-runline` skips adding runline to source files.

Sample outputs:

* [Apache 2.0, Haskell](tests/testdata/apache-acme-hs.out)
* [3-clause BSD, Erlang](tests/testdata/bsd3-acme-erl.out)
* [GPL 2, Ruby](tests/testdata/gpl2-acme-rb.out)
* [LGPL 2.1, C++](tests/testdata/lgpl2.1-acme-cpp.out)
* [MIT, Makefile](tests/testdata/mit-acme-makefile.out)

## Developing

To add a new file type or feature, change [`autogen`](autogen) and add
several files to the [`tests/testdata`](tests/testdata) directory, namely:

* `<feature>.in` - the input file containing command-line args to pass
  to `autogen`
* `<feature>.out` - expected stdout for the test
* `<feature>.err` - expected stderr for the test

To generate the `*.out` and `*.err` files automatically, just add the `*.in`
files and run `make regen`. Then, examine the resulting `*.out` and `*.err`
files.

Other custom tests can be added as separate scripts in the [`tests`](tests)
directory. If the file has the suffix `_test.sh`, it will be automatically
picked up by [`tests/run_all_tests.sh`](tests/run_all_tests.sh) script, which
means that `make test` will automatically run it without any other changes.

Be sure to also add an entry for it in [`tests/BUILD`](tests/BUILD) file for
Bazel to be able to run it as well, including appropriate dependencies on any
data files it may need.

## Testing

You have two options:

* via [Bazel](http://bazel.io/): `bazel test //...`
* via Make: `make test`

Bazel is typically faster, especially when rerunning tests, due to built-in
caching.

## Related work

* [addlicense](https://github.com/google/addlicense)
* [License Up](https://github.com/nikitavoloboev/license-up)

## Contributing

See [`CONTRIBUTING.md`](CONTRIBUTING.md) for details.

## License

Apache 2.0; see [`LICENSE.txt`](LICENSE.txt) for details.

## Disclaimer

This project is not an official Google project. It is not supported by Google
and Google specifically disclaims all warranties as to its quality,
merchantability, or fitness for a particular purpose.
