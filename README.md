# diff_test
A sweet and simple way to test output of a program vs a reference implementation.

### testing .toml file
In order to run tests, you need a `.toml` file in your working directory. `.toml` \[https://github.com/toml-lang/toml\](tom’s obvious, minimal language), is a simple configuration file which we will use to organize our tests.

Our `.toml` testing file will have two main sections: `[config]`, and `[tests]`.

### \[config\]

The `[config]` section must contain the following required arguments:

```
[config]
exec         = "a.out"     # path of the executable file to run
ref_exec     = "ref_a.out" # path of referece executable
build_target = "build"     # command to 'make' -> here, `make build` will be run
```

Of course, you should change the values of the variables above to reflect what’s going on in your work; however, the variable names (‘exec’, etc.) must not change.

### \[tests\]

The `[tests]` section must contain one sub-section per test as follows:

```
[tests.my_first_test]
...
[tests.my_second_test]
```

All variables for a given test are optional. The variables are:

```
[test.example_test]
argv          = ["arg1", "arg2"...] # list of arguments to your program
stdin_file    = 'my_stdin_file'     # a file to send to your program's stdin
created_files = ["my_outfile",...]  # a list of files created by your program. 
```

For the above test and configuration, the command-line code is equivalent:  
`./a.out arg1 arg2 < my_stdin_file`

You may optionally swap `stdin_file` with `stdin_test`, where you provide a 
string of text to be sent directly to stdin. 

### Running Tests

You can run your tests with the command  
`diff_test -t my_toml_file.toml`  
The argument for the toml file is required.

Optionally, to run a subset of one or more tests, you may also pass `-n testnamea testnameb`.

When you run a test, your code will be ‘made’ according to the make command in `[config]`, then, for each test:

1.  Your executable file will be run, and passed the arguments specified with argv/stdin\_file vars
2.  The reference will be run with the same parameters
3.  The results of stdout and stderr will be diff’d using diff-so-fancy (more human-readable diffs)
4.  Valgrind will be run on your executable, again, with the provided parameters.
5.  If you have any filenames in the `created_files` variable, these files will be diff’d against any such files with the same name produced by the reference - to be clear, in order to properly compare it to the output of the reference, after running your program the output of the file is saved by `diff_test` and then the file is removed. This way, when the reference runs and writes to the same file, the data of your output is not lost.
6.  Results are reported!


### Examples
See examples in the sample/ and tests/ folders in the repository


Thanks! And happy coding!
