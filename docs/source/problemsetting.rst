Problemsetting
**************

What is a problem?
==================

Problem is actually a piece of software, usually developed by contest organizers. Generally, it's an
interactor, which executes user's solutions, control their interaction with the game and finally
generates game logs.

It's a good idea to make problem code open source, however, you may want to keep some files (e.g. hidden levels)
in secret till the end of your official contest. It's convinient to develop problems in teams using
git version control system, that's why we have wide GitHub integration. However, it's not a must.

You can explore these example problems if you prefer to learn by example

- Tron (TODO addreference)

Problem files are hosted on AIForces server and can be uploaded directly using web interface or fetched from
git. Only source files (and sometimes tests or statements) should be uploaded. We recommend to build problem
from sources on the server by creating ``build.sh`` script. **It's prohibited to upload executable files**.

Typical toolstack for problemsetting is:

- Python
- HTML
- CSS
- JavaScript
- LaTex
- Bash

However, system interacts with the problem using bash scripts, so if you're familiar with them,
you can replace Python and LaTex with other tools, otherwise you may use default scripts.
However, web visualizer (which is an important part
of the problem) requires HTML, CSS and JS.

Problem is stored as a folder (may be a git repo), and usually has the following structure:

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
      │   │   ├── statements-eng.epub
      │   │   ├── statements-eng.html
      │   │   └── statements-eng.pdf
      │   └── rus
      │       ├── statements-rus.epub
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

