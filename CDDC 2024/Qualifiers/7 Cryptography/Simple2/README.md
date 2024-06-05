# Simple2 (100 pts)
> Acquired AES-encrypted string and KEY, but the length of the encrypted string is a little weird. Decipher the string.
> 
> encryptedData = "6f7e9007dd0882f3f320a08690a230b84fcfa66b483dc4f4352123276622af4cc5c656bf0171c36271700f8f4f0f41d14d7c20baec601c70f670acc8b6037a"
> 
> keyString = "6eba99bf3fac4c92a857b05cff433a39"
>
> Flag format : CDDC24{   }

## Attachments
Downloading the attachment, we get a file simple2.go, which is a source code file written in Go (or golang) programming language. This file has a main function which decrypts a string using AES, given the data and key.

```go
package main

import (
	"crypto/aes"
	"crypto/cipher"
	"encoding/hex"
	"fmt"
	"log"
)

func main() {

	encryptedData := "6f7e9007dd0882f3f320a08690a230b84fcfa66b483dc4f4352123276622af4cc5c656bf0171c36271700f8f4f0f41d14d7c20baec601c70f670acc8b6037a"
	keyString := "6eba99bf3fac4c92a857b05cff433a39"

	key, err := hex.DecodeString(keyString)
	if err != nil {
		log.Fatal(err)
	}
	ciphertext, err := hex.DecodeString(encryptedData)
	if err != nil {
		log.Fatal(err)
	}

	block, err := aes.NewCipher(key)
	if err != nil {
		log.Fatal(err)
	}
	if len(ciphertext)%aes.BlockSize != 0 {
		log.Fatal("The length of the encrypted data is incorrect")
	}

	iv := ciphertext[:aes.BlockSize]
	ciphertext = ciphertext[aes.BlockSize:]

	mode := cipher.NewCBCDecrypter(block, iv)
	mode.CryptBlocks(ciphertext, ciphertext)

	padding := int(ciphertext[len(ciphertext)-1])
	if padding < 1 || padding > aes.BlockSize {
		log.Fatal("This is incorrect padding.")
	}
	for _, val := range ciphertext[len(ciphertext)-padding:] {
		if int(val) != padding {
			log.Fatal("This is incorrect padding.")
		}
	}
	ciphertext = ciphertext[:len(ciphertext)-padding]

	fmt.Printf("Decrypted message: %s\n", string(ciphertext))
}
```

