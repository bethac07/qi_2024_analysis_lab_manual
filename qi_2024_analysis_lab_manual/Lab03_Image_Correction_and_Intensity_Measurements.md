# Image Correction and Intensity Measurements

*Lab authors: Hunter Elliott, Marcelo Cicconet, & Beth Cimini* . 

<small>This file last updated 2024-04-06.</small>

---

## Learning Objectives

- Prepare correction images
- Apply corrections to image data
- Measure intensities with and without corrections

Lab Data: [<u>https://tinyurl.com/qi2024labs</u>](https://tinyurl.com/qi2024labs) 


---

## **Preparing your Image Corrections**

```{note} 
You’re encouraged to try these corrections on your own data. However, we also have some canned data available if you prefer (see link above) 
```

### Create Averaged dark images

```{note} 
If you are using the canned data you can skip this part as we have provided averaged images
```

1.  Load your dark correction images

2.  Average them together by going to stack-\>Z-Project and selecting
    “Average intensity” and clicking OK.

3.  Calculate the mean and STD of the entire dark current image. How
    much variability is there? Is it in a specific spatial pattern? You
    can use the line scan (Analyze-\>plot profile) to measure patterns
    along a particular axis. If you use a horizontal rectangular
    selection it will average along the Y axis and better reveal any
    patterns that may be present.

4.  How does the variability in these images compare to your signal?
    Would you expect different cameras to have more variability here?

5.  Save your averaged dark image for later use.

### Create Averaged and Filtered Flat-field correction (FFC) images

```{note} 
If you are using the canned data you can skip this part as we have provided averaged images
```

1.  Load your flat-field correction images

2.  Average these images together over time by going to
    stack-\>Z-Project and selecting “Average intensity” and clicking OK.

```{tip} 
If you have debris etc. in some of your images, using a median intensity projection instead may better remove these artifacts.
```

3.  How clean is the resulting averaged image? Remember, any errors
    (noise, specks of debris etc) in this image will be propagated to
    your data during the correction process! If it is noisy you can
    apply some filtering to reduce the noise.

```{warning} 
If you are using images from a CMOS camera, it will have variable gain from pixel to pixel and so may appear noisy - this is NOT noise and you do NOT want to filter it away!! Do not apply any filtering to CMOS images!!!
```

4.  Now, correct the offset in your FFC image. Subtract the dark image
    from it using the image calculator. Go to process-\>image
    calculator, select the FFC image as Image1, “subtract” as the
    operation and the dark image as image 2.

5.  Now, normalize your averaged FFC image. Convert it to 32 bit,
    calculate the mean value (you can measure an entire image by
    pressing m with nothing selected), and then divide the image by this
    mean value by going to Process-\>Math-\>Divide.

6.  If you measure the mean of the normalized image what is it? Why do
    we want to do this?

7.  Save your averaged normalized flat-field correction image for later
    use.

8.  How much variability is there in your illumination pattern? You can
    measure this by doing a linescan. Draw a line selection across the
    entire image and then press Ctrl+K to plot the intensity along this
    line. How much does this profile vary across the image? What does
    this variability tell you about when/why these correction images are
    necessary?

## **Measure intensity with and without dark and flat-field correction**

1.  Open the bead images you acquired for flat field correction. If you
    are using canned data this will be “MAX_s1to50_10x SF red beads
    1percent 6micron.tif”

2.  Play around with the display (brightness and contrast) - the beads
    *should* be uniform intensity but they’re not - can you adjust the
    display so you can see the variability in intensity?

3.  Measure the mean intensity in the background (away from any beads).
    What is the value? Why is this?

4.  Open the dark image you prepared earlier (if using canned data, open
    DarkImage.tif).

5.  The offset and dark current are additive, so the correction is a
    subtraction. Subtract your averaged dark image via Process-\>Image
    Calculator.

6.  Save this dark current-corrected image just in case.

7.  Measure the mean intensity in the background of this corrected image
    (away from any beads). What is the value now? Why is this?

8.  Use the “find maxima” feature you learned about before to detect all
    the beads, then measure them by pressing m.

9.  Use Excel to calculate the mean and standard deviation of your bead
    intensities. Also calculate the “coefficient of variation” which is
    the standard deviation divided by mean.

10. Now, open the averaged flat-field correction image you prepared
    earlier ('FlatField-10x SF-bin1-Filter TRITC.tif' if you're using
    canned data).

11. Illumination has a multiplicative effect on intensity, so the
    correction is division. Apply the flat-field correction by dividing
    the dark-current-&-offset-corrected image by the flat-field
    correction image, again via Process-\>Image Calculator.

12. Play around with the brightness and contrast now - do the bead
    intensities appear more uniform? (They should).

13. Again detect the beads in the flat-field corrected image using the
    find maxima tool and measure their intensities.

14. Again use Excel to calculate the mean, standard deviation and
    coefficient of variation (CV) of the bead intensities. How has the
    CV changed? Why is that?

## **Measure intensity with and without local background subtraction**

```{note}
In this example you will deal with variable
background, where we must approximate the background in every pixel of
the image. In samples where the background is not variable you can be
more precise by measuring the background somewhere outside of the sample
and then subtracting that single number from the entire image.

For the sake of time we will have you apply <u>only</u>
a background subtraction to the images in this section of the lab, but
if you wanted to be precise you would want to <u>also</u> apply an
offset and flat field correction to these images.
```

1.  Open the bead images you acquired for local background subtraction.
    If you are using the canned data this will be “10x s fluor mosaic
    variable background selected frame.tif”

2.  Draw a line scan across the field of view (ideally avoiding beads)
    and use the plot profile function to visualize and measure the
    variability in background.

3.  Use the find maxima feature to detect the beads and measure their
    intensity.

4.  Again use Excel to calculate the mean, standard deviation and
    coefficient of variation of the bead intensities. Save this
    spreadsheet for later reference.

5.  Now, apply a local background subtraction

6.  Again draw a line scan across the field of view and use plot profile
    to visualize and measure the background variation. How does it
    compare to before?

7.  Detect the beads and measure their intensity.

8.  Again use Excel to calculate the mean, standard deviation and
    coefficient of variation. Compare these values to those you measured
    in the raw image, before background subtraction. How do they
    compare? Would a background subtraction be important for making
    quantitative intensity measurements of biological samples?

## **Calculate a registration transform between two image channels**

1.  Load one of your tetraspeck bead image sets (two channels from one
    stage position). If you are using the canned dataset this will be
    “tspeck_1024_1518_003_z27.ome.tif” (drag it into BioFormats plugin
    shortcut window).

2.  If your images are in a single composite (2-channel) image, split
    the channels by going to Image-\>Color-\>Split Channels

3.  Open the registration plugin by going to
    plugins-\>registration-\>Descriptor based registration

4.  Choose one of your image channels to be a reference and the other to
    register. It doesn’t matter which is which, but it DOES matter that
    you remember which you chose!!

5.  Make sure that the first three boxes are set to interactive, the
    transformation model is set to rigid (2d) and set the “images
    pre-alignment” to “approximately aligned”. For the rest of the
    settings you can use the defaults.

6.  Now click OK. You will be presented with a preview of the points
    that the algorithm is detecting to align your images. Do these
    settings look familiar? Does this sound like anything we’ve learned
    about?

7.  Adjust the sliders until it is only detecting beads (shown by small
    green circles), then click Done.

8.  Look at the resulting composite (called “fused”). Is the alignment
    better? You should see near-perfect overlap of your spots.

```{note} 
This plugin stores the most recently successful registration transform, so you don’t need to save any file to save your transform.
```

```{note} 
"fused" is a composite image that overlays the 'anchor' and 'registered' images. To switch between 'anchor' and 'registered', create a hyperstack: Image \> Hyperstacks \> Stack to hyperstack.
```

## **Apply the registration transform to align two image channels**

1.  Load a different tetraspeck bead image set (not the same image pair
    you used to calculate the transform). We will apply our registration
    to this image for the sake of the lab, but in practice you would be
    applying this registration to images of your sample of interest. If
    you are using the canned data this will be
    “tspeck_1024_1518_002_z21.ome.tif”

2.  If your images are in a single composite (2-channel) image, split
    the channels by going to Image-\>Color-\>Split Channels.

3.  Go to Plugins-\>Registration-\>Descriptor based registration

4.  Check the “re-apply last model” box, make sure you have the images
    selected in the same order you used to create the transform, and
    click OK.

5.  Take a close look at the composite registered image that the plugin
    produces. How well do the spots overlap now? Are they closer than
    before the registration?

## **Calculate bleedthrough coefficient**

```{note}
For this section we will use canned data from a FRET probe, where bleedthrough / crosstalk is especially strong. These images are from a single-labelled control where only one of the FRET fluorophores is present (CFP). 
```

1.  Open the image of the CFP fluorophore
    “CFPonly_CFP_background_offset_FFC.tif” and the same sample using
    the FRET illumination and filter set
    “CFPonly_FRET_background_offset_FFC.tif”

```{note} 
These images have already had offset, background and flat-field corrections applied to them, so you don’t need to do that now. If you were using your own images you would need to apply these corrections first.
```

2.  We can sneakily use the colocalization tool to calculate the
    bleedthrough coefficient. Go to Analyze-\>Colocalization-\>Coloc 2.

3.  Make sure that Channel 1 is CFP-Only-CFP and Channel 2 is
    CFP-Only-FRET. Set the “Threshold regression” to “Bisection” - this
    doesn’t matter for what we’re doing but it will keep the plugin from
    taking forever. Then click OK and wait a few seconds for the results
    to pop up.

4.  We don’t care about co-localization, so we can ignore any warnings
    and most of the values the software displays, but this plugin also
    fits a line to our 2D image intensity scatter plot/histogram. You
    can view this histogram and the fit by selecting “2D Intensity
    histogram” from the drop-down menu at the top.

5.  How does the fit look? You should have a VERY high Pearson’s R value
    (very close to 1). If everything looks good, write down the *slope*
    of the fit - this is your bleedthrough coefficient.

6.  Now, test out your bleedthrough coefficient by performing a
    bleedthrough correction on the CFP-Only-FRET image - after the
    correction there should be little or no intensity.

    a.  Convert your CFP-Only-CFP image to 32 bit. Then, multiply it by
        the bleedthrough coefficient using Process-\>Math-\>Multiply and
        typing in the bleedthrough coefficient.

    b.  Now, subtract this scaled CFP-Only-CFP image from the
        CFP-Only-FRET image using Process-\>Image Calculator.

    c.  This should almost completely remove the fluorescence in the
        CFP-Only-FRET image, because in this experiment this image is
        completely due to bleedthrough / crosstalk. There may be some
        residual intensity - can you think of why this might be?

## **Bonus Exercises - ImageJ Macros**

- Pick the most painful or annoying analysis task from the labs so far,
  or a new one you want to try. Go to Plugins \> Macros \> Record

- Start performing your task. Notice what happens in the macro recorder.

- When you’re done, click “create” in the macro recorder.

- Close your processed images, re-open your original image(s), and try
  running the macro.

- Look at the text of the macro - try to understand some of the
  commands. How could you adjust your analysis by editing this?

- Save your macro for future use. You can re-open it by simply dragging
  it onto the Fiji toolbar.

## **Bonus Exercises - CellProfiler Background Subtraction and Flatfield Correction (computationally rescuing old data)**

```{note}
This exercise contains several "do this and then wait for 5-10 minutes" steps, so you may feel free to do other bonus exercises (like working on macro writing, or a bonus you didn't yet attempt from previous labs) while you wait.
```

As you may have intuited after using it, CellProfiler was originally designed for high-content, high-throughput screens (though it certainly can work on single images). Proper image correction is harder and in practice sometimes somewhat different in this context, but still important! {cite}`Singh2014-yh`

We've provided you some images {cite}`Gustafsdottir2013-ng` from the Broad Bioimage Benchmark Collection {cite}`Ljosa2012-bf` that have been treated with a set of organelle dyes collectively called Cell Painting. This assay will be discussed later in the course during Beth's seminar. 

Unfortunately, since these images are 10-15 years old, if measured offsets and images to help us calibrate the microscope were made at the time, they're long since lost. But we can still do _some_ background corrections to help us be more confident interpreting _some_ measurements. 

````{admonition} Question for you
What do you think is an example of a measurement that we can be pretty confident in our ability to measure, even without these corrections? What do you think is an example of a measurement we should be more cautious in interpreting?

```{dropdown} Some possible answers
There are a lot of potentially correct answers here, but cell area should probably be reasonably safe (though there might be tiny differences in exactly where your threshold boundary is drawn). It probably _isn't_ a good idea to deeply interpret small changes in correlation values between channels (it rarely is anyway, but especially here!)
```
````

CellProfiler has no ability to "loop" over the same set of images multiple times (to first create a flatfield correction, and then a local background image, and then analysis), so we will need to run 3 separate pipelines, one for each step.

### Pipeline 1 - Create a flatfield correction image for each channel (wait time of about 5-10 minutes during run)

Since we don't have dark images, we have to calculate our flatfield correction from the actual data itself. We do this by averaging all the images together - under certain conditions (see below), this will allow us to approximate the same kind of flatfield correction we'd more typically take at the microscope before collecting data.

```{important}
This kind of averaging-based FFC correction is only appropriate when **ALL** of the following conditions are met; if not, do not attempt!
 - You have a large number of images in each channel (hundreds or thousands)
 - Your intensity is reasonably similar across most fields of view (this means this workflow is prone to problems when, ie, only one in ten cells is bright and the rest are dim)
 - Objects are randomly placed throughout your image (this means this is not appropriate when doing microscopy workflows that start with a low mag scan for objects of interest and then goes back and images each at high mag)
 - Objects are common enough, and your number of images is large enough, such that across all fields of view, each pixel is inside a bright object at least several times
```

- Drag and drop the `BBBC022_20585_AE` folder of images into the Images module.
- Drag and drop pipeline 1 (`01_FFC.cppipe`) into the pipeline panel
- Set where you want the output FFC images to be saved by clicking the `OutputSettings` button  <img src="images/lab03/OutputSettings.png" height="30px" />
- Hit `Analyze Images` <img src="images/lab03/AnalyzeImages.png" height="30px" /> to have CellProfiler create FFC images for each of the 5 channels on 240 images of each.
- Optional - explore the pipeline while it runs.
  - It's fine to do it in the same copy that's running, but you can open a second copy if you're worried. 
  - You can see here that we're subtracting an "offset", but since in this case we don't know what it is, we just set it to zero. But this is where you could put a measured offset if adapting this pipeline for your own use in the future. Remember to scale it 0-1 based on your bit depth!

```{note}
CellProfiler saves FFC images as numpy array (`.npy`) files, which will not play nicely with ImageJ. But you can inspect what they look like in the next step.
```

### Pipeline 2 - Create a background subtraction image for each channel (wait time after of about 5-10 minutes during run)

Next, we will create a background subtraction image, again based on averaging all images in the set. This will help us approximate (though not truly _measure_) the (offset + background) level in this image. As above, this approach is only appropriate because we have many images, in which our objects are randomly distributed.

```{admonition} Question for you
What kinds of things will this kind of background subtraction help correct for? What kinds of things will it not help correct for?
```

- Drag in the `02_BackSub.cppipe` pipeline file to the pipeline panel. It's fine to do this with a pipeline already in there.
- Return to the Images module, and drag in your newly calculated `.npy` flatfield images. If you don't have the raw images loaded in still, drag them in as well.
  - Optional - double click on one or more of the `.npy` files to open them up in CellProfiler's image viewer. What do you notice? (You can also do this after you hit analyze)
- Hit `Analyze Images` <img src="images/lab03/AnalyzeImages.png" height="30px" /> to have CellProfiler create FFC images for each of the 5 channels on 240 images of each.

### Pipeline 3 - Apply your corrections and then perform some segmentations and measurements

```{tip}
Want to learn more about segmenting these images? Visit tutorials.cellprofiler.org and check out the Beginner Segmentation and Advanced Segmentation modules, both of which use this image set!
```