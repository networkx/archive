# NetworkX Developer Discussions: January 24th, 2023

Previous meeting notes available [here](https://github.com/networkx/archive/tree/main/meetings). Please feel free to add topics for discussion and news items below.

**In attendance:** Ross, Dan, Sultan, Mridul,  Matt, Erik, Paula 

## Welcome and news

* New meeting time: Tuesdays, 9:30 - 10:30AM PST

## Important dates/deadlines

* Outreachy term ends ~end of February
* SciPy conference tutorial proposals due: Feb. 22nd

## Follow-up from last week

* [name=Mridul] Update on the memgraph situation w/ numfocus
  - Any news from numfocus?
  
- Noisy PRs from students - should we *politely* reach out to the professor and ask that they submit PRs to his fork first while under active development/for internal review first?
  * https://github.com/erelsgl

## Roadmap

- Our current roadmap: https://networkx.org/documentation/latest/developer/roadmap.html
  * On sustainability:
    - Feedback:
      * reviewing quickly is very important
  * Performance:
    - Main topic: backends + dispatching
      * Focus on backends rather than internal use of scipy/numpy?
      * Lump in the Linear Algebra topic from existing roadmap here
    - Have a backend-focused meeting
  * Documentation
    - Ease of finding information and finding what's available in the library (perhaps explore using `__dir__` to affect tab-complete, etc)
    - Related: make the sidebar/navbar in the website better reflect the organization of the library - improve discoverability
    - Also related: documentation site analytics. What are people even looking at? This will be very helpful in identifying what should get the most attention
    - Divide between a guide & a gallery example - which are most used? Are they helpful?
    - Doc/viz: add gallery examples linking to other graph visualization libraries
  * Potential for user experience as a roadmap item?
    * e.g. above bulletpoint
  * Interoperability (section in current roadmap)
    * Subsume/include in dispatching/backend section?
  * Roadmap topic: ecosystem integration
    - Specifically, thinking about integration with other data structures (numpy arrays, dataframes, xarray etc.)
  * Visualization - currently on the roadmap as "it's important, would like to improve"
  
  * End of the roadmap: things that aren't necessarily part of the roadmap, but something like a "wishlist"
    - Visualization could go here, or at least viz-related topics
    - Things like HyperGraphs? i.e. things that we're looking for concrete feedback/use cases/user input to better flesh out what to do
    
  * Benchmarking - useful addition
    * Consider also: benchmarking for backends/backend implementers
    * Could slot in under "Performance" since benchmarks are critical to evaluation of performance
    
  * Main concern with networkx.org homepage - make link to documentation more prominent
  
  * More domain-specific documentation
    * e.g. biology
  
  * Something to keep in mind: don't lump *everything* into dispatching.



- Roadmap prep: ideas to be discussed/included in the roadmap
  * Updating networkx.org homepage
    - Adopting `scientific-python-hugo-theme`? A different theme?
  * HyperGraphs, Temporal Networks, MixedGraphs - More base classes? Should they go in NX?
  * Dispatching 2.
      - Auto conversion between different backend types.
      - Cacheing of graphs between different backends.
  * standardizing attribute transfers from/to pandas, numpy, xarray
  * visualization API improvements--edge bundling, GraVe, nxviz
  * Benchmarking

## Long-term tracking items

- Adopt `pyproject.toml`?
  * [This article was a nice overview](https://snarky.ca/what-the-heck-is-pyproject-toml/)
  * Since NX doesn't have build deps, there's no strong motivation to do so other than "other projects are doing it"
  * Potential "benefit" - store metadata/configs for additional tooling (e.g. type checkers, linters) in one place[PEP 621](https://peps.python.org/pep-0621/) 

- helpful to have a place for best-practices like nbunch, view vs dict, subgraph vs subgraph.copy()
  * nx-guides would be a good place for this!

- Keyword-only and maybe position-only input. Candidate subpackage to start changing code to keyword-only is the nx_pylab. (see [SLEP009](https://scikit-learn-enhancement-proposals.readthedocs.io/en/latest/slep009/proposal.html) for how sklearn did this)

- We should try to automate the release process as much as possible so that we could remove a human from the release process. conda-forge has a very automated release system. Would a SPEC covering automated releases be reasonable?
    - Not clear what the advanatges of automating the release process would be. The process is [documented](https://github.com/networkx/networkx/blob/main/doc/developer/release.rst) What would make this better?
    - Why would automating things further not cause problems?

- Post NX 3 release: A roadmap review.
  - better work with other packages. pygraphviz needs wheels -- getting there (JM wants this before the NX 3 release)
  - GraphBLAS
  - We should have a "roadmap meeting" 
  - parallel backends? its different from backends, but maybe similar?
  - Other ideas
    - GraphBLAS in the performance section
    - Temporal networks, hypergraphs, other new graph types?
    - Make testing more comprehensive -- mixed node types, hashable nodes, etc. (hypothesis?)
    - Benchmarking suites to at least run locally some timing.
  
- Review public interface: For v4.0 we should be looking at the import structure?  Some subpackages are not imported to the main namespace. We have decided that based on history of how they were first committed. Is there a good way to decide which should require an extra referal: `network.community.modularity` instead of `networkx.modularity`?
  - First decision: what do we *want* the interface to be?
  - Module/pkg `__dir__` provides a nice way to modify discoverability of modules/packages **without** breaking existing code
  - NX-guide idea: model/visualize the package as a DAG (thanks Matt!)

- LaTeX interface
   - #5702 has two interfaces: TikZ and adigraph. The adigraph takes values for 3 node attributes like `G.nodes[2]["width"]=3`, while the TikZ version needs values to be strings like "[width=3]" for lots and lots of parameters allowed by TikZ.
   - Do we want two interfaces? what should the interface be? [Answer: we want a single interface if possible and its OK to make the user learn a little TikZ in order to use it effectively.]

- keywords arrows, arrowstyle, connectionstyle. Can we improve this interface? [#5694](https://github.com/networkx/networkx/pull/5694) and a link to the [current interface](https://github.com/networkx/networkx/blob/2c904d18dc79df3acd64495ef64c6ff4674992a0/networkx/drawing/nx_pylab.py#L537)
    - Now raise UserWarnings when users request features that are not available for the underlying edge-viz object (e.g. LineCollection)
    - Is there a better, more wholistic solution?
    - Need to indicate:
      o FancyArrowPatch or LineCollection
      o The arrowstyle (head or not or both, etc)
      o The connectionstyle (arc bendable etc)
    - Maybe changing the name of `arrows` to `fancyedges` would help
    - New logic could be:  If non-default value for either arrowstyle or connectionstyle then switch to FancyArrowPatch.
    - milestone this for v3.0

- Property-based testing
  * Check out `hypothesis` for setting up property-based testing
  * Developing good property-based tests:
    - Reference "naive" (but correct) implementation
    - Generate random graphs, evaluate them for the property you want
    - Pump these through the naive implementation and collect interesting cases for test suite

- Temporal networks: how to think about these
  * seems to be a lot of interest from different communities, but no standard approach yet...
  * Having a use-case to drive the design of a data structure would be good
  
- Stop building/shipping pdf version of documentation?
  * This is being considered by both scipy and numpy and seems to have garnered mostly positive feedback
    - related scipy issue: https://github.com/scipy/scipy/issues/15635
  * revisit after 2.8 release -- We'll keep it for v3.0 and then look at removing it after that.
  * Keep up-to-date with feedback from other projects that are considering this

- nx-guides
  * We need a contributor guide :book: [name=Mridul]
  * Dependency management for tutorials - how to strike a balance between tools people use and maintainability

- New contributors
  * So many good students/contributions - how do we keep the ball rolling (even if we can only accept 1 or a subset of them for this round)
    - Add something to contributor guide about where to look for good issues (e.g. add missing docstring examples)
    - Link to [first issue tag](https://github.com/networkx/networkx/labels/Good%20First%20Issue) on github?
    
- Revisiting our pytest `slow` mark:
  * The `slow` tests are run w/ each push/PR in the coverage job; however, it's not necessarily true that the slow tests improve coverage!
  * Could save a lot of CI time by having a separate category (e.g. `pytest.mark.comprehensive`?) that is for tests that are slow but *don't* affect coverage. These could be run less frequently (scheduled job?) and potentially save several minutes for each push
    - General consensus: seems like a good idea
    - ~~(Jarrod) maybe worth waiting until after the code removal sprint?~~
      - deprecated code removed

- Other CI-saving ideas
    - All tests should depend on the lint action, so all the other tests fail fast.

## Discussion

- [ ] Social media
    * NetworkX twitter
    * blog.scientific-python.org