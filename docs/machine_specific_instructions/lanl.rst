LANL Institutional Computing
============================

`LANL IC overview and search <https://int.lanl.gov/hpc/institutional-computing/index.shtml>`_

`DST Calendar <http://hpccalendar.lanl.gov/>`_ (within LANL yellow)

Information about Slurm:

* `Introduction to Slurm at LANL <https://hpc.lanl.gov/job-scheduling/index.html#JobScheduling-IntroductiontoSlurm>`_

* `Basic Slurm Guide for LANL HPC Users <https://hpc.lanl.gov/job-scheduling/basic-slurm-guide-for-lanl-hpc-users.html>`_

* `Slurm Command Summary <https://hpc.lanl.gov/job-scheduling/slurm-commands.html>`_

* `Slurm: Running Jobs on HPC Platforms <https://hpc.lanl.gov/job-scheduling/slurm-commands.html#SlurmCommands-SlurmJobSubmission>`_

* `example of batch scripts <https://hpc.lanl.gov/job-scheduling/basic-slurm-guide-for-lanl-hpc-users.html#BasicSlurmGuideforLANLHPCUsers-BatchScriptGenerator>`_

Machine specifications: `grizzly <https://hpc.lanl.gov/platforms/grizzly.html>`_
`badger <https://hpc.lanl.gov/platforms/badger.html>`_
`turquoise network <https://hpc.lanl.gov/networks/turquoise-network/index.html>`_

login: ``ssh -t $my_moniker@wtrw.lanl.gov ssh gr-fe`` or ``ba-fe``

File locations:

* small home directory, for start-up scripts only: ``/users/$my_moniker``

* home directory, backed up: ``/usr/projects/climate/$my_moniker``

* scratch space, not backed up: ``/lustre/scratch3/turquoise/$my_moniker`` or
  ``scratch4``

Check compute time:

* ``sacctmgr list assoc user=$my_moniker format=Cluster,Account%18,Partition,QOS%45``

* Which is my default account? sacctmgr list user $my_moniker

* ``sshare -a | head -2; sshare -a | grep $ACCOUNT | head -1``

* ``sreport -t Hours cluster AccountUtilizationByUser start=2019-12-02 | grep $ACCOUNT``

* check job priority: ``sshare -a | head -2; sshare -a | grep $ACCOUNT``

* `LANL Cluster Usage Overview <https://hpcinfo.lanl.gov>`_ (within LANL yellow)

Check disk usage:

* your home space: ``chkhome``

* total disk usage in Petabytes: ``df -BP |head -n 1; df -BP|grep climate; df -BP |grep scratch``

Archiving

* `turquoise HPSS archive <https://hpc.lanl.gov/data/filesystems-and-storage-on-hpc-clusters/hpss-data-archive/index.html>`_

* archive front end: ``ssh -t $my_moniker@wtrw.lanl.gov ssh ar-tn``

* storage available at: ``cd /archive/<project_name>``

* you can just copy files directly into here for a particular project.

git and compass environment, for all LANL IC machines:

.. code-block:: bash

    module load git
    module use /usr/projects/climate/SHARED_CLIMATE/modulefiles/all/
    module unload python
    source /usr/projects/climate/SHARED_CLIMATE/anaconda_envs/load_legacy_compass.sh

Example compass config file for LANL IC: ``general.config.ocean_turq``

LANL uses slurm. To obtain an interactive node:

.. code-block:: bash

    salloc -N 1 -t 2:0:0 --qos=interactive

Use ``--account=ACCOUNT_NAME`` to change to a particular account.

grizzly, gnu
------------

.. code-block:: bash

    module use /usr/projects/climate/SHARED_CLIMATE/modulefiles/all
    module load gcc/5.3.0
    module load openmpi/1.10.5 netcdf/4.4.1 parallel-netcdf/1.5.0 pio/1.7.2
    make gfortran

Hint: you can put the following line in your bashrc:

.. code-block:: bash

    alias mlgnu='module purge; module load git; module use /usr/projects/climate/SHARED_CLIMATE/modulefiles/all/; module load gcc/5.3.0 openmpi/1.10.5 netcdf/4.4.1 parallel-netcdf/1.5.0 pio/1.7.2; module unload python; source /usr/projects/climate/SHARED_CLIMATE/anaconda_envs/load_legacy_compass.sh; echo "loading modules anaconda, gnu, openmpi, netcdf, pnetcdf, pio for grizzly"'

grizzly, intel 19 with scorpio
------------------------------

.. code-block:: bash

    module purge
    module load friendly-testing
    module load intel/19.0.4 intel-mpi/2019.4 hdf5-parallel/1.8.16 pnetcdf/1.11.2 netcdf-h5parallel/4.7.3 mkl/2019.0.4
    export NETCDF=/usr/projects/hpcsoft/toss3/grizzly/netcdf/4.7.3_intel-19.0.4_intel-mpi-2019.4_hdf5-1.8.16
    export PNETCDF=/usr/projects/hpcsoft/toss3/grizzly/pnetcdf/1.11.2_intel-19.0.4_intel-mpi-2019.4_hdf5-1.8.16
    export PIO=/usr/projects/climate/xylar/libraries/grizzly/scorpio-1.1.6-intel

    make intel-mpi USE_PIO2=true
    # may also use: OPENMP=true DEBUG=true GEN_F90=true

