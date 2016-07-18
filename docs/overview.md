# Overview

## Problem Statement

As efforts like Missing Maps make their way further into the developing world, spending longer
amounts of time away from cheap and fast internet connectivity, there’s a growing need to bring the
standard toolset along for the ride.

## Genesis

[summarize genesis.md, link to it for deeper background]

American Red Cross / Missing Maps workflow involves both OpenMapKit and Field Papers. Wanted to join
them together in something that could be taken into the field and used with limited or no internet
access available.

## Philosophy

When faced with the decision of whether to create a new tool to achieve a well-defined goal, we have
opted to use or adapt existing solutions and create glue components that allow them to work together
with other tools that have already been incorporated into POSM. As POSM is primarily about
collecting and improving geographic data that exists within OpenStreetMap, the OSM ecosystem is the
source of first choice.


We would rather adapt existing tools and combine them with others to achieve goals rather than write
(and support) our own. Software that supports POSM should be Open Source if at all possible.

“Small pieces loosely joined” describes the world we map and develop software within; therefore, it
should also describe the systems we build.

OpenStreetMap’s data model may appear unconventional, but the tools developed within that paradigm
are generally more user-centric than more typical GIS analysis tools.
