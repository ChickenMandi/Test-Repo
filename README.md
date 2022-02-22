![Heriot-Watt University logo](https://upload.wikimedia.org/wikipedia/commons/thumb/0/03/Heriot-Watt_University_logo.svg/600px-Heriot-Watt_University_logo.svg.png)
![C Programming Language](https://upload.wikimedia.org/wikipedia/commons/thumb/1/18/C_Programming_Language.svg/217px-C_Programming_Language.svg.png)

# Who I am

- Student ID: H00385850

# Program design

### Reading:

- **readPPM()** opens a file in 'read' mode and loads the image using **getPPM()**.
- **getPPM()** first scans the magicNumber, width, height, max from the file and stores them in the PPM structure under their respective attributes. The last attribute in the PPM is a matrix that stores the (r,g,b) values for each pixel in the image. A nested for-loop is used to iterate through each pixel in the raster [matrix] from left to right and top to bottom respectively, which is the order in which the pixel's (r,g,b) values are stored in the file. In this way, the values from the file are scanned and stored in the raster [matrix].

### Writing:

- showPPM() is given a PPM and prints its contents to stdout. It prints the magicNumber in the first line, the width and the height on the second, and the max on the third. Finally, similar to reading, a nested for-loop is used to iterate through each pixel in the raster in the same left to right and top to bottom order, but this time, their values are printed out.

### Encoding:

- encode() is given a message and PPM and replaces the red values of random, successive, pixels in the raster with the ascii values of successive characters in the message.

### Decoding:

- decode() is given two PPMs and compares the red values of their pixels. A difference in the values suggests an encoded character and since the characters are in a successive order, they can be appended to a character array in the same order to decode the message.

# Choice of data structures and algorithms

### Pixel Structure

- The Pixel Structure contains 3 attributes:
    - red - Red value
    - green - Green value
    - blue - Blue value

### PPM Structure

- The PPM Structure contains 5 attributes:
    - magicNumber - Identifies the type of ppm image (P3 or P6)
    - width - Number of horizontal pixels
    - height - Number of vertical pixels
    - max - Maximum (r,g,b) values
    - raster - 2D array or matrix that represents the pixels of the image

### PPM Algorithm:

- There are three functions involved: makePPM(), makeRaster(), and makeVector()
    - makePPM() mallocs a new PPM Structure and is given the first four attributes which it assigns respectively. It then assigns the result of makeRaster() to the raster attribute.
    - makeRaster() is given the height and width attributes, with which, it mallocs an array of Pixel pointers with the length of 'height'. This can be thought of as an index column for each row in the matrix. It then iterates through this array using a for-loop and points each pointer to the result of makeVector().
    - makeVector() simply mallocs an array of Pixels of length 'width' and returns it.

### Encoding Algorithm:

- encode() is given a message and a PPM. It derives the pixelCount by multiplying the width and the height. It calculates a randMax as pixelCount/messageLength. A random number is generated between (and including) 1 and randMax. the randMax calculation ensures that even if every random number generated was the maximum, the pixel it represents would never go off the raster. It also keeps track of a pixelNumber and characterIndex so it knows when it has reached the random pixel and what character ascii value to replace that pixel's red value. The way it iterates through each pixel in the raster is the same, yet again, from left to right and top to bottom. The function is also copying the RGB values from the original image to the new image while encoding. It has a condition to only replace values if it hasn't reached the end of the message. It checks if the pixelNumber and the randomNumber are the same (ie. it is on the pixel whose value is to be replaced) but before replacing the value it checks if the red values are already the same. Replacing a red value with the save value would make the character unrecognizable for the decoder so to account for this it increments the randomNumber by 1. This runs the same checks on the next pixel. This repeats until we find a valid pixel whose red value is different by default and can be replaced. Once a characted is encoded, the characterIndex is incremented, the pixelNumber is reset and a new randomNumber is generated so we can encode the next character. This continues until it reach the end of the message and the end of the traverse and copy, after which we return the new encoded image. If for whatever reason, it is unable to encode the entire message because of too many equal values or too many large random numbers, it raises an error and returns NULL.

### Decoding Algorithm:

- decode() is given two PPM images, the original one and the encoded one. First, it mallocs a character array with a length of the number of pixels in the image. It then traverses the rasters of both images in the same order and compares the red values of each pixel. If there is a difference, it stores the red value of the pixel in the encoded image and appends it to the character array. Since the characters were encoded in the same order of traversal, the sequence of characters is preserved. The decoded message is returned.
