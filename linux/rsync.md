

#### Notes from Stack Exchanges 

I would use rsync as it means that if it is interrupted for any reason, then 
you can restart it easily with very little cost. And being rsync, it can even 
restart part way through a large file. As others mention, it can exclude files 
easily. The simplest way to preserve most things is to use the -a flag – ‘archive.’ So:
```
rsync -a source dest
```
Although UID/GID and symlinks are preserved by -a (see -lpgo), your question 
implies you might want a full copy of the filesystem information; and -a doesn't 
include hard-links, extended attributes, or ACLs (on Linux) or the above nor 
resource forks (on OS X.) Thus, for a robust copy of a filesystem, you'll need 
to include those flags:
```
rsync -aHAX source dest # Linux
rsync -aHE source dest  # OS X
```

The default cp will start again, though the -u flag will "copy only when the 
SOURCE file is newer than the destination file or when the destination file 
is missing". And the -a (archive) flag will be recursive, not recopy files if 
you have to restart and preserve permissions. So:
```
cp -au source dest
```

```
rsync -avhW --no-compress --progress /src/ /dst/
```

```
-a is for archive, which preserves ownership, permissions etc.
-v is for verbose, so I can see what's happening (optional)
-h is for human-readable, so the transfer rate and file sizes are easier to read (optional)
-W is for copying whole files only, without delta-xfer algorithm which should reduce CPU load
--no-compress as there's no lack of bandwidth between local devices
--progress so I can see the progress of large files (optional)
```

**Faster Way**
I've seen 17% faster transfers using the above rsync settings over the 
following tar command as suggested by another answer:
```
(cd /src; tar cf - .) | (cd /dst; tar xpf -)
```


