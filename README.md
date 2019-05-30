# Project Almanac: Steganography

1. Introduction

  With this project we're trying to learn more about steganography and it's use with audios. We're going to hide a movie soundtrack in it's poster image, merging the two and then extracting the soundtrack. 
  
  Unlike encryption, that conceals the data by scrambling information, steganography hides the important data in another file, that doesn't appear to be anormal.
  
  As we like very much movies and soundtracks, we chose this theme for our project.
  
2. The Method
 
 We're going to use the MSB Substitution method to hide the audio. It basically replaces the most significant bit in some bytes of the colored image with some data of the audio. As we use colored image in the RGB color channel, the work is done within the 3 sets of 8 bits for each pixel, each byte representing red, green and blue. If we change just a few bits in each pixel (inserting the audio data), the image will stay almost the same to the human eye.
 
 
 Hiding process:
 
 
 First we need to read the cover image;
 
 Convert the audio file into a sequence of bits;
 
 Insert the bits of the audio into each pixel of the image;
 
 Form the stego-image;
 
 
 Retrieving the audio:
 
 
 Read the stego-image;
 
 Check the pixel bits with the MSB algorithm and find the bits of the audio;
 
 Reconstruct the audio;
 
3. Examples

This are the movies we chose:

Guardians of the Galaxy

![alt text](https://github.com/danisivaldi/pdi/blob/master/guardiansofthegalaxy.jpg)

Lalaland

![alt text](https://github.com/danisivaldi/pdi/blob/master/lalaland.jpg)

Pulp Fiction

![alt text](https://github.com/danisivaldi/pdi/blob/master/pulpfiction.jpg)

4. Students:

  Daniel Sivaldi Feres 9912275
  João Souza dos Reis  9293373

> Assigment for Digital Image Processing (SCC0251)
