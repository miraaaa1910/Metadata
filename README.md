# Metadata
Lab 4 Vulnerability Analysis

# 1. Exiftool

I ran exiftool, a powerful command-line application used for reading, writing, and editing meta information in a wide variety of files. By simply typing the command followed by the filename, the tool scans the file's header for hidden tags. While most of the data is standard technical jargon (like resolution and color profiles), I successfully found a manually inserted string in the Comment field “THIS IS THE HIDDEN FLAG”.

![image]( https://github.com/miraaaa1910/Metadata/blob/a27bd29319f158bb5878c78ae9c728a8ad9e7851/Screenshot%202026-04-02%20135007.png )

# 2. Hexeditor

I executed the command hexeditor computer.jpg in the Kali terminal. This opens an interactive interface that maps out the file's DNA. The first thing to check in any forensic investigation is the Header. Look at the very first four bytes at offset 00000000. Data: FF D8 FF E0. These are the standard "Magic Bytes" for a JPEG file. This confirms the file is not a renamed PNG or executable; it is a genuine image file.

On the far right side, the editor attempts to translate the hex code into human-readable text (ASCII). You can clearly see JFIF, ICC_PROFILE, and lcms.

These match the results from my previous exiftool and strings commands. The hex editor shows exactly where this metadata lives—right at the start of the file before the actual image data begins.

![image](https://github.com/miraaaa1910/Metadata/blob/5ffc6d238263e51f24f39d1c964a22741bf17f1a/Screenshot%202026-04-02%20141459.png )

# 3. Binwalk

I ran binwalk dog.jpg. Binwalk is a tool that looks for file signatures inside other files. It discovered that at decimal offset 88221, there is a Zip archive hidden inside the JPEG image. While not shown in the command history, I used binwalk -e dog.jpg to automatically extract the hidden files. This created a new folder _dog.jpg.extracted. Inside that folder, a Zip file (1589D.zip) and a text file were found. Finally, I navigated into the folder and used the cat command to read hidden_text.txt. The Result "THIS IS A HIDDEN FLAG"

![image]( https://github.com/miraaaa1910/Metadata/blob/a27bd29319f158bb5878c78ae9c728a8ad9e7851/Screenshot%202026-04-02%20135811.png )

# 4. Strings

Most of a .jpg file is composed of unreadable binary code used to render pixels. The strings utility ignores all that "garbage" data and only prints sequences of 4 or more printable characters. You can see strings like JFIF, ICC_PROFILE, and lcms. These are standard markers that tell software how to handle the image's colors and format. Further down, you see strings like #*%*%525EE\. These aren't actual words; they are just random bytes in the compressed image data that happen to look like text characters.

![image](https://github.com/miraaaa1910/Metadata/blob/3ad568f4fb12980cbf52f3ad433a6f1d08f4d737/Screenshot%202026-04-03%20185839.png )

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

