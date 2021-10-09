### Sub-organization information

Sub-organization with whom you hope to work(*): NetworkX

### Student Information

**Name(*)**: Mridul Seth <br/>
**Email(*)**: seth.mridul@gmail.com <br/>
**Telephone(*)**: Melange <br/>
**Time zone(*)**: IST (UTC +5:30)<br/>
**IRC handle including network**: mridul_seth@irc.freenode.net<br/>
**Source control username(s)(*)**: MridulS (github)<br/>
**Home Page(s)**: http://mseth.me<br/>
**Blog(s)(*)**: http://mseth.me<br/>
**GSoC Blog RSS feed(*)**: http://mseth.me/feed.xml<br/>

### University Information

**University(*)**: BITS Pilani, KK Birla Goa Campus, India <br/>
**Major(*)**: Mathematics and Electronics Engineering <br/>
**Current Year and Expected Graduation date(*)**: 2nd Year, May 2018 <br/>
**Degree(*) (e.g. BSc, PhD)**:  Dual Degree MSc. (Hons) Mathemetics and B.Engineering (Hons) Electronics Engineering <br/>

### Project Proposal Information

#### Proposal Title (*)

**NetworkX: NetworkX 2.0 and API**

#### Proposal Abstract (*)

The first part of this project deals with cleaning up the the existing graph API. The functions returning both lists and iterators will be changed to return iterators and thus the _iter suffix will be retired. This will be followed by a cleanup and reorganising the existing codebase as required and decided, and getting it ready for the 2.0 release. The next part will be to design and implement a new unified graph API. 

#### Description
 
As NetworkX moves towards NetworkX 2.0 and an iterator/generator reporting model, implementing a better, less redundant API is a must for NetworkX. The central idea of this project (proposal) is to offer to first update the existing graph API, clean up and review the existing code, and update the documentation as required and prepare it for 2.0 release. The second part is design and implement a new generic and unified(?) graph API with a clean design.

The three tentative project deliverables:
* Rename the _iter-suffixed functions over their unsuffixed counterparts
* Investigate and implement a generic interface for accessing graph data
* Use the opportunity presented by a reorganization to implement efficient code for Python 2&3

For functions like neighbors/edges/adjacency which have two functions to report their data, one which returns a list func and the other returns an iterator func_iter. As we are moving towards a iterator reporting paradigm for 2.0 API, the functions returning lists will be changed to return iterators and thus the _iter suffix will be retired. This would be a backward-incompatible change. The third part will be implemented at the same time while working on part 1 and part 2. Efficiency could be improved for Python 2 with the use of the future package(?). Thus making the code more cleaner and easier to maintain. (This has to be discussed more as there are different opinions within the community). It is possible that we drop support for python 2.7 in the coming time, updating the code to be more python 3 efficient will also be considered(?).

As discussed in #1382, 2.0 would try to avoid bringing a lot of new backward incompatible changes. We already have lot of new functionalities. So, the main focus for NetworkX 2.0 should be renaming the _iter functions, and we will be breaking backward compatibility by doing the current changes and it is much easier for people to adapt code to backwards incompatible changes when the number of changes are small and fairly insignificant. This [commit] (https://github.com/ysitu/networkx/commit/bfce79ab2af50f5a7e7769d36b8c40909985e4c9) will be used as the starting point for the renaming of _iter functions. Adding a new single query all function will also be further investigated(?). The first part of the summer will be focused on renaming _iter functions, cleaning and reorganising the existing code as required and decided by the community, updating documentation, tests and getting ready for NetworkX 2.0. The code should be ready to be merged in the master repository.

``` python
def nodes_iter(self, data=False):
     if data:
           return iter(self.node.items())
     return iter(self.node)     
def nodes(self, data=False):
     return list(self.nodes_iter(data=data))
```
will change to
``` python
def nodes(self, data=False):
      if data:
           return iter(self.node.items())
     return iter(self.node)
```
which leads to
```
Ran 2574 tests in 32.606s

FAILED (SKIP=8, errors=246, failures=116)
```

In the above example we can see that nodes_iter will be retired and nodes will now return an iterator instead of the list. There are many algorithms which consider nodes as a list, the code will be adjusted for iterator, which is evident from the fact that changing one function breaks the code significantly. The major part would be to fix these issues for every function which has an _iter suffix in the core graph classes for Di/Multi/Graph the current codebase which use these function as lists.

