```markdown
# Red CTF Write-Up - PicoCTF 2025

**Category:** Forensics  
**Difficulty:** Medium

## Overview

In this challenge, we are tasked with extracting a hidden flag from an image file, `red.png`. The image appears to contain some encoded data that needs to be analyzed and decoded to uncover the flag. This write-up walks through each step to solve the challenge.

---

## Step 1: Analyzing the Image

The first step is to check the image file to gather some metadata and any hidden clues. Start by inspecting the image with the `file` command:

```bash
file red.png
```

This command confirms that the file is a valid PNG image, 128x128 pixels in size.

---

## Step 2: Checking for Embedded Metadata

Next, we use **ExifTool** to check if any hidden information is stored in the image's metadata:

```bash
exiftool red.png
```

From the output, we observe a **Poem** field with the following content:

```
Crimson heart, vibrant and bold,
Hearts flutter at your sight..
Evenings glow softly red,
Cherries burst with sweet life..
Kisses linger with your warmth..
Love deep as merlot..
Scarlet leaves falling softly..
Bold in every stroke.
```

This appears to be just a poetic description of the color red. However, this could be a hint about the flag format, or potentially contain hidden clues.

---

## Step 3: Steganography

The next step is to check whether there is hidden data in the image using steganographic techniques. We will try a few tools:

### 3.1: **Steghide**

Try extracting hidden data using the `steghide` tool:

```bash
steghide extract -sf red.png
```

However, if you're on macOS, you may encounter an error stating that `steghide` is not installed. In that case, you can install it using `brew` (though it’s sometimes not available):

```bash
brew install steghide
```

If `steghide` is unavailable, you may need to use other methods.

### 3.2: **Using zsteg**

If `steghide` doesn't work, we can attempt a different tool, `zsteg`, which is designed for detecting hidden data in images with different bit planes.

```bash
gem install zsteg
```

Then, use `zsteg` to analyze the PNG file:

```bash
zsteg red.png
```

In the output, we find the following base64-encoded string:

```
cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ==
```

This is an indication that there is hidden data embedded in the image.

---

## Step 4: Decoding the Hidden Data

The string above is **Base64** encoded. To decode it, use the following command:

```bash
echo "cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ==" | base64 -d
```

The decoded output will reveal the flag:

```
picoCTF{decoded_base64_string}
```

---

## Step 5: Conclusion

By following these steps—inspecting the image's metadata, analyzing it for steganography, and decoding the hidden Base64 string—we successfully recovered the flag for the **Red CTF Forensics challenge**.

---

## Tools Used

- `file`
- `exiftool`
- `zsteg`
- `base64`

```