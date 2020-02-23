Problemsetting
**************

What is a problem?
==================

A problem is actually a piece of software usually developed by contest
organizers. Generally, it is an interactor, which executes user's solutions,
controls their interaction with the problem and finally generates game logs.

It's a good idea to make the problem's source code open, however, you may want
to keep some files (e.g. hidden levels) secret until the end of your official
contest. It's convenient to develop problems in teams using git version control
system, that's why we have wide GitHub integration. However, it's not a must.

You can take a look at these example problems if you would like to have some
guidance.

- Tron (TODO addreference)

Problem files are hosted on an AIForces server and can be uploaded directly,
using a web interface or fetched from a remote repository (GitHub, BitBucket,
etc). Only source files (and sometimes tests or statements) should be uploaded.
We recommend to build problem from sources on the server by creating a
``build.sh`` script. **It's prohibited to upload executable files**.

Typical toolstack for problemsetting is:

- Python
- HTML
- CSS
- JavaScript
- LaTex
- Bash

The system interacts with the problem using bash scripts, so it is possible to
customize how Python and LaTex are called or use other tools. A plethora of
default scripts is also available. The web visualizer (which is an important
part of the problem) requires HTML, CSS and JS and has no default implementation.

Problem files are stored in a single folder (may be a git repo), and usually
have the following structure:

Before build:
   .. code-block:: text

      ├── bin
      ├── config
      ├── lib
      │   ├── olymp.sty
      │   ├── problem.tex
      │   └── statements.ftl
      ├── private
      ├── problem.yaml
      ├── public
      │   ├── change_log.txt
      ├── scripts
      │   ├── build.sh
      │   ├── check.sh
      │   └── validator.sh
      ├── solutions
      │   ├── ermolin.cpp
      │   ├── starkov.cpp
      │   └── useless.cpp
      ├── src
      │   ├── check.py
      │   ├── test_generator.py
      │   ├── tron.tex
      │   └── validator.cpp
      ├── statements
      │   ├── eng
      │   └── rus
      ├── tests
      └── visualizer
          ├── eng
          │   ├── visualizer-eng.css
          │   ├── visualizer-eng.html
          │   └── visualizer-eng.js
          └── rus
              ├── visualizer-rus.css
              ├── visualizer-rus.html
              └── visualizer-rus.js
After build:
   .. code-block:: text

      ├── bin
      │   └── validator
      ├── config
      │   └── tests.json
      ├── lib
      │   ├── olymp.sty
      │   ├── problem.tex
      │   └── statements.ftl
      ├── private
      │   ├── 01_notes.txt
      │   ├── 02_notes.txt
      │   ├── 03_notes.txt
      │   ├── 04_notes.txt
      │   ├── 05_notes.txt
      │   ├── 06_notes.txt
      │   ├── 07_notes.txt
      │   ├── 08_notes.txt
      │   ├── 09_notes.txt
      │   └── 10_notes.txt
      ├── problem.yaml
      ├── public
      │   └── change_log.txt
      ├── scripts
      │   ├── build.sh
      │   ├── check.sh
      │   └── validator.sh
      ├── solutions
      │   ├── ermolin.cpp
      │   ├── starkov.cpp
      │   └── useless.cpp
      ├── src
      │   ├── check.py
      │   ├── test_generator.py
      │   ├── tron.tex
      │   └── validator.cpp
      ├── statements
      │   ├── eng
      │   │   ├── statements-eng.html
      │   │   └── statements-eng.pdf
      │   └── rus
      │       ├── statements-rus.html
      │       └── statements-rus.pdf
      ├── tests
      │   ├── 01.txt
      │   ├── 02.txt
      │   ├── 03.txt
      │   ├── 04.txt
      │   ├── 05.txt
      │   ├── 06.txt
      │   ├── 07.txt
      │   ├── 08.txt
      │   ├── 09.txt
      │   └── 10.txt
      └── visualizer
          ├── eng
          │   ├── visualizer-eng.css
          │   ├── visualizer-eng.html
          │   └── visualizer-eng.js
          └── rus
              ├── visualizer-rus.css
              ├── visualizer-rus.html
              └── visualizer-rus.js

