# ReNano for HZZ STXS

We rely on the ```Run3HggSTXS``` scripts developed by the STXS team. I have made a small change to be used also in the HZZ case.
Documentation can be found [here](https://run3hggstxs.docs.cern.ch/user-guide/section8.html)
```
git clone ssh://git@gitlab.cern.ch:7999/atarabin/run3hggstxs.git
```
When using the scripts from this repo, you should source the HiggsDNA environment
```
source /cvmfs/sft.cern.ch/lcg/views/LCG_108/x86_64-el9-gcc15-opt/setup.sh
pip install -e .
```
The installation with ```pip``` should be done only the first time

Then we need a CMSSW environment. To produce the ReNano for 2024 I am using the following:
```
cmsrel CMSSW_15_0_18
cd CMSSW_15_0_18/src
cmsenv
git cms-init
git cms-merge-topic 51499
scram b -j 12
```
This version of CMSSW already contains the bugfix [49644](https://github.com/cms-sw/cmssw/pull/49644) for for the HTXS routine in NanoAODv15.
The MR [51499](https://github.com/cms-sw/cmssw/pull/51499) contains the Rivet routine for STXS 1.3

## How to make a ReNano
Pay attention that some commands should be run in the HiggsDNA environemnt and others in the CMSSW environment!

The list of samples should be written down in ```samples.txt```.

The samples can be fetched using (HiggsDNA env):
```
python3 fetch_datasets.py -i samples.txt
```
The list of files will be stored in a json file named ```samples.json```.

Once the ```.json``` is ready, ```generateRenanoConfigs.sh``` contains the skeleton to run the command to produce the CRAB files.

From now on, all CRAB commands should be run inside the CMSSW environment, the HiggsDNA environment should be deactivated to avoid conflicts.

For each file to be reNano-ed, there is a folder. Inside each folder, there is a ```.sh``` file that should be run to submit on CRAB.
Before submitting do not forget to load the certificate. To check the status:
```
crab status -d crab_workarea/crab_HIG-RunIII2024Summer24NanoAODv15-00006/
```
