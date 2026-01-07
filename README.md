# Simulation Exploration Experience (SEE) Starter Project

This repository was created to ease getting started with developing HLA federates in the [Simulation Exploration Experience (SEE)](https://www.simulationexplorationexperience.org) program.
It is intended to be used with the [SEE HLA Starter Kit](https://www.github.com/atreia108/SEE-HLA-Starter-Kit) and includes a basic federate, math and geometry utility libraries, pre-implemented SpaceFOM object, interaction, and encoding classes set up for university teams to modify as needed for their simulations.

## Project Structure

The project is organized as follows:


```
.
├── src/main/java/
│   └── org.see.baseplate/
│       ├── encoding/
│       │   └── ...
│       ├── models/
│       │   ├── objects/
│       │   │   └── ...
│       │   └── interactions/
│       │       └── ...
│       ├── types/
│       │   └── ...
│       └── ExampleFederate.java
└── src/main/resources/
    ├── foms/
    ├── example_federate.conf
    └── logback.xml
```


The `org.see.baseplate` is the root for all project code.
Subfolders are structured with the following intent:
* `encoding`: Coder classes that decode and encode HLA data types to and from their native Java equivalents.
* `models`: Java class definitions of HLA object and interaction classes mentioned in the SpaceFOM.
It includes just about everything covered in v1.0 of the standard.
* `types`: Encapsulations of the data types defined in the SpaceFOM.

Every SEE HLA Starter Kit simulation requires a configuration file `*.conf` to start, present in the `resources` directory.
This **must be changed** to match your test environment. If you build any FOM module extensions to the SpaceFOM, place them in the `foms` directory and uncomment the `fom_directory` parameter in the configuration. (*N.B. The FOMs can be placed elsewhere, this is just how it has been set up by default*.)

The `resources` directory can also be used to place other resource files needed by your simulation,
such as the `logback.xml` file which can be used to interactively log simulation data to the console.

> [!NOTE]
> This project structure can be freely modified to suit different requirements, 
the primary goal is to have it serve as a good starting point for developing HLA federates in SEE.

## Setting Up

First, clone the repository to your machine.
The project uses Maven as its build system, therefore most dependencies should be resolved as soon as you load it into your IDE of choice (tested on Eclipse and IntelliJ).
The Pitch pRTI libraries, however, are not publicly hosted so you have two options:

1. Link the pRTI HLA 4 JAR libraries from the program's installation directory (typically located in `C:\Program Files\Pitch pRTI 6\libs` on Windows) to the project in your IDE. 
If you choose this option, comment out or remove the pRTI dependency declarations in `pom.xml`.
2. Generate the dependencies as third-party JARs
in your local Maven repository. See below for instructions.

The same rule applies to the SEE HLA Starter Kit dependency. Either download a release version from [here](https://www.github.com/atreia108/SEE-HLA-Starter-Kit/releases) and link it to your project manually or generate it as a third-party JAR in your local Maven repository. 

### Generating Dependencies for Local Maven Repository

A third-party JAR library for Maven can be generated as follows:

* Navigate to the root of your cloned version of this repository. Make sure that `pom.xml` is in the same directory level or this won't work (can be verified  by quickly running `dir` on Windows or `ls` on macOS/Linux).
* There are three main dependencies we want: `prti1516-hla4.jar`, `prticore.jar`, and `booster1516.jar`. Run the following commands to generate the dependencies (**remember to enter the path to the JAR files for `-Dfile` and set `-Dversion` to the pRTI version you have installed**):

```shell
$ mvn install install:file -Dfile="<PATH-TO-JAR-FILE>" -DgroupId="se.pitch" -DartifactId="prti1516-hla4" -Dversion="6.1.0" -Dpackaging=jar
$ mvn install install:file -Dfile="<PATH-TO-JAR-FILE>" -DgroupId="se.pitch" -DartifactId="prticore" -Dversion="6.1.0" -Dpackaging=jar
$ mvn install install:file -Dfile="<PATH-TO-JAR-FILE>" -DgroupId="se.pitch" -DartifactId="booster1516" -Dversion="6.1.0" -Dpackaging=jar
```

* If you want to use the SEE HLA Starter Kit as a Maven dependency as well, then download the latest release from GitHub and run the following command as before (**remember to set the correct pathname and version number**):

```shell
$ mvn install install:file -Dfile="<PATH-TO-JAR-FILE>" -DgroupId="org.see" -DartifactId="skf" -Dversion="2.0.0" -Dpackaging=jar
```