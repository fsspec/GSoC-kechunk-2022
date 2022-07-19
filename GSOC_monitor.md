# GSOC 2022 Schedule, Planning and Progress Monitor

This document will be used to record the planning and progress of the [Kerchunk](https://github.com/fsspec/kerchunk) GSOC project discussed in https://github.com/fsspec/kerchunk/issues/166 

Student: [Peter Marsh](https://github.com/peterm790) Mentors: [Rich Signell](https://github.com/rsignell-usgs), [Martin Durant](https://github.com/martindurant)

## Schedule

May 20 - June 12: Community Bonding Period

June 13	- July 25 Phase 1 (July 29 Evaluation Due)

July 25 - September 4 Phase 2 (September 5 - September 12 Submit final Evaluations)


## Progress

### Week 1 (25 May - 1 June)

* Added to ESIP and IOOS Slack to communicate with mentors
* Added to and configured ESIP qhub jupyter lab enviroment
* Read through kerchunk gitter chat history to get up to speed
* Reproduced Kerchunk example notebooks, and updated notebooks which did not work on qhub
* Made my first pull request with the updated examples!

### Week 2 (1 June - 8 June)

* Configured nb-stripout to remove outputs of jupyter notebooks when committing to github. This allows for easy 'diff'ing, where otherwise cell outputs make it impossible to see actual changes in code
* Updated Goes16 example to newer Kerchunk spec, to understand how the Kerchunk spec has changed from using `xarray_open_dataset()` to generate metadata to using native `h5py` and `zarr` methods
* Made my first (very small) comment on an issue in the kerchunk repo helping a user solve a json vs ujson confusion
* Attended the GSOC 2022 virtual summit
* Configured a workflow to create a kerchunk sidecar file for the ERA5 public dataset on aws. 
* While working with the ERA5 dataset encoutered the issue of dealing with fill_value's and set up a method to solve this using the postprocess argument, although I am still working on this to find a potentially neater solution.  

## Week 3 (8 June - 15 June)
* Opened issue https://github.com/fsspec/kerchunk/issues/176 regarding the need to run consolidate again after postprocess when writing output to json 
* Finalised ERA5-pds workflow and opened virtual dataset using kerchunk sidecar file 
* Set up [notebook](https://gist.github.com/peterm790/76e63edbde9a9feccccaee405bcbc4ca) to explore kerchunks handling of fill_value

## Week 4 (15 - 22 June)
* Made pull request https://github.com/fsspec/kerchunk/pull/180 to run postprocess before consolidate, which has now been merged into kerchunk repo
* Made comment https://github.com/fsspec/kerchunk/issues/177#issuecomment-1156835739 in the fill_value vs _FillValue saga, to clarify that xarray only considers fill_value when opening from zarr
* Explored methods to speed up `MultiZarrtoZarr` by running in parrallel and opened https://github.com/fsspec/kerchunk/issues/182 regarding this

## Week 5 (22 - 29 June)
* Made initial investigation into automatically converting NCL XML virtual datasets into kerchunk datasets (https://github.com/fsspec/GSoC-kechunk-2022/issues/7)
* Set up a workflow to produce a kerchunk virtual dataset for the NWM ensemble. (https://github.com/fsspec/GSoC-kechunk-2022/issues/8) (https://discourse.pangeo.io/t/efficient-access-of-ensemble-data-on-aws/2530/7)
* Confirmed https://github.com/fsspec/kerchunk/pull/183 significantly speeds up the combine step in https://github.com/fsspec/GSoC-kechunk-2022/issues/5#issuecomment-1169868172

## Week 6 (29 - 6 July)
* Created a quick tutorial solving a users issue uging the coomap method in https://github.com/fsspec/kerchunk/issues/184#issuecomment-1171015041
* Expanded the ERA5 sidecar files to a large selection of variables
* Discovered it is possible to add variables to an existing kerchunk sidecar file by simply using the python update dictionary method https://nbviewer.org/gist/peterm790/5015b90bb858fcd8ba922c5f764adf4d
* Set up a tutorial of how to open a kerchunk file mapping the ERA5 dataset and how to construct a simple sidecar file for the dataset https://nbviewer.org/gist/peterm790/23bb7a1484e576fa943e0b7e6c69d2e5

## Week 7 (6 - 13 July)
* Set up https://github.com/peterm790/ERA5_Kerchunk_tutorial which contains a simple tutorial to generate a sidecar file for ERA5 as well as an extended tutorial which runs through a number of number of different examples of using `MultiZarrtoZarr.combine`

## Week 8 (13 - 20 July)
* Expanded the prose and descriptions of the tutorials
* Renamed the original Kerchunk tutorial to quick start and added the ERA5 tutorial to the kerchunk docs in https://github.com/fsspec/kerchunk/pull/193
* Had an initial go at adding a convenince function to merge variables to existing datasets in https://github.com/fsspec/kerchunk/pull/196






## Suggested plan moving forward
* Potentially roll the above mentioned *'adding variables technique'* into a kerchunk convenience method, which would allow doing so without having to open the files into memory with `ujson.load()` as this become a problem when the json files become larger. 
* From here it could make sense to continue exploring the concatenating files method from https://github.com/fsspec/kerchunk/issues/134
* Seperate from the above updating the tutorial in the docs to incorporate more varied uses of `multiZarrtoZarr` such as the arguments available in `coomap` could be worth while. Potentially using the dummy datasets used in testing as examples. 
