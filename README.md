# PV+Wind+ES in NREL SAM, PVPMC 2022
Supporting files for my poster presentation at the 2022 PV Performance Modeling Collaborative (PVPMC) workshop in Salt Lake City. The poster was about using NREL SAM to model and optimize renewable energy systems with battery energy storage (ES) for load-matching, including both PV+ES _and_ **PV+Wind+ES** systems. 

An overview of files and folders here:
- [SAM folder](SAM): folder for SAM project and supporting files. 
	- [PV_Wind_ES_in_SAM_rev2.sam](SAM/PV_Wind_ES_in_SAM_rev2.sam): SAM project file for PV+Wind+ES analysis.
	- [SAM_PV_Wind_ES_Results.xlsx](SAM/SAM_PV_Wind_ES_Results.xlsx): Spreadsheet for post-processing PV+Wind+ES results.
	- (plus a few files for PV+ES comparison with HOMER)
- [HOMER](HOMER): folder for HOMER project and supporting files. 
- [SAM_HOMER_setup_notes.md](SAM_HOMER_setup_noted.md): details on inputs to SAM and HOMER for comparing them for PV+ES offgrid modeling. 
- [HOMER SAM Comparison Results_rev3.xlsx]("HOMER%20SAM%20Comparison%20Results_rev3.xlsx"): Spreadsheet that imports and compares unmet load data from the SAM and HOMER PV+ES offgrid models. (_Note that this file has data connections setup to import data from .CSV files exported from SAM and HOMER. Absolute links to .CSV files would need to be updated to update this file on another computer._) 

## Disclaimer:

Everyting here is for reference/information purposes only. Do your homework before making real-world decisions based on any model! As George Box is [attributed with saying](https://en.wikipedia.org/wiki/All_models_are_wrong), "All models are wrong, but some are useful."
