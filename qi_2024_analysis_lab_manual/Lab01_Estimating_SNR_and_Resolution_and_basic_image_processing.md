# Estimating SNR & Resolution

---

*Lab authors: Hunter Elliott & Marcelo Cicconet*

<small>This file last updated 2024-04-01.</small>

---

## Learning Objectives

- [ ] Get familiar with Fiji

- [ ] Estimate SNR

- [ ] Estimate resolution

- [ ] Reduce noise by filtering

- [ ] Subtract background

- [ ] Detect point sources via LoG

Lab Data: [<u>https://bit.ly/qi2023labs</u>](https://bit.ly/qi2023labs)

---

## **SNR**

- Open Fiji

- Open the image stack “One point source 67nm pix variable SNR.ome.tif”
  by dragging the image onto Fiji

- You may need to adjust the lookup table. Press Ctrl+Shift+C (or Image
  \> Adjust \> Brightness/Contrast ...) to bring up the brightness and
  contrast window. Adjust the levels until you can see the noise, so you
  can see the full range of the signal. Note that this is not altering
  the data, just how it is visualized!

- Check the image properties in Image-\>Properties. Is the pixel size
  right (67nm)? If not, correct it. It’s always important to be sure
  that the image metadata has been correctly stored and read, as this
  will be used to convert measurements from pixels into microns.

- Scroll through the stack back and forth a few times. Start from the
  lowest SNR image and then scroll backwards - when can you first make
  out the spot by eye? Write this number down so you can see if you can
  beat it computationally!

- Try detecting local maxima in the stack.

  - Go to a medium-SNR image (approximately mid-way through the stack)

  - Go to Process-\>Find Maxima. Check the “Preview point selection”
    box, make sure the “Light background” box is un-checked and the
    “Exclude edge maxima” box is checked. The "Prominence" parameter is
    how bright a pixel must be relative to its neighbors to be counted.
    Increase this value until you have only one detection on the
    selected frame.

  - Now, open the provided macro to try this detection on all images.
    Drag and drop the file “FindStackMaxima.ijm” onto Fiji (main window,
    not the Bioformats shortcut window) to open the macro
    (alternatively, use File \> Open ...).

  - Click “Run” and when the box pops up, type in the noise tolerance
    ("Prominence") you selected in step above, then click OK.

  - Scroll through the resulting stack - it will have a single white
    pixel at each location where a point was detected. What is the last
    image (lowest SNR) that was correctly detected (i.e. only one point
    was found).

  - See how far into the stack you can get accurate detection by trying
    different noise tolerances. Hint: You can also change the output to
    “Count” to make it easier to see on what frame things go wrong. Make
    a note of your personal best (highest frame number with only 1
    detection).

- Try measuring the SNR

  - Click the rectangle selection on the Fiji toolbar.

  - Select a region in the background and then press “m” to measure it.

  - Look at the results window. An important measurement may be missing.
    If so, go to Results-\>Set Measurements... and add the missing
    measurement. You can now close the results window and measure your
    background area again.

  - Now measure the peak intensity by holding your mouse cursor over the
    brightest area in the PSF. The intensity value will be displayed in
    the ImageJ toolbar.

  - What is the approximate SNR by this method? Is the peak intensity we
    measured the best way to approximate SNR?

  - Repeat this measurement on a few frames. How does the SNR you
    measure compare to the detection error (number of incorrectly
    detected points) that you saw above?

  - When you’re done with this, ask the TAs -- they have a cheat-sheet
    for the correct SNRs.

---

## **Resolution**

- Quick and dirty resolution estimation by FWHM

  - Go to a high-SNR frame and click on the line selection on the Fiji
    toolbar

  - Draw a line through the middle of the PSF.

  - Now perform a linescan by pressing Ctrl+K or going to Analyze-\>Plot
    Profile

  - Can you estimate the size of the PSF? You can hold the mouse over
    the plot to get specific intensity and X values. Try measuring the
    width of the peak at one half of the maximum value. This is called
    the Full-Width at Half-Maximum (FWHM).

  - Now try to repeat this on a lower SNR frame. Quite a bit harder to
    estimate here, right?

- Measuring amplitude and resolution by gaussian fitting

  - Select one of your linescan plot windows. Copy the data by going to
    Data \> “Copy 1st Dataset”

  - Go to Analyze-\>Tools-\>Curve Fitting

  - In the drop-down box, select “Gaussian”

  - Clear the data from the box, and paste in your data by pressing
    Ctrl+V or right-click and select Paste

  - Click Fit. Look at the resulting gaussian fit and fitted gaussian
    parameters:

    - *a* is your background - this should closely match the mean value
      of the rectangular measurement you made before

    - *b* *minus a* is the amplitude of your gaussian. Is this a better
      or worse estimation of the signal strength than the peak value you
      measured before?

    - *d* is the standard deviation (STD, sigma) of the gaussian. How is
      this related to resolution? You can convert this STD to FWHM by
      multiplying by 2.355. How does this compare to your previous
      measurement?

    - *c* is the estimated point source position along the line you
      drew. You can use the same line and repeat this process on a lower
      SNR image - how does the estimated position change? Even with
      gaussian fitting we will have significant error at low SNR!!

---

## **Filtering**

- Filtering to handle noise

  - First, duplicate the stack so we can later compare the filtered and
    un-filtered images. Press Ctrl+SHIFT+D or go to Image-\>Duplicate,
    be sure the “duplicate stack” box is checked.

  - Apply a PSF-matched Gaussian filter to the raw data. Go to
    Process-\>Filters-\>Gaussian Blur. What sigma should we use? (Hint:
    Remember the gaussian fitting we did before!)

  - Scroll through the resulting stack - how far into the stack can you
    visually identify the point now? How does this compare to the
    unfiltered stack?

  - Note that both the peak brightness in the point AND the background
    standard deviation have decreased. Which decreased more? What is the
    resulting SNR in the filtered stack? How does this compare to the
    original un-filtered stack?

  - Now try finding local maxima again using the FindStackMaxima macro.
    How far into the stack can you go now before your detection fails?
    (It’s OK to exclude edge maxima due to edge-effects in the
    filtering)

  - Try out a few different filter sizes, both bigger and smaller than
    PSF-matched. Try detecting points in the resulting filtered stack.
    What are the pros & cons of different filter sizes? Why is
    PSF-matched a good choice?

- Spatial Sampling and variable point intensities

  - You are demo-ing two cameras. One is a highly sensitive EMCCD with
    16 micron pixel sizes and one is the brand new for the year 2135
    “TalleyCam-3000” with 1 micron pixels, but lower sensitivity.

  - Compare the two stacks “variable brightness point sources 160nm pix
    variable SNR” and “variable brightness point sources 10nm pix
    variable SNR”. Visually, which camera makes it easier to see the
    points at a given SNR?

    - *NOTE: The SNR is equivalent in the two stacks at each frame.*

  - Scroll through the stacks and try some of the point detection and
    localization exercises above. Note how the variable spot intensities
    make it more challenging to accurately detect the spots.

  - In what situations would you prefer to have the EMCCD? That is, what
    experiments and analysis would be best suited to this camera?

  - In what situations would you prefer to have the TalleyCam-3000? That
    is, what experiments and analysis would be best suited to this
    camera?

  - NOTE: These are simulated images and an artificial scenario. In
    reality, which camera would make it easier to achieve high SNR?

- Laplacian of Gaussian Filtering (LoG)

  - Load the “variable background 500 point sources--” image by simply
    dragging it onto the Fiji toolbar

  - Perform laplacian of gaussian filtering on the image using FeatureJ:
    Plugins-\>FeatureJ-\>FeatureJ Laplacian

    - *NOTE*: This uses the FeatureJ plugin. If this plugin is not
      installed, go to Help \> Update \> Manage update sites, check the
      'ImageScience' box, close that window, click 'Apply Changes', and
      restart Fiji.

  - The “smoothing scale” is the sigma of the laplacian of gaussian
    filter. What should this be set to? Select it and then click OK

  - Notice that the resulting image is 32 bit. Mouse over some of the
    pixels to see their values. Why does FeatureJ convert the image to
    32 bit?

  - The resulting image has bright spots as negative values. Why is
    this? Invert the image by going to Edit-\>Invert

  - Now find maxima in this image. How accurate is the count? Is there
    now a larger range of noise tolerances which give an accurate
    result?

  - If you feel like it, try LoG filtering on the “noisier” image as
    well.

---

## **Bonus**

- If you have the data, try the techniques you learned today on some of
  your own bead data to estimate the SNR and resolution. What was your
  best resolution? Your worst SNR?

- Come up with your own way to visualize detection accuracy vs. SNR in
  one of the example variable SNR stacks from above. Save your plot for
  discussion tomorrow morning.

  - Compare this plot with different filtering approaches.
