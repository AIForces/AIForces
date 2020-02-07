Problemsetting
**************

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

- All files listed in configuration file must have diffrent filenames (not paths, but **filenames**)
- You can't include binary files in the problem, unless they are created during ``build.sh``
- All filenames must be in ASCII
- All scripts will run in a firejail. Use only software you're allowed to. No internet access, no filesystem access outside the problem folder.
- All the source files must not take more than 5 megabytes.

Availiable software
===================

All your scripts will run in a firejail, which means you can't access any files outside your folder.
It means that you should rely **only on software listed below**.

If you don't have ``bin`` folder, it will be created and all software executable
from the list will be copied there. Also, all the neccesarry folders will be added to
``PATH`` for the current session. So you can use this software as usual.

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
       ru_RU: Крестики-Нолики
       en_US: Tic Tac Toe
   # Brief description
   decription: The game may be a bit boring, but don't you like it after all?
   # Estimated difficulty from 1 to 5
   difficulty: 1
   # Time limit per move in microseconds
   time-limit: 1000
   # RAM limit in bytes
   memory-limit: 268435456
   # Minimal and maximal players number
   minimal_players: 2
   maximal_players: 2


   # Example of solutions to be checked. 
   solutions:
       ermolin:
           name: Nikolay Ermolin.
           file: solutions/ermolin.cpp
           language: g++17
           used-for-pretests: true
           used-as-example: true
           expected-verdicts:
               - TL
               - ML
       starkov:
           name: Svyatoslav Starkov.
           file: solutions/starkov.cpp
           language: g++17
           used-for-pretests: true
           used-as-example: false
           expected-verdicts:
               - OK

   # Relative paths to problem's files

   # Test set configuration
   # Might be created during build of the problem
   tests_config: config/tests.json

   # Scripts to build/clean the problem and checker script
   scripts:
       builder: scripts/doall.sh
       wiper: scripts/wipeall.sh
       validator: scripts/validate.sh
       checker: scripts/check.sh

   # Localized visualizers
   visualizer:
       ru_RU:
           html: "/visualizer/ru_RU/visualizer-ru_RU.html"
           css: "/visualizer/ru_RU/visualizer-ru_RU.css"
           js: "/visualizer/ru_RU/visualizer-ru_RU.js"
       en_US:
           html: "/visualizer/en_US/visualizer-en_US.html"
           css: "/visualizer/en_US/visualizer-en_US.css"
           js: "/visualizer/en_US/visualizer-en_US.js"

   # Localized statements
   statements:
       ru_RU:
           pdf: "/statements/ru_RU/statements-ru_RU.pdf"
           html: "/statements/ru_RU/statements-ru_RU.html"
           epub: "/statements/ru_RU/statements-ru_RU.epub"
       en_US:
           pdf: "/statements/en_US/statements-en_US.pdf"
           html: "/statements/en_US/statements-en_US.html"
           epub: "/statements/en_US/statements-en_US.epub"

   # Other files you may want to share with users.
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
             ru_RU: Крестики-Нолики
             en_US: Tic Tac Toe

description
^^^^^^^^^^^
   **Required: false**

   Suggested brief description of the problem.

   Example
      .. code-block:: yaml

         decription: The game may be a bit boring, but don't you like it after all?

difficulty
^^^^^^^^^^
   **Required: false**

   Suggested estimated difficulty from 1 to 5.

   Example
      .. code-block:: yaml

         difficulty: 1

time-limit
^^^^^^^^^^
   **Required: false**

   Suggested per-move time limit for the problem (in microseconds).

   Example
      .. code-block:: yaml

         time-limit: 1000

memory-limit
^^^^^^^^^^^^
   **Required: false**

   Suggested per-move memory limit for the problem (in microseconds).

   Example
      .. code-block:: yaml

         memory-limit: 268435456

minimal/maximal players
^^^^^^^^^^^^^^^^^^^^^^^
   **Required: true**

   Minimal and maximal players number for the game.

   Example
      .. code-block:: yaml

         # Minimal and maximal players number
         minimal_players: 2
         maximal_players: 2

solutions
^^^^^^^^^
   **Required: false**

   Author's and tester's solutions.
   Language is one of the supported languages (TODO addreference).
   List **all** possible verdicts of the solution in the expected-verdicts. It is needed to verify that
   checker performs challenges correctly.
   Other fields are self-explanatory.


   Example
      .. code-block:: yaml

         # Example of solutions to be checked. 
         solutions:
             ermolin:
                 name: Nikolay Ermolin.
                 file: solutions/ermolin.cpp
                 language: g++17
                 used-for-pretests: true
                 used-as-example: true
                 expected-verdicts:
                     - TL
                     - ML
             starkov:
                 name: Svyatoslav Starkov.
                 file: solutions/starkov.cpp
                 language: g++17
                 used-for-pretests: true
                 used-as-example: false
                 expected-verdicts:
                     - OK

