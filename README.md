# Project Almanac: Steganography

Final assignment for Digital Image Processing. The final code can be found in this git repository or [here](code.py)

| Students                  | NUSP    | 
|:-------------------------:|:-------:| 
| Daniel Sivaldi Feres      | 9912275 |
| João Pedro Souza dos Reis | 9293373 |


## 1. Introduction

With this project we're trying to learn more about steganography and it's use with audios. We're going to hide a movie audio in it's poster image, merging the two and then extracting the audio later. 
  
Unlike encryption, that conceals the data by scrambling information, steganography hides the important data in another file, that doesn't appear to be anormal.
  
As we like very much movies and soundtracks, we chose this theme for our project.
  
## 2. The Method
 
We're going to use the MSB Substitution method to hide the audio. It basically replaces the most significant bit in some bytes of the colored image with some data of the audio. 

As we use colored image in the RGB color channel, the work is done within the 3 sets of 8 bits for each pixel, each byte representing red, green and blue. If we change just a few bits in each pixel (inserting the audio data), the image will stay almost the same to the human eye.
 
 
### Hiding process:

The libraries we use are __imageio__ and __matplotlib__ to read the images and make some adjustments, __numpy__ to use arrays and matrices for the algorithm part, __AudioSegment__ from __pydub__ and __wave__ to deal with the audio in _.mp3_ and _.wav_ formats

```python
import numpy as np
import imageio
import matplotlib.image as mpimg
from matplotlib import pyplot as plt
from pydub import AudioSegment
import wave
```

To begin, we read the movie poster image

```python
poster1 = mpimg.imread("five_hundred_days_of_summer_xxlg.jpg")
```

Then the audio. Note that we open the audio (wich is a _.mp3_ file) and convert it to a _.wav_, because .......

```python
songmp31 = AudioSegment.from_mp3("Sing Street Riddle of the Model clip - in cinemas May 20.mp3")
songmp31.export("Kalimba.wav", format="wav")
audio = wave.open("Kalimba.wav", mode = None)
```

To prepare for the insertion in the image, we transform the _.wav_ file into an array of bytes, so we can manipulate then easier

```python
allbytes = audio.readframes(audio.getnframes())
allbytes = bytearray(allbytes)
```

Now we can finally apply the MSB Substitution method.

Using the _for_ loop, we run trough the whole array of bytes of the audio

```python
for i in range(len(payload)):
```

Then we check the index _i_, with the operator _mod_, and modify each part of the _r_, _g_ and _b_. We put it in the string format and change the byte i.e. for the _red_ part we transform the original 8 bits into 2 bits with audio information + 6 least significant bits of the original image

```python
if i % 3 == 0:
  r = '{0:08b}'.format(image2d[j][0])
  newR = payload[i][0:2] + r[2:]
  x = newBitGuardian(x)
if i % 3 == 1:
  g = '{0:08b}'.format(image2d[j][1])
  newG = payload[i][0:2] + g[2:]
  x = newBitGuardian(x)
if i % 3 == 2:
  b = '{0:08b}'.format(image2d[j][2])
  newB = payload[i][0:2] + b[2:]
  x = newBitGuardian(x)
```

Also, there's a auxiliary function we made __newBitGuardian()__ .....

```python
def newBitGuardian(x):
  if(x % 10 == 0 ):
    return 2
  else:
    return x+2
```

Finally, we write the new _r_, _g_ and _b_ into the output image and begin the loop again

```python
output[j] = [int(newR),int(newG),int(newB)]
j+=1
```

### Retrieving the audio:

 Read the stego-image; 
 Check the pixel bits with the MSB algorithm and find the bits of the audio;
 Reconstruct the audio;


## 3. Examples

This are the movies we chose:

Guardians of the Galaxy

<a href="url"><img src="https://github.com/danisivaldi/pdi/blob/master/guardiansofthegalaxy.jpg" align="left" height="30%" width="30%" ></a>


Lalaland

<a href="url"><img src="https://github.com/danisivaldi/pdi/blob/master/lalaland.jpg" align="left" height="30%" width="30%" ></a>

Pulp Fiction

<a href="url"><img src="https://github.com/danisivaldi/pdi/blob/master/pulpfiction.jpg" align="left" height="30%" width="30%" ></a>
