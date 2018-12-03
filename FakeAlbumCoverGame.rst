
.. raw:: html

   <div class="alert alert-block alert-info" style="margin-top: 20px">

.. raw:: html

   <h1 align="center">

 Make Fake Album Cover Game

.. raw:: html

   </h1>

Table of Contents
-----------------

Our goal is to create randomly generated album covers with:

.. raw:: html

   <div class="alert alert-block alert-info" style="margin-top: 20px">

.. raw:: html

   <ol>

.. raw:: html

   <li>

Learn how to use the function display\_cover

.. raw:: html

   </li>

.. raw:: html

   <li>

Loading a random page from Wikipedia

.. raw:: html

   </li>

.. raw:: html

   <li>

Extracting the Title of the Article

.. raw:: html

   </li>

.. raw:: html

   <li>

 Displaying the Album Cover

.. raw:: html

   </li>

.. raw:: html

   </ol>

.. raw:: html

   <p>

.. raw:: html

   </p>

Estimated Time Needed: 60 min

.. raw:: html

   </div>

.. raw:: html

   <hr>

Inspiration: `Fake Album Covers <https://fakealbumcovers.com/>`__

Import libraries
^^^^^^^^^^^^^^^^

.. code:: python

    from IPython.display import Image as IPythonImage
    from PIL import Image
    from PIL import ImageFont
    from PIL import ImageDraw

Helper function to superimpose text on image
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    def display_cover(top,bottom ):
        """This fucntoin
        """
        import requests
        
        name='album_art_raw.png'
        # Now let's make get an album cover.
        # https://picsum.photos/ is a free service that offers random images.
        # Let's get a random image:
        album_art_raw = requests.get('https://picsum.photos/500/500/?random')
        # and save it as 'album_art_raw.png'
        with open(name,'wb') as album_art_raw_file:
           album_art_raw_file.write(album_art_raw.content)
        # Now that we have our raw image, let's open it 
        # and write our band and album name on it
        img = Image.open("album_art_raw.png")
        draw = ImageDraw.Draw(img)
    
        # We'll choose a font for our band and album title, 
        # run "% ls /usr/share/fonts/truetype/dejavu" in a cell to see what else is available,
        # or download your own .ttf fonts!
        band_name_font = ImageFont.truetype("/usr/share/fonts/truetype/dejavu/DejaVuSans-Bold.ttf", 25) #25pt font
        album_name_font = ImageFont.truetype("/usr/share/fonts/truetype/dejavu/DejaVuSans-Bold.ttf", 20) # 20pt font
    
        # the x,y coordinates for where our album name and band name text will start
        # counted from the top left of the picture (in pixels)
        band_x, band_y = 50, 50
        album_x, album_y = 50, 400
    
        # Our text should be visible on any image. A good way
        # of accomplishing that is to use white text with a 
        # black border. We'll use the technique shown here to draw the border:
        # https://mail.python.org/pipermail/image-sig/2009-May/005681.html
        outline_color ="black"
    
        draw.text((band_x-1, band_y-1), top, font=band_name_font, fill=outline_color)
        draw.text((band_x+1, band_y-1), top, font=band_name_font, fill=outline_color)
        draw.text((band_x-1, band_y+1), top, font=band_name_font, fill=outline_color)
        draw.text((band_x+1, band_y+1), top, font=band_name_font, fill=outline_color)
    
        draw.text((album_x-1, album_y-1), bottom , font=album_name_font, fill=outline_color)
        draw.text((album_x+1, album_y-1), bottom , font=album_name_font, fill=outline_color)
        draw.text((album_x-1, album_y+1), bottom , font=album_name_font, fill=outline_color)
        draw.text((album_x+1, album_y+1), bottom , font=album_name_font, fill=outline_color)
    
        draw.text((band_x,band_y),top,(255,255,255),font=band_name_font)
        draw.text((album_x, album_y),bottom,(255,255,255),font=album_name_font)
    
        return img

