# gnu

This repository contains a simple implementation of the GAP SCSCP 
server providing Gnu(n) - the number of groups of order n. It is
meant to be an example demonstrating how a remote procedure call 
protocol, called SCSCP, may be used to provide mathematical services
to clients representing computer algebra systems and other software.

This version uses Cubefree package for cubefree orders, otherwise
it delegates to NrSmallGroups. The latter is enhanced by SglPPow 
package which extends NrSmallGroups for groups of order p^7 for 
p>11 and for groups of order 3^8.

The demo version of this service is running in the cloud. To use
it, read the file `gnuclient.g` into GAP. Then you will be able
to use functions `Gnu` and `GnuWithExplanation`, for example:

```
gap> Gnu(1024);
49487365422
gap> Gnu(3^8);
1396077
gap> GnuWithExplanation(2304);
[ 15756130, "http://dx.doi.org/10.1016/j.jalgebra.2013.09.028" ]
gap> GnuWithExplanation(13^7);
[ 1600573, "using NrSmallGroups from SglPPow 1.1" ]
```

You may try the following to see various cases that may appear:

```
gap> List([105,128,2004,10000,2304,3^8,7^2*5^2*11*19],Gnu);
[ 2, 2328, 10, false, 15756130, 1396077, 8 ]
gap> List([105,128,2004,10000,2304,3^8,7^2*5^2*11*19],GnuWithExplanation);
[ [ 2, "using NrSmallGroups and the GAP Small Groups Library" ], 
  [ 2328, "using NrSmallGroups and the GAP Small Groups Library" ], 
  [ 10, "using NrSmallGroups and the GAP Small Groups Library" ], 
  [ false, "the library of groups of size 10000 is not available" ], 
  [ 15756130, "http://dx.doi.org/10.1016/j.jalgebra.2013.09.028" ], 
  [ 1396077, "using NrSmallGroups from SglPPow 1.1" ], 
  [ 8, "using NumberCFGroups from CubeFree 1.15" ] ]
```

This is work in progress. Latter you will find here details of:
* setting up your own SCSCP server
* using Gnu(n) locally

### Contributing to the database

The file `gnudata.g` contains some precomputed values of Gnu(n) for
orders which are not covered by the GAP Small Groups Library, CubeFree
and SglPPow packages. Contributions providing more values of Gnu(n) 
are welcomed. They may be submitted as issues or pull requests. When 
a new issue or pull request will be created, you will see a template
asking for additional details needed to verify your calculation and
keep provenance of the data.

You may find missing groups, for exampe, by reading `gnuclient.g` into
GAP and then executing the following code:

```
gap> Filtered([2001..2100], n -> Gnu(n)=false);
[ 2024, 2025, 2040, 2048, 2052, 2058, 2064, 2072, 2079, 2080, 2088 ]
```
or, if you wish to see the progress of calculation
```
for n in [2001..2100] do Print(n,"\c"); if Gnu(n)=false then Print("\n"); else Print("\r");fi;od;
```
Some suggestions are published in the `WISHLIST.md` file.

You may generate parts of the submission automatically using the `grpconst.sh` scriopt, for example:

```
$ ./grpconst.sh 50531
****************************************
Constructing all groups of order 50531
****************************************
#I  compute extensions of 5 groups of order 2197
#I  start extending group 1
...
...
****************************************
GAP      4.8.3
CubeFree 1.15
GrpConst 2.5
SglPPow  1.1
Runtime: 411 ms
Isomorphic groups eliminated!
Gnu( 50531 ) = 5
```
Then you will only need to add the description of the computer used for the computation.


