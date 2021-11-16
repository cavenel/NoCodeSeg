<p float="left">
  <img src="figures/deepmib-demo.gif" />
  <img src="figures/inference-demo.gif" /> 
  <img src="figures/qupath-demo.gif" />
</p>

# NoCodeSeg: Deep segmentation made easy!

This is the official repository for the manuscript *"Code-free development and deployment of deep segmentation models for digital pathology"*, submitted to Frontiers in Medicine. 

The repository contains trained deep models for epithelium segmentation of HE and CD3 immunostained WSIs, as well as source code relevant for importing/exporting annotations/predictions in [QuPath](https://qupath.github.io/), from [DeepMIB](http://mib.helsinki.fi/downloads.html), and [FastPathology](https://github.com/AICAN-Research/FAST-Pathology). See [here](https://github.com/andreped/NoCodeSeg#data) for how to download the 251 annotated WSIs.

## Getting started

[![Watch the video](figures/youtube-thumbnail.jpg)](https://youtu.be/9dTfUwnL6zY)

A video tutorial of the proposed pipeline was published on [YouTube](https://www.youtube.com/watch?v=9dTfUwnL6zY&ab_channel=HenrikSahlinPettersen).
It demonstrates the steps for: 
* Installing the softwares
* QuPath
  * Loading a project and exporting annotations as tiles/patches
  * Importing predictions for MIB and FastPathology as annotations
* MIB
  * Loading tiles into MIB
  * Preprocessing tiles for training
  * Configuring and training deep segmentation models
  * Exporting the trained model into the ONNX format
* FastPathology
  * Creating configuration file for ONNX model
  * Importing ONNX model
  * Create project, and load WSIs into project
  * Select which WSI to render
  * Deploying model and render predictions on top of the WSI in real time

## Data
The 251 annotated WSIs are being processed before publishing on [DataverseNO](https://dataverse.no/), where it can be downloaded and used for free by anyone.

### Reading annotations
The annotations are stored as tiled, pyramidal TIFFs, which makes it easy to generate patches from the data without the need for any preprocessing. Reading these files and working with them to generate training data, is already described in the [tutorial video](https://github.com/andreped/NoCodeSeg#getting-started) above.

TL;DR: Load TIFF as annotations in QuPath using provided [groovy script](https://github.com/andreped/NoCodeSeg/blob/main/source/importPyramidalTIFF.groovy) and [exporting](https://github.com/andreped/NoCodeSeg/blob/main/source/exportTiles.groovy) these as labelled tiles.

### Reading annotation in Python
However, if you wish to use Python, the annotations can be read exactly the same way as regular WSIs (for instance using [OpenSlide](https://pypi.org/project/openslide-python/)):
```
import openslide

reader = ops.OpenSlide("path-to-annotation-image.tiff")
patch = reader.read_region(location=(x, y), level, size=(w, h))
reader.close()
```

Pixels here will be one-to-one with the original WSI. To generate patches for training, it is also possible to use [pyFAST](https://pypi.org/project/pyFAST/), which does the patching for you. For an example see [here](https://fast.eriksmistad.no/python-tutorial-wsi.html#autotoc_md133).

## Citation
Please, consider citing our paper, if you find the work useful:
<pre>
  @MISC{pettersen2021NoCodeSeg,
  title={Code-free development and deployment of deep segmentation models for digital pathology},
  author={Henrik S. Pettersen, Ilya Belevich, Elin S. Røyset, Erik Smistad, Eija Jokitalo, Ingerid Reinertsen, Ingunn Bakke, and André Pedersen},
  year={2021},
  eprint={some.numbers},
  archivePrefix={arXiv},
  primaryClass={eess.IV}}
</pre>
