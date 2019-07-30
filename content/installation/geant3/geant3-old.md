+++
title = "Installing Geant3 - Older Versions"
menuTitle = "Geant3 Older Versions"
chapter = false
hidden = true
weight = 1
+++

The following instructions apply to the installation of the versions < 2.0.

<ol>
<li>First get the Geant3 source from the 
<a href="/drupal/content/download-vmc">Download  page</a>.
<br><br>
<li>No special configuration option for Root installation is needed with the actual version of Root. 
<p>
<ul>
<li>With old Root v5.20/00, you have to specify your Fortran compiler: <br />
<bash>
--with-f77=g77
</bash>
    &nbsp;</li>
</ul>
<li>geant3 requires Pythia6 library.  You can follow  ROOT installation instructions  or directly the           <a href="http://home.thep.lu.se/%7Etorbjorn/pythiaaux/recent.html"> Pythia site. </a>    <br />
    &nbsp;</li>

<li>To install geant3:<br />
<bash>
cd geant3
make
</bash>
</li>
</ol>