## Approach
I am unfamiliar with the Go programming language. As such, I am using an online [compiler](https://www.programiz.com/golang/online-compiler/ "Programiz") to run the code.

Attempting to run the program as it is, we get the message "The length of the encrypted data is incorrect".

![image](https://github.com/ram-nush/writeups/assets/75689075/54b20329-fc74-4755-accb-e5dfe24aff0e)

Counting the number of characters in the encryptedData variable using [CyberChef](https://gchq.github.io/CyberChef/#input=NmY3ZTkwMDdkZDA4ODJmM2YzMjBhMDg2OTBhMjMwYjg0ZmNmYTY2YjQ4M2RjNGY0MzUyMTIzMjc2NjIyYWY0Y2M1YzY1NmJmMDE3MWMzNjI3MTcwMGY4ZjRmMGY0MWQxNGQ3YzIwYmFlYzYwMWM3MGY2NzBhY2M4YjYwMzdh), we get 126 hex characters. This is two short of 128 hex characters (or 64 bytes), a power of 2. The data is likely missing two hex characters, either from the front or the back.

![image](https://github.com/ram-nush/writeups/assets/75689075/9c9d97be-535a-46db-ad6f-06f1df1bbdeb)

One way to find out the missing characters is to brute-force, since the total number of possibilities is a mere 16 * 16 = 256. It is simple to enumerate and test all possibilities by adding a for loop in the code. Since the syntax of Go is slightly different from the languages I am used to (C++, Java), I searched for some [example codes](https://gobyexample.com/ "Go by Example") in Go.

The changes made are as follows:
- for loop at the beginning of the main function to append every combination of two hex characters to the original encrypted data
- printing messages and continue statements for incorrect padding, before the log.Fatal() line executes.

```go
package main

import (
	"crypto/aes"
	"crypto/cipher"
	"encoding/hex"
	"fmt"
	"log"
)

func main() {

	encryptedData := "6f7e9007dd0882f3f320a08690a230b84fcfa66b483dc4f4352123276622af4cc5c656bf0171c36271700f8f4f0f41d14d7c20baec601c70f670acc8b6037a"
	keyString := "6eba99bf3fac4c92a857b05cff433a39"

    // append all possible combinations
    for i := 0; i < 256; i++ {
      h := fmt.Sprintf("%02x", i)
      fmt.Println(h)
      eD := encryptedData + h
        
	    key, err := hex.DecodeString(keyString)
	    if err != nil {
        log.Fatal(err)
	    }
	    ciphertext, err := hex.DecodeString(eD)
	    if err != nil {
        log.Fatal(err)
	    }

	    block, err := aes.NewCipher(key)
	    if err != nil {
		    log.Fatal(err)
	    }
	    if len(ciphertext)%aes.BlockSize != 0 {
		    log.Fatal("The length of the encrypted data is incorrect")
	    }

	    iv := ciphertext[:aes.BlockSize]
	    ciphertext = ciphertext[aes.BlockSize:]

	    mode := cipher.NewCBCDecrypter(block, iv)
	    mode.CryptBlocks(ciphertext, ciphertext)

    	padding := int(ciphertext[len(ciphertext)-1])
	    if padding < 1 || padding > aes.BlockSize {
        print("incorrect padding 1\n")
		    continue;
		    log.Fatal("This is incorrect padding.")
	    }
	    for _, val := range ciphertext[len(ciphertext)-padding:] {
		    if int(val) != padding {
          print("incorrect padding 2\n")
          continue;
			    log.Fatal("This is incorrect padding.")
		    }
	    }
	    ciphertext = ciphertext[:len(ciphertext)-padding]

	    fmt.Printf("Decrypted message: %s\n", string(ciphertext))
    }
}
```

The output of the code gives us the flag, when the missing two chararacters at the back are 09.

![image](https://github.com/ram-nush/writeups/assets/75689075/32d50c40-cb37-4fb3-9ac1-b39ab4c3881c)

Scrolling through the rest of the output, I find that the flag (without the closing brace) is leaked at other values (23, 41, a1). This means that the last two characters only affect the value of the last character of the plaintext. This kind of behaviour is similar to using stream ciphers, where plaintext is encrypted byte by byte.

<img src="https://github.com/ram-nush/writeups/assets/75689075/222ef13b-4e25-45d8-881b-c92e6a44c42a" width="500">
<img src="https://github.com/ram-nush/writeups/assets/75689075/afedc158-a8e6-41b0-8e6e-6b8bc80b4485" width="500">
<img src="https://github.com/ram-nush/writeups/assets/75689075/65b54405-6812-43a4-937e-28f92b9fce49" width="500">


### What type of cipher is AES?
However, [AES](https://www.geeksforgeeks.org/advanced-encryption-standard-aes/ "GeeksforGeeks") is in fact a block cipher, which encrypts the plaintext in pre-determined size blocks (128 bits or 16 bytes). Each block is independent of other blocks, so the last few characters in the encrypted string will only affect the last block of plaintext.

## Flag
```
CDDC24{P4ssw0rd-Strength-M4tters}
```

## Thoughts
In the real world, it is quite common for either the data or the key to lack a few characters. Some reasons might be copying incorrectly or network issues when transmitting the information. This challenge is quite simple, as per the challenge name. Although I was unfamiliar with Go and AES, these can be learned through online resources. Since the approach is the same, I was able to solve the challenge within a short time.
