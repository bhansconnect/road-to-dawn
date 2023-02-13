# Road to Dawn

This is going to be a long project and in all likeliness never get finished.
As part of the project, I am also likely to commit many of my precursor projects that are just for learning.
The goal of this project is to eventually get [Dawn OS](http://users.atw.hu/gerigeri/DawnOS/index.html) running on an FPGA.
Despite being a super simple CPU design, there are going to be a lot of memory related hurdles to this work:

1. Dawn OS requires 256MB of ram plus 64MB more for hardware mapping. This is more than on most cheap fpga boards or even external chips that connect over pmod
This means that either things have to run off of slow flash, or I need to buy some more expensive boards.
With that in mind, a sold caching system will be needed to get any sort of decent performance.

1. Related to the last point. Dawn OS directly maps the entire video frame to memory.
This will use a lot of memory. 24 bits per pixel, hundreds of thousands, if not millions, of pixels, that all need to be bused around at 60 Hz.
Though it can abitrarily be resized, however it is setup, it needs to be streamable over vga/hdmi.
This will imply very tight timings to actually get all of the pixels output correctly.

1. Subleq is horrible when it comes to memory dependencies. It does 1 icache read, 2 dcache reads, 1 dcache write, and potentially a jump on every instruction.
This is an easy way to keep a cpu constantly stalled. This makes prefetching and good caching even more important.

1. The minimum frequency to even really get a boot is about 20 MHz. A lot of cheap fpgas can run at 100 MHz, but with any reasonble amount of logic will run at 20-50 MHz.
If I want the system to vaguely be usable, more accurately, 100+ MHz would be preferred. So tight timing considerations are definitely needed.



This will definitely not be an easy project especially because I want to make all of the RTL.
I plan to try and simulate and formally verify everything. Though I am pretty new to both.
Hopefully I will be able to get the entire system running super slow in a simulation.


I am currently not sold on what tool I will use for implementation. I think I may try [amaranth hdl](https://github.com/amaranth-lang/amaranth) (previously nmigen), but honestly I am concerned about the choice.
Otherwise, I would likely use verilog with verilator for simulation. I am also open to other programmable hdl, like chisel or clash, but amaranth seems like a simpler choice.