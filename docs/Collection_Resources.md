# Collection Resources

Collection resource represent a set of individual resources, possibly with an unbounded size. Any single API call needs to limit the size of the response, and also the time to compute the response, so unbounded collections must support pagination. Collection resources also may have special parameters for filtering and sorting. Collection resources __MUST__ be given plural names, unless a plural is not available (like _furniture_).

Every resource **SHOULD** be identified by an identifier, called `id` in the response data.

Details are found in the following sections.

+ [Pagination](Pagination.md)
+ [Filtering](Filtering.md)
+ [Sorting](Sorting.md)

Filtering, Sorting and Pagination operations **MAY** all be performed on a collection. When these operations are performed together, the evaluation order **MUST** be:

+ Filtering.
+ Sorting.
+ Pagination.
