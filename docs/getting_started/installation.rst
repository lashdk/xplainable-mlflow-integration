Installation
=========================

Quickstart
------------
PyPI is the distribution channel for xplainable release versions. The best way
to install it is with pip::

    pip install xplainable


Optional dependencies
-----------------------
To use xplainable's embedded GUI in jupyter, you will need to install
``xplainable`` with the ``gui`` extra::


    pip install xplainable[gui]


To use xplainable's advanced plotting functions, you will need to install
``xplainable`` with the ``plotting`` extra::


    pip install xplainable[plotting]


Environment
-------------------------------

Reproducible Installs
~~~~~~~~~~~~~~~~~~~~~

As libraries get updated, results from running your code can change, or your
code can break completely. It's essential to be able to reconstruct the set of
packages and versions you're using. Best practice is to:

 1. use a different environment per project you're working on,
 2. record package names and versions using your package installer; each has it's own metadata format for this:

   * **Conda:** conda environments and environment.yml
   * **Pip:** virtual environments and requirements.txt
   * **Poetry:** virtual environments and pyproject.toml
