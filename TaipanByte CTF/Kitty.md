# Meow meow meow meow
## Challenge
- Steganography
- TaipanByte CTF
- 25 Points

### Description
>Meow meow meow meow
>
>Download: 3.jpg

Challenge URL: https://chart.taipanbyte.ru/

<img width="630" height="912" alt="image" src="https://github.com/user-attachments/assets/78aaad92-2709-47ba-a503-63fa37bd342d" />

### Objective
Find the hidden flag within the provided cat image using steganography techniques.
![3](https://github.com/user-attachments/assets/0043421b-2de2-451c-9b56-a8b85221bd9c)


## Analysis
This challenge presents an image of a cat (3.jpg) with the playful description "Meow meow meow meow". The category #Stego immediately indicates this is a steganography challenge where data is hidden within the image.

At first glance, the image appears to be a normal photograph of a cat. However, steganography challenges often hide information in ways that aren't immediately visible to the naked eye.

## Theory
**Steganography**:
Steganography is the practice of concealing information within another message or physical object. In digital steganography, data can be hidden in images using several techniques:

**LSB (Least Significant Bit) Steganography**:
- One of the most common techniques
- Modifies the least significant bits of pixel color values
- These changes are imperceptible to the human eye
- Can hide text, images, or other data within an image

**LSB Half**:
- A variant that uses only half of the LSB data
- Helps reduce noise and make hidden data more readable
- Often reveals hidden text or patterns more clearly

**Visual Steganography**:
- Sometimes data is hidden in plain sight using very faint text
- Text may be nearly invisible against similar-colored backgrounds
- Requires careful examination or contrast adjustment

## Solution

### Method 1: Visual Inspection (AI-Assisted)
Using AI image analysis (Gemini), a very faint text was discovered in the upper right corner of the original image. The text is almost invisible, blending with the white background.

Upon close inspection of this area, the flag can be found: `flag{funny_st3g4}`

### Method 2: LSB Steganography Analysis
1. **Upload the image to StegOnline**: Navigate to https://georgeom.net/StegOnline/image

2. **Upload the file**: Click "Upload" and select the 3.jpg image

3. **Apply LSB Half extraction**: 
   - Select the "LSB Half" option from the available extraction methods
   - This reveals the hidden data embedded in the least significant bits of the image

4. **View the result**: The tool displays the hidden flag embedded in the image

<img width="604" height="604" alt="image" src="https://github.com/user-attachments/assets/cbf633d6-5df7-494c-88c5-13975190b2c6" />

**Flag**: `flag{funny_st3g4}`

## How to avoid
When creating steganography challenges:
- LSB steganography can be easily detected with automated tools like StegOnline, zsteg, or steghide
- For more secure hiding:
  - Use encrypted steganography
  - Combine multiple steganography techniques
  - Use less common bit planes (not just LSB)
  - Apply password protection to embedded data
- Visual text hiding is vulnerable to AI image analysis and contrast adjustments
- Consider using more sophisticated techniques like DCT (Discrete Cosine Transform) steganography
