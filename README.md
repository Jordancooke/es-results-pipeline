# es-results-pipeline

**To set the pipeline:** `Fly -t spt sp -p BMISG-Pipeline -c pipeline.yml` while inside of
the repository.

**To unpause the pipeline:** `Fly -t spt up -p BMISG-Pipeline`

**To delete the pipeline:** `Fly -t spt dp -p BMISG-Pipeline`

_These requirements assume that the `spt` target has been set up, if it hasn't please refer
to the following documentation: https://collaborate2.ons.gov.uk/confluence/display/ESD/Fly_

# The pipeline is split into 3 Major sections:

**Installing-dependencies:**

This section contains the build of the docker image and the upload to ECR.

**Quality-checks:**

The quality checks contain all the jobs for linting and testing the pull requests which 
are raised for each pipeline. These are all triggered when a pull request is raised in a
respective repository.

**Deployment:**

This section contains all of the server deployment jobs of the pipeline. This are 
triggered when a merge is made to the master branch. This way our AWS code is always up to 
date with what is on the master branch.

# 'Gotcha's'

The pipeline makes use of caching when building images in order to speed up the process, a known drawback to using caching is that if there isnt an image with the appropriate tag(enrichment, ingest, etc), then the build will fail. To counteract this there is an on_error step on the quality checks which will build a fresh image from master if building using cache fails.<br>
<br>
There is a scenario which could(albeit rarely) occur. If a pull request takes place with a change of requirements then a new image will be built. It won't be until a new pull request that changes requirements that a new image will be built. So there is a possibility that a bad image from an erroneous pull request could be used in the quality checks of subsequent requests.
