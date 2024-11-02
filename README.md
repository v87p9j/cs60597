java c
Assignment 4 (Part I)
IME: Image Manipulation and Enhancement 
1 Introduction 
In this series of assignments, you will build an image processing application. You will create text-based and interactive GUI-based user interfaces for this program. Although you will implement a few simple and not-so-simple image processing effects, your system will have "good bones" through design that will streamline adding more image manipulations. 
1.1 Images
Whatever the file format (jpg, png, bmp, etc.) an image is basically a sequence of pixels. Each pixel has a position in the image (row, column) and a color. For a greyscale image there is only one value per pixel. On a scale of 0-1, 0 indicates black and 1 indicates white. Values in between indicate shades of    grey. This value is traditionally represented using an integer. The number of bits used to store this value dictateshow many "levels of grey" are supported. For example, an 8-bit representation creates 256 distinct levels (including black and white).
1.1.1 Color RepresentationFor a color image,the pixel's color is represented by breaking it into individual components (usually 3) in  some way. The most common representation is the red-green-blue (RGB) model. In this model, a color is represented by three numbers (components): red, green, blue. Any color is a combination of these three base colors. On a scale of 0-1, black is represented as (0,0,0),white as (1,1,1) and bright, pure yellow is  represented as (1,1,0). There are several ways of measuring "brightness" or "intensity":
· Value: the maximum value of the three components for each pixel
· Intensity: the average of the three components for each pixel.
· Luma: the weighted sum 0.2126r+0.7152g+0.0722b
The three components can also be represented using integers. Each component is also called a
"channel". If 8 bits are used per channel, this creates the (traditionally-termed) 24-bit image! Some formats also support transparency (e.g. the PNG format): this is done by adding a fourth component.
To see how various colors can be made using these base colors: open Intellij -> Type "Ctrl+Shift+A" on Windows or "Command+Shift+A" on Mac and type "show Color Picker". When selected, you can pick   different colors and observe their red, green and blue values to get an intuition for how this works).
1.2 Image Processing
The ubiquity of images is matched by the plethora of image manipulation programs and services. All phones with a camera also include apps that allow us to brighten, darken, blur or crop photos. Instagram has''filters" that convert a picture into something more interesting. Programs on traditional computers range from very simple photo manipulators to sophisticated Photoshop-like applications. The technical field of image processing itself is deeply rooted in math and signal processing theory. But the basics of image representation and manipulation are quite simple to understand. All image processing applications boil down to manipulating individual pixel colors. Some operations require computing properties or statistics on an image. For example, to increase the contrast in an image, one has to ensure that the intensities of pixels span a wider range (and hence can be thought of as an operation on the histogram of an image).
1.3 Sample Image Application
If you have not worked with image processing applications before, we recommend
Gimp(http://www.gimp.org). Gimp is a free, open-source powerful image processing application. If you have access to Photoshop then that will be enough as well. We encourage you to find and tryout the  image processing operations that we will introduce in these assignments using these applications, to understand them better.
2 A sampling of image processing operations 
2.1 Example Image Processing: Visualizing components
One of the easiest ways to analyze a color image is to separately inspect the components (channels) of  its pixels. This provides insight into the overall color balance of the image, or which components seem to store more information than others. This information can be used to compress the image better (by discarding data from or using fewer bits for its less-important channels) or make image processing faster (by only working on some channels). 
The easiest way to visualize the channel of an image is to create an image, where the red, green and blue values for a pixel are all set to the value of a particular channel. This creates a greyscale visualization of a channel. For example, if a pixel in the original image has the color (120,234,23), then the corresponding pixel to visualize the red component would have the color (120,120,120),for green   (234,234,234) and so on. In this way, one can create images that show individual components of an     image (red, green, blue). 
2.2 Example Image Processing: Combining channels into one image
One can split an image into 3 channels as stated above. Then one can individually process these images, and later combine them back into a single color image. Each pixel of this final image gets its red, green and blue values from the three images respectively. 
2.3 Example Image Processing: Image FlippingAnother relatively straightforward operation is to flip the image horizontally or vertically. This does not   change the size of the image. Flipping can be combined too (horizontal followed by vertical flip, or vice versa). In the starter code, you will find some examples of flipped images.
2.4 Example Image Processing: Brightening or darkening an image
Brightening or darkening an image corresponds to doing arithmetic on its colors. This operation is
implemented by simply adding a positive constant to the red, green and blue components of all pixels
and clamping to the maximum possible value (i.e 255 for an 8-bit representation) if needed. Similarly
darkening corresponds to subtracting a positive constant from the values of all pixels and clamping to the minimum value (e.g. 0) if needed.
3 Filtering 
A basic operation in many image processing algorithms is filtering. A filter has a "kernel" which is a 2D    array of numbers, having odd dimensions (3x3, 5x5, etc.). Given a pixel in the image and a channel, the result of the filter can be computed for that pixel and channel.
As an example, consider a 3x3 kernel being applied for the pixel at (5,4) of the red channel of an
image(row 5, column 4). We place the center of the kernel at the particular pixel (e.g. kernel[1][1]
overlaps pixel (5,4) in channel 0. This aligns each number in the kernel with a corresponding number in   that channel (kernel[0][0] aligns with pixel (4,3), kernel[1][2] aligns with pixel(5,5),etc.). The result of the  filter is calculated by multiplying together corresponding numbers in the kernel and the pixels and adding them. If some portions of the kernel do not overlap any pixels, those pixels are not included in the computation. The picture below illustrates the filter operation (not using the same numbers as above).

As with earlier image operations, one may have to clamp the resulting values in the range filtering.
3.1 Applications of ﬁltering
Many image processing operations can be framed in terms of filtering (i.e. they are a matter of designing an appropriate kernel and filtering the image with it).
3.1.1 Image blur
Blurring an image is probably the simplest example of filtering. We can use a 3x3 filter as follows:
This is called a ''Gaussian" blur (imagine the values to be heights of a 3D bar graph to see how they form. a coarse bell-like Gaussian surface). Blurring can be done by applying this filter to every channel of
every pixel to produce the output image. Here is an example of blurring an image this way (in order: original image, blurred, blurred again):
Image not loaded
Image not loaded )
The filter can be repeatedly applied to an image to blur it further. The last image above shows the result of applying the same filter to the second image.
3.1.2 Image sharpening
Image sharpening is in some ways, the opposite of blurring. Sharpening accentuates edges (the boundaries between regions of high contrast), thereby giving the image a ''sharper" look.
Sharpening is done by using the following filter:
As with blurring, it is possible to repeatedly sharpen the image. Here is an example of sharpening an image (in order: original image, sharpened, sharpened more):
Image not loaded
4 Linear Color transformations 
Filtering modifies the value of a pixel depending on the values of its neighbors. Filtering is applied
separately to each channel. In contrast, a color transformation modifies the color of a pixel based on its own color. Consider a pixel with color (r,g,b). A color transformation results in the new color of this pixel to be (r',g',b') such that each of them are dependent only on the values (r,g,b).
A simple example of a color transformation is a linear color transformation. A linear color transformation is simply a color transformation in which the final red, green and blue values of a pixel are linear
combinations of its initial red, green and blue values. For example, if the initial color is (r,g,b),then the final red value being 0.3r+0.4g+0.6b is a linear color transformation.
Linear color transformations can be succinctly represented in matrix form. as follows:

where  are numbers. Using the above notation, the final values will be:
r' = a1ur+a12g+asbg= azr+az2g+azsbb=ag1r+a32g+assb
Similar to filtering, clamping may be necessary after a color transformation to save and display an image properly.
4.1 Applications of color transformations
There are many image processing operations that can be expressed as color transformations on
individual pixels (i.e. implementing them is a matter of using the correct numbers j on all pixels of the image).
3.1.1 GreyscaleA simple operation is to convert a color image into a greyscale image. A greyscale image is composed  only of shades of grey (if the red, green and blue values are the same, it is a shade of grey). There are many ways to convert a color image into greyscale. A simple way is to use the following colortransformation: r'=d=b=0.2126r+0.7152g+0.0722b . Luma is useful for ( high-definition television (https://en.wikipedia.org/wiki/Rec._709)). This can be expressed as a color transformation in matrix form. as:

The effect of this conversion is illustrated below:
Image not loaded
3.1.2 Sepia tone
Photographs taken in the 19th and early 20th century had a characteristic reddish brown tone. This is    referred to as a sepia tone (see the origin of this term (https://en.wikipedia.org/wiki/Sepia_(color))). Most image processing programs allow an operation to convert a normal color ima代 写Assignment 4 (Part I) IME: Image Manipulation and EnhancementJava
代做程序编程语言ge into a sepia-toned image. This conversion can be done using the following color transformation:

The effect of this conversion is illustrated below (original image, sepia-toned result):

4 What to do 
For this assignment, you must create a complete, fully-functional, tested program that has the following
features:
· Load an image from an ASCII PPM, JPG or PNG file (see below).
· Create images that visualize individual R,G,B components of an image.
· Create images that visualize the value, intensity or luma of an image as defined above.
· Flip an image horizontally or vertically.
· Brighten or darken an image.
· Split a single image into 3 images representing each of the three channels.
· Combine three greyscale image into a single color image whose R,G,B values come from the three images.
· Blur or sharpen an image as defined above.
· Convert an image into sepia as defined above.
· Save an image to an ASCII PPM, JPG or PNG file (see below).
· Allow a user to interact with your program to use these operations, using text-based scripting (see below).
4.1 The PPM image format
The PPM format is a simple, text-based file format to store images. It basically contains a dump of the red, green and blue values of each pixel, row-wise.
Some code has been provided to you to read a PPM file. You may use and manipulate this code in any way you see fit.
4.2 Support for conventional ﬁle formats
Although PPM is a simple and standard image format, it is not very popular. In order to make your
application more usable, you will add support to import and export images in popular file formats, such as JPEG and PNG. Look at the ImageIO class 
(https://docs.oracle.com/javase/7/docs/api/javax/imageio/ImageIO.html)in Java to see how one can read and write images of many popular file formats. Find a way to incorporate this into your application.Note: When learning about the  ImageIO  class, you may come across the  BufferedImage  class. Although it is tempting to use this as your representation, observe that this class is from the  awt  package. This is  a package for graphical user interfaces (i.e. view). It is not appropriate to have your model dependent on packages that are view-specific.
4.3 Text-based scripting
Your program should support loading, manipulating and saving images using simple text-based
commands. Here is a list of example test commands you should support. You should support this syntax exactly.
1. load image-path image-name: Load an image from the specified path and refer it to henceforth in the program by the given image name.
2. save image-path image-name: Save the image with the given name to the specified path which should include the name of the file.
3. red-component image-name dest-image-name: Create an image with the red-component of the
image with the given name, and refer to it henceforth in the program by the given destination name.   Similar commands for green, blue, value, luma, intensity components should be supported. Note that the images for value, luma and intensity will be greyscale images.
4. horizontal-flip image-name dest-image-name: Flip an image horizontally to create a new image, referred to henceforth by the given destination name.
5. vertical-flip image-name dest-image-name: Flip an image vertically to create a new image, referred to henceforth by the given destination name.
6. brighten increment image-name dest-image-name: brighten the image by the given increment to   create a new image, referred to henceforth by the given destination name. The increment may be positive (brightening) or negative (darkening).
7. rgb-split image-name dest-image-name-reddest-image-name-green dest-image-name-blue: split the given image into three images containing its red, green and blue components respectively. These
would be the same images that would be individually produced with the red-component, green- component and blue-component commands.
8. rgb-combine image-name red-image green-image blue-image: Combine the three images that are
individually red, green and blue into a single image that gets its red, green and blue components from the three images respectively.
9. blur image-name dest-image-name: blur the given image and store the result in another image with the given name.
10. sharpen image-name dest-image-name: sharpen the given image and store the result in another image with the given name.
11. sepia image-name dest-image-name: produce a sepia-toned version of the given image and store the result in another image with the given name.
12. run script-file: Load and run the script. commands in the specified file.
The following shows a sample sequence of script. commands using the above syntax. Support several ways of entering these commands into your program. Your scripting must also support single-line comments as shown below.
#load koala.ppm and call it 'koala ' 
load images/koala.ppm koala 
#brighten koala by adding 10 
brighten 10 koala koala-brighter 
#flip koala vertically 
vertical-flip koala koala-vertical 
#flip the vertically flipped koala horizontally 
horizontal-flip koala-vertical koala-vertical-horizontal 
#create a greyscale using only the value component, as an image koala-greyscale 
value-component koala koala-greyscale 
#save koala-brighter 
save images/koala-brighter.ppm koala-brighter 
#save koala-greyscale 
save images/koala-gs.ppm koala-greyscale 
#overwrite koala with another file 
load images/upper.ppm koala 
#give the koala a red tint 
rgb-split koala koala-red koala-green koala-blue 
#brighten just the red image 
brighten 50 koala-red koala-red 
#combine them back, but by using the brightened red we get a red tint 
rgb-combine koala-red-tint koala-red koala-green
save images/koala-red-tint.ppm koala-red-tint
**Your program is expected to handle errors suitably (i.e. you cannot assume that each text command will be input accurately). **
3.3 Important points to think about
· What does the model represent? For each class you design, you should be able to clearly articulate what entity a single object of that class represents.
· What are possible ways to represent image data? Which ones seem more helpful for this application?
· Although you are implementing some specific operations in this assignment, you will likely implement others in subsequent assignments.
· Remember to think from not only the implementor's perspective (the person that is implementing a
class) but also the client perspective (the people or classes that are using the class). What
operations might they reasonably want to perform? What observations might they reasonably want to make?
· Remember not to fall into the trap of assuming that there is only one way to use your model. You do not know if there will be only one view, and what kind of user interaction will need to be supported.
· There may come a time in the semester when you will be asked to share your code with other
students, or see others'code. So your design, code and documentation is not "for your eyes only".
· While you are allowed to use any classes within the JDK, make sure that they are appropriate for the part of the program you are designing. For example, it is not OK to use a class from the JDK that is    related to GUIs and views, in your model representation because it adds an unnecessary
dependency between your model, and a view-specific package.
4 High-level Design Principles 
This assignment's open-endedness is strategic, to simulate a real-world situation where you may have a high-level idea of what is to be accomplished, but details are known progressively. This is what makes    design challenging. As you make your design, keep the following in mind:
· Try to extrapolate from this assignment. What kinds of things may becoming? How can you make your design flexible to withstand changes?
· Your design may need to be enhanced as you add features. Adding a new feature without changing   any of your design is ideal: prepare to achieve this goal as much as possible. But in cases where you cannot, strive towards isolating and minimizing changes. Editing an existing design is the worst option,because it can break other things.
· This series of assignments expects you to combine the design techniques you have learned in this    and previous courses. It would help to review what you have learned, and identify when they may be useful.
5 What to submit 
A complete submission must contain: · All your code in the src/ folder
· All your tests in the test/ folder
· At least one example image (different from any of the ones provided to you in this assignment), and versions of it for all of the processing that you have implemented, named suitably in a res/ folder.
Please use images of a reasonable size, because there is a limit to the submission size. You may
use any program to resize an image. Please use images of smaller sizes. There is a file size limit of 7MB on the server. Inability to submit on time due to file size issues is not a legitimate 
reason for an extension or accepting submissions by email. 
· An image of a class diagram showing your design (excluding tests). The class diagram should show names of classes and interfaces, signature of methods and relationships (inheritance and
composition). Do not show field names they may clutter the diagram. We expect these to be not hand-scribbled, you stand to lose points for submitting a picture of a hand-drawn diagram). 
· A text README file explaining your design. Your README file should give the graders an overview of what the purposes are for every class, interface, etc. that you include in your submission, so that they can quickly get a high-level overview of your code. It does not replace the need for proper
Javadoc!
· A script. of commands that your program will accept, that will load at least one file, run some operations on it and save results in other files. Include in your README file instructions on how we can run this script using your program (e.g. "provide this file as a command line 
argument", "type this script. when the program runs", etc.). 
· A citation for the source of your image (in the README file). It is your responsibility to ensure that you are legally allowed to use any images, and your citation should be detailed enough for us to
access the image and read its terms of usage. If an image is your own photograph or other original work, specify that you own it and are authorizing its use in the project. Please do not use images provided or shown in this assignment as your examples, as you are not their owner. 



         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