You can use your own folder structure (it's not recommended) and describe it in
the ``problem.yaml``.

Problem consists of the following parts:

- Configuration
- Tests (may be not needed for some problems)
- Builder
- Checker
- Validator (recommended, but not mandatory)
- Solutions (recommended, but not mandatory)
- Statements
- Visualizer

Let's talk them over one by one.

General rules
=============

- All folder names starting with ``__ai`` are reserved by the AIForces.
  Please use other folder names.
- All files must have different filenames (not paths, but **filenames**
  including the extension)
- You can't have binary files in the problem, unless they are created by
  ``build.sh``
- All filenames must only contain ASCII characters
- All scripts will run in a firejail. Use only software you're allowed. No
  internet access, no filesystem access outside the problem folder.
- All the source files must not take more than 5 megabytes.
- Use ISO-639-2_ Codes for the Representation of Names of Languages.

.. _ISO-639-2: https://www.loc.gov/standards/iso639-2/php/code_list.php

Available software
===================

All your scripts will run in a firejail, which means you can't access any files
outside your folder. It means that you can use **only the software listed below**.

Additionally, all the necessary binaries will be added to ``PATH`` for the
current session. You can use this software as usual:

List of software

- gcc
- python2.7
- python3.8
- pypy2.7
- pypy3.8
- java smth
- pascal smth
- haskell smth
- latexpdf
- latex
- dvipdf
- A LOT MORE

In all python venvs the following packages are installed

- aitestlib
- numpy
- loguru
- A LOT MORE

Configuration file
==================

In the root directory of the problem folder, you **must** include a problem
configuration file named ``problem.yaml``.

Below is an example of a correct configuration file:

.. code-block:: yaml

   # problem.yaml
   # AIforces problem configuration file.
   # See https://aiforces.readthedocs.io/en/latest/problemsetting.html#configuration-file for details

   ---
   # Config file version
   version: "1.0"

   # Default settings for the problem
   # They will be imported to the aiforces,
   # but might be changed by the managers.
   short-name: tic-tac-toe
   name:
       rus: Крестики-Нолики
       eng: Tic Tac Toe
   # Brief description
   description:
      rus: Просто классика
      eng: The game may be a bit boring, but don't you like it after all?
   # Per-move time limit
   time-limit: 5 s
   # RAM limit
   memory-limit: 512 MB
   # Players number. May be a single integer or range (e.g 2-4)
   players: 2


   # Author's and tester's solutions.
   solutions:
       ermolin:
           name: Nikolay Ermolin.
           file: solutions/ermolin.cpp
           language: g++17
           access: public
           type: pretest
       starkov:
           name: Svyatoslav Starkov.
           file: solutions/starkov.cpp
           language: g++17
           access: private
           type: checker-verifier:TL
       kekov:
           name: Useless solution
           file: solutions/useless.cpp
           language: pypy3
           access: private

   # Relative paths to problem's files

   # Test set configuration
   # Might be created during build of the problem
   tests_config: config/tests.json

   # Scripts to build/clean the problem and checker script
   scripts:
       builder: scripts/doall.sh
       validator: scripts/validate.sh
       checker: scripts/check.sh

   # Localized visualizers
   visualizer:
       rus:
           html: "/visualizer/rus/visualizer-rus.html"
           css: "/visualizer/rus/visualizer-rus.css"
           js: "/visualizer/rus/visualizer-rus.js"
       eng:
           html: "/visualizer/eng/visualizer-eng.html"
           css: "/visualizer/eng/visualizer-eng.css"
           js: "/visualizer/eng/visualizer-eng.js"

   # Localized statements
   statements:
       rus:
           pdf: "/statements/rus/statements-rus.pdf"
           html: "/statements/rus/statements-rus.html"
       eng:
           pdf: "/statements/eng/statements-eng.pdf"
           html: "/statements/eng/statements-eng.html"

   # Other files you want to share with users.
   public_files:
   - public/instruction.txt
   - public/change.log
   ...

Supported settings
------------------

.. note::
   The presence of any key that isn’t documented here will make the build fail.
   This is to avoid typos and provide feedback on invalid configurations.

version
^^^^^^^
   **Required: true**

   Version of the configuration file. You're currently reading about 1.0

   .. warning::
      Please, put the version into quotes. Otherwise, YAML may mark it as a floating point number.

   Example
      .. code-block:: yaml

         version: "1.0"

short-name
^^^^^^^^^^
   **Required: true**

   Short name of the problem (not the displayname), matches `^[a-zA-Z0-9_\-=+.,!]{4,20}$ <https://regex101.com/r/OsZJss/1>`_.

   Example

      .. code-block:: yaml

         short-name: tic-tac-toe

name
^^^^
   **Required: false**

   Display name of the problem given in all needed languages.

   Example
      .. code-block:: yaml

         name:
             rus: Крестики-Нолики
             eng: Tic Tac Toe

description
^^^^^^^^^^^
   **Required: false**

   Suggested brief description of the problem (localized).

   Example
      .. code-block:: yaml

         description:
            rus: Просто классика
            eng: The game may be a bit boring, but don't you like it after all?

time-limit
^^^^^^^^^^
   **Required: false**

   Suggested per-move time limit for the problem.
   You can use following units:

   - ms - millisecond (1/1000 of a second)
   - s - second

   Value can't be more than 1 minute.

   Example
      .. code-block:: yaml

         time-limit: 5 s

memory-limit
^^^^^^^^^^^^
   **Required: false**

   Suggested per-move memory limit for the problem.
   You can use following multiples:

   - `B` for bytes
   - `kB` for kilobytes
   - `KiB` for kibibytes
   - `MB` for megabytes
   - `MiB` for mebibyte
   - `GB` for gigabyte
   - `GiB` for gibibyte

   Value can't exceed 1 GiB.

   Example
      .. code-block:: yaml

         memory-limit: 512 MB

players
^^^^^^^
   **Required: true**

   Number of players which compete together. May be a single integer or a range.

   Example
      .. code-block:: yaml

         players: 2

      .. code-block:: yaml

         players: 2-4

solutions
^^^^^^^^^
   **Required: false**

   Describes author's and tester's solutions.

   - ``access`` may be private, protected or public.

   - ``language`` is one of the :ref:`supported programming languages <languages-label>`.

   You may set the ``type`` of the solution to ``pretest`` or ``checker-verifier:[VERDICT]``,
   where ``[VERDICT]`` is one of :ref:`system verdicts <verdicts-label>`. If you
   omit this field, the solution will not serve any purpose, but still will be
   saved on disk.

   Read more about :ref:`solutions-label`

   Example
      .. code-block:: yaml

         # Authors and testers solutions.
         solutions:
             ermolin:
                 name: Nikolay Ermolin.
                 file: solutions/ermolin.cpp
                 language: g++17
                 access: public
                 type: pretest
             starkov:
                 name: Svyatoslav Starkov.
                 file: solutions/starkov.cpp
                 language: g++17
                 access: private
                 type: checker-verifier:TL
             kekov:
                 name: Useless solution
                 file: solutions/useless.cpp
                 language: pypy3
                 access: private

tests_config
^^^^^^^^^^^^
   **Required: true**

   JSON file which stores the test configuration. May be created by the ``build.sh`` script.

   Example
      .. code-block:: yaml

          tests_config: config/tests.json

scripts
^^^^^^^

   Problem script files.

   :ref:`builder-label` script compiles the task into a ready state.
   :ref:`validator-label` script reads a test file and checks it for validity.
   :ref:`checker-label` script starts and runs challenges and produces logs.

   builder
      **Required: false**
   validator
      **Required: false**
   checker
      **Required: true**

   Example
      .. code-block:: yaml

         scripts:
             builder: scripts/doall.sh
             validator: scripts/validate.sh
             checker: scripts/check.sh


visualizer
^^^^^^^^^^
   **Required: true**

   Visualizer web page files, localized for several languages.

   Example
      .. code-block:: yaml

         visualizer:
             rus:
                 html: "/visualizer/rus/visualizer-rus.html"
                 css: "/visualizer/rus/visualizer-rus.css"
                 js: "/visualizer/rus/visualizer-rus.js"
             eng:
                 html: "/visualizer/eng/visualizer-eng.html"
                 css: "/visualizer/eng/visualizer-eng.css"
                 js: "/visualizer/eng/visualizer-eng.js"

statements
^^^^^^^^^^
   **Required: true**

   Problem statements in different formats and different languages.

   Example
      .. code-block:: yaml

         rus:
             pdf: "/statements/rus/statements-rus.pdf"
             html: "/statements/rus/statements-rus.html"
         eng:
             pdf: "/statements/eng/statements-eng.pdf"
             html: "/statements/eng/statements-eng.html"


public_files
^^^^^^^^^^^^
   **Required: false**

   Any other files that you want to share with the participants.

   Example
      .. code-block:: yaml

         public_files:
         - public/instruction.txt
         - public/change.log



Tests
=====

One challenge configuration is called a *Test*. It can be, for example, one
level of the game. If you need to prepare files describing the tests, you can
do it while building or upload them together with the problem.

What you **must** create is a test configuration file and add its path to the
problem config. This file, however, may be generated by the *Builder*. This
file looks like the following JSON:

.. code-block:: json

   [
      {
         "id": 0,
         "name": "Mega Level 1",
         "file": "tests/01",
         "public-description": "First and most simple test in the testset",
         "hidden-description": "The solution is quite simple, just ..."
      }

      {
         "id": 1,
      }
   ]

Actually, you may omit all keys except the ``id``. If your problem does not have
different levels, you can create only one test.


Scripts
=======

The system is interacting with your problem via bash scripts so you can run
another scripting language in it (e.g. Python). All arguments are passed to the
script as JSON strings through the env vars.

File descriptors for I/O are inherited from the parent, their integer values
are also passed through env vars.

Your stdout/stderr will be saved for internal use.

.. _builder-label:

Builder
=======

The builder script produces end files from sources. It usually includes a:

- Compiling visualizer
- Compiling checker, test_generator, validator
- Generating tests and test config

Current problem settings are presented as JSON in the ``AI_PROBLEM_SETTINGS``
env variable the same way they are described in ``problem.yaml``.

.. code-block:: json

   {
       "short-name": "tic-tac-toe",
       "name": {
            "rus": "Крестики-Нолики",
            "eng": "Tic Tac Toe"
        },
       "description": {
           "rus": "Просто классика",
           "eng": "The game may be a bit boring, but don't you like it after all?"
       },
       "time-limit": "5 s",
       "memory-limit": "512 MB",
       "players": "2"
   }

Any builder logs written to stderr will be saved for internal use.

.. _checker-label:

Checker
=======

The checker is the script that governs players and their interaction. It has
the following interface:

Arguments are provided in the ``AI_CHECKER_ARGS`` env variable as JSON

players_cmds
   Pre-made bash commands to start solutions
players_files
   The solutions binary files (needed for firejail)
test_id
   Current test id
test_description_fd
   File descriptor integer value for the test file (if such was provided)
time_limit
   Per move time limit in milliseconds
memory_limit
   RAM limit in bytes.
result_log_fd
   File descriptor integer value to write result_log JSON
game_log_fd
   File descriptor integer value to write game_log file
streams_log_fd
   File descriptor integer value to write interaction logs

.. code-block:: json

   {
       "players_cmds": [
          "python /path/to/file.py",
          "/path/to/exec"
       ],
       "players_files": [
          "/path/to/file.py",
          "/path/to/exec"
       ],

       "test_id": 1,
       "test_description_fd": 3,
       "time_limit": 1000,
       "memory_limit": 536870912,
       "result_log_fd": 4,
       "game_log_fd": 5,
       "streams_log_fd": 6
   }

Any checker logs written to stderr will be saved for internal use.

Stream logs should have the following format, integer keys represent time
moments (ticks):

.. code-block:: json

   [
      {
         "stdin": {
            "0": "Stdin",
            "3": "Followed stdin given after 3 tick."
         },
         "stdout": {
            "0": "First response",
            "3": "Last response"
         },
         "stderr": {
            "0": "My debug output",
            "3": "My last debug output"
         }
      },
      {
          "..." : "Same for all other players"
      }
   ]

Result logs should have the following format

.. code-block:: json

   [
      {
         "verdict": "OK",
         "score": 500,
         "comment": "solution worked OK"
      },

      {
         "verdict": "TL",
         "score": 200,
         "comment": "solution experienced TLE on tick 678."
      }

      {
          "..." : "Same for all other players"
      }
   ]

Game logs are designed by you, so make sure they are constructed in a way,
what would be easy for a visualizer to process.

.. _validator-label:

Validator
=========

A validator is a script which reads test files from stdin and checks them for
validity. Any logs written to stderr will be saved for internal use. If the test
happens to be incorrect, the validator must finish with a non-zero code.
Validation is performed automatically by the ``problem-verifier``.

.. _solutions-label:

Solutions
=========

Solutions are created by problem authors and testers. They serve 3 purposes:

- Show examples of solutions to the participants
- Verify that the checker works correctly
- Pretest the submissions

Solution access modifiers are supported. Solutions can be either public, private
or protected.

- Public solutions are used as example solutions for participants
- Protected solutions can be played against, but the source code is kept private
- Private solutions can't be interacted with directly

Solutions marked as pretests **must** finish with the ``OK`` verdict. They are
used as opponents for the participant solutions on pretests. The only aim of a
pretest is to warn participants in case their solution behaves badly. Bear in
mind that it's a good idea to make pretests public or at least protected, so
that participants could see why their solution fails the pretests.

When you create a checker verifier, design it in such a way, that it would get
the same verdict with any opponent and test number. Use this expected verdict
in the solution configuration.

Also, all solutions used for pretests are used as checker verifiers and are
expected to have ``OK`` verdicts. You don't have to add them to the list
explicitly.

Statements
==========

Statements are usually written in LaTex and compiled into several different
formats (like pdf or html) for the sake of convenience. You have to compile
separate statement files for each language. They should be compiled by
``build.sh`` or by you before the upload.

Visualizer
==========

A visualizer is a webpage which uses the Challenge API (TODO Addreference) to
access logs of a challenge and visualize the game in a convenient way. It will
be embedded in a webpage using an ``iframe``.
