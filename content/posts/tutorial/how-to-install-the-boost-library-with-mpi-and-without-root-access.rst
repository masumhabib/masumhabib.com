How to Install the Boost Library with MPI and without Root Access
#################################################################
:date: 2014-03-04 11:54
:category: Tutorial
:slug: how-to-install-the-boost-library-with-mpi-and-without-root-access
:status: published

**UPDATE-1:** For Boost v1.56.0,
``boost_1_56_0/tools/build/v2/user-config.jam`` is not used by the build
system, use ``boost_1_56_0/project-config.jam`` instead.

Boost is huge collection of C++ libraries. It is complementary to the
standard C++ library. Like all other libraries, you'll need root access
to install the Boost library on Unix and Unix like systems. If you are
not the system admin, you can compile and install it in your home
folder. In this tutorial I'll show how to compile and install Boost in
your home directory without root access.

Boost library has a C++ wrapper (``boost::mpi``) for lower level MPI
routines. By default, the MPI libraries are not enabled in boost. In
this tutorial, we will discuss how to enable and compile ``boost::mpi``
or any other libraries that needs compilation.

In this tutorial I assume that you are familiar with command line and
GNU/Linux. The following procedure should be similar in other Unix like
operating systems.

Download the latest version of boost source code from
`here <http://www.boost.org/users/download/>`__, put it under
``/home/username/src`` folder and untar the archive.

.. code-block:: bash

    cd ~/src
    tar -xvf boost_1_55_0.tar.gz

A new folder named ``boost_1_55_0`` will be created. To configure
boost for your host/cluster, change directory to ``boost_1_55_0`` and
use ``bootstrap.sh`` command.

.. code-block:: bash

    cd boost_1_55_0
    ./bootstrap.sh --help
    ./bootstrap.sh --show-libraries

Here, the ``./bootstrap.sh --help`` command will show you a help
message and ``./bootstrap.sh --show-libraries`` all available
libraries you need to compile.

In order to install Boost in your home directory, use ``--prefix``
option for ``bootstrap.sh``. You can enable or disable libraries using
``--with-libraries`` or ``--without-libraries`` options, respectively.

To build Boost with MPI, use:

.. code-block:: bash

    ./bootstrap.sh --prefix=/home/username/usr --with-libraries=mpi

If you want to build everything issue the following command:

.. code-block:: bash

    ./bootstrap.sh --prefix=/home/username/usr --without-libraries=

To compile ``boost::mpi``, we need to find out the MPI wrapper for C++
compiler (``mpic++``). The ``mpic++`` script comes with MPICH2 (and
probably with OpenMPI) library. To find the path of ``mpic++``, issue
the following command:

.. code-block:: bash

  $ which mpic++

In standard GNU/Linux installations, ``mpic++`` is located at either
``/usr/local/bin`` or ``/usr/bin``. To build MPI add full path to
``mpic++`` or just ``mpic++`` at the end of
``boost_1_55_0/tools/build/v2/user-config.jam`` file:

.. code-block:: bash

    # MPI
    using mpi : /usr/bin/mpic++ ;

or,

.. code-block:: bash

    # MPI
    using mpi : mpic++ ;

Configuration is done, let's make and install it:

.. code-block:: bash

    ./b2 install | tee install.log

the log will be saved in install.log file so that you can go back andcheck if something goes wrong.

Boost libraries will be installed in ``/home/username/usr/lib`` and the
header files will be installed in ``/home/username/usr/include/boost``
folder.

Include boost in your program using:

.. code-block:: bash

    #include <boost/mpi.hpp>
    #include <boost/serialization/string.hpp>
    #include <boost/serialization/access.hpp>
    #include <boost/serialization/split_free.hpp>
    namespace mpi = boost::mpi;

etc. To compile your program, you will need to add
``-I/home/username/usr/include`` option while invoking the compiler.

