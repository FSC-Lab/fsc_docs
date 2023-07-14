CMake
=====

We use CMake as the standard C++ build system due to its ubiquity.
This section gets you up to speed on using CMake for our purposes (building research code).

Nevertheless, we encourage you to investigate alternative build systems in the eventual case that your project depends a 3rd project project that is **not** built on CMake.

#. GNU Autotools

   - Notably used by: vim, gcc, etc. and `IPOPT <https://github.com/coin-or/Ipopt>`__

#. `Waf <https://waf.io/>`__

   - Notably used by `Ardupilot <https://github.com/ardupilot/ardupilot.git>`__

#. `Meson <https://mesonbuild.com/>`__

#. `Gradle <https://gradle.org/>`__

   - Used to build C++ code for Android

CMake Scripting
---------------

.. TODO(Longhao Qian)


Advanced
~~~~~~~~

Namespaced Targets
''''''''''''''''''

Dependent projects found by ``find_package`` and ``FetchContent`` (or
``add_subdirectory``-ing a git submodule) differ in one significant aspect.
In the former case, the dependent project's target(s) are properly exported, and given a
namespace, e.g. for GoogleTest

.. code:: cmake

   find_package(GTest REQUIRED)
   # more logic
   target_link_libraries(test_my_library PRIVATE GTest::gtest GTest::gtest_main)

where ``GTest::`` is the namespace. But in the latter case, the dependent project's
targets are **directly** made available, with no provision for namespacing. This is
expected since in this case the dependent project is assumed to be **embedded** into
yours.

Therefore, if you want your project to be acquired by ``FetchContent`` or as a git
submodule, you should emulate namespaces through

.. code:: cmake

   # Unique (and private) library name 
   add_library(mylib_core src/core.cpp) 
   
   # Strip pseudo-namespace since install(...) will give it a namespace
   set_target_properties(mylib_core PROPERTIES EXPORT_NAME core)

   # Define an public library name with a pesudo-namespace as an alias
   add_library(mylib::core ALIAS mylib_core)

   # Install the libraries for find_package
   install(TARGETS mylib_core EXPORT mylibTargets ...)

   # Export the libraries for find_package, adding a true namespace
   install(EXPORT mylibTargets NAMESPACE mylib:: ...)
   




