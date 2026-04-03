# Metadata
Lab 4 Vulnerability Analysis

# 1. Exiftool

I ran exiftool, a powerful command-line application used for reading, writing, and editing meta information in a wide variety of files. By simply typing the command followed by the filename, the tool scans the file's header for hidden tags. While most of the data is standard technical jargon (like resolution and color profiles), I successfully found a manually inserted string in the Comment field “THIS IS THE HIDDEN FLAG”.

![image]( https://github.com/miraaaa1910/Metadata/blob/a27bd29319f158bb5878c78ae9c728a8ad9e7851/Screenshot%202026-04-02%20135007.png )

# 2. Hexeditor

![image]()
![image]()

# 3. Binwalk

I ran binwalk dog.jpg. Binwalk is a tool that looks for file signatures inside other files. It discovered that at decimal offset 88221, there is a Zip archive hidden inside the JPEG image. While not shown in the command history, I used binwalk -e dog.jpg to automatically extract the hidden files. This created a new folder _dog.jpg.extracted. Inside that folder, a Zip file (1589D.zip) and a text file were found. Finally, I navigated into the folder and used the cat command to read hidden_text.txt. The Result "THIS IS A HIDDEN FLAG"

![image]( https://github.com/miraaaa1910/Metadata/blob/a27bd29319f158bb5878c78ae9c728a8ad9e7851/Screenshot%202026-04-02%20135811.png )
![image]() 

# 4. Strings
![image]()

# 5. File

I have a file named solitaire.exe. This would be an executable program, but the file command reveals the truth. The system identifies it as PNG image data. It was simply given the wrong file extension to trick the user.

To work with the file properly, the user renames it using the mv (move) mv solitaire.exe solitaire.png.

Then I run the tools from my previous examples to see if anything is hidden. Binwalk to confirms it is a PNG and shows some Zlib compressed data (which is normal for PNGs). Exiftool displays the image metadata ($640 \times 449$ pixels, RGB with Alpha). No obvious "flags" are visible in the metadata this time.

Finally, I use the strings command combined with grep ‘strings solitaire.png | grep -i "flag".  Strings pull out every readable sequence of characters from the binary file, and grep searches those results for the word "flag". In this case, the search returned nothing (blank output), meaning the flag isn't stored as plain text inside the file.

![image]( https://github.com/miraaaa1910/Metadata/blob/a27bd29319f158bb5878c78ae9c728a8ad9e7851/Screenshot%202026-04-02%20140803.png )

![image]( https://github.com/miraaaa1910/Metadata/blob/a27bd29319f158bb5878c78ae9c728a8ad9e7851/Screenshot%202026-04-02%20141002.png )

I run file rubiks.jpg. Even though the name ends in .jpg, the system reports that it is actually PNG image data. I use the mv (move/rename) command to change the extension from .jpg to .png. Running file rubiks.png confirms the file is now correctly label. 

![image]( https://github.com/miraaaa1910/Metadata/blob/a27bd29319f158bb5878c78ae9c728a8ad9e7851/image.png )
![image]( https://github.com/miraaaa1910/Metadata/blob/a27bd29319f158bb5878c78ae9c728a8ad9e7851/Screenshot%202026-04-02%20141226.png )

