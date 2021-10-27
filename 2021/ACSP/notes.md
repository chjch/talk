# Presentation notes

## Title Slide

Thank you for joining with me today, my name is Changjie Chen.

I am currently a postdoc associate at the Florida Institute for Built
Environment Resilience, also known as FIBER, which is a relatively newly
founded institute at the University of Florida.

FIBER conducts research, synthesis, and design at the interface of human, built
environment and ecological systems to respond to the complex threats faced by
Florida and many regions around the world.

The title of my presentation today is Integrating Parallel Computing into
land-use suitability modeling, a case study in orange county, Florida.

This study aims at providing a complete solution to integrate High Performance
Computing into a task that is central to land use planning—GIS-based
suitability analysis.

## Slide 2.1

To begin with, I'd like to talk about some of the challenges planners faced
in land-use planning.

First, conflicting purposes among stakeholders.
Here the stakeholders refers to different interest groups, such as property
developers, environmentalist, farmers, business owners.
Their interests do not fit. And, more often, they go against with one another.

Another challenge involves uncertainties due to the nature that planning is
looking ahead of time.
These uncertainties may cause unintended consequences of a land-use plan that
is simply unfathomable at the time when the plan is formulated.

The third one is kind of related to the first one, but more tangible is that
how to incorporate the so-called soft data.
They are people's preferences, priorities, judgements, or sometimes just
intuitions.

The importance of these soft data can be easily underestimated or sometimes
ignored, but they are deeply rooted in the communicative (or procedural)
rationality of planning.

## Slide 2.2

As a critical technique widely adopted in land use planning, suitability
analysis provides a useful framework to deal with these challenges.

Broadly defined, land use suitability aims at identifying the most appropriate
spatial pattern for future land uses according to specified requirements,
preferences, or precursors of some activity.

The concept of suitability analysis wins favor with land-use practitioners
because of its advantages in synthesis of community values, expert knowledge,
and the result from GIS analysis.

But the technique also possess some shortcomings.
First, comparing to a stochastic model, suitability analysis is a deterministic
process that is relatively more time consuming from identifying criteria,
acquiring data, and not to mention the numerous geoprocessing functions.

Also, since the process consists of many sub-processes that aren't automated
but manually conducted by planners, the process has very limited
reproducibility.

With the advancement in high-performance and cloud computing technology, I
believe it is the time that we can integrate such enhancement in computing
power into the suitability modeling.

## Slide 3.1

The model I used to test the integration of parallel computing is the Land use
conflict identification strategy, also known as LUCIS, which was originally
developed using ModelBuilder in ArcMap.

LUCIS is a goal driven GIS model that produces a spatial representation of
probable patterns of future land use.

This diagram shows the conceptual framework of LUCIS.
As you can see that it includes a hierachical structure from sub-objectives to
objectives, and to goals, each one of which consists of one or more
geoprocessing functions.

## Slide 3.2

The term parallelization refers to turning a sequence of linearly operated
tasks into a program that have multiple components run simultaneously.

The idea is simple, the more workers you have the faster your job gets
done, in this case the workers are CPUs.
Although, the actual implementation is not as straightforward.

Three components were involved to achieve the integration.

First, I developed an open-source Python package dedicated to GIS-based land
use suitability, which called PyLUSAT.
It is cross-platform which is crucial because virtually all supercomputers
today run on a Linux system.
PyLUSAT follows vector-based GIS routines.
That choice is worth discussing but not in the scope of my presentation today.

The second component is a database system that supports concurrent connections
by multiple CPUs for data Input and output.
I chose to use PostgreSQL database and its spatial extension PostGIS.
It is arguably the most advanced database in the open-source world.

The third component is designing the structure of the computing process.

Let us look at each component in details.

## Slide 3.3

The name of PyLUSAT stands for Python for Land-Use Suitability Analysis Tools.

It is developed using open-source Python libraries only.
You can refer to its dependencies in this image.

It has been published on the official Python Project Index, or PyPI.
Currently, the software is at the version of 0.5.2.

