# Metadata Image — EXIF & File Analysis

## 1. Ocean.jpg

![Ocean.jpg](images/ocean.jpg)

### 1.1 Online Tool

Using `exif.tools`

### 1.2 Using command

```bash
exiftool /home/Afnan/Downloads/ocean.jpg

2. Computer.jpg

https://images/computer.jpg
2.1 Online Tool

Using hexed.it

https://images/hexed.png
2.2 Using command
bash

hexeditor /home/afnan/Downloads/computer.jpg

https://images/hexeditor1.png
https://images/hexeditor2.png
3. dog.jpg
3.1 Using command
bash

binwalk /home/Afnan/Downloads/dog.jpg
binwalk -e /home/Afnan/Downloads/dog.jpg
cd _dog.jpg.extracted
ls
cat hidden_text.txt

4. computer.jpg
4.1 Using online tools

https://images/computer_online.png
4.2 Using command
bash

strings computer.jpg

https://images/strings.png
5. Solitaire
5.1 Using command
bash

file solitaire.exe

6. Rubiks

https://images/rubiks.jpg
6.1 Using command
bash

file rubiks.jpg

Output:
text

(afnan@Afnan)-[~/Downloads] - file rubiks.jpg
rubiks.jpg: PNG image data, 609 x 640, 8-bit/color RGBA, non-interlaced