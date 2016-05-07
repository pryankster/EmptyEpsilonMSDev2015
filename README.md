
# Notes for building EmptyEpsilon with MSDEV 2015 Community

Assumes that &lt;root&gt; = c:\src\github\EmptyEpsilon, and that the solution directory is c:\src\github\EmptyEpsilon\EmptyEpsilonMSDEV2015

These are the steps that I went through to build a project for EmptyEpsilon in Visual Studio 2015 (Community)

* Check out files in &lt;root&gt;
    * git clone https://github.com/daid/SeriousProton.git
    * git clone https://github.com/daid/EmptyEpsilon.git

* You should now have:
    * &lt;root&gt;
    * &lt;root&gt;/EmptyEpsilonMSDEV2015
    * &lt;root&gt;/EmptyEpsilon
    * &lt;root&gt;/SeriousProton

* Created Solution &lt;root&gt;\EmptyEpsilonMSDEV

    * Create C++ Project "SeriousProton"

    * Properties -&gt; Configuration Properties -&gt; General
        * Project Defaults -&gt; Configuration Type -&gt; Static Library (.lib)

    * Add Source files from &lt;root&gt;\SeriousProton
    * Add Source files from &lt;root&gt;\SeriousProton\Box2D

    * NuGet packages:
        * sfml: ( sfml-system sfml-audio sfml-network sfml-window sfml-graphics)

    * Extra CFLAGS
        * NOMINMAX (to make windows.h not include #define min and #define max)
        * _CRT_SECURE_NO_WARNINGS (should use fopen_s, fopen is deprecated)
        * __PRETTY_FUNCTION__=__FUNCSIG__

    * Add Extra Includes:
        * $(SolutionDir)..\SeriousProton\src
        * $(SolutionDir)..\dirent\include

* Created C++ Project "EmptyEpsilon"
    * Add Reference -&gt; Project -&gt; SeriousProton

    * NuGet packages:
        * Make sure sfml projects are added
        * OpenAL-soft

    * Extra CFLAGS:
        * NOMINMAX
        * _CRT_SECURE_NO_WARNINGS
        * __PRETTY_FUNCTION__=__FUNCSIG__

    * Add Extra Includes:
        * $(SolutionDir)..\SeriousProton\src
        * $(SolutionDir)..\EmptyEpsilon\src

# C++ Template Notes:

Visual C++ (as of 2015 version) has a "bug" w.r.t template
instantiation vs. gcc.  The gcc compiler will instantiate template
specializations at link time as long as they are defined in any
translation unit.  VC++ will only instantiate a specialization if 
it occurs in the same translation unit (.obj).

I have moved the template specializations into '.hpp' files
which are included in the .h file for VC++ and in the .cpp file
for gcc.

# Issues
* I have not done much testing -- it compiles, it runs on one system.  
  I haven't tested the networking code
* There are thousands of warnings, mostly related to size_t being 
  __uint64 and many things assuming that it's an int.
