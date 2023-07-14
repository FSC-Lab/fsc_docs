C++ Style Guide
***************

Automatic Tooling
=================

We **encourage** you to use automatic tools (a *formatter*) to take
charge of formatting. If you use a formatter and follow our guidelines
in using it, you *may* skip reading about code style.

We use the ``clang-format`` formatter. You can obtain it in two ways:

#. If you are using Vscode, install the ``vscode-clangd`` extension from
   the Vscode marketplace. Accept all prompts during installation.

   ``clangd`` is a language server that provides ``clang-format`` as an
   integral component alongside more advanced syntax highlighting and
   autocomplete features. `More details
   here <https://marketplace.visualstudio.com/items?itemName=llvm-vs-code-extensions.vscode-clangd>`__.

   Additionally, you are strongly recommended to turn on
   ``editor.formatOnSave``

#. If you work on the command-line, install the standalone
   ``clang-format`` tool with

   ::

      sudo apt-get install clang-format

   then format some file of your choosing by running

   ::

      clang-format -i <my_file>.cpp

The behavior of ``clang-format`` is controlled by a configuration file.
To be consistent with the rest of us, put a file named ``.clang-format``
with the following content in your ``HOME`` directory, i.e. usually
``/home/<username>`` on linux systems

::

   ---
   BasedOnStyle: Google
   ---
   IndentWidth: 2
   ColumnLimit: 80
   Language: Cpp
   AlignTrailingComments: true

Style
=====

Style guide
-----------

Indentation
~~~~~~~~~~~

Indention **will** be 2 spaces everywhere. Tabs **will not** be used.

-  Right

   .. code:: c++

      int main(int argc, char** argv) {
        if (argc > 1) {
          std::cout << "Hello: " << argv[1] << "\n";
        } else {
          std::cout << "Hello world!\n";
        }
      }

-  Wrong

   .. code:: c++

      int main() {
          if (argc > 1) {
              std::cout << "Hello: " << argv[1] << "\n";
          } else {
              std::cout << "Hello world!\n"; // Wrong! 4 spaces
          }
      }

Braces
~~~~~~

Opening braces **shall** be on the same line as the

-  Function declaration

-  Class/Struct declaration

-  ``if``/``else`` statements

-  ``for``/``while`` statements

that introduced it. Closing braces **shall** be on their own line.

Statements that follow a control-flow statement
``if``/``else``/``for``/``while`` **shall** be enclosed by braces

-  Right

   .. code:: c++

      int DoAction(int condition) {
        // More logic
        for (int i = 0; i < 10; ++i) {
          // More logic
        }
        return result;
      }

      if (condition) {
        return DoAction(condition);
      }

-  Wrong

   .. code:: c++

      int DoAction(int condition) {
        for (int i = 0; i < 10; ++i) 
        { // Wrong! brace on new line
            // More logic
        }
        // More logic
        return result; 
      }

      if (condition) 
        return DoAction(condition); // Wrong! No braces after if

Naming of files
~~~~~~~~~~~~~~~

Filenames **shall** be all lowercase and with underscores between words.

.. list=table:: Table of file extension associations
   :widths: 25 75
   :header-rows: 1

   * - Extension
     - File contents
   * - ``.cpp``
     - C++ source files
   * - ``.c``
     - C source files
   * - ``.hpp``
     - C++ header files
   * - ``.h``
     - C header files OR C++ header files that may be ``#include``-ed by C files

-  Right

   .. code:: bash

      hello_world.cpp
      utility.hpp

-  Wrong

   .. code:: bash

      hello_world.cc # .cc extension 
      HelloWorld.cpp # Wrong case

Naming of types
~~~~~~~~~~~~~~~

Names of *types*, including classes, structures, and aliases to other
types **shall** have a uppercase letter for the first letter of each
word and no separation between words.

-  Right

   .. code:: c++

      class SimulationSolver;
      struct SimulationSolverConfig;
      using CallbackType = std::function<void(bool)>;

-  Wrong

   .. code:: c++

      struct simulation_solver;

Naming of variables
~~~~~~~~~~~~~~~~~~~

Names of common variables **shall** be all lowercase with underscores
between words.

-  Right

   .. code:: c++

      int foo_bar = 1; // snake_case

-  Wrong

   .. code:: c++

      int fooBar = 1; // Java style camelCase is reserved for something else
      int FooBar = 1; // Pascal style CamelCase

Names of private members of classes **shall** be named like common
variables with a trailing underscore

-  Right

   .. code:: c++

      class SimulationSolver {
       private:
        TimeType time_;
        StateType state_;
      }

Names of public members of structures **shall** be named like common
variables

-  Right

   .. code:: c++

      struct SimulationSolverConfig {
        std::string solver_type;
        double fixed_step;
      };

Names of compile-time constants, including ``constexpr`` variables and
enumerators, **shall** have a *k*-prefix, a uppercase letter for the
first letter of each word and no separation between words.

-  Right

   .. code:: c++

      constexpr double kLooseTolerance = 1e-10;
      enum class Colors {
        kRed,
        kGreen,
        kBlue
      };

-  Wrong

   .. code:: c++

      constexpr double LOOSE_TOLERANCE = 1e-10; // Older ALL_CAPS style

*Hungarian notation*, i.e. using alphabetic prefixes to encode qualities
of a variable such as signedness, being a pointer, a boolean flag, a
semaphore, etc. **will not** be used.

-  Wrong

   .. code:: c++

      char** pszOwner; // Pointer-to zero-terminated string

Naming of Functions
~~~~~~~~~~~~~~~~~~~

Names of functions **shall** have a uppercase letter for the first
letter of each word and no separation between words.

-  Right

   .. code:: c++

      double RosenbrockFunction(const std::vector<double>& x);

-  Wrong

   .. code:: c++

      double rosenbrock_function(const std::vector<double>& x);

Names of member functions of classes **shall** have a uppercase letter
for the first letter of each word *except the first* and no separation
between words

-  Right

   .. code:: c++

      class SimulationSolver {
        bool resetSolver(); // Java style camelCase
        // NOTE: First letter is NOT capitalized. 
      };

   Names of *property* style getter and setters **shall** shall be the
   same as that of the underlying variable without the trailing
   underscore

-  Right

   .. code:: c++

      class SimulationSolver {
       public:
        const StateType& state() const { return state_; } // Getter
        StateType& state() { return state_; } // Setter
       private:
        StateType state_;
      }

Naming of macros
~~~~~~~~~~~~~~~~

Names of macros **shall** be all uppercase with an underscore between
words

-  Right

   .. code:: c++

      #define CHECK(expr) do { if (!(expr)) { throw std::runtime_error(#expr " has failed"); } } while (0)

-  Wrong

   .. code:: c++

      // Particularly bad example demonstrated by Windows API
      // Collides with std::max
      #define max(lhs, rhs) ((lhs) > (rhs)) ? (lhs) : (rhs);
