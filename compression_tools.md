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
Source
-------

Link: 


[File compression Digitalocean](https://www.digitalocean.com/community/tutorials/an-introduction-to-file-compression-tools-on-linux-servers(
