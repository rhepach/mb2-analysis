# mb2-analysis

Analysis scripts for ManyBabies 2, corresponding to the Child Development registered report, [Schuwerk, Kampis et al. (accepted pending data collection)](https://psyarxiv.com/x4jbm/). 

For the analysis files of the pilot, look towards the 'Pilot analysis' release or the 'pilot_final' branch.

Subdirectories explanations:

* `helper` stores general helper functions
* `metadata` stores functions that create metadata for preprocessing/analysis
* `import_scripts` contains import scripts for the main dataset.
  * Important notes on how to do this task are in the [MB2 data import guide](https://docs.google.com/document/d/1MEEQicPc1baABDHFasbWoujvj2GwfBGarwrzyS2JQtM/edit).
* `data` (will be created while running the pipeline is gitignored )
  * `01_xy_data` - xy data will be downloaded from osf into this
  * `01_demographic_data` - demographic data will be downloaded from osf into this
  * `02_intermediates` - preprocessing steps will save intermediate rds files here
  * `03_preprocessed` - the result of the preprocessing steps - to be consumed by the analysis script


Proposed changes to folder structure:
* data ->  (as that's all that's being commited into that folder anyway)
* create new folder `data` (gitignored), and move the following things there



Issues to deal with:

- download demographic data!

- Pipeline is glued together, but needs sanity checking/visualization to see if it is working correctly (it probably isn't right now)

- more integrity checking is needed in between operations to validate that data is not lost on accident(need to have a look at every step once more data is here)


- passing the final/correct column names to resample_xy_trial - we could redesign the function to make it agnostic?

- How to standardize pupil sizes?

- cleanup the pipline (a lot of intermediate steps can be combined to make this more compact)

## column-wise validations to be implemented
Basic columns need to be validated/tested before we pass them on. 

These are: lab_id, participant_id, media_name, x, y, t, pupil_left, pupil_right

### lab_id
- should be in the list 
- should be characters

### participant_id
- should be characters
- should be unique within lab (how to check) 
- check if it's unique between labs? uniquify?
- from alvin: how to check participant_id: filter for the rows where t is less than lag(t), i.e., timestamps where t “resets”. then see if length(ppt_id) == length(unique(ppt_id))

### media name
- should be characters
- check again that they're in the list
- check that they're unique within participant_id

### x, y
- numbers
- within coordinate system (check coordinate system)
- not all NA within participant_id, lab_id
- different from one another within participant_id, lab_id

### t
- range of t should be on the order of 10^5 and not 10^8
- check that resampling interval is correct
- check that they're unique within participant id, lab id, and media name

### pupil_left, pupil_right
- check that units are scaled
- check not all NA within participant_id, lab_id

