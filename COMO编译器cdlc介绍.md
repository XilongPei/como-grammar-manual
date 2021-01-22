# COMO编译器cdlc介绍





```
$ ./cdlc --help
Compile a .cdl file and generate metadata, or generate C++ codes from the metadata.
Usage: cdlc [options] file
Options:
  --help                   Display command line options
  --version                Display version information
  -dump-ast                Display the AST of the .cdl file
  -dump-metadata           Display the metadata generated from the .cdl file
  -c                       Compile the .cdl file
  -d <directory>           Place generated C++ files into <directory>
  -gen                     Generate C++ files
  -i <directory>           Add <directory> to the .cdl files search paths
  -metadata-file <file>    Set <file> as the metadata source file
  -metadata-so <.so file>  Set <.so file> as the metadata source file
  -mode-client             Set the "client" mode which is used to generate
                           C++ files for components caller
  -mode-component          Set the "component" mode which is used to compile
                           .cdl files of components
  -mode-runtime            Set the "runtime" mode which is used to compile
                           .cdl files of comort
  -save-metadata <file>    Save the metadata into <file>
  -split                   Generate interface or class declarations into seperate files when
                           in the "component" mode or the "client" mode
```

