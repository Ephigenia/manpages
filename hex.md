# Hex

Printing the contents of a file as hexdump in the shell:
	
	hexdump -C <filename> | less
	
This will show the content of `<filename>` in a scrollable view in the console:

	00000000  40 63 68 61 72 73 65 74  20 22 55 54 46 2d 38 22  |@charset "UTF-8"|
	00000010  3b 0a 0a 0a 2f 2a 21 0a  41 6e 69 6d 61 74 65 2e  |;.../*!.Animate.|
	00000020  63 73 73 20 2d 20 68 74  74 70 3a 2f 2f 64 61 6e  |css - http://dan|
	00000030  65 64 65 6e 2e 6d 65 2f  61 6e 69 6d 61 74 65 0a  |eden.me/animate.|
	00000040  4c 69 63 65 6e 73 65 64  20 75 6e 64 65 72 20 74  |Licensed under t|
	00000050  68 65 20 4d 49 54 20 6c  69 63 65 6e 73 65 0a 0a  |he MIT license..|
