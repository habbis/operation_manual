# Compression Tools

Info about compression tools.

compress file gzip
-----------------------------------
```
$ gzip sourcefile
```
recursively compress
```
$ gzip -r directory1
```

stats:
```
$ gzip -l test.gz
```
decompress
```
$ gzip -d test.gz
```

Compress tar with gzip
---------------------------------------------
```
$ tar czvf compressed.tar.gz directory1
```
Peek inside 
```
$ tar tzvf compressed.tar.gz
```

Decompress
```
$ tar xzvf compressed.tar.gz
```

Tar with bzip 
------------------------
```
$ tar cjvf bzipcompressed.tar.bz2 directory2
```
Peek inside
```
$ tar tjvf bzipcompressed.tar.bz2
```
Decompress
```
$ tar xjvf bzipcompressed.tar.bz2
```
Tar with xz
--------------------

compress
```
$ tar cJvf xzcompressed.tar.xz directory3
```
Peek 
```
$ tar tJvf xzcompressed.tar.xz
```
Decompress
```
$ tar xJvf xzcompressed.tar.xz
```


## Parallel Compression 

To install the tools, you can just turn to your repos.

```
sudo apt install pigz pbzip2 pxz # Debian/Ubuntu
sudo dnf install pigz pbzip2 pxz # Fedora
sudo pacman -Sy pigz pbzip2 pxz   # Arch Linux
```


Using Parallel Compression Tools with Tar

You can either tell tar to use a compression program with the --use-compression-program option, or you can use a little bit simpler command flag of -I. An example of the syntax for any of these tools would be like this:

```
tar -I pigz -cf linux-5.10-rc3.tar.gz linux/
tar -I pbzip2 -cf linux-5.10-rc3.tar.bz2 linux/
tar -I pxz -cf linux-5.10-rc3.tar.xz linux/
```

Source
-------

Link: 

https://www.maketecheasier.com/compress-archives-using-all-cpu-cores-tar/

[File compression Digitalocean](https://www.digitalocean.com/community/tutorials/an-introduction-to-file-compression-tools-on-linux-servers(
