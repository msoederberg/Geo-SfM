---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.12
    jupytext_version: 1.7.1
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# GCP markers - generation and detection

While other GCP marker types exist, we here focus on the use of either ArUco or Agisoft Metashape markers.
Both marker types are of the binary fiducial sort, and, in principal, machine readable.  

## GCP generation and detection

Slight differences exist in the way both are generated and detected, with Metashape markers strongly relying on the Metashape GUI and ArUco markers being mostly programming-based.
Both, however, require well-established real world coordinates to be worth the effort.

```{admonition} Leave generated markers as-is
:class: warning

Most GCPs feature black-white markers.
It is important to not change the white background colour, nor to remove white or black borders.
These are needed for the automated detection of the markers, and are inherent to their correct functioning.
```

### Real world coordinates

As mentioned previously, real world coordinates should be measured consistently across all markers, i.e., always opt for the same measuring location and method of measurement.

Correct and systematic archiving of the locations is furthermore key to automisation later on.
As part of the standardised project environment, it is therefore encouraged to format all GCP IDs and their corresponding real world coordinates in the following manner:

```
1,15.83193045,78.33763246,224.6672
2,15.83061642,78.33836842,221.3598
3,15.82920799,78.3390649,202.7514
4,15.82944846,78.33972477,184.2172
5,15.83092679,78.33907352,183.6924
6,15.83094077,78.33962431,164.2002
7,15.83085931,78.34003023,152.5401
8,15.82992376,78.34011196,169.7791
```

Herein the first column represents the GCP ID, followed by longitude, latitude, and altitude with the EPSG:4326 CRS.
Once completed, make sure to save the file as a comma-separated-value (csv) file with name *gcp_table.csv* in the expected directory:

```
{project_directory}\data_directory\gcps\prepared\gcp_table.csv
```

```{admonition} Correct GCP IDs and order
:class: warning

Always make sure the GCP numbers in the *gcp_table.csv* file correspond with the actual GCP IDs.
They should therefore always be unique (i.e., used only once).

Furthermore, be aware that the file uses longitude-latitude formatting as opposed to the common latitude-longitude order!
```

### Agisoft Metashape

```{figure} assets/meta_markers.png
:name: meta_markers

Two Metashape markers generated with the parameters listed in {numref}`metashape_markers`.
```

To generate Metashape GCP markers, proceed to *Tools/Markers/Print markers...* to open the  {ref}`Print Markers dialog <metashape_markers>`.
Multiple parameters can be set, including the *bit-encoding* of the marker (*Marker type*); the *Center point radius* of the centre dot; how many markers to include per  (*Targets per page*); and the *Font size*.

```{figure} assets/metashape_markers.png
:name: metashape_markers

Overview of the Metashape marker generation window.
```

```{admonition} Centre dot size
:class: warning

When using Metashape markers, it is extremely important to chose an appropriate *Center point radius*, as it effectively determines the detectability of the marker.
When the centre dot is no longer visible, Metashape will have great difficulty in automatically detecting the marker.
```

### ArUco

While the Metashape markers are exported all at once, more freedom is given to the generation of {ref}`ArUco markers <aruco_markers>`.
Multiple options exist, ranging from [online marker generators](https://chev.me/arucogen/) to the use of the popular [OpenCV image processing suite](https://docs.opencv.org/master/d5/dae/tutorial_aruco_detection.html), featuring both c++ and Python implementations.

One can even generate the ArUco markers from within the iFrame below:

<div>
<iframe title="Online ArUco generator" width="100%" src="https://chev.me/arucogen/"></iframe>
</div>

Herein the *Dictionary* corresponds to the *bit-encoding* used by the Metashape markers.
It is slightly more intuitive, however, as the {No}x{No} indicates the number of rows and columns of the marker.
The *Marker ID* is a fixed identification number of a given marker within the same dictionary.
Keep in mind, however, that markers from different dictionaries may have the same *Marker ID*, yet will have very different features!
Finally, the *Marker size* determines the size of the marker.
Make sure to pick an acceptable size.

Why not give it a shot yourself, and see how the markers change whilst you change the parameters?

Finally, you can save the marker by clicking *open*, and printing it as a PDF.

```{admonition} Programming approach
:class: seealso

For those interested in using the programming approach, please read the [OpenCV image processing suite article](https://docs.opencv.org/master/d5/dae/tutorial_aruco_detection.html) linked to above, as well as the example tutorial available at [Scientific Python](https://mecaruco2.readthedocs.io/en/latest/notebooks_rst/Aruco/aruco_basics.html).
This allows for far more freedom, including the automated generation of multiple markers in a circle as used for hand-sized sample digitisation.
```

### Manual detection

#### Automated detection

##### Metashape

##### ArUco

```{admonition} Python installation required
:class: warning

The following steps require the [installation of Python and the automated metashape scripts](../about/software#python).
```

Prior to automated detection, we will have to configure a settings or parameter file for the [automated metashape scripts](../l4/python.ipynb).

Head over to the [tutorial](../l4/python.ipynb) and put together a minimal working example (MWE) configuration consisting of:

- [minimal configuration parameters](../l4/python.md#minimal-yaml-configuration-file)
- [Add photos](../l4/python.md#additional-parameters)
- [GCPs - detection](../l4/python.md#additional-parameters)
- [GCPs - add to project additional parameters](../l4/python.md#additional-parameters)

Make sure to place this file in the *config directory* of the [standardised project environment](../l1/tutorial.md#a-standardised-project-environment).

Then proceed with the [Python tutorial](../l4/python.md) to subsequently add all images to the Metashape project; to detect the GCPs in all the images; to add the detected GCPs and corresponding real world coordinates to the Metashape project.