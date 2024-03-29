---
title: QGIS on macOS with Homebrew
date: 2018-03-12 14:31:26
header:
  overlay_image: assets/images/blog/2018/QGIS_logo_2017.svg.png
  teaser: assets/images/blog/2018/QGIS_logo_2017.svg.png
categories:
  - GIS
  - Personal
  - Technology
tags:
  - homebrew
  - macOS
  - QGIS
---
**Update Monday, 26th of March**: Almost the next day or the following day I published this post [KyngChaos](http://www.kyngchaos.com) just published the QGIS 3 release for macOS. You can download [here](http://www.kyngchaos.com/software/qgis).

* * *

Just a couple days ago I was about to write a post about how difficult, or almost impossible, is to install the last versions of QGIS on macOS right now. Everything started, I believe, when [gdal](http://www.gdal.org) package got update at the beginning of the week and that broke my install of QGIS 2 even thought, and to be honest, I really don't know if all this mess was caused for that update, or by the update of the python formulae in Homebrew. Anyhow, usually these problems are solved just rebuilding the app from source using the new packages, but in this occasion that solution wasn't working.

Homebrew decided that on the first of March the behavior of the Python formulae and the name, and I believe the name of the formulae, was going to change to be more consistent. To sum a little bit up, after the change `python` was going to point to `python3` in your shell and `python2` is going point to so `python2` —or something like that. However, the change in the command in the CLI  broke a truckload of third party software, and seems that also wasn't compliant with [Python's policy](https://www.python.org/dev/peps/pep-0394/). So… they decided [to change some things back](https://discourse.brew.sh/t/python-and-pep-394/1813) —CLI commands, no formulae names— and now seems that `python` ➜ `python2` and `python3` ➜ `python3`. If you are confused, don't worry, am I also. Besides, since I'm not an expert in the matter and I've perhaps missed something in all this chaos. In consequence… **[I really don't know!](https://media1.tenor.com/images/499592e35e9cdf56bfdcb11eaf9d891d/tenor.gif?itemid=5607416)**.

Anyhow and right now, with the current state of Homebrew and the [OSGeo/homebrew-osgeo4mac](https://github.com/OSGeo/homebrew-osgeo4mac) tap, it's quite difficult to install the last versions of QGIS in macOS. A few weeks ago, QGIS released QGIS 3 Girona, however it's only possible to install it right now in Windows and Linux. Usually on macOS the releases has been a little bit slower than in other platforms basically because QGIS has really few developers able to work on macOS and seems that things aren't as easy as in other platforms. Traditionally, Homebrew has come to the rescue with [OSGeo/homebrew-osgeo4mac](https://github.com/OSGeo/homebrew-osgeo4mac) tap, which you can build your own version of QGIS, but that repo hasn't been update almost since the beginning of December. This time the delay not only mean that the macOS users not only can't install the last small incremental change version, but that for more than 3 weeks we aren't able to install the last version, which means a big leap from version 2 to 3.

This left the macOS users a little bit hanging although we still can download the _official_ build for macOS in the [KyngChaos](http://www.kyngchaos.com/software/qgis){.reference.external} download page, where you can find the 2.18.14 version. The problem is that version isn't even the last version of the [long-term repository release](https://en.wikipedia.org/wiki/Long-term_support), which is 2.18.17.

Nevertheless, this weekend I was able to rebuild the last version of the LTR of QGIS 2 and the developer version of QGIS3. I really don't know how stable is the QGIS 3 developer version, but at least at first sight everything works and the app even open faster than the QGIS 2.

## How to install the last version of QGIS 2?

What I did was just reinstall `python`, `python2`, `gdal`, `gdal2`, and finally `qgis2`. More or less as follows:

```shell
brew reinstall python2 python bison &&
brew reinstall gdal2 \
    --with-armadillo --with-complete --with-libkml \
    --with-opencl --with-postgresql --with-unsupported
brew reinstall qgis2 --with-grass --with-saga-gis-lts
```

After that and compiling here and there, I was able to open QGIS.app again.

This left me with QGIS in `/usr/local/Cellar/qgis2/2.18.14`. But I want to have QGIS.app in `/Applications` folder so I have two options. First one is run in the following command in the terminal, as advised by Homebrew:

```shell
brew linkapps [--local]
```

However, I prefer to move the actual app to the `/Applications` folder and create a symbolic link on the `/Cellar` folder.

```shell
mv -f /usr/local/Cellar/qgis2/2.18.14/QGIS.app /Applications
ln -s /Applications/QGIS.app /usr/local/Cellar/qgis2/2.18.14/QGIS.app
```

Yet, weren't we going to install the last version of QGIS? This isn't the last version. OK…

### QGIS 2.18.17

To install the last version you need to change the the QGIS formulae to make it to download the last version from the LTR. Since Homebrew isn't anything more than a git with Ruby scripts I recommend you to make a branch in the repository and change the formula. You aren't going to change anything in Homebrew, but in a branch of tap, in the [OSGeo/homebrew-osgeo4mac](https://github.com/OSGeo/homebrew-osgeo4mac) tap. I did the following.

First, I create a branch and checked out on it.

```shell
cd /usr/local/Homebrew/Library/Taps/osgeo/homebrew-osgeo4mac
git checkout -b QGIS2.18.17
brew edit qgis2
```

Now you can edit the formula to be able to install QGIS 2.18.17. ~You can see how I modify mine~~[^1] below that has highlighted the lines I've changed.

```ruby
<pre class="nums:true lang:ruby mark:12-13 decode:true" title="qgis2.rb" data-url="https://raw.githubusercontent.com/luisspuerto/homebrew-osgeo4mac/QGIS2.18.17/Formula/qgis2.rb">
```

Now you just commit.

```shell
git stage Formula/qgis2.rb
git commit -m "qgis update to 2.18.17"
```

And now you just reinstall QGIS and move to `/Applications`:

```shell
brew reinstall qgis2 --with-grass --with-saga-gis-lts
mv -f /usr/local/Cellar/qgis2/2.18.17/QGIS.app /Applications
ln -s /Applications/QGIS.app /usr/local/Cellar/qgis2/2.18.17/QGIS.app
```

{% include figure image_path="assets/images/blog/2018/Screen-Shot-2018-03-12-at-10.27.28.png" alt="QGIS 2.18.17 splash screen" caption="QGIS 2.18.17 splash screen" %}{: .align-center}

{% include figure image_path="assets/images/blog/2018/Screen-Shot-2018-03-12-at-10.27.49.png" alt="QGIS 2.18.17 interface" caption="QGIS 2.18.17 interface" %}{: .align-center}

### Matplotlib

Since [Homebrew](https://github.com/Homebrew)/[homebrew-science](https://github.com/Homebrew/homebrew-science) has been deprecated and some of the formulae wasn't migrated to the [Homebrew](https://github.com/Homebrew)/[homebrew-core](https://github.com/Homebrew/homebrew-core) this left us with a message at the end of the QGIS' compilation asking us to install [matplotlib](https://matplotlib.org) via pip.

The following Python modules are needed by QGIS during run-time:

```shell
The following Python modules are needed by QGIS during run-time:

    matplotlib, pyparsing

You can install manually, via installer package or with `pip` (if availble):

    pip install <module>  OR  pip-2.7 install <module>
```

I had a problem and when I tried to install using `pip install matplotlib` threw me an error, though.

```shell
Collecting matplotlib
  Using cached matplotlib-2.2.0-cp27-cp27m-macosx_10_6_intel.macosx_10_9_intel.macosx_10_9_x86_64.macosx_10_10_intel.macosx_10_10_x86_64.whl
Requirement already satisfied: subprocess32 in /usr/local/lib/python2.7/site-packages (from matplotlib)
Requirement already satisfied: backports.functools-lru-cache in /usr/local/lib/python2.7/site-packages (from matplotlib)
Requirement already satisfied: pyparsing!=2.0.4,!=2.1.2,!=2.1.6,>=2.0.1 in /usr/local/lib/python2.7/site-packages (from matplotlib)
Requirement already satisfied: cycler>=0.10 in /usr/local/lib/python2.7/site-packages (from matplotlib)
Requirement already satisfied: numpy>=1.7.1 in /usr/local/lib/python2.7/site-packages (from matplotlib)
Requirement already satisfied: kiwisolver>=1.0.1 in /usr/local/lib/python2.7/site-packages (from matplotlib)
Requirement already satisfied: pytz in /usr/local/lib/python2.7/site-packages (from matplotlib)
Requirement already satisfied: python-dateutil>=2.1 in /usr/local/lib/python2.7/site-packages (from matplotlib)
Requirement already satisfied: six>=1.10 in /usr/local/lib/python2.7/site-packages (from matplotlib)
Requirement already satisfied: setuptools in /usr/local/lib/python2.7/site-packages (from kiwisolver>=1.0.1->matplotlib)
Installing collected packages: matplotlib
Exception:
Traceback (most recent call last):
  File "/usr/local/lib/python2.7/site-packages/pip/basecommand.py", line 215, in main
    status = self.run(options, args)
  File "/usr/local/lib/python2.7/site-packages/pip/commands/install.py", line 342, in run
    prefix=options.prefix_path,
  File "/usr/local/lib/python2.7/site-packages/pip/req/req_set.py", line 784, in install
    **kwargs
  File "/usr/local/lib/python2.7/site-packages/pip/req/req_install.py", line 851, in install
    self.move_wheel_files(self.source_dir, root=root, prefix=prefix)
  File "/usr/local/lib/python2.7/site-packages/pip/req/req_install.py", line 1064, in move_wheel_files
    isolated=self.isolated,
  File "/usr/local/lib/python2.7/site-packages/pip/wheel.py", line 345, in move_wheel_files
    clobber(source, lib_dir, True)
  File "/usr/local/lib/python2.7/site-packages/pip/wheel.py", line 323, in clobber
    shutil.copyfile(srcfile, destfile)
  File "/usr/local/Cellar/python@2/2.7.14_3/Frameworks/Python.framework/Versions/2.7/lib/python2.7/shutil.py", line 97, in copyfile
    with open(dst, 'wb') as fdst:
IOError: [Errno 13] Permission denied: '/usr/local/lib/python2.7/site-packages/matplotlib/legend.pyc'
```

Basically I had a ownership problem, that can be solve using `sudo`.

```shell
sudo pip install matplotlib
```

Or fixing the [ownership](https://stackoverflow.com/questions/27870003/pip-install-please-check-the-permissions-and-owner-of-that-directory) like this:

```shell
sudo chown -R $(echo $USER) ~/Library/Caches/pip
```

Now, I was able to install.

## How to install QGIS 3?

Well, this is a little bit more challenging, but just a little bit. The only version of a Homebrew formula that is available to install QGIS 3 is the developer version one that is in this [qgis](https://github.com/qgis)/[homebrew-qgisdev](https://github.com/qgis/homebrew-qgisdev) repo-tap. So I just have to tap that repo:

```shell
brew tap qgis/homebrew-qgisdev
```

However, I needed to make changes in the formula to be able to install because it has a dependency on Matplotlib on [Homebrew](https://github.com/Homebrew)/[homebrew-science](https://github.com/Homebrew/homebrew-science) and as we learned before that formula doesn't exist anymore. In this case, it isn't like in the QGIS 2, that it builds anyway and then ask you to install Matplotlib. It just doesn't build and throws an [error](https://github.com/qgis/homebrew-qgisdev/issues/50).

I have two options here. I can just delete or comment the line where the dependency is called, or I can replace it for the legacy formula, which is located in the [brewsci](https://github.com/brewsci)/[homebrew-science](https://github.com/brewsci/homebrew-science) tap. Either way I proceeded in the same way as I did previously.

First, I created a branch and edited the formulae.

```shell
cd /usr/local/Homebrew/Library/Taps/qgis/homebrew-qgisdev
git checkout -b matplotlib-fix
brew edit qgis3-dev
```

Then, I just committed my edits

```shell
git add Formula/qgis3-dev.rb
git commit -m "fix for matplotlib"
```

Before I tried to build I have to do two more tweaks to be able to compile without [errors](https://github.com/qgis/homebrew-qgisdev/issues/66#issuecomment-372069535). First I have to reinstall [Bison](https://www.gnu.org/software/bison/).

```shell
brew reinstall bison
```

And then I have to modify the file `/usr/local/bin/pyrcc5` and change `pythonw2.7` to `python3`.

```sh
#!/bin/sh
exec python3 -m PyQt5.pyrcc_main ${1+"$@"}
```

Now I could build QGIS 3 developer edition:

```shell
brew install --no-sandbox qgis3-dev --with-grass --with-saga-gis-lts
```

{% include figure image_path="assets/images/blog/2018/Screen-Shot-2018-03-12-at-10.27.58.png" alt="QGIS 3 dev splash screen" caption="QGIS 3 dev splash screen" %}{: .align-center} 

{% include figure image_path="assets/images/blog/2018/Screen-Shot-2018-03-12-at-10.28.22.png" alt="QGIS 3 interface" caption="QGIS 3 interface" %}{: .align-center} 

After I finished building the QGIS 3, I moved the app from the Homebrew Cellar to the `/Applications` folder in the same way I moved QGIS 2, but since I want to keep QGIS 2 and QGIS 3, in the process I renamed the app to `QGIS 3.app`.

```shell
mv /usr/local/opt/qgis3-dev/QGIS.app /Applications/QGIS\ 3.app
ln -s /Applications/QGIS\ 3.app /usr/local/opt/qgis3-dev/QGIS.app
```

## Final thoughts

I hope that in the near future the situation improve and we can enjoy the last version of QGIS on macOS in a easier way. Perhaps even without needed to compile. However, in this very moment isn't the case.

You have to take into account that with this method you are installing the QGIS 3 developer version, so probably isn't going to be really stable, or perhaps you are going to have no problem. I guess that you can install the stable version using the QGIS3-dev formulae, you just have to change the values `branch =>` to `"release-3_0"` in line 37 and `version` to `"3.0"` on line 38.

```ruby 
class Qgis3DevUnlinkedFormulae < Requirement
  fatal true
  satisfy(:build_env => false) { !qt4_linked && !pyqt4_linked && !txt2tags_linked }

  def qt4_linked
    (Formula["qt"].linked_keg/"lib/QtCore.framework/Versions/4").exist?
  rescue
    return false
  end

  def pyqt4_linked
    (Formula["pyqt"].linked_keg/"lib/python2.7/site-packages/PyQt").exist?
  rescue
    return false
  end

  def txt2tags_linked
    Formula["txt2tags"].linked_keg.exist?
  rescue
    return false
  end

  def message
    s = "Compilation can fail if these formulae are installed and linked:\n\n"

    s += "Unlink with `brew unlink qt` or remove with `brew uninstall qt`\n" if qt4_linked
    s += "Unlink with `brew unlink pyqt` or remove with `brew uninstall pyqt`\n" if pyqt4_linked
    s += "Unlink with `brew unlink txt2tags` or remove with `brew uninstall txt2tags`\n" if txt2tags_linked
    s
  end
end

class Qgis3Dev < Formula
  desc "User friendly open source Geographic Information System"
  homepage "https://www.qgis.org"

  url "https://github.com/qgis/QGIS.git", :branch => "release-3_0"
  version "3.0"

  option "without-ninja", "Disable use of ninja CMake generator"
  option "without-debug", "Disable debug build, which outputs info to system.log or console"
  option "without-qt5-webkit", "Build without webkit based functionality"
  option "without-pyqt5-webkit", "Build without webkit python bindings"
  option "without-server", "Build without QGIS Server (qgis_mapserv.fcgi)"
  option "without-postgresql", "Build without current PostgreSQL client"
  option "with-globe", "Build with Globe plugin, based upon osgEarth"
  option "with-grass", "Build with GRASS 7 integration plugin and Processing plugin support (or install grass-7x first)"
  option "with-oracle", "Build extra Oracle geospatial database and raster support"
  option "with-orfeo5", "Build extra Orfeo Toolbox for Processing plugin"
  option "with-r", "Build extra R for Processing plugin"
  option "with-saga-gis-lts", "Build extra Saga GIS for Processing plugin"
  # option "with-qt-mysql", "Build extra Qt MySQL plugin for eVis plugin"
  option "with-qspatialite", "Build QSpatialite Qt database driver"
  option "with-api-docs", "Build the API documentation with Doxygen and Graphviz"
  option "with-3d", "Build with 3D Map View panel"

  depends_on Qgis3DevUnlinkedFormulae

  # core qgis
  depends_on "cmake" => :build
  depends_on "ninja" => [:build, :recommended]
  depends_on "bison" => :build
  depends_on "flex" => :build
  if build.with? "api-docs"
    depends_on "graphviz"
    depends_on "doxygen"
  end

  depends_on :python3

  depends_on "qt" # keg_only
  depends_on "osgeo/osgeo4mac/qt5-webkit" => :recommended # keg_only
  depends_on "sip"
  depends_on "pyqt"
  depends_on "osgeo/osgeo4mac/pyqt5-webkit" => :recommended
  depends_on "qca"
  depends_on "qtkeychain"
  depends_on "qscintilla2"
  depends_on "qwt"
  depends_on "qwtpolar"
  depends_on "gsl"
  depends_on "sqlite" # keg_only
  depends_on "expat" # keg_only
  depends_on "proj"
  depends_on "spatialindex"
  # depends_on "homebrew/science/matplotlib" # deprecated
  depends_on "brewsci/science/matplotlib"
  depends_on "fcgi" if build.with? "server"
  # use newer postgresql client than Apple's, also needed by `psycopg2`
  depends_on "postgresql" => :recommended
  depends_on "libzip"

  # needed for PKI authentication methods that require PKCS#8->PKCS#1 conversion
  depends_on "libtasn1"

  # core providers
  depends_on "osgeo/osgeo4mac/gdal2" # keg_only
  depends_on "osgeo/osgeo4mac/gdal2-python" # keg_only
  depends_on "osgeo/osgeo4mac/oracle-client-sdk" if build.with? "oracle"
  # TODO: add MSSQL third-party support formula?, :optional

  # core plugins (c++ and python)
  if build.with?("grass") || (HOMEBREW_PREFIX/"opt/grass7").exist?
    depends_on "osgeo/osgeo4mac/grass7"
    depends_on "gettext" # keg_only
  end

  # Not until osgearth is Qt5-ready
  # if build.with? "globe"
  #   # this is pretty borked with OS X >= 10.10+
  #   depends on "open-scene-graph"
  #   depends on "homebrew/science/osgearth"
  # end

  depends_on "gpsbabel" => :optional
  # TODO: remove "pyspatialite" when PyPi package supports spatialite 4.x
  #       or DB Manager supports libspatialite >= 4.2.0 (with mod_spatialite)

  # TODO: what to do for Py3 and pyspatialite?
  # depends on "pyspatialite" # for DB Manager
  # depends on "qt-mysql" => :optional # for eVis plugin (non-functional in 2.x?)

  # core processing plugin extras
  # see `grass` above
  depends_on "osgeo/osgeo4mac/orfeo5" => :optional
  depends_on "r" => :optional
  depends_on "osgeo/osgeo4mac/saga-gis-lts" => :optional
  # TODO: LASTools straight build (2 reporting tools), or via `wine` (10 tools)
  # TODO: Fusion from USFS (via `wine`?)

  # TODO: add one for Py3
  #       (only necessary when macOS ships a Python3 or 3rd-party isolation is needed)
  # resource "pyqgis-startup" do
  #   url "https://gist.githubusercontent.com/dakcarto/11385561/raw/e49f75ecec96ed7d6d3950f45ad3f30fe94d4fb2/pyqgis_startup.py"
  #   sha256 "385dce925fc2d29f05afd6508bc1f46ec84c0bc607cc0c8dfce78a4bb93b9c4e"
  #   version "2.14.0"
  # end

  needs :cxx11

  def install
    ENV.cxx11

    # when gdal2-python.rb loaded, PYTHONPATH gets set to 2.7 site-packages...
    #   clear it before calling any local python3 functions
    ENV["PYTHONPATH"] = nil
    if ARGV.debug?
      puts "python_exec: #{python_exec}"
      puts "py_ver: #{py_ver}"
      puts "brewed_python?: #{brewed_python?}"
      puts "python_site_packages: #{python_site_packages}"
      puts "python_prefix: #{python_prefix}"
      puts "qgis_python_packages: #{qgis_python_packages}"
      puts "gdal_python_packages: #{gdal_python_packages}"
      puts "gdal_python_opt_bin: #{gdal_python_opt_bin}"
      puts "gdal_opt_bin: #{gdal_opt_bin}"
    end

    # Vendor required python3 pkgs if they are missing
    # TODO: this should really be a requirements.txt in src tree
    py_req = %w[
      future
      psycopg2
      python-dateutil
      httplib2
      pytz
      six
      nose2
      pygments
      jinja2
      pyyaml
      requests
      owslib
    ].freeze

    orig_user_base = ENV["PYTHONUSERBASE"]
    ENV["PYTHONUSERBASE"] = libexec/"python"
    system HOMEBREW_PREFIX/"bin/pip3", "install", "--user", *py_req
    ENV["PYTHONUSERBASE"] = orig_user_base

    # Set bundling level back to 0 (the default in all versions prior to 1.8.0)
    # so that no time and energy is wasted copying the Qt frameworks into QGIS.

    # Install custom widgets Designer plugin to local qt plugins prefix
    mkdir lib/"qt/plugins/designer"
    inreplace "src/customwidgets/CMakeLists.txt",
              "${QT_PLUGINS_DIR}/designer", lib/"qt/plugins/designer".to_s

    # Fix custom widgets Designer module install path
    mkdir lib/"python#{py_ver}/site-packages/PyQt5"
    inreplace "CMakeLists.txt",
              "${PYQT5_MOD_DIR}", lib/"python#{py_ver}/site-packages/PyQt5".to_s

    # Install db plugins to local qt plugins prefix
    if build.with? "qspatialite"
      mkdir lib/"qt/plugins/sqldrivers"
      inreplace "src/providers/spatialite/qspatialite/CMakeLists.txt",
                "${QT_PLUGINS_DIR}/sqldrivers", lib/"qt/plugins/sqldrivers".to_s
    end
    if build.with? "oracle"
      inreplace "src/providers/oracle/ocispatial/CMakeLists.txt",
                "${QT_PLUGINS_DIR}/sqldrivers", lib/"qt/plugins/sqldrivers".to_s
    end

    args = std_cmake_args
    args << "-DCMAKE_BUILD_TYPE=RelWithDebInfo" if build.with? "debug" # override

    cmake_prefixes = %w[
      qt5
      qt5-webkit
      qscintilla2
      qwt
      qwtpolar
      qca
      gdal2
      gsl
      geos
      proj
      libspatialite
      spatialindex
      expat
      sqlite
      libzip
      flex
      bison
      fcgi
    ].freeze
    # Force CMake to search HB/opt paths first, so headers in HB/include are not found instead;
    # specifically, ensure any gdal v1 includes are not used
    args << "-DCMAKE_PREFIX_PATH=#{cmake_prefixes.map { |f| Formula[f.to_s].opt_prefix }.join(";")}"

    args += %w[
      -DENABLE_TESTS=FALSE
      -DENABLE_MODELTEST=FALSE
      -DQGIS_MACAPP_BUNDLE=0
      -DQGIS_MACAPP_INSTALL_DEV=FALSE
      -DWITH_QWTPOLAR=TRUE
      -DWITH_INTERNAL_QWTPOLAR=FALSE
      -DWITH_ASTYLE=FALSE
      -DWITH_QSCIAPI=TRUE
      -DWITH_STAGED_PLUGINS=TRUE
      -DWITH_GRASS=FALSE
      -DWITH_CUSTOM_WIDGETS=TRUE
    ]

    args << "-DWITH_QTWEBKIT=#{build.with?("qt5-webkit") ? "TRUE" : "FALSE"}"

    # Prefer opt_prefix for CMake modules that find versioned prefix by default
    # This keeps non-critical dependency upgrades from breaking QGIS linking
    args << "-DGDAL_LIBRARY=#{Formula["gdal2"].opt_lib}/libgdal.dylib"
    args << "-DGEOS_LIBRARY=#{Formula["geos"].opt_lib}/libgeos_c.dylib"
    args << "-DGSL_CONFIG=#{Formula["gsl"].opt_bin}/gsl-config"
    args << "-DGSL_INCLUDE_DIR=#{Formula["gsl"].opt_include}"
    args << "-DGSL_LIBRARIES='-L#{Formula["gsl"].opt_lib} -lgsl -lgslcblas'"

    args << "-DWITH_SERVER=#{build.with?("server") ? "TRUE" : "FALSE"}"

    args << "-DPOSTGRES_CONFIG=#{Formula["postgresql"].opt_bin}/pg_config" if build.with? "postgresql"

    args << "-DWITH_GRASS7=#{(build.with?("grass") || brewed_grass7?) ? "TRUE" : "FALSE"}"
    if build.with?("grass") || brewed_grass7?
      # this is to build the GRASS Plugin, not for Processing plugin support
      grass7 = Formula["grass7"]
      args << "-DGRASS_PREFIX7='#{grass7.opt_prefix}/grass-base'"
      # Keep superenv from stripping (use Cellar prefix)
      ENV.append "CXXFLAGS", "-isystem #{grass7.prefix.resolved_path}/grass-base/include"
      # So that `libintl.h` can be found (use Cellar prefix; should not be needed anymore with QGIS 2.99+)
      # ENV.append "CXXFLAGS", "-isystem #{Formula["gettext"].include.resolved_path}"
    end

    args << "-DWITH_GLOBE=#{build.with?("globe") ? "TRUE" : "FALSE"}"
    if build.with? "globe"
      osg = Formula["open-scene-graph"]
      opoo "`open-scene-graph` formula's keg not linked." unless osg.linked_keg.exist?
      # must be HOMEBREW_PREFIX/lib/osgPlugins-#.#.#, since all osg plugins are symlinked there
      args << "-DOSG_PLUGINS_PATH=#{HOMEBREW_PREFIX}/lib/osgPlugins-#{osg.version}"
    end

    args << "-DWITH_ORACLE=#{build.with?("oracle") ? "TRUE" : "FALSE"}"
    if build.with? "oracle"
      oracle_opt = Formula["oracle-client-sdk"].opt_prefix
      args << "-DOCI_INCLUDE_DIR=#{oracle_opt}/include/oci"
      args << "-DOCI_LIBRARY=#{oracle_opt}/lib/libclntsh.dylib"
    end

    args << "-DWITH_QSPATIALITE=#{build.with?("qspatialite") ? "TRUE" : "FALSE"}"

    args << "-DWITH_APIDOC=#{build.with?("api-docs") ? "TRUE" : "FALSE"}"

    args << "-DWITH_3D=#{build.with?("3d") ? "TRUE" : "FALSE"}"

    # nix clang tidy runs
    args << "-DCLANG_TIDY_EXE="

    # if using Homebrew's Python, make sure its components are always found first
    # see: https://github.com/Homebrew/homebrew/pull/28597
    ENV["PYTHONHOME"] = python_prefix

    # handle custom site-packages for keg-only modules and packages
    ENV.append_path "PYTHONPATH", python_site_packages
    ENV.append_path "PYTHONPATH", libexec/"python/lib/python/site-packages"

    # handle some compiler warnings
    # ENV["CXX_EXTRA_FLAGS"] = "-Wno-unused-private-field -Wno-deprecated-register"
    # if ENV.compiler == :clang && (MacOS::Xcode.version >= "7.0" || MacOS::CLT.version >= "7.0")
    #   ENV.append "CXX_EXTRA_FLAGS", "-Wno-inconsistent-missing-override"
    # end

    ENV.prepend_path "PATH", libexec/"python/bin"

    mkdir "build" do
      # editor = "/usr/local/bin/bbedit"
      # cmake_config = Pathname("#{Dir.pwd}/#{name}_cmake-config.txt")
      # cmake_config.write ["cmake ..", *args].join(" \\\n")
      # system editor, cmake_config.to_s
      # raise
      system "cmake", "-G", build.with?("ninja") ? "Ninja" : "Unix Makefiles", *args, ".."
      # system editor, "CMakeCache.txt"
      # raise
      system "cmake", "--build", ".", "--target", "all", "--", "-j", Hardware::CPU.cores
      system "cmake", "--build", ".", "--target", "install", "--", "-j", Hardware::CPU.cores
    end

    # Fixup some errant lib linking
    # TODO: fix upstream in CMake
    dy_libs = [lib/"qt/plugins/designer/libqgis_customwidgets.dylib"]
    dy_libs << lib/"qt/plugins/sqldrivers/libqsqlspatialite.dylib" if build.with? "qspatialite"
    dy_libs.each do |dy_lib|
      MachO::Tools.dylibs(dy_lib.to_s).each do |i_n|
        %w[core gui native].each do |f_n|
          sufx = i_n[/(qgis_#{f_n}\.framework.*)/, 1]
          next if sufx.nil?
          i_n_to = "#{opt_prefix}/QGIS.app/Contents/Frameworks/#{sufx}"
          puts "Changing install name #{i_n} to #{i_n_to} in #{dy_lib}" if ARGV.debug?
          dy_lib.ensure_writable do
            MachO::Tools.change_install_name(dy_lib.to_s, i_n.to_s, i_n_to, :strict => false)
          end
        end
      end
    end

    # Update .app's bundle identifier, so other installers doesn't get confused
    inreplace prefix/"QGIS.app/Contents/Info.plist",
              "org.qgis.qgis3", "org.qgis.qgis3-hb-dev"

    py_lib = lib/"python#{py_ver}/site-packages"
    py_lib.mkpath
    ln_s "../../../QGIS.app/Contents/Resources/python/qgis", py_lib/"qgis"

    ln_s "QGIS.app/Contents/MacOS/fcgi-bin", prefix/"fcgi-bin" if build.with? "server"

    doc.mkpath
    mv prefix/"QGIS.app/Contents/Resources/doc/api", doc/"api" if build.with? "api-docs"
    ln_s "../../../QGIS.app/Contents/Resources/doc", doc/"doc"

    # copy PYQGIS_STARTUP file pyqgis_startup.py, even if not isolating (so tap can be untapped)
    # only works with QGIS > 2.0.1
    # doesn't need executable bit set, loaded by Python runner in QGIS
    # TODO: for Py3
    # (libexec/"python").install resource("pyqgis-startup")

    bin.mkdir
    qgis_bin = bin/name.to_s
    touch qgis_bin.to_s # so it will be linked into HOMEBREW_PREFIX
    qgis_bin.chmod 0755
    post_install
  end

  def post_install
    # configure environment variables for .app and launching binary directly.
    # having this in `post_intsall` allows it to be individually run *after* installation with:
    #    `brew postinstall -v <formula-name>`

    app = prefix/"QGIS.app"
    tab = Tab.for_formula(self)
    opts = tab.used_options

    # define default isolation env vars
    pthsep = File::PATH_SEPARATOR
    pypth = python_site_packages.to_s
    pths = %w[
      /usr/bin
      /bin
      /usr/sbin
      /sbin
      /opt/X11/bin
      /usr/X11/bin
      #{opt_libexec}/python/bin
    ]

    # unless opts.include?("with-isolation")
    #   pths = ORIGINAL_PATHS.dup
    #   pyenv = ENV["PYTHONPATH"]
    #   if pyenv
    #     pypth = pyenv.include?(pypth) ? pyenv : pypth + pthsep + pyenv
    #   end
    # end

    unless pths.include?(HOMEBREW_PREFIX/"bin")
      pths = pths.insert(0, HOMEBREW_PREFIX/"bin")
    end

    # set install's lib/python#{py_ver}/site-packages first, so app will work if unlinked
    pypths = %W[
      #{opt_lib}/python#{py_ver}/site-packages
      #{opt_libexec}/python/lib/python/site-packages
      #{pypth}
    ]

    pths.insert(0, gdal_opt_bin)
    pths.insert(0, gdal_python_opt_bin)
    pypths.insert(0, gdal_python_packages)

    if opts.include?("with-gpsbabel")
      pths.insert(0, Formula["gpsbabel"].opt_bin.to_s)
    end

    envars = {
      :PATH => pths.join(pthsep),
      :PYTHONPATH => pypths.join(pthsep),
      :GDAL_DRIVER_PATH => "#{HOMEBREW_PREFIX}/lib/gdalplugins",
      :GDAL_DATA => "#{Formula["gdal2"].opt_share}/gdal",
    }

    # handle multiple Qt plugins directories
    qtplgpths = %W[
      #{Formula["qt"].opt_prefix}/plugins
      #{HOMEBREW_PREFIX}/lib/qt/plugins
    ]
    envars[:QT_PLUGIN_PATH] = qtplgpths.join(pthsep)

    proc_algs = "Contents/Resources/python/plugins/processing/algs"
    if opts.include?("with-grass") || brewed_grass7?
      grass7 = Formula["grass7"]
      # for core integration plugin support
      envars[:GRASS_PREFIX] = "#{grass7.opt_prefix}/grass-base"
      begin
        inreplace app/"#{proc_algs}/grass7/Grass7Utils.py",
                  "'/Applications/GRASS-7.{}.app/Contents/MacOS'.format(version)",
                  "'#{grass7.opt_prefix}/grass-base'"
        puts "GRASS 7 GrassUtils.py has been updated"
      rescue Utils::InreplaceError
        puts "GRASS 7 GrassUtils.py already updated"
      end
    end

    unless opts.include?("without-globe")
      osg = Formula["open-scene-graph"]
      envars[:OSG_LIBRARY_PATH] = "#{HOMEBREW_PREFIX}/lib/osgPlugins-#{osg.version}"
    end

    # TODO: add for Py3
    # if opts.include?("with-isolation") || File.exist?("/Library/Frameworks/GDAL.framework")
    #   envars[:PYQGIS_STARTUP] = opt_libexec/"python/pyqgis_startup.py"
    # end

    # envars.each { |key, value| puts "#{key.to_s}=#{value}" }
    # exit

    # add env vars to QGIS.app's Info.plist, in LSEnvironment section
    plst = app/"Contents/Info.plist"
    # first delete any LSEnvironment setting, ignoring errors
    # CAUTION!: may not be what you want, if .app already has LSEnvironment settings
    dflt = `defaults read-type \"#{plst}\" LSEnvironment 2> /dev/null`
    `defaults delete \"#{plst}\" LSEnvironment` if dflt
    kv = "{ "
    envars.each { |key, value| kv += "'#{key}' = '#{value}'; " }
    kv += "}"
    `defaults write \"#{plst}\" LSEnvironment \"#{kv}\"`
    # add ability to toggle high resolution in Get Info dialog for app
    hrc = `defaults read-type \"#{plst}\" NSHighResolutionCapable 2> /dev/null`
    `defaults delete \"#{plst}\" NSHighResolutionCapable` if hrc
    `defaults write \"#{plst}\" NSHighResolutionCapable \"True\"`
    # leave the plist readable; convert from binary to XML format
    `plutil -convert xml1 -- \"#{plst}\"`
    # make sure plist is readble by all users
    plst.chmod 0644
    # update modification date on app bundle, or changes won't take effect
    touch app.to_s

    # add env vars to launch script for QGIS app's binary
    qgis_bin = bin/name.to_s
    rm_f qgis_bin if File.exist?(qgis_bin) # install generates empty file
    bin_cmds = %W[#!/bin/sh\n]
    # setup shell-prepended env vars (may result in duplication of paths)
    unless pths.include? HOMEBREW_PREFIX/"bin"
      pths.insert(0, HOMEBREW_PREFIX/"bin")
    end
    # even though this should be affected by with-isolation, allow local env override
    pths << "$PATH"
    pypths << "$PYTHONPATH"
    envars[:PATH] = pths.join(pthsep)
    envars[:PYTHONPATH] = pypths.join(pthsep)
    envars.each { |key, value| bin_cmds << "export #{key}=#{value}" }
    bin_cmds << opt_prefix/"QGIS.app/Contents/MacOS/QGIS \"$@\""
    qgis_bin.write(bin_cmds.join("\n"))
    qgis_bin.chmod 0755
  end

  def caveats
    s = <<-EOS.undent
      Bottles support only Homebrew's Python3

      QGIS is built as an application bundle. Environment variables for the
      Homebrew prefix are embedded in QGIS.app:
        #{opt_prefix}/QGIS.app

      You may also symlink QGIS.app into /Applications or ~/Applications:
        brew linkapps [--local]

      To directly run the `QGIS.app/Contents/MacOS/QGIS` binary use the wrapper
      script pre-defined with Homebrew prefix environment variables:
        #{opt_bin}/#{name}

      NOTE: Your current PATH and PYTHONPATH environment variables are honored
            when launching via the wrapper script, while launching QGIS.app
            bundle they are not.

      For standalone Python3 development, set the following environment variable:
        export PYTHONPATH=#{qgis_python_packages}:#{gdal_python_packages}:#{python_site_packages}:$PYTHONPATH

    EOS

    s += <<-EOS.undent
      If you have built GRASS 7 for the Processing plugin set the following in QGIS:
        Processing->Options: Providers->GRASS GIS 7 commands->GRASS 7 folder to:
           #{HOMEBREW_PREFIX}/opt/grass7/grass-base

    EOS

    s
  end

  test do
    output = `#{bin}/#{name.to_s} --help 2>&1` # why does help go to stderr?
    assert_match /^QGIS is a user friendly/, output
  end

  private

  def brewed_grass7?
    Formula["grass7"].opt_prefix.exist?
  end

  def python_exec
    if brewed_python?
      Formula["python3"].opt_bin/"python3"
    else
      py_exec = `which python3`.strip
      raise if py_exec == ""
      py_exec
    end
  end

  def py_ver
    `#{python_exec} -c 'import sys;print("{0}.{1}".format(sys.version_info[0],sys.version_info[1]))'`.strip
  end

  def brewed_python?
    Formula["python3"].linked_keg.exist?
  end

  def python_site_packages
    HOMEBREW_PREFIX/"lib/python#{py_ver}/site-packages"
  end

  def python_prefix
    `#{python_exec} -c 'import sys;print(sys.prefix)'`.strip
  end

  def qgis_python_packages
    opt_lib/"python#{py_ver}/site-packages".to_s
  end

  def gdal_python_packages
    Formula["gdal2-python"].opt_lib/"python#{py_ver}/site-packages".to_s
  end

  def gdal_python_opt_bin
    Formula["gdal2-python"].opt_bin.to_s
  end

  def gdal_opt_bin
    Formula["gdal2"].opt_bin.to_s
  end

  def module_importable?(mod)
    `#{python_exec} -c 'import sys;sys.path.insert(1, "#{gdal_python_packages}"); import #{mod}'`.strip
  end
end
```

Anyhow, I think I'm going to keep the 3.1 version for a while.


[^1]: No, you can see that because I deteletd the branch... Sorry. 
