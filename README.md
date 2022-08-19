# PV+Wind+ES in NREL SAM, PVPMC 2022
Supporting files for my poster presentation at the 2022 PV Performance Modeling Collaborative (PVPMC) workshop in Salt Lake City.

See [SAM_HOMER_setup_notes.md](SAM_HOMER_setup_noted.md) for details on inputs to SAM and HOMER for comparing them for PV+ES offgrid modeling. 



Outline:
- Summary
	- NREL SAM can model renewable energy systems with battery energy storage (ES) for load-matching. This can include off-grid systems (microgrids) or 24x7x365 "energy matching" scenarios, e.g., for corporations with sustainability goals that want to go beyond simple annual net zero configurations [1, 2]. 
	- This work shows that SAM and HOMER (v2.68, "Legacy") can get similar results in optimizing PV+ES microgrids.
	- This work highlights a few ways to model PV+ES _and_ *PV+Wind+ES* systems, including "hacks" to model PV+Wind+ES with parametric sizing.
- "Offgrid" PV+ES in SAM
	- Grid Outage
		- Start with a PV+ES performance model (Detailed PV-Battery or PVWatts-Battery) with a Distributed financial model (to include electric load).
		- In the Grid Outage tab, set the critical load to be 100% of electric load, and edit the grid outage array to be 1 in all timestaps (*TIP: create an array of 1s in an external tool like Excel and then copy/paste into SAM*). Note that the default minimum state of charge during an outage (10%) is different from the standard battery dispatch default (30%) - adjust as needed. 
		- [figure: screenshot of grid outage setup]
		- Primary Outputs: "Critical load unmet percentage (year 1) (%) [`annual_crit_load_percentage`], or other related critical load outputs.
		- _Note: very small (e.g., 1e-5%) unmet load percentages may show up, presumably due to rounding and efficiency calculations, and can be treated as zero._
		- Increase PV and/or battery size until critical load needs are sufficiently met. 
		- *TIP: use "Quick setup..." in the parametric tool to create independent ranges of battery energy capacities and PV sizes. Set the battery desired power rating to be equal to the maximum possible PV size outside of the parametric tool for a simplified first pass.*
	- Grid Target dispatch
		- This option requires a few extra steps, but offers additional equipment combinations through the fuel cell model, as will be described later.  
		- Similar to using the grid outage, select a performance model with PV+ES and a Distributed financial model. 
		- For battery dispatch, use "Input grid power targets" and set the monthly power target to zero for all months. 
		- [figure: screenshot of battery dispatch setup]
		- Primary Outputs: "Electricity load total in each yeah (kWh)" (`annual_electric_load`) and "Annual energy imported from grid (kWh)" (`annual_import_to_grid`), unmet load in each year is the ratio of the two annual values. 
		- Increase PV and/or battery size until annual energy from the grid is sufficiently close to zero.
		- [figure: screenshot of stacked time series of electricty to load sources]
- SAM vs HOMER (Legacy) for PV+ES
	- SAM can give very similar results to the "legacy" version of HOMER (v2.68) using methods described above using parametric sizing of PV and batteries.
	- A sample case was setup using a residential Detailed PV-Battery case in SAM and generally replicating it in HOMER, considering PV sizes of 5-30 kW and batteries of 20-100 kWh. See [3] for details. 
	- Annual unmet load results were very similar:
		- SAM (Grid Outage): [figure: SAM grid outage heatmap table]
		- SAM (Grid Target): [figure: SAM grid target heatmap table]
		- HOMER: [figure: HOMER heatmap table]
		- Noteable difference: 20 kW PV, 60 kWh battery, unmet load rounds to 0.00% in the SAM (Grid Outage) results, but is >0.00% in HOMER and SAM (Grid Target)
	- Runtime: SAM ran the 30 parametric cases in about 20 seconds (for both grid outage and grid target dispatch modes), while HOMER ran in about 1 second.  
- Wind+PV+ES in SAM
	- SAM doesn't offer PV+Wind+ES models (yet), but there are workarounds. 
	- The Generic-Battery model can import generation profiles from other open cases (e.g., setup a PV case, a second case for wind, and Generic-Battery as the third case that imports the wind and solar profiles) and then dispatch a battery. 
	- _*But what about using the parametric tool for sizing PV and wind?*_
	- Use a Fuel Cell - PV - Battery case, where the fuel cell operates to match the profile of a the wind generation.
		- Create a separate wind case, modeling the minimum single "block" of wind (single turbine or minimum size of a group of turbines for the project)
		- Enable a battery
		- Set fuel cell nameplate to the capacity of a single "block" of wind, minimum unit output to 0%, annual degradation to 0%, startup and shutdown times to 0 hours, and ramp up/down limits to 1e30. 
		- Under Dispatch, set fuel cell dispatch to "Input dispatch", edit the time series array, and paste in the wind case generation profile (System power generated (kW) exported from Data Tables, then copying just the generation profile without headers and timestamps using a tool like Excel)
		- For off-grid/load matching, set bettery dispatch to grid target with a monthly target of 0. 
			- _NOTE: The Grid Outage option is available for Fuel Cell models, but batteries do not currently charge from Fuel Cells (our wind proxy) [4]._
		- *TIP: If SAM will be used for a financial analysis, make sure to set the fuel cost in the Operating Costs tab to $0.*
		- For parametric sizing optimization, use "Number of units in stack" (fuelcell_number_of_units) to vary the installed wind capacity. Adjust PV and ES capacity as described previously. 
		- [figure: screenshop of PV+Wind+ES parametric interface]
		- [figure: heatmap of results]
		- [figure: stacked stepped time series of interesting interval]

Model Files and Additional Details
https://github.com/williamhobbs/PV-Wind-ES-in-SAM-PVPMC-2022/
		
References and Resources

[1] https://www.bloomberg.com/press-releases/2022-03-07/constellation-launches-sustainability-partnership-with-microsoft-featuring-24-7-365-real-time-carbon-free-energy-matching 

[2] https://www.theverge.com/2022/6/9/23160508/corporate-renewable-energy-misleading-rec-power-purchase-climate

[3] https://github.com/williamhobbs/PV-Wind-ES-in-SAM-PVPMC-2022/blob/main/SAM_HOMER_setup_notes.md

[4] https://github.com/NREL/SAM/issues/1130

---
## Disclaimer:
Anything here is for reference/information purposes only. Do your homework before making real-world decisions based on any model!

> All models are wrong, but some are useful. - generally attributed to [George Box](https://en.wikipedia.org/wiki/All_models_are_wrong)
