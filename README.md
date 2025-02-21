# ![nc-scout](img/nc-scout.png)

## Index
* [Description](#description)
* [Dependencies](#dependencies)
* [Usage](#usage)
* [Build](#build-instructions)
* [Installation](#installation)

## Description
nc-scout is a simple naming convention checker tool. It allows you to search directories for non-matching filenames to a naming convention. It is a personal tool I wanted for system cleanliness that became a larger project. It currently only supports predefined regular expressions as defined [here](src/naming.c), although, I have played with the idea of creating a configuration file based approach, where you could create a much more customized search regimen.

## Dependencies
* make
* POSIX-compliant system (Linux, MacOS)

## Usage
The layout of a nc-scout command.

nc-scout [OPTION]? [COMMAND] [CONVENTION] [DIRECTORY]

### Options
| Flag              | Description                                                |
|-------------------|------------------------------------------------------------|
| `-h, --help`      | Show a helpful message.                                    |
| `-v, --version`   | Show what version of the program you are using.            |
| `-f, --full-path` | Display matches by their full path name.                   |
| `-m, --matches`   | Print matches instead of non-matches.                      |

### Commands
|Command   | Description                                                                         |
|----------|-------------------------------------------------------------------------------------| 
| `search` | Search a directory for files with a filename body that matches a naming convention. |


### Conventions
| Convention        | Example                                                    |
|-------------------|------------------------------------------------------------|
| `flatcase`        | examplefilename.txt                                        |
| `camelcase`       | exampleFileName.txt                                        |
| `pascalcase`      | ExampleFileName.txt                                        |
| `snakecase`       | example_file_name.txt                                      |
| `constantcase`    | EXAMPLE_FILE_NAME.txt                                      |
| `kebabcase`       | example-file-name.txt                                      |
| `cobolcase`       | EXAMPLE-FILE-NAME.txt                                      |

### What is the Filename Body of a Filename?
The **filename body** is the text of a file's full filename, ignoring leading periods and file extentions. The final period itself and the text that follows it is what is defined as the file extention. `search` is only performed on the filename body of a filename.

```bash
# Matches: 
nc-scout search snakecase --matches ./
.example_file           # The leading period is ignored, resulting in the filename body 'example_file', which is snakecase.
example_file.txt        # The file extention '.txt' is ignored, resulting in the filename body 'example_file', which is snakecase.
.example_file.RAR       # The file extention '.RAR' is ignored, resulting in the filename body 'example_file', which is snakecase.

# Non-matches: 
nc-scout search flatcase ./
..example_file          # The leading period is ignored, resulting in the filename body '.example', which is not snakecase.
example_file.exe.txt    # The file extention '.txt' is ignored, resulting in the filename body 'example_file.exe', which is not snakecase.

```

### Strict vs. Lenient:
The default enforcement of naming conventions for a search is lenient, although, using
the `-s` or `--strict` option, you can strictly enforce the naming convention for that search.

Strict enforcement means that the naming convention **must** be present in it's entirety in the filename body, while lenient enforcement means that the naming convention **could** be present in it's entirety in the filename body if more text is added, but not removed or changed.

Example File: **example.txt** <em>(filename body is 'example')</em>

Matches strictly:
* **flatcase** - The filename body <em>'example'</em> is all lowercase, so it exactly matches the convention.

Matches leniently:
* **kebabcase** - The filename body <em>'example'</em> could be extended to <em>'example-file'</em> to match the convention in its entirety
* **snakecase** - The filename body <em>'example'</em> could be extended to <em>'example_file' to match the convention in its entirety
* **camelcase** - The filename body <em>'example'</em> could be extended to <em>'exampleFile' to match the convention in its entirety

## Build Instructions
To begin, clone the project and go to the root of the repository:
```bash
git clone https://github.com/BlinkDynamo/nc-scout.git nc-scout
cd nc-scout/
```

Build the binary:
```bash
make
``` 

Run tests (optional):
```bash
make check
```

## Installation
If the project built successfully, you can now either use the binary directly from the repository, or install it to your system.

To use the binary directly:
```bash
cd build/
# called directly from the build directory.
./nc-scout --help
```

To install the binary and use it systemwide:
```bash
sudo make install
# called from /usr/local/bin/
nc-scout --help
```

Should you want to clean the build/ and tests/ directories:
```bash
make clean
```

To uninstall the binary:
```bash
sudo make uninstall
```
