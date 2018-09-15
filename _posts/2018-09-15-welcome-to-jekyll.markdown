---
layout: post
title:  "Welcome! Now Toolup You Miserable Son of Bitch"
date:   2018-09-15 16:22:07 -0400
comments: true
categories: ghdl
---
## Our first job will be tooling up so we can compile and simulate VHDL.

While I teach part-time for Doulos I can't afford the preferred tool of choice in the
hardware world, [Modelsim](https://www.mentor.com/products/fv/modelsim/) so I'll
be using [GHDL](https://github.com/ghdl/ghdl) an open source compiler and
simulator for VHDL.  Another free choice is to use a Doulos sponsored product
[EDA Playground](https://www.edaplayground.com/) but I prefer to use [vim](https://www.vim.org)
for editing so... let's get started.

### 1. Get GHDL.  Grab the [pre-compiled distribution](https://github.com/ghdl/ghdl/releases).  

  I'm on macOS and I'm using the llvm version.  Once installed verify with checking
  the version using `ghdl -v`.

```bash
  ghdl -v
  GHDL 0.34 (tarball) [Dunoon edition]
   Compiled with GNAT Version: GPL 2015 (20150428-49)
   llvm code generator
  Written by Tristan Gingold.
  Copyright (C) 2003 - 2015 Tristan Gingold.
  GHDL is free software, covered by the GNU General Public License.  There is NO
  warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

### 2. Hello World

  Ok head over to the [documentation for GHDL](https://ghdl.readthedocs.io/en/stable/about.html) 
  and checkout the [Quick Start Guide](https://ghdl.readthedocs.io/en/stable/using/QuickStartGuide.html)
  and let's compile the shit out of this thing. Put the following VHDL code in a
  file called `hello_world.vhd`

```vhdl
  --  Hello world program.
  use std.textio.all; -- Imports the standard textio package.

  --  Defines a design entity, without any ports.
  entity hello_world is
  end hello_world;

  architecture behaviour of hello_world is
  begin
     process
        variable l : line;
     begin
        write (l, String'("Hello world!"));
        writeline (output, l);
        wait;
     end process;
  end behaviour;
 ``` 

  Now since we are not assholes we are going to organize our code and compile
  into a library.  By default `ghdl` is not an opinionated kind of tool but when we are just starting out
  _anything goes_ kinda sucks. So before going any further let's create a
  `src/vhdl/hello_world` directory off of your home dir.

  ```bash
  cd # go home 
  mkdir -p src/vhdl/hello_world
  mv hello_world.vhd ~/src/vhdl/hello_world/
  ```

  This isn't exactly how I would organize VHDL code but that is for another
  post... _Hint: never put enitities and architectures in the same file_

### 3. Compile, Elaborate and Simulate

  We are in the home stretch you miserable son of bitch.  This fucking code
  should compile without any errors so let's try. I find ghdl's interface
  unusual.  Again, another post we will make it act like modelsim...but for now
  we'll use their interface.

  ```bash
  cd ~/src/vhdl/hello_world
  mkdir -p work # store ghdl compiled files here
  ghdl -a --std=08 --workdir=work hello_world.vhd
  ghdl -e --std=08 --workdir=work hello_world
  ghdl -r --std=08 --workdir=work hello_world
  Hello world!
  ```

### 4. Inspection and Reflection

  Ok, we did it, hopefully.  If you didn't then keep trying.  It's possible I
  fucked up this tutorial some way.  If I did please comment.

  You should have a directory listing that looks like:

  ```
  drwxr-xr-x   6 chadb  staff   192B Sep 15 17:39 ./
  drwxr-xr-x  16 chadb  staff   512B Sep 15 17:37 ../
  -rw-r--r--   1 chadb  staff   2.3K Sep 15 17:39 e~hello_world.o
  -rwxr-xr-x   1 chadb  staff   1.0M Sep 15 17:39 hello_world*
  -rw-r--r--   1 chadb  staff   380B Apr 27 16:30 hello_world.vhd
  drwxr-xr-x   4 chadb  staff   128B Sep 15 17:38 work/
  ```

  The work directory should look like: 


  ```
  drwxr-xr-x  4 chadb  staff   128B Sep 15 17:38 ./
  drwxr-xr-x  6 chadb  staff   192B Sep 15 17:39 ../
  -rw-r--r--  1 chadb  staff   3.6K Sep 15 17:38 hello_world.o
  -rw-r--r--  1 chadb  staff   233B Sep 15 17:38 work-obj08.cf
  ```


  **Well this is one of those tutorials that you probably didn't learn shit.**  I
  hate crappy tutorials like this.  
  
  The best way to learn is to _fuck shit up_ and then learn how to fix up the
  horrible mess you created.  Edit `hello_world.vhd` and fuck it up on purpose
  and investigate the ghdl errors.  I'll try and jack code up more in
  future tutorials and ramblings. Until then keep Jackin'


{% if page.comments %}
<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://vhdl-hippie.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
{% endif %}