tests_config
^^^^^^^^^^^^
   **Required: true**

   JSON file, which stores tests' configuration. May be created during run of ``build.sh`` script.

   Example
      .. code-block:: yaml

          tests_config: config/tests.json

scripts
^^^^^^^
   
   Files of problem scripts.

   (TODO addreference)
   Builder is preparing the problem for working, wiper removes all the files created by builder.
   Validator script reads test file and strctly checks it validity. Read more. 
   Checker script starts runs challenges and produces logs. Read more.

   builder
      **Required: false**
   wiper
      **Required: false**
   validator
      **Required: false**
   checker
      **Required: true**

   Example
      .. code-block:: yaml

         scripts:
             builder: scripts/doall.sh
             wiper: scripts/wipeall.sh
             validator: scripts/validate.sh
             checker: scripts/check.sh


visualizer
^^^^^^^^^^
   **Required: true**

   Visualizer web page files, localized for several languages.

   Example
      .. code-block:: yaml

         visualizer:
             ru_RU:
                 html: "/visualizer/ru_RU/visualizer-ru_RU.html"
                 css: "/visualizer/ru_RU/visualizer-ru_RU.css"
                 js: "/visualizer/ru_RU/visualizer-ru_RU.js"
             en_US:
                 html: "/visualizer/en_US/visualizer-en_US.html"
                 css: "/visualizer/en_US/visualizer-en_US.css"
                 js: "/visualizer/en_US/visualizer-en_US.js"

statements
^^^^^^^^^^
   **Required: true**

   Statements of the problem, given in different formats and different languages.

   Example
      .. code-block:: yaml

         ru_RU:
             pdf: "/statements/ru_RU/statements-ru_RU.pdf"
             html: "/statements/ru_RU/statements-ru_RU.html"
             epub: "/statements/ru_RU/statements-ru_RU.epub"
         en_US:
             pdf: "/statements/en_US/statements-en_US.pdf"
             html: "/statements/en_US/statements-en_US.html"
             epub: "/statements/en_US/statements-en_US.epub"


public_files
^^^^^^^^^^^^
   **Required: false**

   Any other files that you want to share with the problem.

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

Actually, you may omit any informatoin except ``id``. If your game is not about different levels, create only one test.


Builder
=======

Builder is the script aiming to build the problem from sources. It usually includes:

- Compiling visualizer
- Compiling checker, test_generator, validator
- Generating tests and test config

Also, you need to create wiper, which cancels all the changes. It is checked by the problem verifier.
(TODO / TODISCUSS. Do we need it? Why can we just ``git stash`` or smth like that?) 

Checker
=======

Checker is a script used to perform challenges between multiple players. Interface works as follows:

Command line arguments

   --players_cmds cmds
      ready bash commands to start solutions
   --players_files files
      binary files of the solutions (needed for firejail)
   --test_id id
      test id number
   --test_file file
      test file (if was mentioned)
   --time_limit seconds
      Per move time limit
   --memory_limit bytes
      RAM limit
   --streams_log_file file
      Filename of the output stdin/stdout/stderr log
   --game_log_file file
      Filename of the output game log
   --result_log_file file
      Filename of the output game result file, includes scores and verdicts.

Checker should produce logs of the following format:

Any checker logs written to stderr will be saved for internal use.

TODO streams, game, result log format

Validator
=========

Validator is a script, which reads test file from the stdin and checks it for validity.
Any logs written to stderr will be saved for internal use. If test is incorrect, validator must finish the process
with non-zero exit cdde. Validation is performed automatically by ``problem-verifier``.

Solutions
=========

Authors' and testers' solutions are used

- as examples for different programming languages
- to run pretests
- to verify that checker works in a correct way.

Include solutions with different expected verdicts to check that problem perfroms challenges correctly.
You do not need to compile solution file in ``build.sh``, problem verifier will do it.

Statements
==========

Statements are usually written in LaTex and compiled into 3 diffeent formats(pdf, html, epub) for comfortable usage.
You need to compile separate statements for each language. They must be compiled during ``build.sh``.

Visualizator
============

Visualizer is a webpage, which uses API (how? TODO) to download logs of the challenge and visualize the game in a
convinient way.

TODO