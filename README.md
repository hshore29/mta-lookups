# MTA Lookup Files
The MTA publishes a huge amount of open data, from turnstile counts to train statuses and bus locations. However, not all of the metadata associated with those feeds is updated and much of it is not easily joinable. This is my attempt to rationalize some of the metadata for these data sets.

The *source* folder includes copies of the original files published by the MTA; noted below are the original paths and retrieval dates. The *clean* folder includes my rationalized versions of those files and mappling files that can be used to join across them.

## Cleaned Files

**remote_booth_station.csv**: this is a rationalized Remote-Booth-Station file with the following improvements over the MTA's currently posted copy:
* Includes stations that have opened since 2015 (Hudson Yards, 2nd Av Subway, WTC Cortlandt, plus new exits on the L and others)
* **status**: current status for the booth; Open, Closed, Exit-only, or Vending Only (several booths in the MTA lookup file are locations with MetroCard vending machines outside transit stations, like LGA, Eltingville Park, or the MetroCard Buses).
* **tracked_as**: in the MTA's turnstile data, some control areas are grouped together, despite the areas still being labeled separately on schematics. Exit-only control areas or those with just one or two HEETs will typically be grouped with a nearby control area. This column notes which C/A this area is tracked under in the turnstile data (which also typically match the markings on the security doors and booths).
* **has_booth**: flag for whether this control area still has a token booth (many were removed in 2010)
* **prior_remotes** / **prior_station_names**: list of remote units and station names a booth had since ~2009
* **station_id** / **station_id_2** / **station_id_3** / **station_name**: the MTA station ids directly accessible from this booth, plus the primary station's name
* **complex_id** / **complex_name**: the id and name of the station complex the booth is part of

**booth_station.csv**: I mapped each booth code to one or more station ids. Since some control areas provide access to multiple stations, up to three are specified for each booth roughly in order of proximity. Only direct access is noted, for indirect access you would look for other stations with the same complex id.
There are several booth codes that don't provide access to an MTA station; I filled in placeholder station ids and names for those at the end of the file. This includes vending-only "control areas" like the MetroCard Bus, but also non-MTA systems that accept MetroCards like the PATH or JFK AirTrain.

## Sources

**Remote-Booth-Station.xls**: lookup provided by the MTA as metadata for their turnstile data set. Maps Booths (aka C/As or Control Areas) to Remotes (stations or sections of stations) and station information like name, line, and division. Since 10/18/14 all these columns have been included in the turnstile data files themselves. Given the absence of certain stations and what names other stations have, I think this file was last updated in mid-2012; although it includes references to long-shuttered entrances dating at least to the 90s if not earlier.
* Location: http://web.mta.info/developers/resources/nyct/turnstile/Remote-Booth-Station.xls
* Retrieved on 9/5/22
* Last Updated ~2012
* CSV version also included

**Remote-Booth-Station_2010.csv**: this is an older ~2009 edition of the Remote Booth Station file, found elsewhere on GitHub.
* Location: https://github.com/jehiah/mta-turnstiles/blob/master/data/Remote-Booth-Station.csv
* Retrieved on 9/5/22
* Last Updated 8/11/10

**Remote-Booth-Station_Turnstile.csv**: booth information pulled from the MTA's weekly turnstile data. All metadata from 10/2014 onward is from the turnstile files themselves; prior to that I populated metadata using the official Remote-Booth-Station file. The code used to generate this file is in **turnstiles.py**.
* Source: http://web.mta.info/developers/turnstile.html
* Retrieved on 11/19/22
* Data from 5/5/2010 through 11/11/22

**Stations.csv**: file provided by the MTA as metadata for their GTFS schedule and real time data. Includes MTA station ids, matched to GTFS stops, grouped by complex id.
* Source: http://web.mta.info/developers/developer-data-terms.html
* Retrieved on 9/13/22
* Last updated 8/3/22

**StationComplexes.csv**: additional file including complex names (complexes are groupings of connected stations).
* Source: http://web.mta.info/developers/developer-data-terms.html
* Retrieved on 9/13/22
* Last updated 7/7/22

**2009_nyct_booth_closures.pdf**: due to the 2008 global financial crisis, in January 2009 the MTA prepared a plan to cut costs by shuttering token booths and reducing the number of booth attendants. As part of the proposal, this document was prepared, which includes labeled blue prints of every station with impacted booths. The plan didn't end up being enacted until 2010, and some changes may have been made, but this document seems largely accurate - all kiosks were removed, and most stations had all but one booth closed.
* Source: https://web.archive.org/web/20090305201018/http://www.mta.info/mta/09/2009_nyct_booth_closures.pdf (linked from https://en.wikipedia.org/wiki/Kew_Gardens–Union_Turnpike_station) - the report doesn't seem to be hosted on the MTA site anymore
* Retrieved 6/21/20
* Report dated 1/2009

**2009_nyct_booth_closures.csv**: my transcription of the booth details from the blueprints. Includes station name, lines, borough, and booth / control area designation, plus additionally:
* **Current** / **Proposed**: the control area's booth status, using MTA shorthand - F/T is a full-time booth, P/T is a part-time booth, K is a kiosk (booth with no sales, just customer service), X is an automated entry control area (no booth).
* **Description**: my shorthand for where the control area is located. Access to one direction is indicated with N/B or S/B (north-bound or south-bound) while Mez (mezzanine) indicates access to both directions; then I note the closest cross street; finally a cardinal direction if multiple control areas are at the same intersection.
* **Additional C/As**: some control areas that never had booths (often exit-only or single-HEET C/As) don't get their own code, so when codes are reused I list the secondary C/As here with the same notation as the Description column. Ex: Clinton-Washington Avs has four exits, but while the SB exits have different codes (N112 and N112A) the NB exits are both just coded N111; so N111's description is *NB Clinton Ave* with an additional C/A at *NB Washington Av*.
* **Notes**: any additional notes from the blueprints, about ongoing work or station changes.

**New York MPS Station Maps.pdf**: in 2004 and 2005, 47 subway stations were added to the National Register of Historic Places. The applications are available online via the National Archives catalog, and each includes blueprints of the station in question. The blueprints include booth numbers and locations; this PDF collects just the blueprint pages from the MPS applications.

**New York MPS Stations.csv**: my transcription of the booth details from the blueprints.
* **Ref**: the reference number of the application
* **Received**: date that the NRHP recieved the application
* **Date**: date that the attached blueprints were drawn / had changes to fare control areas
* **Station ID**: blueprints since 1995 include the MTA's Station ID number
* **Booth**: the booth number if labled on the blueprints. "X" and "Exit-only" indicate control areas that were open but had no booth ("X" denotes automated entry); "Closed" indicates control areas that existed but were closed
* **Description**: my shorthand for where the control area is located, see 2009 booth closure description
* **Notes**: my additional notes clarifying locations for some areas

**FEIS Documents**: NYC requires environmental impact statements for some large projects, which includes their impact on transit. Frequently, these projects will include blueprints or diagrams of subway stations. All final EIS documents are posted on the NYC Planning website here: https://www.nyc.gov/site/planning/applicants/eis-documents.page; below I've included excerpts of the FEIS documents that include subway station plans. Control areas are sometimes referred to as "fare arrays".

* **FEIS Innovation QNS.pdf**: section 13.H, dated 9/9/22
* **FEIS River Ring.pdf**: section 12.G, dated 11/5/21
* **FEIS NoHo-SoHo.pdf**: section 14.G, dated 10/8/21
* **FEIS 343 Madison Avenue.pdf**: section 9 pg 41-50, dated 9/10/21
* **FEIS Gowanus Neighborhood Rezoning.pdf**: section 14.H, dated 9/10/21
* **FEIS Acme Fish Expansion.pdf**: section 10.H, dated 3/26/21
* **FEIS East Harlem Rezoning.pdf**: section 14.H, dated 9/19/17
* **FEIS Greater East Midtown Rezoning.pdf**: section 12.4, dated 5/26/17
* **FEIS East Midtown Rezoning and Related Actions.pdf**: section 12.8, dated 9/27/13
* **FEIS Silvercup West.pdf**: section 10.0, dated 5/24/06

Additional environmental impact statements not found on the NYC Planning website, with links:

* **FEIS Pennsylvania Station.pdf**: section 14 figures 8-10, dated 6/30/22, from https://esd.ny.gov/penn-station-area-final-environmental-impact-statement-feis
* **FEIS Atlantic Yards.pdf**: section 13 figures 1, 2, dated 11/2006, from https://esd.ny.gov/subsidiaries_projects/ayp/ay_feis.html

**FEIS.csv**: my transcription of the booth details from the blueprints found in the FEIS documents, in a similar format to the NRHP and Booth Closure transcriptions.

**1988/1991 MTA Board Meeting Minutes**: User Kew Gardens 613 on Wikipedia took pictures of a number of batches of MTA board meeting minutes from the late 80s/early 90s detailing a wave of booth closures; links to those pictures are catalogued on their user page here: https://en.wikipedia.org/wiki/User:Kew_Gardens_613/List_of_closed_New_York_City_Subway_entrances#Closed_street_entrances. I scanned through those and used them to fill in missing booth codes, many of which are still open but as HEETs which the MTA doesn't track as separate C/As.