Coming to the next part of the project, the new API part. The most important part of the project and probably difficult is to define an appropriate design specification. Further the redesigned API should have a feasible design plan and shouldn't affect (lower) the performance drastically. There are many ideas discussed in #1246. This new API should be able to give an unified interface to access graph data which should be independent of the base class, it could be OrderedGraph, ThinGraph, etc. which is implemented in #1314. It should also work with alternate backends like a SQL based one. Currently Di/Multi/Graphs have different APIs, this project would also further investigate the idea of having a unified API for different base classes. #1246 comment. As there will be many different opinions regarding the new API design, discussion regarding the design specification with the community will be a major part of the new API, which will be mainly done during the community bonding period and the first part of summer. Initially something like [Design Specification] (https://github.com/networkx/networkx/wiki/Design-Specification ) was suggested but it had mixed opinions. At the same time the new implementation will go through rigorous performance testing with the old API. 

Although the second part is a lot of work and might be difficult to implement in the summer, I'll be working on it during the major part of the second part of summer and after GSoC, and will make sure that the new API is ready for the next major release.  The new API will be implemented in a different branch. The project will end with updating the documentation, unit tests, adding more examples, and writing a wrap up report on significant changes in NetworkX 2.0.

Note: (?) -> to be discussed further with the NetworkX community
 

#### Timeline

A rough timeline:
 
##### April 27 - May 25 (Pre GSoC Period-Community Bonding Period)

* Working on issues and pending PRs in [2.0](https://github.com/networkx/networkx/milestones/networkx-2.0)
* Working on issues like [1120](https://github.com/networkx/networkx/issues/1120), [#793](https://github.com/networkx/networkx/issues/793), [#730](https://github.com/networkx/networkx/issues/730)
* Discussion about the API Design specification
* Getting more familiar with the core code and graph classes.
 
##### May 25 - July 6
 
* Working on renaming _iter functions.
* Updating the documentation and tests as necessary.
* Further discussion about the API design.
* Cleaning and reorganising the current codebase as required and decided.
* By this time the graph api will be ready for 2.0.
* Get feedback for the code and solving bugs that pop up.
* Some more work on [2.0](https://github.com/networkx/networkx/milestones/networkx-2.0) (?)
* Making sure that the code is good enough to be merged in the master repo and ready for 2.0 release
 
##### July 7 - August 21

* Working on the implementation of the new API design.
* Cleaning and reorganising the current codebase as required and decided.
* Final wrap up report detailing the changes in 2.0, and progress on the implementation of new API.
 
##### August 21 - (Post GSoC)

* Continue working on the new API design and get it ready for the next major version release. :)
 
#### Link to a patch/code sample, preferably one you have submitted to your sub-org (*)

* Expose transitive_clousure and antichains in the public API (https://github.com/networkx/networkx/pull/1413) [MERGED]
* Added dijkstra version with node weights (https://github.com/networkx/networkx/pull/1350 ) [OPEN]
* Added power function for simple graphs (https://github.com/networkx/networkx/pull/1399) [OPEN]
* Non isomorphic trees generator (https://github.com/networkx/networkx/pull/1427) [OPEN]
* Added data keyword in G.neighbors (https://github.com/networkx/networkx/pull/1344 ) [CLOSED]


### Other Schedule Information

* Please indicate any vacations or other time off that you may be taking or expecting to take over the course of the summer. Any time that you would not be expecting to work should be noted here, along with a reason. (e.g. "June 12-15th, travel for my sister's wedding" "May 30, midterm exam") Google expects you to work 40h/week for the entire GSoC period, so consider how you will make up any lost time (you may have to start coding during community bonding, for example).

 * May 3-15th University Exams (Community bonding period)

* Have you applied with any other organizations? If so, do you have a preferred project/org? (This will help us in the event that more than one organization decides they wish to accept your proposal.)

 * Yes, I am also applying to GNS3, another sub org in PSF. I would love to work with both the organisations but I would prefer to work with NetworkX as I have been using/ working with them for a longer time. 

### Sub-organization specific information

#### Background

I have done formal course work on graphs and networks as part of my college courses. I have been using Python and Git for more than 2 years now, and I am comfortable with them. I am also comfortable with working on C. I work on an OS X machine with vim and sublime as my text editors. I started contributing to open source projects a year back, I did some contributions to sympy. I work with the registration office of my university, and have created many softwares to facilitate course registration and to generate time tables for the students.

I started using NetworkX in early December, 2014 and started contributing to the open source project in early February, 2015.

My PR regarding the data keyword in G.neighbors [#1344](https://github.com/networkx/networkx/pull/1344) lead me to choose this project for GSoC. Further issues can be seen at https://github.com/networkx/networkx/issues?q=author%3AMridulS 

I plan to invest 40 hours per week during the summer for this project. And about 10-15 hours per week for pre and post GSoC period. I plan to continue working for networkx after GSoC. I will also be writing a weekly blog about the progress during the summer. 
