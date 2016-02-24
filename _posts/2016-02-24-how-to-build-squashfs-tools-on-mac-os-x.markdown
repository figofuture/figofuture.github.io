---
layout: post
title:  "How to build squashfs tools on Mac OS X"
subtitle:   ""  
date:       2016-02-24
author:     "figo"
header-img: ""
tags:
    - Mac OS X
    - squashfs
    - mac
    - 2016
---
To squash/unsquash a Linux squashfs filesystem on a Mac, you can build the squashfs tools.

A few modifications of the source are required for OS X:

The FNM_EXTMATCH flag doesn’t exist in the fnmatch library on OS X. This just means that you can’t use glob patterns with mksquashfs.
The constants to figure out how many CPUs the machine has are in the sysctl header instead of the sysinfo header.
The C99 compiler has different semantics for the inline keyword, so inline functions should also be static.
Instead of llistxattr(), lgetxattr(), lsetxattr(), Apple uses a XATTR_NOFOLLOW flag on the non-link equivalent functions.
Here’s a complete download-and-build recipe:
{% highlight ruby %}
curl -OL http://iweb.dl.sourceforge.net/project/squashfs/squashfs/squashfs4.2/squashfs4.2.tar.gz
tar xzvf squashfs4.2.tar.gz
cd squashfs4.2/squashfs-tools

sed -i.orig 's/FNM_EXTMATCH/0/; s/sysinfo.h/sysctl.h/; s/^inline/static inline/' mksquashfs.c unsquashfs.c

cat <<END >> xattr.h
#define llistxattr(path, list, size) \
  (listxattr(path, list, size, XATTR_NOFOLLOW))
#define lgetxattr(path, name, value, size) \
  (getxattr(path, name, value, size, 0, XATTR_NOFOLLOW))
#define lsetxattr(path, name, value, size, flags) \
  (setxattr(path, name, value, size, 0, flags | XATTR_NOFOLLOW))
END

make
sudo cp mksquashfs unsquashfs /usr/local/bin
{% endhighlight %}
Usage notes:

The filesystem you’re unsquashing probably has root-owned files in it, so you’ll have to use sudo to run unsquashfs as root.

The limit of open files is somewhat low on a Mac, so you’ll get a bunch of errors if you try to unsquash a whole filesystem:

write_file: failed to create file squashfs-root/path/to/file, because Too many open files

Just change the limit in the shell before running unsquashfs:
{% highlight ruby %}
ulimit -n 2000
sudo unsquashfs filesystem.squashfs
{% endhighlight %}
There seems to be a bug when drawing the progress bar that causes Floating point exception, which you can workaround by using the -n flag.
