# Genesis

The idea of the POSM came at the [HOT Summit 2015](http://wiki.openstreetmap.org/wiki/HOT_Summit) at the [American Red Cross headquarters](http://www.openstreetmap.org/way/66409265#map=19/38.89636/-77.04556) in Washington, DC. Here we presented the first release of OpenMapKit. At that time, OpenMapKit only had the Android component. We were relying on a 3rd party ODK Aggregate server in the cloud to submit survey data. This solution was a great start, and it served our needs when we had a good internet connection at the base camp of an urban survey.

The primary use case of OpenMapKit is to enable a fleet of volunteers to check out a phone and survey an assigned area. This is a standard Missing Maps workflow used by the Red Cross, World Bank, Doctors Without Borders (MSF), and other NGOs. It is a big, all day event, where people come together and enhance OpenStreetMap for a given community.

We use a suite of tools to get the job done, and we typically have 30+ volunteers involved. We gather data in the field both with OpenMapKit and Field Papers, and when we return to base camp, we validate and submit this data ultimately to OpenStreetMap.

Usually this work is conducted in vulnerable communities in developing countries. When we are working in an urban area, we can usually find an internet connection, but the reliability and bandwidth is often very limited. When 30+ people try to access OpenDataKit and OpenStreetMap services in the cloud, __the internet connection is guaranteed to grind to a halt!__

In fact, this problem presents itself everywhere, including the Red Cross HQ in Washington, DC. Further, groups such as the MSF conduct surveys in very remote, rural areas. The team will go days without an internet connection; cloud services are simply _out of reach_.

The question at hand is:

>How do you sync data to the cloud when you are somewhere so remote that internet access is simply not an option?

## Inspiration

We are not the only ones using OpenDataKit to collect data. In fact, ODK is the most widely used data collection tool for mobile electronic surveys. There is a large ecosystem of tools and services, and it has become a standard. For years people have been gathering GPS points using this tool, and OpenMapKit's primary contribution is the ability to collect OSM data and directly tag OSM data from the Android app.

At the 2015 HOT Summit, we encountered several organizations that use ODK, and the one pioneer in particular that stuck out was Ivan Gayton from Doctors Without Borders.

Ivan had a backpack full of [Intel Edison](https://en.wikipedia.org/wiki/Intel_Edison)s, a tiny, low power embedded system that runs linux and has integrated wifi. With 4 GB of SSD space and a 500Mhz dual core Intel Atom CPU, we don't have very much power, but it is more than enough to power an ODK Aggregate Server.

Ivan, having lead the OSM mapping effort in [Lubumbashi, DRC](http://www.openstreetmap.org/node/27564973#map=12/-11.6712/27.5125) and other areas in Sub-Saharan Africa, was interested in using OpenMapKit, but he would need a solution that fully functions offline. So, to explore this concept, we took our computers outside, plugged in a fresh Intel Edision into our USB ports, and installed the classic [ODK Aggregate](https://opendatakit.org/use/aggregate/). After 45 minutes of admin, we got it running. And, we were able to submit OSM data in a survey from OpenMapKit to the device.

## Putting it All Together

As I continued working with the Red Cross, [SpatialDev](http://spatialdev.com/) entered a new phase of work--a project to bring OpenMapKit to the next level. The initial impetus for me was to build a server-side component for OpenMapKit, a simple web service for users to submit their survey data. Still, we wanted to have a deeper integration with OpenStreetMap tools. And, the offline problem had to be solved with more than just OpenDataKit. To achieve this goal, we began a partnership with [Stamen Design](http://stamen.com/), the creator of [Field Papers](field-papers.html). The premise: _integrate OpenMapKit with Field Papers to create a single, cohesive experience_.

The workflow used in [Missing Maps](http://www.missingmaps.org/) surveys and mapathons involves a suite of many open source software components. The initial idea was to use the Intel Edison as the server component for our simple ODK Aggregate server, now called OpenMapKit Server. In fact, OpenMapKit Server runs quite well on the Intel Edison. Our goal, however, was larger--we wanted to run our own OpenStreetMap server, as well as Field Papers on our low powered device.

### More Power

Running our own OpenStreetMap using existing software requires a bit more horsepower than the Intel Edison provides. We began a software integration process, now solidified as POSM Build, on several pieces of hardware: the [Raspberry Pi](https://en.wikipedia.org/wiki/Raspberry_Pi), the [Brck Pi](http://www.brck.com/2014/11/brckpi/), as well as the [Intel NUC](https://en.wikipedia.org/wiki/Next_Unit_of_Computing). Long story short, we needed more power than the Pi had to offer, and the Intel NUC was just right.


