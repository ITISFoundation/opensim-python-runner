# OpenSim Python Runner
OpenSim Python Runner is an open-source library for modeling and simulating the neuromusculoskeletal system developed at Standford university. This Service brings its functionality into [oSparc](https://github.com/ITISFoundation/osparc-simcore/). 

#![Github-CI](https://github.com/ITISFoundation/opensim-python-runner/workflows/Github-CI%20Push/PR%20opensim-python-runner/badge.svg)

## Description

oSparc Service that allows running any python-based script using the [OpenSim library](https://opensim.stanford.edu/) either as simple script of complex Python application inside a zip file (with or without requirements.txt file).

You can find more information on how to use Python scripting services in oSparc [here](https://docs.osparc.io/#/docs/tutorials/python_runner).

More information about OpenSim can be found on the [OpenSim support page](http://opensim.stanford.edu/support/index.html) and [SimTK project website](https://simtk.org/home/opensim).

## Versions
### 1.0.0
First release, running opensim-core 4.0 with Python bindings (Python 3.8).
Visualizing models is not possible yet.

## Useful resources for developers and users of OpenSim
This Service is based on the [opensim-python Docker image](https://hub.docker.com/r/stanfordnmbl/opensim-python) provided by the OpenSim developers. 

Docker files are available on [OpenSim Containers repository on GitHub](https://github.com/opensim-org/opensim-containers).

Related packages can be found on the [OpenSim Project on Github](https://github.com/opensim-org).

The [OpenSim Documentation](https://simtk-confluence.stanford.edu:8443/display/OpenSim/OpenSim+Documentation) has some information on [Scripting in Python](https://simtk-confluence.stanford.edu:8443/display/OpenSim/Scripting+in+Python) with OpenSim.
Example Python scripts can be found [here](https://github.com/opensim-org/opensim-core/tree/master/Bindings/Python/examples) and in the corresponding installation folder inside the Service.


## Development
### Build and test the image
```console
make help                                # shows Makefile help

make devenv                              # creates python virtual environment
source .venv/bin/activate                # activates python virtual environment
make build                               # builds oSparc compatible docker image
make info-build                          # shows oSparc compatible docker image
make tests                               # tests oSparc compatible docker image
```
## More testing options
### Create a shell to test from inside the container
You can also test it from inside the container by runing an example script located in input/input_1.
```console
make shell                                # starts a shell instead of running the container.
sh service.cli/run                        # run from the newly created shell
```
This should print a table of values:
```latex
              | /forceset/bice|               | 
          time| ps|fiber_force|    elbow_angle| 
--------------| --------------| --------------| 
           0.0|       1.180969|      1.5707963| 
           1.0|      57.467961|     0.77115447| 
           2.0|      22.313274|      1.5676392| 
           3.0|      54.512645|      1.4421041| 
           4.0|      33.297678|      1.5084202| 
           5.0|      33.945575|      1.5179674| 
           6.0|      36.891207|      1.5021832| 
           7.0|      35.141379|      1.5071866| 
           8.0|      35.483595|      1.5079728| 
           9.0|      36.801201|      1.5067746| 
          10.0|       34.77286|       1.507112| 
```
And save a file called SimpleArm.osim to the output folder.

### Deploy locally together with oSparc sofware stack.
After you have created the image, tag it:
```console
docker tag local/opensim-python-runner:production registry:5000/simcore/services/comp/opensim-python-runner:1.0.0                               # Tag the image for the local registry
```
Clone and deploy osparc-simcore locally as shown [here](https://github.com/ITISFoundation/osparc-simcore/#getting-started). These are the necessary steps:

``` console
# clone repo
git clone https://github.com/ITISFoundation/osparc-simcore.git
cd osparc-simcore

# show setup info and build core services
make info build

# make a local registry (to later push your newly-created image)
make local-registry

# push the image to the local registry
docker push registry:5000/simcore/services/comp/opensim-python-runner:1.0.0

# starts swarm and deploys services
make up-prod

# open front-end in the browser
xdg-open http://127.0.0.1.nip.io:9081/
```
Now the Service can be loaded on oSparc and used as any other [osparc-python-runners](https://docs.osparc.io/#/docs/tutorials/python_runner).
## CI/CD Integration

### Github-Actions

[github-ci.yml](https://github.com/ITISFoundation/osparc-python-runner/blob/master/.github/workflows/github-ci.yml)