1) Learn how to use the function display\_cover 
------------------------------------------------

The function **display\_cover** selects a random image from
https://picsum.photos/ and will help us superimpose two strings over the
image. The parameter **top** is the string we would like to superimpose
on the top of an image. The parameter bottom is the string we would like
to display on the bottom of the image. The function does not return the
image but returns an object of type Image from the Pillow library; the
object represents a PIL image.

.. code:: python

    img=display_cover(top='top',bottom='bottom')

To save the image, we use the method **save** . The argument is the file
name of the image we would like to save in this case 'sample-out.png'

.. code:: python

    img.save('sample-out.png')

Finely we use **IPythonImage** to read the image file and display the
results.

.. code:: python

    IPythonImage(filename='sample-out.png')

**Question 1)** Use the **display\_cover** function to display the image
with the name Python on the top and Data Science on the bottom. Save the
image as **'sample-out.png'**.



Part 2: Loading a random page from Wikipedia 
---------------------------------------------

In this project, we will use the request library, we used it in the
function **display\_cover**, but you should import the library in the
next cell.

.. code:: python

    import requests

The following is the URL to the page

.. code:: python

    wikipedia_link='https://en.wikipedia.org/wiki/Special:Random'

**Question 2)** Get Wikipedia page is converted to a string

Use the function **get** from the **requests** library to download the
Wikipedia page using the **wikipedia\_link** as an argument. Assign the
object to the variable **raw\_random\_wikipedia\_page**.

.. code:: python

    #hint: requests.get()

Use the data attribute **text** to extract the XML as a text file a
string and assign the result variable **page**:

.. code:: python

    
    print(page)

Part 3: Extracting the Title of the Article 
============================================

**Question 3 (part 1)** Use the title of the Wikipedia article as the
title of the band. The title of the article is surrounded by the XML
node title as follows: **<title>title - Wikipedia</title>** . For
example, if the title of the article was Python we would see the
following: **<title>Python - Wikipedia</title>**. Consider the example
where the title of the article is Teenage Mutant Ninja Turtles the
result would be: **<title>Teenage Mutant Ninja Turtles -
Wikipedia</title>**. The first step is to find the XML node **<title>**
and **</title>**\ indicating the start and end of the title. The string
function **find** maybe helpful, you can also use libraries like
**xlxml**.


**Question 3 (part 2)** Next get rid of the term \*\* - Wikipedia\*\*
from the title and assign the result to the **band\_title** For example
you can use the function or method **strip** or **replace**.



**Question 4)** Repeat the second and third step, to extract the title
of a second Wikipedia article but use the result to **album\_title**


If you did everything correct the following cell should display the
album and band name:

.. code:: python

    print("Your band: ", band_title)
    print("Your album: ", album_title)

Part 4: Displaying the Album Cover 
-----------------------------------

Use the function **display\_cover** to superimpose the band and album
title over a random image, assign the result to the variable
**album\_cover **.

**Question 5)** use the function display\_cover to display the album
cover with two random article titles representing the name of the band
and the title of the album.


Use the method save to save the image as **sample-out.png**:


Use the function **IPythonImage** to display the image


About the Authors:
~~~~~~~~~~~~~~~~~~

`James Reeve <https://www.linkedin.com/in/reevejamesd/>`__ James Reeves
is a Software Engineering intern at IBM.

`Joseph
Santarcangelo <https://www.linkedin.com/in/joseph-s-50398b136/>`__ has a
PhD in Electrical Engineering, his research focused on using machine
learning, signal processing, and computer vision to determine how videos
impact human cognition. Joseph has been working for IBM since he
completed his PhD.

.. raw:: html

   <hr>

Copyright © 2018
`cognitiveclass.ai <cognitiveclass.ai?utm_source=bducopyrightlink&utm_medium=dswb&utm_campaign=bdu>`__.
This notebook and its source code are released under the terms of the
`MIT License <https://bigdatauniversity.com/mit-license/>`__.
