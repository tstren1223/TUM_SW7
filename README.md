# TUM_SW7
1:
Find Bugs
In this exercise, we want to find bugs by applying static analysis using SpotBugs.

How to run SpotBugs
For this exercise, we recommend running the SpotBugs via your local IDE. However, you can also use the SpotBugs GUI.

Install the SpotBugs plugin for IntelliJ
Open the IntelliJ settings (File -> Settings) and navigate to the section Plugins. Open the tab Marketplace and search for SpotBugs and install the plugin. You may have to restart your IDE afterwards.

Note: Right after installing, IntelliJ may display the following error: Do not request resource from classloader using path with leading slash. You can simply ignore this error and it should not appear again.

Run SpotBugs using IntelliJ
To execute the SpotBugs static code analysis, you have to perform the following steps:

Right click any project file (on the left hand side).
Navigate to SpotBugs -> Analyze Project Files Not Including Test Files. The project does not contain any tests, therefore we can simply use this option to skip the execution for test files. (You can also analyze individual files)
A toolwindow opens that displays the SpotBugs findings with a short description of the problem.
<img width="945" alt="image" src="https://github.com/tstren1223/TUM_SW7/assets/64294878/1b9d844c-c19c-4c2f-9b08-e252937736b3">
Run SpotBugs using SpotBugs GUI
In comparison to the SpotBugs IDE plugin, the SpotBugs GUI does not execute the code analysis itself, but simply displays a priorly generated report. Therefore, you have to generate the report to use the results in the GUI.

This can be achieved by running ./gradlew clean spotbugsMain from your preferred CLI (Command Line Interface). This command executes the SpotBugs code analysis for your main source code files (excluding test code) and generates a report file under the file path build/spotbugs-report.xml. Open this file in the SpotBugs GUI to see a graphical displaying of the findings.

Your Task
The SpotBugs Plugin/GUI should display 7 bugs in 2 different categories:

Correctness (5 -> 2,2,1)
Bad practice (2)
Inspect the problems and navigate to the source code. Read the detailed error messages and think about a possible solution.

 Fix the found bugs 3 of 3 tests passing
Note: The static code analysis features of Artemis are disabled for this exercise.

2:
Find Bugs 2
In this exercise, you will learn how to use Infer, a static code analysis tool developed by an engineering team at Meta, to find bugs in a code.

Before you can start working on this exercise, make sure you have installed the system-specific Docker version on your computer.

Update: If your machine has an ARM processor, we recommend to complete the exercise inside a virtual machine with any x86_64 OS (preferably Ubuntu 22.04 x86_64).

Build the Infer Image
To begin, clone the exercise repository and navigate to the exercise directory in the command line. Alternatively, you can open a command line window in your IDE, which will automatically place you in the exercise directory.

In the exercise repository, you will find a Dockerfile. So once you are in the correct directory, you can build the Infer Docker image. Run the following command: docker build -t infer .

Note: Building the container image might take some time.

Run the code analysis
After successfully building the container image, you can start the application in a container using the following command: docker run -it --rm -v $PWD/src:/eist23l08p02-exercise infer /bin/bash. This command runs the container and opens a bash shell within it. Once inside the container, navigate to the exercise directory.

Fix the bugs
Now, you are ready to execute the Infer static code analysis. Run the following command to analyze the code: infer run -- javac de/tum/in/ase/eist/*.java

Infer should display 2 errors and 2 warnings. Your task is to inspect these problems and fix the bugs in the code.

Note: The static code analysis features of Artemis are disabled for this exercise.

 Fix the found bugs 3 of 3 tests passing
If you make changes to the code, you don't need to rebuild the Docker image. Simply run the analysis command again.

Traces
This step is optional, but for more detailed information about the bugs found by Infer, you can examine the traces. While still running the container, type the following command to generate an HTML report: infer explore --html. This command will provide you with a path to the HTML report. To view the report, you can either exit the container and use the command line to open the report in your browser by following the provided path, or you can navigate to the location of the HTML report in your file explorer and open it by double-clicking on it. The HTML report will contain information about the bugs, including their traces.

Clean Up
Once you have fixed the bugs, you can stop and exit the container by using the exit command. Note that exiting the container will also remove it, because of the --rm flag added to the docker run command.

After executing the Infer analysis, you may notice the presence of .class files in your src folder. These files are generated by Infer during its own compilation process. You will also find infer-out direcory in src. You do not need to do anything with those files, but if you want, you can delete them.

3:
Dynamic code analysis with AddressSanitizer
In this exercise we want to find bugs using AddressSanitizer. This tool, in contrast to SpotBugs and Infer, is a dynamic analysis tool and, because of that, it finds the bugs during runtime. The dynamic nature of AddressSanitizer analysis allows it to find more complex bugs, for example in heap allocation, which would not be detected by a static analysis tool.

AddressSanitizer is already included in LLVM and GCC compilers, and needs no additional installation. (If you have problems getting AddressSanitizer to work, check if your compiler is up-to-date)

Compiling
To run AddressSanitizer on your machine (Linux and MacOS), simply add the -fsanitize=address flag to your bash command (E.g. gcc -O1 -fsanitize=address -fno-omit-frame-pointer main.c) or use the given Makefile. To use the Makefile, navigate to your assignment folder in your terminal and type the bash command make or make all. This will compile 'bakery.c' normally to bakery.out and with AddressSanitizer to 'asan.out'.

Running
Now you can run an example without AddressSanitizer with make run and four examples with AddressSanitizer by executing make runasan1, make runasan2, make runasan3 or make runasan4 with predefined example arguments or manually by calling the file with your own arguments.

The given program template is a simple bakery program. When running the program through the command line, you need to add a flag with some arguments: ./<filename>.out -c <count of bakeries> <bakery names separated by a single space> <pastries for each bakery separated by a single space> an example call would be ./asan.out -c 3 "bakery1 bakery2 bakery3" "Croissants Bread Pretzels". Now bakery1 has Croissants, bakery2 has Bread and bakery3 has Pretzels.

Finding Bugs
Four mistakes have creeped into the program and its your job to fix them. Run the program through AddressSanitizer and fix the errors.

The errors that AddressSanitizer can detect are as followed:

Using heap memory after free() (heap-use-after-free)
Using heap memory out of bounds of specified space (heap-buffer-overflow)
Using stack memory out of bounds of specified space (stack-buffer-overflow)
Using global memory out of bounds of specified space (global-buffer-overflow)
The full documentation and examples for these errors is on the GitHub page for AddressSanitizer.

Tests
 Compile 1 of 1 tests passing
 Compile with AddressSanitizer 1 of 1 tests passing
 Checking output with AddressSanitizer 2 of 2 tests passing
 Checking output 2 of 2 tests passing
