ProcessTracker - The Framework
##############################

Why ProcessTracker?
*******************
Data integration tools and frameworks do not have a universal way to work together, track processes and
interdependencies, and the data those processes utilize.  While an enterprise scheduler can help with dependency
management, it won't necessarily help with a lot of the other auditing and process management.  Enter ProcessTracker.

ProcessTracker is a framework for managing data integration processes and their data in a central place, with the goal
being to be as tool agnostic as possible so that it can be used in any data integration environment, regardless of
tool stack.  As long as the ProcessTracker methodology is followed, it can be adapted to be used by any data integration tool or
data driven process - from ETL/ELT tools to tools like R, Spark, and beyond.

It it our hope that other implementations will be released over time for the various tools/languages that are used.  As
we either build them, or find out about them, we will list them below.

ProcessTracker and ExtractTracker
*********************************
There are two primary functions of the ProcessTracker framework.  ProcessTracker manages and audits data integration
processes while ExtractTracker manages and audits the data extracts that are utilized between processes.  For tool
implementations the same concepts apply even though they are not implemented in quite the same way as the code package
implementations.