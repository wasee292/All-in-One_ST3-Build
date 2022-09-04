# CP with Sublime Text

This repository is a setup guide of Sublime Text (ST) for Competetive Programming in multiple languages - C, C++, Java, Python, etc.,

## Idea

* Modify layout of ST for easy viewing of source code, inputs and respective output
* Compile, run, read input (from file) and write output using a single keyboard stroke combination
    * Linux, Windows - `ctrl + shift + B`
    * Mac - `cmd + shift + B`

## Prerequisites

* Programming languages should be installed in the machine and their **paths should be properly exported**

## Steps

### 1\. Setting the layout of ST window \([reference](https://nikolaloncar.com/sublime-text-custom-layout-tutorial-inside-out-with-examples/)\)

* Open Sublime console by clicking <code>ctrl/control + `</code>
* It appears at the bottom of the window
* Paste the below command in the console terminal 
`window.set_layout({  'cols':[0,0.5,1.0],'rows':[0.0,0.75,1.0], 'cells':[[0,0,2,1],[0,1,1,2],[1,1,2,2]]})`
* Click `enter/return`
* Restart Sublime Text

### 2\. Creating a directory to work with

* A directory (folder) needs to be created where all our files i.e., source code, input & output files, shell script will be present. Basically, this is the directory from where we will be working on

### 3\. Shell script \- autorun\.sh

```
# !/bin/sh
FILE_NAME=$1
FILE_BASE_NAME="${FILE_NAME%.*}" # extract the base file name
FILE_EXTENSION="${FILE_NAME#*.}" # extract the file extension
INPUT_FILE="input.txt"
OUTPUT_FILE="output.txt"

case $FILE_EXTENSION in
	"c" )
		gcc-12 $FILE_NAME -o $FILE_BASE_NAME && ./$FILE_BASE_NAME < $INPUT_FILE > $OUTPUT_FILE
		;;
	"cpp" )
		g++-12 $FILE_NAME -o $FILE_BASE_NAME && ./$FILE_BASE_NAME < $INPUT_FILE > $OUTPUT_FILE
		;;
	"java" )
		javac $FILE_NAME && java $FILE_BASE_NAME < $INPUT_FILE > $OUTPUT_FILE
		;;
	"py" )
		python3 $FILE_NAME < $INPUT_FILE > $OUTPUT_FILE
		;;
esac
```

### 4\. Input, output and source code files

* Create the files `input.txt`, `output.txt`, `prog.c`, `prog.cpp`, `Prog.java`, `prog.py`, etc., as per your need in the same directory that we created earlier

### 5\. Custom Build System \([reference](https://www.sublimetext.com/docs/build_systems.html)\)

* Go to `Tools > Build System > New Build System`
* Paste the below code snippet in the window (and be sure to write the actual absolute path to `autorun.sh` file)

```
{
	"variants":
	[
		{
			"cmd":
			[
				"sh /absolute/path/to/autorun.sh $file_name"
			],
			"name": "Autorun",
			"shell": true,
			"working_dir": "$file_path"
		}
	]
}
```

* Give the file a name and save it (remeber the name of the file)

### 6\. Keyboard shortcut \([reference](https://www.sublimetext.com/docs/key_bindings.html#user-bindings)\)

* Go to `Preferences > Key Bindings`
* It opens a two paned window. We should edit file in the right pane only
* Paste the below content in the file

```
[
	{"keys": ["command+shift+b"], "command": "build", "args": {"build_system": "/absolute/path/to/custom_build_name.sublime-build", "variant": "Autorun"}}
]
```
* In the above file, use `ctrl` instead of `command` in `keys` for non MacOS machines
* The value for `variant` should be same as `name` in Step 5

### Done!!!
Once the setup is done, you can start writing code in your favourite language in a file. Paste inputs in the `input.txt` file and hit `command + shift + B` to see it work.