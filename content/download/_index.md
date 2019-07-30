+++
menuTitle = "Download"
chapter = false
weight = 1
pre = "<b>1. </b>"
+++

### Chapter 1

# Download

<h2>Tar files (pro versions)</h2>

<h3>Pro versions</h3>

<table cellspacing="0" width="100%">
	<tbody>
		<tr>
			<td width="15%"><font color="#00aa00" size="-0">geant3: </font></td>
			<td width="15%"><i>version 321+_vmc.2.7 </i></td>
			<td width="30%"><a href="https://root.cern.ch/download/vmc/geant3+_vmc.2.7.tar.gz">geant3+_vmc.2.7.tar.gz </a></td>
			<td width="40%"><i>Tested with ROOT 6.14/06 </i></td>
		</tr>
		<tr>
			<td width="15%"><font color="#00aa00" size="-0">geant4_vmc: </font></td>
			<td width="15%"><i>version 4.0.p1 </i></td>
			<td width="30%"><a href="https://root.cern.ch/download/vmc/geant4_vmc.4.0.p1.tar.gz">geant4_vmc.4.0.p1.tar.gz </a></td>
			<td width="40%"><i>Tested with ROOT 6.14/06 <br />
			Geant4 10.05<br />
			(with embedded CLHEP 2.4.1.0),<br />
			VGM 4.4, Garfield master at 07c0ec50a0d3086b3 (*) <br /></i>
                        </td>
		</tr>
		<!-- 
       <tr>
            <td width="15%"><font size="-0" color="#00aa00"> fluka_vmc: </font></td>
            <td width="15%"><i> version 0.5 </i></td>
            <td width="30%">not distributed</td>
            <td width="40%"><i> Tested with ROOT  5.26/00 <br />
            FLUKA 2008.3c.vmc </i
iteration></td>
        </tr>
-->
	</tbody>
</table>
<left>
<p>
(*) with fixes in MR #1 in https://gitlab.cern.ch/garfield/garfieldpp </i>
</p>

<p>In general, the VMC packages can be built with the Root versions which they were tested with and higher, and Geant4 VMC with the Geant4 version which it was tested with including the patches. Note that the Geant4 patches released after the Geant4 VMC tag do not appear in the table above, it is however recommended to update Geant4 with each patch release.</p>

<h3>Old versions</h3>

<table cellspacing="0" width="100%">
	<tbody>
		<tr>
			<td width="15%"><font color="#00aa00" size="-0">geant3: </font></td>
			<td width="15%"><i>version 321+_vmc.2.6 </i></td>
			<td width="30%"><a href="https://root.cern.ch/download/vmc/geant3+_vmc.2.6.tar.gz">geant3+_vmc.2.6.tar.gz </a></td>
			<td width="40%"><i>Tested with ROOT 5.34/38 and 6.14/06 </i></td>
		</tr>
	</tbody>
</table>

<p>&nbsp;</p>

<h2>Git</h2>

<p>The sources are also available from GitHub and can be obtained in the similar way as the ROOT source.</p>

<h3>Download geant3:</h3>

<p>Development version (the whole repository):</p>

```bash 
git clone http://github.com/vmc-project/geant3.git
```

<p>To switch to pro tagged version 2.7:</p>

```bash
cd geant3 
git checkout v2-7
```

<p>To switch to old tagged version 2.6<br />
(For older versions see the correspondent tag and the required version of ROOT in <a href="/root/vmc/geant3+_vmc_versions.txt">the table</a>):</p>

```bash 
cd geant3 
git checkout v2-6
```
<h3>Download geant4_vmc:</h3>

<p>Development version (the whole repository)</p>

```bash 
git clone http://github.com/vmc-project/geant4_vmc.git
```

<p>To switch to tagged tagged version 4.0.p1:</p>

```bash cd geant4_vmc 
git checkout v4-0-p1
```

<p>To switch to old tagged version 3.6.p3 (compatible with Geant4 10.04.x):</p>

```bash 
cd geant4_vmc 
git checkout v3-6-p3
```

<p>The list of new developments, bug fixes and the required versions of ROOT and Geant4 for each version can be found in <a href="/root/vmc/geant4_vmc_versions.txt"> the history file</a>.</p>

<p>Download Geant4 - from the <a href="http://geant4.web.cern.ch/geant4/"> Geant4 Web site </a></p>

<!--
<h3>Download fluka_vmc:</h3>
<p><em>The access to the fluka_vmc SVN repository is restricted.  You need first to obtain a permission to use fluka_vmc from the FLUKA team, by writing (e-mail) to the head of the FLUKA Scientific Committee,</em> <a href="mailto:Giuseppe.Battistoni@mi.infn.it"><em> Giuseppe Battistoni</a><em>; then you can address a request for the access to the fluka_vmc  SVN repository to </em><a href="mailto:peter.hristov@cern.ch"><em>Peter Hristov</em></a><em>. </em></p>
<p>Development version (svn trunk)</p>
<pre class="code">svn co https://alisoft.cern.ch/fluka_vmc/trunk fluka_vmc </pre>
<p>Tagged version 0.5 <br />
(For older versions see the correspondent tag and  the required versions of ROOT and Geant4 in<a href="/root/vmc/fluka_vmc_versions.txt"> the history file</a>):</p>

```bashsvn co https://alisoft.cern.ch/fluka_vmc/tags/v0-5 fluka_vmc </bash>
<p>Download FLUKA:</p>
<p>FLUKA is obtained from the<a href="http://pcfluka.mi.infn.it/"> FLUKA Web site </a>via the standard licensing procedure described on this site.</p>
-->