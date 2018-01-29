### Potentials
<p style="font-size:85%;">The following potentials are used for benchmarking:</p>

* <p style="font-size:85%;"><b>in.intel.airebo</b> - Polyethylene benchmark with AIREBO</p>
* <p style="font-size:85%;"><b>in.intel.dpd</b> - Dissipative Particle Dynamics</p>
* <p style="font-size:85%;"><b>in.intel.eam</b> - Copper benchmark with Embedded Atom Method</p>
* <p style="font-size:85%;"><b>in.intel.lc</b> - Liquid Crystal w/ Gay-Berne potential</p>
* <p style="font-size:85%;"><b>in.intel.lj</b> - Atomic fluid (LJ Benchmark)</p>
* <p style="font-size:85%;"><b>in.intel.rhodo</b> - Protein (Rhodopsin Benchmark)</p>
* <p style="font-size:85%;"><b>in.intel.sw</b> - Silicon benchmark with Stillinger-Weber</p>
* <p style="font-size:85%;"><b>in.intel.tersoff</b> - Silicon benchmark with Tersoff</p>
* <p style="font-size:85%;"><b>in.intel.water</b> - Coarse-grain water benchmark using Stillinger-Weber</p>

### Benchmarking results
<p style="font-size:85%;">In order to investigate the performance of different acceleration packages, the following four modes are used:</p>

* <p style="font-size:85%;"><b>“ref”</b> – default MPI only implementation. No acceleration packages used</p>
* <p style="font-size:85%;"><b>“use-intel”</b> – a package with optimised implementation for Intel CPUs by running in single, mixed, or double precision with vectorization</p>
* <p style="font-size:85%;"><b>“user-omp”</b> – a package with optimised OpenMP implementation</p>
* <p style="font-size:85%;"><b>“kokkos”</b> – a package with optimised implementation using Kokkos accelerator abstraction framework</p>

<p style="font-size:85%;">Each potential is run with the four modes above.</p>
------

<p style="font-size:85%;"><b>AIREBO:</b></p>
<p style="font-size:85%;">The plot on the left hand side below shows the performance for a fixed-size system (522240 atoms) scaling from 1 computer node to 10, 
whilst the plot on the right hand side is the scaled-size performance for runs with 18651 atoms/proc. Click on the picture for the large version.</p>
[<img src="../img/airebo_strong_perf.png" width="300" height="225">](../img/airebo_strong_perf.png) [<img src="../img/airebo_weak_perf.png" width="300" height="225">](../img/airebo_weak_perf.png)

------

<p style="font-size:85%;"><b>DPD:</b></p>
<p style="font-size:85%;">The plot on the left hand side below shows the performance for a fixed-size system (512000 atoms) scaling from 1 computer node to 10, 
whilst the plot on the right hand side is the scaled-size performance for runs with 18286 atoms/proc. Click on the picture for the large version.</p>
[<img src="../img/dpd_strong_perf.png" width="300" height="225">](../img/dpd_strong_perf.png) [<img src="../img/dpd_weak_perf.png" width="300" height="225">](../img/dpd_weak_perf.png)

------

<p style="font-size:85%;"><b>EAM:</b></p>
<p style="font-size:85%;">The plot on the left hand side below shows the performance for a fixed-size system (512000 atoms) scaling from 1 computer node to 10, 
whilst the plot on the right hand side is the scaled-size performance for runs with 18286 atoms/proc. Click on the picture for the large version.</p>
[<img src="../img/eam_strong_perf.png" width="300" height="225">](../img/eam_strong_perf.png) [<img src="../img/eam_weak_perf.png" width="300" height="225">](../img/eam_weak_perf.png)

------

<p style="font-size:85%;"><b>LC:</b></p>
<p style="font-size:85%;">The plot on the left hand side below shows the performance for a fixed-size system (524288 atoms) scaling from 1 computer node to 10, 
whilst the plot on the right hand side is the scaled-size performance for runs with 18725 atoms/proc. Click on the picture for the large version.</p>
[<img src="../img/lc_strong_perf.png" width="300" height="225">](../img/lc_strong_perf.png) [<img src="../img/lc_weak_perf.png" width="300" height="225">](../img/lc_weak_perf.png)

------

<p style="font-size:85%;"><b>LJ:</b></p>
<p style="font-size:85%;">The plot on the left hand side below shows the performance for a fixed-size system (512000 atoms) scaling from 1 computer node to 10, 
whilst the plot on the right hand side is the scaled-size performance for runs with 18286 atoms/proc. Click on the picture for the large version.</p>
[<img src="../img/lj_strong_perf.png" width="300" height="225">](../img/lj_strong_perf.png) [<img src="../img/lj_weak_perf.png" width="300" height="225">](../img/lj_weak_perf.png)

------

<p style="font-size:85%;"><b>RHODO:</b></p>
<p style="font-size:85%;">The plot on the left hand side below shows the performance for a fixed-size system (512000 atoms) scaling from 1 computer node to 10, 
whilst the plot on the right hand side is the scaled-size performance for runs with 18286 atoms/proc. Click on the picture for the large version.</p>
[<img src="../img/rhodo_strong_perf.png" width="300" height="225">](../img/rhodo_strong_perf.png) [<img src="../img/rhodo_weak_perf.png" width="300" height="225">](../img/rhodo_weak_perf.png)

------

<p style="font-size:85%;"><b>SW:</b></p>
<p style="font-size:85%;">The plot on the left hand side below shows the performance for a fixed-size system (512000 atoms) scaling from 1 computer node to 10, 
whilst the plot on the right hand side is the scaled-size performance for runs with 18286 atoms/proc. Click on the picture for the large version.</p>
[<img src="../img/sw_strong_perf.png" width="300" height="225">](../img/sw_strong_perf.png) [<img src="../img/sw_weak_perf.png" width="300" height="225">](../img/sw_weak_perf.png)

------

<p style="font-size:85%;"><b>TERSOFF:</b></p>
<p style="font-size:85%;">The plot on the left hand side below shows the performance for a fixed-size system (512000 atoms) scaling from 1 computer node to 10, 
whilst the plot on the right hand side is the scaled-size performance for runs with 18286 atoms/proc. Click on the picture for the large version.</p>
[<img src="../img/tersoff_strong_perf.png" width="300" height="225">](../img/tersoff_strong_perf.png) [<img src="../img/tersoff_weak_perf.png" width="300" height="225">](../img/tersoff_weak_perf.png)

------

<p style="font-size:85%;"><b>WATER:</b></p>
<p style="font-size:85%;">The plot on the left hand side below shows the performance for a fixed-size system (512000 atoms) scaling from 1 computer node to 10, 
whilst the plot on the right hand side is the scaled-size performance for runs with 18286 atoms/proc. Click on the picture for the large version.</p>
[<img src="../img/water_strong_perf.png" width="300" height="225">](../img/water_strong_perf.png) [<img src="../img/water_weak_perf.png" width="300" height="225">](../img/water_weak_perf.png)

------
