# \#04 recimg (200 pts)
> You recently learned that your organization's personnel were using Netcat to transfer important files over the network. With this information, they hired a hacker to intercept the packets and obtain a PCAP file, which they need to analyze to recover the important image files.

## Attachments
Downloading the attachments, we get a recimg.pcap file, which can be analysed with Wireshark.

![capture](https://github.com/ram-nush/writeups/assets/75689075/6009f8a8-ed13-4ab0-a2f1-25b1d366f46d)

## Approach
### Getting image file data
Clicking on Statistics from the menu bar and then on Conversations, we get that the file contains only one TCP connection between the source 172.20.10.3 and destination 13.125.155.5, lasting 4834 packets.

![conversation](https://github.com/ram-nush/writeups/assets/75689075/5bbb6419-6719-4a92-8ae4-1143e85847e8)

Looking at packet 4, we see that some of the first few bytes of the data field has the string PNG. This means that the image file sent has been fragmented. We need to find a way to reassemble the data together to recreate the image file.

![start of file](https://github.com/ram-nush/writeups/assets/75689075/bc0a9b99-7c73-4033-a8e4-1f2330781ad6)

There has to be an easy way, so I searched online. From this Wireshark [webpage](https://osqa-ask.wireshark.org/questions/26786/grouping-by-conversations/ "grouping"), I found that the Follow Stream technique can be used to view the raw data from all the packets grouped together. We can do so by clicking the specific conversation and then the Follow Stream button on the Conversations tab.

![raw](https://github.com/ram-nush/writeups/assets/75689075/27f23123-33a1-4327-bb85-217f2e70b502)

In the "Show data as" drop-down field, I changed the option from ASCII to Raw to get the bytes of the image file, and saved the file with the .png extension. Opening the file in Photos, I get a file with randomly colored pixels.

![random](https://github.com/ram-nush/writeups/assets/75689075/0c09674f-e467-4386-bed5-237c918b80b0)

### Image file or image files?
I got an image that opened correctly, but the values of the pixels appeared random. With a file size of close to 7MB, it seemed too big for an image file. Reading through the challenge description, it led me to believe that there are actually multiple PNG files in the raw data.

I opened the file in [Hex Editor Neo](https://freehexeditorneo.com/), a software which can view and edit hex bytes. Searching for all occurrences of the string "PNG" within the file, I get 11 occurrences.

![find](https://github.com/ram-nush/writeups/assets/75689075/ed58d833-6c69-4d1f-bbcb-94a72b877e00)

![result](https://github.com/ram-nush/writeups/assets/75689075/17429b2b-4531-49bd-972b-4d411566ef09)

Using the Find Previous and Next buttons, I was able to locate each PNG string and segmented the bytes accordingly. I pasted the bytes of the first PNG file in a new file and renamed the file extension to png. This gave another image with random pixel values.

Repeating the process for the next few PNG files, I noticed that the bytes always ended at about 0xc0670, which was a bit strange. This likely means that these files will give the same random image on opening.

![random length](https://github.com/ram-nush/writeups/assets/75689075/fe56f93a-f4c6-4d1e-814f-822fffd4f1a1)

On the sixth PNG file, the file size was significantly smaller, ending at 0x01da0. Renaming the extension and opening the file, we get the flag as an image.

![different length](https://github.com/ram-nush/writeups/assets/75689075/5c8836cc-aed5-456d-a01e-f74c43b07408)

![flag](https://github.com/ram-nush/writeups/assets/75689075/f9c7d16b-360e-4c38-a08a-55e0cca39301)

## Flag
```
CDDC2024{Pack3tF1ndImageR3covery}
```

## Thoughts
Before attempting this challenge, I had limited knowledge of Wireshark. Through this challenge, I have learnt how to follow streams and extract data using Wireshark. After seeing the random image on opening the whole PNG file, I was able to figure out that there were multiple files present, which has improved my problem-solving skills. Overall, it was a great challenge to solve.
