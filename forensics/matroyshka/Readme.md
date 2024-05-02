Run `file` against `flag_in_here.7z`, and see that it just gives the output "data"

Research the magic bytes of 7z, and find that it is `37 7A BC AF 27 1C`
https://en.wikipedia.org/wiki/List_of_file_signatures

Use `hexeditor` to fix the magic bytes of the 7z file.

Repeatedly unzip the created file with `7z x`

Run `exiftool where\ am\ i | grep SUCTF`

Flag: `SUCTF{1_4m_V3RY_C0NFU53D}`