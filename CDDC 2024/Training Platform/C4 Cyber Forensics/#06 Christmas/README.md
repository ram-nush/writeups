# \#06 Christmas (100 pts)
> Let's solve the problem by finding the hidden code buried in the snow.

## Attachments
Downloading the attachments, we get two files: fox.bmp and snow.png

<img src="fox.bmp" width="300">
<img src="snow.png" width="300">

## Approach
The snow file seems to be completely white, so I decided to use [StegOnline](https://georgeom.net/StegOnline/upload "StegOnline"), an online steganography tool. I uploaded the snow file and clicked on the Browse Bit Planes button. Each bit plane allows us to view the values of each bit for all image pixels. One way to hide information is to set specific values for one bit in all image pixels. We can't see the difference with our eyes, so we cannot detect that the image has hidden information. Switching to either of Red 0, Blue 0, Green 0 shows us a number 1225 in white.

![password](https://github.com/ram-nush/writeups/assets/75689075/2f0baf3f-8b12-4119-af2d-43553e6e24ce)

This number appears to be a password for some form of steganography. This password is likely used to hide a file inside the fox file. I tried to use [steghide](https://www.kali.org/tools/steghide/ "steghide"), a steganography tool in Kali Linux, with the password given to extract files from the fox file. It did not work. This means that the file was hidden using other steganography software.

![attempt](https://github.com/ram-nush/writeups/assets/75689075/09233de4-bd6b-4111-a0d2-b97600105ec6)

I decided to use [OpenStego](https://www.openstego.com/ "OpenStego"), a popular steganography tool used to hide data within files. Giving the fox file and the password, we are able to extract a flag.txt file from the fox file.

<img src="https://github.com/ram-nush/writeups/assets/75689075/3e48a717-ed96-476b-abaa-e47a581e4968" width="500">

Opening the text file, we get the flag.

![flag](https://github.com/ram-nush/writeups/assets/75689075/8b92af13-175b-4d6d-938f-db99c1432b0f)

## Flag
```
CDDC2024{i_l0vE_y0u}
```

## Thoughts
This challenge involved the use of steganography tools (StegOnline) and methods (Least Significant Bit). I have learned that any steganography software can be used to hide files. The only way to get the hidden file is to use the same software that hid the file and extract the hidden file. In any steganography challenge, I would have to use a list of steganography tools to figure out the tool used to hide the file.
