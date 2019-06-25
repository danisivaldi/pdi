# Project Almanac: Steganography

Final assignment for Digital Image Processing. The final code can be found in this git repository or [here](code.py)

| Students                  | NUSP    | 
|:-------------------------:|:-------:| 
| Daniel Sivaldi Feres      | 9912275 |
| João Pedro Souza dos Reis | 9293373 |


## 1. Introduction

With this project we're trying to learn more about steganography and it's use with audios. We're going to hide a movie scene audio in it's poster image, merging the two.

Unlike encryption, that conceals the data by scrambling information, steganography hides the important data in another file, that doesn't appear to be anormal.
  
As we like very much movies, we chose this theme for our project.

  
## 2. The Method
 
We're going to use the MSB Substitution method to hide the audio. It basically replaces the most significant bit in some bytes of the colored image with some data of the audio. We read a paper tha talked about the method and as we wanted a diferente approach for the hiding algorithm, we thought we should try it.

As we use colored image in the RGB color channel, the work is done within the 3 sets of 8 bits for each pixel, each byte representing red, green and blue. If we change just a few bits in each pixel (inserting the audio data), the image will stay almost the same to the human eye.
 
 
### Hiding process:

The libraries we use are __imageio__ and __matplotlib__ to read the images and make some adjustments, __numpy__ to use arrays and matrices for the algorithm part, __AudioSegment__ from __pydub__ to deal with the audio in _.mp3_ format

```python
import numpy as np
import imageio
import matplotlib.image as mpimg
from matplotlib import pyplot as plt
from pydub import AudioSegment
```

To begin, we read the movie poster image

```python
poster1 = mpimg.imread("five_hundred_days_of_summer_xxlg.jpg")
```

To prepare for the insertion in the image, we open and transform the _.mp3_ file into an array of bytes, so we can manipulate then easier

```python
data = open("HER (Theodore Memories).mp3", 'rb').read()
data = extendBytes(data)
```

As noticed, we also apply a function __extendBytes()__ to transform the audio bytes. We basically take groups of 2 bits of each byte and extend them, forming a 8 space array, with the first two corresponding the bits of the audio, and the rest of zeros. Intuitively, this loop runs 4 times, as a byte has 8 bits

```python
def extendBytes(payload):
  newBytes = []
  for i in range (len(payload)):
    for j in range(4): 
      shift = (192 >>(2*j))
      newNumber = payload[i] & shift
      newBytes.append('{0:08b}'.format(newNumber))
  for j in range(4): 
    newBytes.append('{0:08b}'.format(0))
  return newBytes
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

Also, there's a auxiliary function we made __newBitGuardian()__ that redo what __extendBytes()__ did. It jumps the 8 bits arrays we formed, taking only the first 2 bits, wich are the bits corresponding to the audio byte

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

## 3. Results

This are the movies we chose:

<a href="url"><img src="https://github.com/danisivaldi/pdi/blob/master/her_xxlg.jpg" align="left" height="30%" width="30%" ></a>
<a href="url"><img src="https://github.com/danisivaldi/pdi/blob/master/spiderman.jpg" align="left" height="30%" width="30%" ></a>
<a href="url"><img src="https://github.com/danisivaldi/pdi/blob/master/500.jpg" align="left" height="30%" width="30%" ></a>
***

And the results after we apply the hiding algorithm:

<a href="url"><img src="https://github.com/danisivaldi/pdi/blob/master/her_msb.jpg" align="left" height="30%" width="30%" ></a>
<a href="url"><img src="https://github.com/danisivaldi/pdi/blob/master/spiderman_msb.jpg" align="left" height="30%" width="30%" ></a>
<a href="url"><img src="https://github.com/danisivaldi/pdi/blob/master/500_msb.jpg" align="left" height="30%" width="30%" ></a>
***


## 4. Retry

As we can cleary see, the results weren't great. The MSB method wasn't effective, even after we modified our approach numerous times. The movie poster can still be recognized, but its not ideal. 

To retry, we applied a diferent algorithm, this time LSB. It's the same idea, but this time we change the least significant bits of the bytes of the image to hide audio data.

The only bit of code that changes between the methods is the computing of the new _r_, _g_ and _b_. This time we add the audio data before the other bits

Then

```python
newR = payload[i][0:2] + r[2:]
```

and now

```python
newR = payload[i][0:2] + r[2:]
```

Again, the movies:

<a href="url"><img src="https://github.com/danisivaldi/pdi/blob/master/her_xxlg.jpg" align="left" height="30%" width="30%" ></a>
<a href="url"><img src="https://github.com/danisivaldi/pdi/blob/master/spiderman.jpg" align="left" height="30%" width="30%" ></a>
<a href="url"><img src="https://github.com/danisivaldi/pdi/blob/master/500.jpg" align="left" height="30%" width="30%" ></a>
***


And the results after we apply the hiding algorithm:

<a href="url"><img src="https://github.com/danisivaldi/pdi/blob/master/her_lsb.jpg" align="left" height="30%" width="30%" ></a>
<a href="url"><img src="https://github.com/danisivaldi/pdi/blob/master/spiderman_lsb.jpg" align="left" height="30%" width="30%" ></a>
<a href="url"><img src="https://github.com/danisivaldi/pdi/blob/master/500_lsb.jpg" align="left" height="30%" width="30%" ></a>
***

## 4. Conclusion

To wrap it up, we'd like to present some results of our assignment.
* Project idea
  * As we love music and movies, firs we wanted to hide an entire album into a album cover, but that hard beacuse we couldn't find big images for album covers. We changed our minds a lot about the final idea, but we kept finding problems either with image or audio size.
  * The algorithm of choice wasn't the best. We put in a lot of time trying to make the MSB work, and that overloaded us.
* Audio handling
  * We were limited by the format. Working with diferent types of audios wouldn't be as easy, because the libraries we had only let us work with _.wav_ and _.mp3_ files if we wanted to convert it to bytes. We had a lot of work trying to use _.wav_ files, because we had to convert it from _.mp3_ and convert it to bytes manually, but in the end it didn't work the way we wanted. 
  * On the same line, the size of the audio was also a problem. We tried some compression techniques but they weren't capable to make the audio files smaller enough to fit the images. We had to use rather small audios for the algorithm to work, but they were still significant for our purpose.
* Image hiding
  * For a next step in the project, we could try to take a large audio, maybe the movie itself, and strip it into loads of smaller parts, to fit various images and scramble then with another algorithm, making the hiding security better.
  * We could also try diferent approches for the method, using another steganography algorithm.
