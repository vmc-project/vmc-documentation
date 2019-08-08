+++
title = "Geometry Definition & Navigation"
menuTitle = "Geometry Definition & Navigation"
chapter = false
hidden = false
showhidden = true
weight = 1
+++

<p>The VMC support two ways of geometry definition:</p>
<ul>
    <li>via <a href="ftp://root.cern.ch/root/doc/chapter19.pdf">           Root geometry package (TGeo) </a></li>
    <li>via <a href="../../../../../../../root/htmldoc/TVirtualMC.html">            TVirtualMC interface </a>  (historically the first way)</li>
</ul>
<p>The first (newer) way is recommended for new users, the way via VMC is kept for   a backward compatibility.</p>
<p>Since the version 2.0, user can choose between Geant4 native navigation and G4Root navigation, if geometry is define via TGeo. The choice of the navigation is done via the option specified with creating <a href="http://ivana.home.cern.ch/ivana/g4vmc_html/classTG4RunConfiguration.html">  TG4RunConfiguration </a> object (see   <a href="http://ivana.home.cern.ch/ivana/examples_html/index.html">  examples </a> for more details):</p>
<ul>
    <li><font face="Courier" color="#00aa00">            <kbd><code>geomVMCtoGeant4</code></kbd> </font>  - geometry defined via VMC, G4 native navigation</li>
    <li><font face="Courier" color="#00aa00">            <code>geomVMCtoRoot</code> </font>     - geometry defined via VMC, Root navigation</li>
    <li><font face="Courier" color="#00aa00">            <code>geomRoot</code> </font>          - geometry defined via Root, Root navigation</li>
    <li><font face="Courier" color="#00aa00">            <code>geomRootToGeant4</code> </font>  - geometry defined via Root, G4 native navigation</li>
    <li><font face="Courier" color="#00aa00">           <code>geomGeant4</code> </font>        - geometry defined via Geant4, G4 native navigation</li>
</ul>
<p>Below we shortly comment the implementation of these options:</p>
<ul>
    <li><font face="Courier" color="#00aa00"><code>geomVMCtoGeant4</code> </font>  <br />
    The interfaces to functions for geometry definitions provided by VMC were strongly inspired by Geant3; the implementation of these functions in Geant4 VMC is therefore made with use of the G3toG4 tool provided by Geant4.</li>
    <li><font face="Courier" color="#00aa00"> <code>geomVMCtoRoot</code> </font> <br />
    The implementation of this option uses            <a href="../../../../../../../root/htmldoc/TGeoMCGeometry.html">           TGeoMCGeometry </a> class provided in the root/vmc package. The same class is used also by TGeant3TGeo and TFluka for supporting user geometry defined via VMC.</li>
    <li><font face="Courier" color="#00aa00"><code>geomRoot</code> </font> <br />
    If geometry is defined via Root and G4Root navigation is selected, Geant4 VMC only converts the parameters defined in <a href="../../../../../../../root/htmldoc/TGeoMedium.html">            TGeoMedium </a> objects to Geant4 objects.</li>
    <li><font face="Courier" color="#00aa00"><code>geomRootToGeant4</code> </font> <br />
    The geometry defined via TGeo is converted in Geant4  geometry           using the external           <a href="http://ivana.home.cern.ch/ivana/VGM.html">           Virtual Geometry Model </a> (VGM), which has replaced the old one-way converters from Geant4 VMC (G4toXML, RootToG4), removed from Geant4 VMC with the version 1.7. In the VGM, these convertors has been generalized and improved.</li>
    <li><font face="Courier" color="#00aa00"><code>geomGeant4</code> </font> <br />
    User Geant4 detector construction class can be passed to Geant4 VMC via user defined run configuration class (see <a href="http://root.cern.ch/drupal/content/user-geant4-classes"> this page </a>). If Geant4 VMC is built with VGM, geometry can be exported in Root using the built-in command:<br />
    <bash>/vgm/generateRoot 
</bash> and reused in Geant3 or Fluka (when available) VMC simulation.</li>
</ul>

<h3>Useful commands</h3>

Geant4 VMC implements various commands which allow users to get more information about their application setup:
<bash>/mcDet/printMaterials
/mcDet/printMaterialsProperties
/mcDet/printMedia
/mcDet/printVolumes
   prints materials, material properties, tracking media, volumes
</bash>

<p>
In Geant4 VMC, there is by default set a step limit 10 cm for all materials with density lower than 0.001 g/cm3. In the development version (SVN trunk), this default setting can be overridden via the following commands:
<bash>/mcDet/setLimitDensity 1.e-6 g/cm3
/mcDet/setMaxStepInLowDensityMaterials 1 mm
</bash>

<p>
How to apply Geant4 commands in a Root user session is explained at the section on <a href="/drupal/content/user-interfaces"> User Interfaces</a>. 

<h3>Geometry in XML</h3>
<p>The VGM provides also the XML exporters which enable to generate XML files with geometry description in the AGDD or GDML formats. If Geant4 VMC is compiled with USE_VGM option, geometry can be exported to XML using the built-in commands: </p>
<bash>
/vgm/generateAGDD [volumeName]
/vgm/generateGDML [volumeName]
</bash>
If volumeName is not specified, the whole geometry volume tree is exported.</p>
<p>The geometry can be then browsed and visualized using  <a href="http://hrivnac.home.cern.ch/hrivnac/Activities/Packages/GraXML">  GraXML tool </a>.</p>

<h3>"MANY" positions</h3>
<p>As Geant4 does not support overlapping geometries, the positions with MANY option  are not allowed with Geant4 navigation.</p>
<p>In case of <font face="Courier" color="#00aa00"><code>geomVMCtoGeant4</code></font> option, user has a possibility to identify all ONLY volumes that overlap with the MANY ones using TVirtualMC::Gsbool() function for each overlap. This info is then used to perform automatically Boolean operations on the concerned solids. The volume with a "MANY" position can have only this position if Gsbool is used.</p>
<p>In case of <font face="Courier" color="#00aa00"><code>geomRootToGeant4</code></font> option, the MANY option is ignored and if present in geometry, Gean4 geometry will be incorrect.</p>
<p>There is no limitation on use of the MANY option with G4Root navigation  (options <font face="Courier" color="#00aa00">            <code>geomVMCtoRoot</code></font><font face="Courier">, </font><font face="Courier" color="#00aa00">            <code>geomRoot</code></font>).

