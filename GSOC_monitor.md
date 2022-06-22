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
