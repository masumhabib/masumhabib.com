Does Hyper-Threading Improve Performance of Computationally Intensive Programs?
###############################################################################
:date: 2012-11-17 15:58
:category: Miscellaneous
:slug: does-hyper-threading-improve-performance-of-computationally-intensive-programs
:status: draft

Hyper-Threading (HT) is Intel's one of the latest and greatest
technologies found in almost all of their latest processors. In HT
technology, one physical processor is presented as two logical
processors by duplicating some of the registers and certain parts of the
processor. Each of these processors share the same execution unit. This
means that although your operating system (OS) sees two processors, in
reality your program only runs in one execution unit.

Although Intel claims up to a 30% performance improvement compared to an
otherwise identical, non-simultaneous multithreading processor,
according to
`Wikipedia <http://en.wikipedia.org/wiki/Hyper-threading>`__, the
performance improvement is rather application dependent. Therefore, it
is natural to ask if HT provides any significant performance boost in
computationally intensive softwares. I put this to a test when I was
creating a technical specification for our research group's new compute
cluster.

For the bench-marking tests, I used my Thinkpad E420 laptop with an
Intel(R) Core(TM) i3 onboard running `Gentoo
GNU/Linux <http://www.gentoo.org/>`__. The Core i3 processor has two
physical cores with HT technology. Each physical core has two logical
processors. The HT technology can be enabled or disabled from the BIOS
setup. With the HT enabled and disabled, the OS sees four and two
processors, respectively.

All the bench-marking tests reported below were run from a command
terminal with the GUI disabled (no desktop environment) so that maximum
amount of system resources are available for the test program. Here is
the specification of my test system:

.. raw:: html

   <div align="center">

+-------------------+----------------------------------------------------------------+
| Hardware:         | Lenovo Thinkpad Edge 420 Laptop                                |
+-------------------+----------------------------------------------------------------+
| Processor:        | Intel(R) Core(TM) i3-2310M CPU @ 2.10GHz                       |
+-------------------+----------------------------------------------------------------+
| Memory:           | 4GB DDR2                                                       |
+-------------------+----------------------------------------------------------------+
| OS:               | Gentoo GNU/Linux with customized Linux 3.2.12 x86\_64 kernel   |
+-------------------+----------------------------------------------------------------+
| Math Libraries:   | Standard BLAS and LAPACK libraries from Gentoo repositories.   |
+-------------------+----------------------------------------------------------------+
| Test Program:     | Quantum Espresso, a density functional theory code             |
+-------------------+----------------------------------------------------------------+

.. raw:: html

   </div>

The test program, `Quantum
Espresso <http://www.quantum-espresso.org/>`__ (QE), is a highly
computationally intensive software for calculating electronic structure
of materials using density functional theory. It is a free and open
source software. All the simulations I ran for this bench-marking can be
found in the installation folder in a standard QE setup.

In order to make sure that QE actually runs parallel in multiple CPUs, I
first turned off the HT technology and ran the same simulation using 1
CPU and 2 CPUs. The time it takes to complete the simulations are as
follows:

.. raw:: html

   <div align="center">

.. raw:: html

   <table class="tbl-data">
   <thead>
   <tr>
   <th style="vertical-align: middle; text-align: center;" rowspan="2" width="40%">

Simulations

.. raw:: html

   </th>
   <th style="text-align: center;" colspan="2">

Time (sec)

.. raw:: html

   </th>
   </tr>
   <tr>
   <th style="text-align: center;">

1 CPU

.. raw:: html

   </th>
   <th style="text-align: center;">

2 CPUs

.. raw:: html

   </th>
   </tr>
   </thead>
   <tbody>
   <tr>
   <td>

Si band structure under static electric field

.. raw:: html

   </td>
   <td style="text-align: center;">

375

.. raw:: html

   </td>
   <td style="text-align: center;">

216

.. raw:: html

   </td>
   </tr>
   </tbody>
   </table>

.. raw:: html

   </div>

The data presented in the table above confirms that in two physical CPUs
the simulation runs 1.74 times as fast as it runs in one physical CPU.
Having confirmed the scaling of the program, its now time to examine the
HT. In the table below, we present completion times for four simulations
using two physical CPUs and four logical CPUs (using the HT technology).

.. raw:: html

   <div align="center">

.. raw:: html

   <table class="tbl-data">
   <thead>
   <tr>
   <th style="vertical-align: middle; text-align: center;" rowspan="2" width="40%">

Simulation

.. raw:: html

   </th>
   <th style="text-align: center;" colspan="2">

Time (sec)

.. raw:: html

   </th>
   </tr>
   <tr>
   <th style="text-align: center;">

HT off
 (2 physical CPUs)

.. raw:: html

   </th>
   <th style="text-align: center;">

HT on
 (4 logical CPUs)

.. raw:: html

   </th>
   </tr>
   </thead>
   <tbody>
   <tr>
   <td>

Geometry optimization of CO

.. raw:: html

   </td>
   <td style="text-align: center;">

48

.. raw:: html

   </td>
   <td style="text-align: center;">

56

.. raw:: html

   </td>
   </tr>
   <tr>
   <td>

FeO band structure using LDA+U

.. raw:: html

   </td>
   <td style="text-align: center;">

60

.. raw:: html

   </td>
   <td style="text-align: center;">

60

.. raw:: html

   </td>
   </tr>
   <tr>
   <td>

Si band structure under static electric field

.. raw:: html

   </td>
   <td style="text-align: center;">

216

.. raw:: html

   </td>
   <td style="text-align: center;">

223

.. raw:: html

   </td>
   </tr>
   <tr>
   <td>

Self test

.. raw:: html

   </td>
   <td style="text-align: center;">

721

.. raw:: html

   </td>
   <td style="text-align: center;">

738

.. raw:: html

   </td>
   </tr>
   </tbody>
   </table>

.. raw:: html

   </div>

The data shown in the table above clearly demonstrates that there is no
performance gain when the HT is enabled. Rather, for some cases
performance is degraded with the HT enabled.

The lack of performance gain, however, is not surprising considering
that in the HT technology one physical execution unit is shared by two
logical processors. This can be understood as follows. Suppose we have
one physical CPU with the HT enabled which means that our programs and
the OS sees two logical CPUs. So, if we schedule two computationally
intensive programs (threads) to run in these two logical CPUs, one
program (thread) has to wait while the other is running since only one
execution unit is shared by these two programs. This is equivalent to
running these two programs one after another in one physical CPU. Thus,
in one physical CPU the program would run almost as fast as in two
logical CPUs. If we used two physical CPUs instead of two logical CPUs,
both programs (threads) would run simultaneously in two separate
execution units.

Unlike the computationally intensive programs, regular programs like
word processors and browsers should gain significant performance
improvement using the HT technology. These programs perform a lot of
data read/write from/to disks and networks. So, when one program is
waiting for data read/write, other program can use the execution unit
and thereby improve the performance.

Although in these tests I used an ordinary laptop equipped with an
ordinary PC CPU, the result should hold for server-grade Intel Xeon
processors as well.

So, if you are planning on building a computer to run your simulations,
a compute cluster or a super computer, you might want to keep in mind
that the HT technology will not be able to provide a performance boost.
