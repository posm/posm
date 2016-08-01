# Overview

## Problem Statement

As efforts like Missing Maps spread further into the developing world, they spend longer
amounts of time away from internet connectivity. There’s a growing need to update the standard toolset to support these connectivity challenges and thus allow it to be used more widely.

## Genesis

[summarize genesis.md, link to it for deeper background]

American Red Cross / Missing Maps workflow involves both OpenMapKit and Field Papers. We wanted to join
them together into something that could be carried into the field and used with limited or no internet
access available.

## Philosophy

When faced with the decision of whether to create a new tool to achieve a well-defined goal, we have
opted to use or adapt existing solutions and create glue components that allow them to work together
with other tools that have already been incorporated into POSM. As POSM is primarily about
collecting and improving geographic data that exists within OpenStreetMap, the OSM ecosystem is the
source of first choice. Key themes for POSM include:

* __Open Source__: We would rather adapt existing tools and combine them with others to achieve goals rather than write
(and support) our own. With this in mind, software that supports POSM should be Open Source if at all possible.

* __“Small pieces loosely joined”__ describes the world we map and develop software within; therefore, it
should also describe the systems we build.

* __User Focused__: OpenStreetMap’s data model may appear unconventional, but the tools developed within that paradigm
are generally more user-centric than more typical GIS analysis tools.