grizzly, intel 17 and pio 1
---------------------------

*Note: Intel 19 above is preferred*

.. code-block:: bash

    module purge
    module use /usr/projects/climate/SHARED_CLIMATE/modulefiles/all
    module load intel/17.0.1
    module load openmpi/1.10.5 netcdf/4.4.1 parallel-netcdf/1.5.0 pio/1.7.2
    make ifort

badger, gnu
-----------

.. code-block:: bash

    module use /usr/projects/climate/SHARED_CLIMATE/modulefiles/spack-lmod/linux-rhel7-x86_64

    # IC mods
    module load gcc/6.4.0
    module load openmpi/2.1.2
    module load cmake/3.12.1
    module load mkl

    # spack mods
    module load openmpi/2.1.2-bheb4xe/gcc/6.4.0/netcdf/4.4.1.1-zei2j6r
    module load openmpi/2.1.2-bheb4xe/gcc/6.4.0/netcdf-fortran/4.4.4-v6vwmxs
    module load openmpi/2.1.2-bheb4xe/gcc/6.4.0/parallel-netcdf/1.8.0-2qwcdbn
    module load openmpi/2.1.2-bheb4xe/gcc/6.4.0/pio/1.10.0-ljj73au

    export NETCDF=/usr/projects/climate/SHARED_CLIMATE/software/badger/spack-install/linux-rhel7-x86_64/gcc-6.4.0/netcdf-fortran-4.4.4-v6vwmxsv33t7pmulojlijwdbikrvmwkc
    export PNETCDF=/usr/projects/climate/SHARED_CLIMATE/software/badger/spack-install/linux-rhel7-x86_64/gcc-6.4.0/parallel-netcdf-1.8.0-2qwcdbnjcq5pnkoqpx2s7um3s7ffo3xd
    export PIO=/usr/projects/climate/SHARED_CLIMATE/software/badger/spack-install/linux-rhel7-x86_64/gcc-6.4.0/pio-1.10.0-ljj73au6ctgkwmh3gbd4mleljsumijys/

    make gfortran


Building Scorpio on Grizzly
---------------------------

Installation of PIO follows from the following pre-existing module files:

.. code-block:: bash

    module purge
    module load friendly-testing
    module load intel/19.0.4 intel-mpi/2019.4 hdf5-parallel/1.8.16 pnetcdf/1.11.2 netcdf-h5parallel/4.7.3 mkl/2019.0.4
    # note the following MPAS-O assumed location variables
    export NETCDF=/usr/projects/hpcsoft/toss3/grizzly/netcdf/4.7.3_intel-19.0.4_intel-mpi-2019.4_hdf5-1.8.16/
    export PNETCDF=/usr/projects/hpcsoft/toss3/grizzly/pnetcdf/1.11.2_intel-19.0.4_intel-mpi-2019.4_hdf5-1.8.16/

.. note::

    DO NOT use openmpi/3.1.5 as there is a bug (RMIO
    `Output from MPAS-O unreadable for large 1.8M cell mesh <https://github.com/MPAS-Dev/MPAS-Model/issues/576>`_).

PIO2 from `E3SM-Project/scorpio <https://github.com/E3SM-Project/scorpio>`_
was used, specifically tag ``scorpio-v1.1.0`` with the following build command
(note use of intel compilers):

.. code-block:: bash

    CC=mpiicc FC=mpiifort cmake \
        -DCMAKE_INSTALL_PREFIX=/usr/projects/climate/SHARED_CLIMATE/software/grizzly/pio/1.10.1/intel-19.0.4/intel-mpi-2019.4/netcdf-4.7.3-parallel-netcdf-1.11.2/ \
        -DPIO_ENABLE_TIMING=OFF -DNetCDF_Fortran_PATH=/usr/projects/hpcsoft/toss3/grizzly/netcdf/4.7.3_intel-19.0.4_intel-mpi-2019.4_hdf5-1.8.16 \
        -DPnetCDF_Fortran_PATH=/usr/projects/hpcsoft/toss3/grizzly/netcdf/4.7.3_intel-19.0.4_intel-mpi-2019.4_hdf5-1.8.16 \
        -DNetCDF_C_PATH=/usr/projects/hpcsoft/toss3/grizzly/netcdf/4.7.3_intel-19.0.4_intel-mpi-2019.4_hdf5-1.8.16  \
        -DPnetCDF_C_PATH=/usr/projects/hpcsoft/toss3/grizzly/pnetcdf/1.11.2_intel-19.0.4_intel-mpi-2019.4_hdf5-1.8.16 ..

build with ``make`` and install with ``make install``.  Then, when you want to
use it for MPAS builds, set the following environment variable:

.. code-block:: bash

    export PIO=/usr/projects/climate/SHARED_CLIMATE/software/grizzly/pio/1.10.1/intel-19.0.4/intel-mpi-2019.4/netcdf-4.7.3-parallel-netcdf-1.11.2/
