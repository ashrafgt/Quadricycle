# Python and its Ecosystem: A High Level View

## 1. Python's Inner Workings:

Python is used for a wide range of software use-cases for its ease of use and availability of libraries and tools built by its massive community.

It's important to understand the basics of how a language works, from the source code to execution on the hardware, and one of the first distinctions we make in this topic is whether a language is complied or interpreted.

### a. Compiled vs Interpreted Languages:

#### Compiled Languages:

Languages such as Pascal, C, C++, and Go go through a compilation process where the high-level source code is generally transformed into one of the following:
- **Assembly Language**: low-level instructions that are very close to the hardware and that need an assembler to be converted into machine code
- **Object Code**: a portion of incomplete machine code that requires a linking process to connect it with the libraries it's using (note: not all compiled languages go through this phase)
- **Machine Code**: a binary containing instructions executed directly on the hardware (e.g. `.exe` files)

#### Interpreted Languages:
Languages such as PHP, Ruby, and Javascript go through a translation process where the high-level source code is generally transformed into bytecode, a set of portable instructions (not specific to any type of operating system or hardware) executed by an interpreter which is a operating-system-specific and hardware-specific program that translates bytecode into instructions directly executed on the machine.

### b. CPython Execution Model:
Python is, in reality, a specification for a language, rather than just an implementation. Because of this, a wide array of implementations exist such as: IronPython, PyPy, Jython, IPython, and CPython, the default and most popular implementation.

CPython (just like most "interpreted") lanaguage implementation really mixes compilation and interpretation:
1. The process starts with source code: `.py` files
2. Followed by a compilation into byecode:`.pyc` files (located in `__pycache__` directories near their source files)
3. Ending with intepretation by the PVM (Python Virtual Machine) runtime

Other known Python implementations are:
- **IPython**: the origin of the Jupyter project, focused on interactivity and distributed computing
- **PyPy**: the Just-In-Time compiled, highly perfomant implementation, based on RPython translation
- **IronPython and Jython**: .NET and Java implementations of Python, on one hand enabling the users of these frameworks to create applications using the shorter and simpler Python syntax, and on the other hand exposing the powerful features and libraries of these frameworks to Python users

### c. Python Program Architecture:

Python programs are composed of modules (`.py` files), from which we can distinguish three main types:
- **Top Module**: entrypoint module that contains the `main()` function
- **Standard Library Modules**: standard modules that come with Python by default, usually found under `/usr/lib/{PYTHON_VERSION}` (e.g. `os`, `json`)
- **Non-Standard Library Modules**: 
    - **Custom Modules**: custom modules eventually imported by the top module, usually found in the same directory as the top module or in any colon-separated directories listed in `$PYTHONPATH`
    - **External Modules**: modules that are installed using Python package managers such as `pip`, usually found under `/usr/local/lib/{PYTHON_VERSION}/dist-packages` or `{HOME_DIRECTORY}/.local/lib/{PYTHON_VERSION}/site-packages` (e.g. `tensorflow`, `seaborn`)

When running Python, the contents of `sys.path` show the list of directories from where modules can be imported.


Read more about the internals of Python here: https://leanpub.com/insidethepythonvirtualmachine/read


## 2. Python's Ecosystem:

Building software in Python involves a lot of elements, which we try to present briefly.

### Package Managers:
Software systems built with Python rarely only rely on standard library modules or custom modules written by the application's developers. It often relies on modules written and distributed by the community.

The two most used ones are `pip` (included with Python) and `conda`. The most popular package repositories are `pypi` (Python Package Index) and `anaconda` repository channels.

### Environment Managers:

### Debuggers:

### Interactive Servers:

### Linters:
