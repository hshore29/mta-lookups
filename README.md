# MTA Lookup Files
The MTA publishes a huge amount of open data, from turnstile counts to train statuses and bus locations. However, not all of the metadata associated with those feeds is updated and much of it is not easily joinable. This is my attempt to rationalize some of the metadata for these data sets.

The *source* folder includes copies of the original files published by the MTA; noted below are the original paths and retrieval dates. The *clean* folder includes my rationalized versions of those files and mappling files that can be used to join across them.

## Cleaned Files

**remote_booth_station.csv**: this is a rationalized Remote-Booth-Station file with the following improvements over the MTA's currently posted copy:
* Includes stations that have opened since 2015 (Hudson Yards, 2nd Av Subway, WTC Cortlandt, plus new exits on the L and others)
* **is_active** flag for whether the booth is currently active
* **has_booth** flag for whether this control area still has a token booth (many were removed in 2010)
* **historic** flag for whether the booth was deactivated / closed prior to 2010
* Both current and prior station names

## Sources

**Remote-Booth-Station.xls**: lookup provided by the MTA as metadata for their turnstile data set. Maps Booths (aka C/As or Control Areas) to Remotes (stations or sections of stations) and station information like name, line, and division. Since 10/18/14 all these columns have been included in the turnstile data files themselves. Given the absence of certain stations and what names other stations have, I think this file was last updated in mid-2015; although it includes references to long-shuttered entrances dating at least to the 90s if not earlier.
* Location: http://web.mta.info/developers/resources/nyct/turnstile/Remote-Booth-Station.xls
* Retrieved on 9/5/22
* Last Updated ~2015

**Remote-Booth-Station_2010.csv**: this is an older ~2010 edition of the Remote Booth Station file, found elsewhere on GitHub.
* Location: https://github.com/jehiah/mta-turnstiles/blob/master/data/Remote-Booth-Station.csv
* Retrieved on 9/5/22
* Last Updated 8/11/10

**Remote-Booth-Station_2022.csv**: since most of the metadata columns are now present in the turnstile dataset itself, I made this as a simple pivot of the 4/23/22 turnstile data.
* Source: http://web.mta.info/developers/data/nyct/turnstile/turnstile_220423.txt
