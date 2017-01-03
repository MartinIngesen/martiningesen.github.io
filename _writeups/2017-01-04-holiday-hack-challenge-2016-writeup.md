---
layout:     post
title:      "Holiday Hack Challenge 2016 writeup"
subtitle:   "Who abducted Santa?"
date:       2017-01-03 00:00:00
author:     "Martin Ingesen"
header-img: "img/post-bg-hexdump.jpg"
permalink:  "writeups/holiday-hack-challenge-2016-writeup/"
---

### Preface
This weekend my friend and I went on a Game of Thrones and Silicon Valley marathon. I'm currently without an internet connection at home,
so Nikolai "acquired" the files at his home on his laptop. For various reasons we had to transfer the files over to my Mac using a USB-drive to view it in the living-room.

Turns out, when copying a few of the files, he also accidentally deleted them (Using the power of Linux), therefore recovering it easily would be a tough task.

Luckily, I had a VM of Kali Linux running locally, so I started digging into its forensics tools!

### Preparations

Kali Linux has a tool built in called "Foremost". It is quite simple, it has a large list of file signatures, and simple searches through a blob until it finds a signature it recognizes.
It will then carve out the file from the binary blob and save it neatly in a folder. This sounds promising!

But first I needed to create that blob. Easily enough, I plugged in the USB-drive and ran ```dd if=/dev/<disk-name> of=~/usbblob```, which created an identical copy of the whole drive (luckily it was only about 4GB large). This has to be the **first** thing you do after deleting a file, because if you write to the disk afterwards, it might overwrite the bits and bytes where the files were once stored on the disk, making it hard (if not impossible) to recover the file completely. Preferably, one would use a USB write-blocker,  but I didn't have that at the time.

### Extending Foremost

Unfortunately, Foremost doesn't have any built-in support for .mkv-files, which was the filetype we were looking for. Therefore we had to add it ourself. Foremost's documentation is pretty clear in that regard, simply edit the file ```/etc/foremost.conf``` and follow the instructions there! Simple!

Well, it's *almost* that simple. To successfully carve out the file, we need some information about the Matroska media container format. The documentation explicitly says:

    For each file type, the configuration file describes the file's extension, whether the header and footer are case sensitive, the maximum file size, and the header and footer for the file. The footer field is optional, but header, size, case sensitivity, and extension are not!

So we need:

- File's extension
- is the header and footer case sensitive?
- Maximum file size
- Header bytes
- Footer bytes (optional)

**File extension**

Simple enough, *mkv*.

**Is the header and footer case sensitive?**

No, we don't need to be case sensitive here. Let's just catch everything that matches.

**Maximum file size**

We have to do a rough estimate here, this value is in bytes. The other videofiles had a size around 1.5GB, giving us 1 500 000 000 bytes.

**Header bytes**

Simple. I had some other mkv-files laying around, so I ran ```hexdump <file.mkv> | head``` on a couple of them, and figured that the header signature for the mkv-format is something along the lines of ```\x1a\x45\xdf\xa3```.

**Footer bytes (optional)**

It doesn't seem like mkv has a common footer, so we simply leave this empty.

### Editing foremost.conf

I could then simply edit the ```/etc/foremost.conf``` file and add the details above. Foremost follows a special pattern for adding new filetypes, namely:

    filename      case sensitive? (y/n)   size        header                footer
    =======================================================================================
    mpg                  y               20000000    \x00\x00\x01\xba      \x00\x00\x01\xb9

(each value should simply be delimited by a single tab)

This gives us:

    filename      case sensitive? (y/n)   size          header             footer
    =====================================================================================================
    mpg                  y               20000000       \x00\x00\x01\xba   \x00\x00\x01\xb9
    mkv                  n               1500000000     \x1a\x45\xdf\xa3


### Profit!

Running foremost on the binary blob now yields a folder named "mkv" and inside it we find the two lost-but-found-again video files! Let the binge-watching continue!
