## .gz files

Most .gz files or ”gzip archives” get corrupted by transferring the files in ASCII mode over FTP. The best way to fix this is to try retransmitting in the correct mode. But if this is not possible there a ways to recover them.

#### gzrt (gzip recovery toolkit) aka gzrecover
This tool can help with recovering corrupted gzip archives. 
But sometimed it is not enough because the file might still have errors in it. Then other tools can help to gain access to the files. Like cpio if it is a tar archive or TestDisk if it is a disk file. 