However, you can use your own structure (but it's not recommended) and describe it in the ``problem.yaml``.

Problem consists of the following parts:

- Configuration
- Tests (may be not needed for some problems)
- Builder
- Checker
- Validator (recommended, but not mandatory)
- Solutions (recommended, but not mandatory)
- Statements
- Visualizator

Let's talk them over one by one.

General rules
=============

- All folder names, starting with ``__ai`` are reserved by the AIForces. Please use other folder names.
- All files listed in configuration file must have diffrent filenames (not paths, but **filenames**)
- You can't include binary files in the problem, unless they are created during ``build.sh``
- All filenames must be in ASCII
- All scripts will run in a firejail. Use only software you're allowed to. No internet access, no filesystem access outside the problem folder.
- All the source files must not take more than 5 megabytes.
- Use ISO-639-2_ Codes for the Representation of Names of Languages.

.. _ISO-639-2: https://www.loc.gov/standards/iso639-2/php/code_list.php

Availiable software
===================

All your scripts will run in a firejail, which means you can't access any files outside your folder.
It means that you should rely **only on software listed below**.

Also, all the neccesarry folders will be added to ``PATH`` for the current session.
So you can use this software as usual.

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

In the root directory of your problem folder, you **must** include problem configuration file named ``problem.yaml``.

Below is the example of correct configuration file:

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
   decription:
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
           epub: "/statements/rus/statements-rus.epub"
       eng:
           pdf: "/statements/eng/statements-eng.pdf"
           html: "/statements/eng/statements-eng.html"
           epub: "/statements/eng/statements-eng.epub"

   # Other files you want to share with users.
   public_files:
   - public/instruction.txt
   - public/change.log
   ...

Supported settings
------------------

.. note::
   The presence of any other key that isn’t documented here will make the build to fail.
   This is to avoid typos and provide feedback on invalid configurations.

version
^^^^^^^
   **Required: true**

   Version of the configuration file. You're currently reading about 1.0

   .. warning::
      Please, put version into quotes. Otherwise, YAML may mark it as a floating point number.

   Example
      .. code-block:: yaml

         version: "1.0"

short-name
^^^^^^^^^^
   **Required: true**

   Short name of the problem (not displaying), matches ``^[a-zA-Z0-9_\-=+.,!]{4,20}$``.

   Example
        
      .. code-block:: yaml

         short-name: tic-tac-toe

name
^^^^
   **Required: false**

   Suggested name of the problem given in all needed languages.

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

         decription:
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

   Number of players, which compete together. May be a single integer or range of integers.

   Example
      .. code-block:: yaml

         players: 2

      .. code-block:: yaml

         players: 2-4

solutions
^^^^^^^^^
   **Required: false**

   Describes Author's and tester's solutions.

   - ``access`` may be private, protected or public.

   - ``language`` is one of the :ref:`supported programming languages <languages-label>`.

   You may set ``type`` of the solution to ``pretest`` or ``checker-verifier:[VERDICT]``,
   where ``[VERDICT]`` is one of :ref:`system verdicts <verdicts-label>`. If you omit the field, the 
   solution will not serve the particular purpose, but still will be saved.

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
                 acess: private

tests_config
^^^^^^^^^^^^
   **Required: true**

   JSON file, which stores tests configuration. May be created during run of ``build.sh`` script.

   Example
      .. code-block:: yaml

          tests_config: config/tests.json

scripts
^^^^^^^
   
   Files of problem scripts.

   :ref:`builder-label` is preparing the problem for working.
   :ref:`validator-label` script reads test file and strctly checks it validity. Read more. 
   :ref:`checker-label` script starts runs challenges and produces logs. Read more.

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

   Statements of the problem, given in different formats and different languages.

   Example
      .. code-block:: yaml

         rus:
             pdf: "/statements/rus/statements-rus.pdf"
             html: "/statements/rus/statements-rus.html"
             epub: "/statements/rus/statements-rus.epub"
         eng:
             pdf: "/statements/eng/statements-eng.pdf"
             html: "/statements/eng/statements-eng.html"
             epub: "/statements/eng/statements-eng.epub"


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

One challenge configuration is called *Test*. It can be for example, one level from the game.
If you need to prepare files, describing the tests, you can do it while building or upload them with the problem.

What you **must** create is a tests configuration file and add path it to problem's config. However,
this file may be generated during run of *Builder*. Tests configuration
looks like this JSON:

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

Actually, you may omit any informatoin except ``id``. If your problem is not about different levels, create only one test.


.. _builder-label:

Builder
=======

Builder is the script aiming to build the problem from sources. It usually includes:

- Compiling visualizer
- Compiling checker, test_generator, validator
- Generating tests and test config

Current problem's settings are served as JSON in ``AI_PROBLEM_SETTINGS`` enviromental
variable the same way thay are described in ``problem.yaml``.

.. code-block:: json

   {
       "short-name": "tic-tac-toe",
       "name": {
            "rus": "Крестики-Нолики",
            "eng": "Tic Tac Toe"
        },
       "decription": {
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

Checker is a script used to perform challenges between multiple players. Interface works as follows:

Arguments to the checker are given in ``AI_CHECKER_ARGS`` in JSON format

players_cmds
   Ready bash commands to start solutions
players_files
   binary files of the solutions (needed for firejail)
test_id
   Current test id
test_description_fd
   File descriptor integer value to read data, describing the test (if was provided).
time_limit
   Per move time limit in milliseconds
memory_limit
   RAM limit in bytes.
result_log_fd
   File descriptor integer value to write result_log JSON.
game_log_fd
   File descriptor integer value to write game_log file.
streams_log_fd
   File descriptor integer value to write interaction logs.

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

Streams log should have the following format. Integer numbers represent time moments (ticks):

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

Result log should have the following format

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

Game log is designed in your way, make it easy to read by the visualizer. 

.. _validator-label:

Validator
=========

Validator is a script, which reads test file from the stdin and checks it for validity.
Any logs written to stderr will be saved for internal use. If test is incorrect, validator must finish the process
with non-zero exit code. Validation is performed automatically by ``problem-verifier``.

.. _solutions-label:

Solutions
=========

Solutions are created by problem's authors and testers. They serve 3 purposes:

- Show examples of solutions to the participants
- Verify that checker works correctly
- Pretesting the submissions.

Solution access modifiers are supported. Solution can be public, private or protected.

- Public solutions are used as example solutions for participants
- Protected solutions can be played with, but source code is kept private
- Private solutions can't be interacted with directly.

Solutions marked as pretests **must** finish with the ``OK`` verdict. They are used as opponents
for the participant's solutions on pretests. The only aim of pretest is to warn participant in case
his solution behaves wrongly. Mind that it's a good idea to make pretests public or at least
protected, so that participants could see why their solution fails the pretests.

When you create a checker verifier, design it in such way, that it get the same verdict with any opponent and test number.
Use this expected verdict in solution configuration.

Also all solutions, used for pretests, are used as checker verifiers and are expected to have ``OK`` verdict.
You don't have to add them to the list one more time.

Statements
==========

Statements are usually written in LaTex and compiled into 3 diffeent formats(pdf, html, epub) for comfortable usage.
You need to compile separate statements for each language. They should be compiled during ``build.sh`` or before the upload.

Visualizator
============

Visualizer is a webpage, which uses Challenge API (TODO Addreference) to download logs of the challenge and visualize the game in a
convinient way. It will embeded to an AIForces frontend using ``iframe``.