# NetworkX Roadmap Discussions:  August 13th, 2020

### Topics  Roadmap??

Please add...

0. Greetings, Introductions, etc
1. Release schedule: Maybe shift earlier for 2.5 stuff that's there now. Then pygraphviz move to v2.0 and pre-v3.0 cleanup in late Fall with v3.0 in late winter?  Once that is ready, v3.1 should follow with many new features (viz, algorithms).
2. Matrix form:  numpy.array or scipy.sparse??   Quirks of the scipy sparse matrix treatment came up in the Spring last week. Common methods like .sum(axis=1) and .todense() shift to arrays, so it is hard to know which form we're working with.  Is it worth shifting all Scipy.sparse matrix to using an array interface in place of the matrix interface? --> csgraph? also PyData Sparse? --> NXEP that looks at Scipy.csgraph, PyData.sparse and patterns of interface 
4. It'd be nice to have weighted edges handled the same throughout the package. Currently we sometimes use a string to denote the edge attribute and sometimes (started in weighted shortest_paths) we provide either a string or a function which could compute the weights on the fly. There is a `_weight_function()` function defined in weighted.py that processes the input and provides a function for the algorithm to use.  PR #4085 is a good start toward implementing this function interface more broadly using decorators.  Questions arise around what ```is_weighted()``` should do: maybe rename to ```all_edges_have_weight()```. Also should we handle default values. And how to handle setting weight=None.  NXEP?!?!
5. Currently NodeDataView and NodeView and EdgeView have "contains" and set-like operations. EdgeDataView does not have "contains". Can we get that to work? (Trouble is what key to look up--the (node,data-value) (u,v,data-value) matches the iterator, but is not hashable for some data-values (like data=True).
6. Can we find ways to onboard newcomers more efficiently? Will Peppo, whom I know from LinkedIn, will be joining in the call, and has some ideas to share and possibly put in a PR for. see #4089 as well
7. Generators:  is there a way to construct either edge generators or graph generators from the dev side while providing an efficient interface to the user side. path_graph --> edge generator, barabasi_albert_graph --> graph generator.
   - A bit of background: related to the `create_using` kwarg
   - Followup: a bit of research on the `create_using` history
8. visualization: how do we support and expand on nx_pylab.py? Can be enough for its own package, but how do we make that work -- better to be a subpackage?

New topics: tidelift funding... (check out options)

### Discussions

 - Great feedback/ideas on organizing sprints, encouraging contributors, etc (thanks @willpeppo!)
   * Community engagement - "ambassador" role? Someone to help map contributors to things they can help on in a more hands-on fashion (mentoring)
   * Applying "triage" principle to contributors - map contributors based on their level to appropriate issues. For example - if a contributor is trying to learn how to code, then a small issue/change that focuses on using GitHub etc. may be appropriate.
   * Include a pre-sprint Orientation for newer contributors - e.g. do a screen share that walks through the development process for an issue
   * An interesting observation: have to look at the contribution guide from multiple (e.g. 5) OSS projects to get a good idea of general best-practices for development

[^1]: Please provide feedback on your preferred platform for online sprints. We're open to trying different platforms with the ultimate goal of finding a platform that is accessible to all users.