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