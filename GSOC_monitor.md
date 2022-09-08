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

## Week 9 (20 - 27 July)
* Merge_vars convenience function modified and now merged into main
* Configured a docker image containing a pangeo python enviroment https://ghcr.io/peterm790/pangeo and a minimal kerchunk enviroment https://ghcr.io/peterm790/kerchunk utilising https://github.com/iameskild/repo2registry for use with kbatch and cronjob scripts. Which means kerchunk files can now be updated daily. 
* Set up an example script [create](https://nbviewer.org/gist/peterm790/53e2157770368331de936ca3ba8943d2) and [open](https://nbviewer.org/gist/peterm790/530ff13df08db2f569fc85f76a6e2bb1) LiveOcean forecast data. Discussions on this now tracked at https://github.com/fsspec/GSoC-kechunk-2022/issues/6

## Week 10 (27 - 03 August)
* Updated HRRR to utilise the new scan_grib module. And updated the case study in the Kerchunk docs to match this https://github.com/fsspec/kerchunk/pull/206
* investigated modifying combine to accomodate a list of lists input from scan_grib but decided the user should instead be writing each grib message as an individual json which the above case study now reflects. Still to check if changing this slightly to us `fs.cat()` could provide a speed up. https://github.com/fsspec/kerchunk/compare/main...peterm790:kerchunk:grib2_combine

## Week 11 (03 - 10 August)
* created a script to generate a dashboard from the ERA5 kerchunked data. This however is not running very smoothly and I suspec may be to do with the very small chunk sizing in the origin Netcdf files. 
* Updated the ERA5 tutorial to instead be native restructured text and no longer rely on pandoc. This is now in a new PR https://github.com/fsspec/kerchunk/pull/208 and the older PR has been closed. 
* Experimented with a way to open the range of HRRR grib messages in an xarray [datatree](https://github.com/xarray-contrib/datatree). This works to some extent but definitely still needs some work. https://gist.github.com/peterm790/b844fe0410d399f9ad8658377c744149
* Modified LiveOcean forecast reference update script to utilise etags to monitor file changes. https://github.com/fsspec/GSoC-kechunk-2022/issues/6#issuecomment-1210462818

## Week 12 (10 - 17 August)
* Meeting with Eskild from Quansight to understand how kbatch works and troubleshoot. 
* Spent some time understanding Kubernetes and trying to configure K9s
* New tutorial https://github.com/fsspec/kerchunk/pull/208 now merged.
* Updated HRRR case study in docs to point to new gist: https://nbviewer.org/gist/peterm790/92eb1df3d58ba41d3411f8a840be2452
* Had a meeting with Parker MacCready to trouble shoot implementing LiveOcean kerchunk workflow.

## Week 13 (17 - 24 August)
* Setup gist to investigate to what extent `combine` can be sped up and continued discussion in https://github.com/fsspec/kerchunk/issues/200#issuecomment-1220547197 
* This led to `fs.cat` implementation being merged: https://github.com/fsspec/kerchunk/pull/213
* Had an initial go at configuring `combine` to run in parallel here simply as a convenience function that takes the same arguments as `MultiZarrtoZarr`: https://github.com/fsspec/kerchunk/compare/main...peterm790:kerchunk:dask_convenience_function

## Week 14 (24 - 31 August)
* PR to Pangeo forge to fix Kerchunk reference argument error, merged to main https://github.com/pangeo-forge/pangeo-forge-recipes/pull/399
* Set up tutorial to open HRRR forecast as a datatree using Kerchunk: https://nbviewer.org/gist/peterm790/2439b1fe5fc781a9cc40281c9855affe

## Week 15 (31 - 07 September)
* Completed finally summary blog post: link tbd





