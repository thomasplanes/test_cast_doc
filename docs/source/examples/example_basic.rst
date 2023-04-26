Basic example
=============

FaIR v2.1 is object-oriented and designed to be more flexible than its
predecessors. This does mean that setting up a problem is different to
before - gone are the days of 60 keyword arguments to ``fair_scm`` and
we now use classes and functions with fewer arguments that in the long
run should be easier to use. Of course, there is a learning curve, and
will take some getting used to. This tutorial aims to walk through a
simple problem using FaIR 2.1.

The structure of FaIR 2.1 centres around the ``FAIR`` class, which
contains all information about the scenario(s), the forcer(s) we want to
investigate, and any configurations specific to each species and the
response of the climate.

A run is initialised as follows:

::

   f = FAIR()

To this we need to add some information about the time horizon of our
model, forcers we want to run with, their configuration (and the
configuration of the climate), and optionally some model control
options:

::

   f.define_time(2000, 2050, 1)
   f.define_scenarios(['abrupt', 'ramp'])
   f.define_configs(['high', 'central', 'low'])
   f.define_species(species, properties)
   f.ghg_method='Myhre1998'

We generate some variables: emissions, concentrations, forcing,
temperature etc.:

::

   f.allocate()

which creates ``xarray`` DataArrays that we can fill in:

::

   fill(f.emissions, 40, scenario='abrupt', specie='CO2 FFI')
   ...

Finally, the model is run with

::

   f.run()

Results are stored within the ``FAIR`` instance as ``xarray`` DataArrays
or Dataset, and can be obtained such as

::

   print(fair.temperature)

Multiple ``scenarios`` and ``configs`` can be supplied in a ``FAIR``
instance, and due to internal parallelisation is the fastest way to run
the model (100 ensemble members per second for 1750-2100 on my Mac for
an emissions driven run). The total number of scenarios that will be run
is the product of ``scenarios`` and ``configs``. For example we might
want to run three emissions ``scenarios`` – let’s say SSP1-2.6, SSP2-4.5
and SSP3-7.0 – using climate calibrations (``configs``) from the UKESM,
GFDL, MIROC and NorESM climate models. This would give us a total of 12
ensemble members in total which are run in parallel.
