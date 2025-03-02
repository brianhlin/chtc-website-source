---
highlighter: none
layout: guide
title: Use Custom Linux Versions in CHTC
guide:
    category: Special use cases
    tag:
        - htc
--- 


By default, CHTC-managed submit servers automatically add a job 
requirement that requires jobs to run on servers running our primary operating system unless otherwise specified by the user. There are two options to override this
default: 

1. [Using a Container (recommended)](#option-1-using-a-container-recommended)
1. [Requesting a Specific
Operating System](#option-2-requesting-a-specific-operating-system).

## Option 1: Using a Container (Recommended)

Using a container to provide a base version of Linux will allow you to 
run on any nodes in the HTC system, and not limit you to a subset of nodes. 

After finding a container with the desired version of Linux, just follow our instructions 
for [Docker](docker-jobs.html) or [Singularity/Apptainer](apptainer-htc.html) jobs. 

Note that the default Linux containers on Docker Hub are often missing commonly installed 
packages. Our collaborators in OSG Services maintain a few curated containers with a 
greater selection of installed tools that 
can be seen here: [Base Linux Containers](https://portal.osg-htc.org/documentation/htc_workloads/using_software/available-containers-list/#base)

## Option 2: Requesting a Specific Operating System

At any time, you can require a specific operating system 
version (or versions) for your jobs. This option is more limiting because 
you are restricted to operating systems used by CHTC, and the number of nodes 
running that operating system. 

### Require CentOS Stream 8 (previous default) or CentOS Stream 9

To request that your jobs run on servers with CentOS 8 **only**, add the
following line to your submit file:

``` {.sub}
chtc_want_el8 = true
```

To request that your jobs run on servers with CentOS 9 **only**, add 
the following line to your submit file: 

``` {.sub}
chtc_want_el9 = true 
```

> Note that after May 1, 2024, CentOS9 will be the default and CentOS8 will be phased out 
> by the middle of summer 2024. If you think your code relies on CentOS8, make sure to 
> see our [transition guide](htc-el8-to-el9-transition.html) or talk to the facilitation 
> team about a long-term strategy for running your work. 

### Use Both CentOS Stream 8 (previous default) and CentOS Stream 9 (current default)

To request that your jobs run on computers running **either** version of 
CentOS Linux, add the following requirements line to your submit file:

``` {.sub}
requirements = (OpSysMajorVer == 8) || (OpSysMajorVer == 9)
```
> Note: these requirements are not necessary for jobs that use Docker containers; 
> these jobs will run on servers with any operating system automatically. 

The advantage of this option is that you may be able to access a
larger number of computers in CHTC. Note that code compiled on a
newer version of Linux may not run older versions of Linux. Make
sure to test your jobs specifically on both CentOS Stream 8 and CentOS Stream 9
before using the option above.

Does your job already have a requirements statement? If so, you can
add the requirements above to the pre-existing requirements by using
the characters `&&`. For example, if your jobs already require large
data staging:

``` {.submit}
requirements = (Target.HasCHTCStaging == true) 
```

You can add the requirements for using both operating system versions like so: 

``` {.sub}
requirements = (Target.HasCHTCStaging == true) && ((OpSysMajorVer == 8) || (OpSysMajorVer == 9))
```

