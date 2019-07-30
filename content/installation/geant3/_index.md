+++
title = "Installing Geant3"
menuTitle = "Geant3"
chapter = false
hidden = false
showhidden = true
weight = 2
+++

<p>The following instructions apply to the installation since the version 2.0. For the installation of the previous versions (1.x) see <a href="/drupal/content/installing-geant3-older-versions"> Installing geant3 - Older Versions</a> page.</p>

<h3>Geant3 (with VMC)</h3>

<p>
Geant3 with VMC requires ROOT.
</p>

<p>Since version 2.0, Geant3 with VMC is installed with CMake. To install geant3:</p>

<ol>
	<li>First get the Geant3 source from the <a href="/drupal/content/download-vmc">Download page</a>. We will assume that the Geant3 package sits in a subdirectory

<bash>/mypath/geant3
</bash>
	</li>
	<li>Create build directory alongside our source directory
	<bash>
$ cd /mypath
$ mkdir geant3_build
$ ls
 geant3 geant3_build    
</bash>
	</li>
	<li>To configure the build, change into the build directory and run CMake:
	<bash>
$ cd /mypath/geant3_build 
$ cmake -DCMAKE_INSTALL_PREFIX=/mypath/geant3_install /mypath/geant3
</bash>

	<p>If ROOT environment was defined using thisroot.{c}sh script, there is no need to provide the path to its installation. Otherwise, they can be provided using -DROOT_DIR cmake option.</p>
        <p> Since version 2.1, the Geant3 library is built by default in <code>RelWithDebInfo</code> build mode (Optimized build with debugging symbols).
        This default can be changed via the standard CMake option CMAKE_BUILD_TYPE. The other useful values are <br>
        <code>Release</code> : Optimized build, no debugging symbols <br>
        <code>Debug</code> : Debugging symbols, no optimization <br>
       </p>
	</li>
	<li>After the configuration has run, CMake will have generated Unix Makefiles for building Geant3. To run the build, simply execute make in the build directory:
	<bash>
$ make -jN
</bash>
	where N is the number of parallel jobs you require (e.g. if your machine has a dual core processor, you could set N to 2).

	<p>If you need more output to help resolve issues or simply for information, run make as</p>

	<cpp>
$ make -jN VERBOSE=1
</cpp>
	</li>
	<li>Once the build has completed, you can install Geant3 to the directory you specified earlier in CMAKE_INSTALL_PREFIX by running
	<cpp>
$ make install
</cpp>
	</li>
</ol>

For the old instructions see:

- [Older Versions](geant3-old)


