Leaving Arch Linux for Raspbian on RPi
######################################
:date: 2012-09-30 22:52
:category: Opinion
:slug: leaving-arch-linux-for-raspbian-on-rpi
:status: published

I am done with `Arch Linux <http://archlinux.org>`__. I love the
simplicity of Arch. I love "The Arch Way." I love the pacman package
manager. But the arch guys change things too often. It is too bleeding
edge for me. I had enough. I have removed it from my laptop, my wife's
laptop and from my `Raspberry Pi <http://raspberrypi.org>`__ (RPi).

About a year ago, I came across Arch Linux. I went to their website,
read their introduction, and fell in love with "The Arch Way."
Afterwards, I removed ubuntu from my laptop and installed arch following
their beginners' guide. I learned a ton about linux during the
installation. I got it right at the first attempt. I enjoyed being in
charge of everything on my computer.

But it cost me too much time. Every now and then after a system update
one or two package would break, something would not work, some too much
up-to-date software would contain so many bugs that those become
unusable.

While I was keeping my own machine quite up-to-date by updating
two/three times a week, I was not updating my wife's laptop that
frequently. As a result, I forgot that they have moved ``/lib`` to
``/usr/lib`` and that if you are not careful enough you could break your
system. Murphy's law was correct, I was not careful enough and I broke
the system. It took me a few hours of wrestling with the system
involving ``chrooting`` from an ubuntu live system. At this point, I
thought that arch was taking too much time. After that I just removed
arch from our laptops and installed ubuntu.

A few weeks back, I was able to break the `Arch Linux
Arm <http://archlinuxarm.org>`__ running on my RPi that hosts this
website. I was upgrading from soft-float kernel to latest hard-float
kernel featuring the turbo mode. Apparently, my ``/boot`` partition was
not mounted by default and so while upgrading the system, the firmware
and the kernel was not updated but all the libraries were updated. As a
result, I had hard-float enabled library and soft-float enabled kernel.
Moreover, I forgot about the ``/lib`` and ``/usr/lib`` stuff. Voila, I
broke my system. Unfortunately, this time I was not able to fix it. I
was not able to ``chroot`` the RPi since I did not have an arm
hard-float kernel installed anywhere.

The next story was short, I gave up. I installed raspbian in my RPi in
new SDCARD, moved my Wordpress installation to a it.

I am not going back to Arch Linux, ever. I learned a lot of stuff from
it, but I don't need it anymore. I just can't afford the cost (time).