It is free to use simply do a pip install will work.
And for more details you can also check this GitHub repository.

## Slide 3.4

The PostgreSQL database is the underlying relational database management
system or RDBMS, which support concurrent connections made by different CPUs to
a single database instance.

This is a very critical feature.
As you may aware, that not all GIS database, such as the File GeoDatabase from
Esri, support concurrent connections.

The PostGIS extension adds spatial support to PostgreSQL it features enhanced
spatial indexing, advanced geometry manipulation capability, 3-D and topology
support, and operational support for vector and raster data.

## Slide 3.5

The image on the left shows a conceptual model of how individual tasks are
routed in the entire parallelization structure.

Since tasks are independent modeling different aspects of different land-use
categories, the overall structure is a task or functional-based parallelism
comparing to data parallelism, such as the Hadoop system that you may have
known.

Using the simple Linux utility resource management (SLURM), a boot script will
fire up other available CPU resource to run individual tasks simultaneously.

The test was running on HiPerGator, a supercomputer at the University of
Florida, using 95 computing cores.

## Slide 4.1

The study area I chose to work with is Orange County in Florida.

The county is at central Florida with about a thousand square miles.

Perhaps the most noticeable fact about the county is that it houses the
metropolitan Orlando.

The county has a fairly complex biophysical system, featuring nearly 400 lakes
and ponds, and numerous wetlands, as well as two major rivers run across the
county—St. Johns and Kissimmee river.

## Slide 4.2

The county is also one of the fastest growing region in the state in terms of
population.

Between 1970 and the most recent census in 2020, the county's population grew
three times, with an average decennial rate of 32.68%.

The rapid increase in population went hand in hand with some major historic
events, such as Disney World and Universal Studio.

It is projected that the population will reach 2 million in 2045.

Today, the county have lost a lot of land where it used to be groves and
orchards by which the county was originally named after.

## Slide 5.1

As a pilot study, I used 52,910 developable parcels in Orange county, that is
the parcel does not not contain any waterbody or currently used for
single-family housing, or for any type of heavy industries.

This image shows the overall urban suitability for all the parcels included
in the analysis.

The process took roughly 5 minutes and a half to complete using 95 CPUs on
HiPerGator.

I also tested the time cost using my laptop, which has a 4-core CPU.
The same computing task took a little bit over 46 minutes, which still is not
bad comparing with using ModelBuilder in ArcMap.

And as anticipated, the total time cost is noticeably bounded by the job that
took longest to complete.

## Slide 5.2

With this process, results of suitability analyses for all land uses are saved
on the database.

(next)

If we zoom in to one of the community in Orange County called Winter Garden,
we can visualize and closely check the result through this animation.

## Slide 5.3

Another application that can take advantage of the enhanced computing speed
is simulation.

In this case, we looked at a site selection problem which tries to identify
the most suitable parcel among the six shown in this map for mixed-use
development that ideally is suitable for multi-family residential, commercial,
and retail uses.

The simulation is done 120 times through randomizing the reciprocal matrix of
the Analytic Hierarchy Process adopted by the LUCIS model.

## Slide 5.4

These point clouds shows suitability of these six parcels in all 120 scenarios
in three dimension, again multi-family, retail, and commercial.

So, we can clearly identify that parcel A has the highest suitability in all
aspects.

## Slide 6.1

This pilot study proved that integrating parallel computing into land-use
suitability analysis is indeed feasible and can save planners tremendous time
in running geoprocessing functions.

The capacity of massively simulating alternative scenarios also gives rise to
new strategy in land-use planning practices.
For example, scenario planning, which is a useful technique to deal with the
uncertainties, can be performed in a probabilistic-based approach thanks to
the time saved in creating a single scenario.

Regional planning can also take advantage of the improved speed to process
large spatial datasets.

Last but not least, the streamlined process enables meaningful changes in
public engagement and the communicative approach of planning.
For example, with a careful setup, planners can generate scenario using
community inputs in almost real-time during a public hearing meeting.

It is definitely a meaningful realization of the so-called citizen science.
