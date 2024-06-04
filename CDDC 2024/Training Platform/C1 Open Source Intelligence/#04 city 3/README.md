# \#04 city 3 (100 pts)
> What is the name of this lake?
>
> Replace the xxx with the name of the lake CDDC2024{xxx_lake}, lower case

## Attachments
Downloading the attachments, we get a picture of a lake. The only other clue is that the image was taken from Google.

![city3](https://github.com/ram-nush/writeups/assets/75689075/f3180f3d-9ecb-4090-b730-924bdcacaff3)

## Approach
To identify the location of any image, [Google Lens](https://lens.google/ "Google Lens") is the best tool to use. It takes in an input image and compares it with billions of images on the Internet. Searching the entire image on Google Lens, I get mixed results.

![mixed](https://github.com/ram-nush/writeups/assets/75689075/601a667a-6122-4d8a-8e69-3f810b568fc5)

I decided to crop the important part of the image, which includes the houses on the right. Searching that part of the image on Google Lens, all the results point to Danau Batur Lake.

![result](https://github.com/ram-nush/writeups/assets/75689075/3d341210-2b92-49f8-94b5-61856de59784)

To confirm the exact location the picture was taken in, I searched Danau Batur Lake on Google Earth. There are a few places around the lake. The one that matches the picture the closest is Kuburan Terunyan View Point.

![unnamed](https://github.com/ram-nush/writeups/assets/75689075/68f0b2c5-f040-4af9-9c55-709d1b2df225)

Placing the Street View man on the View Point, we get the exact image as the one given.

![unnamed](https://github.com/ram-nush/writeups/assets/75689075/9eb400b6-7ee5-4777-afe4-1b0404f5bf87)

We have confirmed that the lake is Danau Batur lake. In some cases, it is called Danau Batur lake and Batur lake in other cases. Since the challenge description does not mention "no spaces" in the answer, we can deduce that the answer is Batur lake.

## Flag
```
CDDC2024{batur_lake}
```

## Thoughts
This challenge involves the use of Google Lens. This particular image did not have much unique features compared to other similar challenges. I have learned to crop just the important parts of the image. This cropped image has specific features, so Google Lens will be able to identify the image better.
