BioJava Changelog
-----------------

BioJava 7.0.1
==============================
### Fixed
* The jar packages now contain pom.properties files #1057
* Some minor improvements in a few biojava-structure methods #1058

BioJava 7.0.0
==============================
### Breaking
* BioJava is now compiled at JDK 11 compatibility level. It will not work anymore under older JREs (e.g. JRE 8). 
This is the main reason for the major version bump.

### Added
* Better handling of downloads: file download validation #1024

### Fixed
* Upgrade to latest ciftools-java, fixes non-US locale issue #1049
* Issue with some edge cases in CIF parsing #1054
* Minor security issue #1046
 

BioJava 6.1.0
==============================
### Added
* GenBankWriterHelper - method that uses the original locus line rather than creating a new one, preventing loss of information
* GenBankReader - the ability to successfully parse GenBank files with a LOCUS ID containing white space
* GenBankReader - the ability to successfully parse GenBank files missing a LOCUS ID
* Aromaticity calculation in biojava-aa-prop module

### Fixed
* GenBankWriter - String Formatter error when key or value of Qualifier has character "%", #886
* GenBankWriter - outputting db_xref feature qualifiers
* GenBankWriter - outputting the accession version and GI ID
* GenBankWriter - outputting feature locations containing joins and partial locations
* GenBankReader - reading locations split over multiple lines
* GenBankReader - set if feature qualifier values require quotes
* Local alignment with linear gap penalty was producing an NPE, #1036
* New default server for PDB files. Note that from Aug 2023 older versions of BioJava will 
not be able to fetch PDB files unless PDB.FILE.SERVER system property is used to override 
the default server
* Dependency upgrades
* Some bug and security fixes

BioJava 6.0.5
==============================
### Fixed
* Null handling in a few places related to loading PDB, CIF, mmtf files (issue introduced in 6.0.0). #1019

BioJava 6.0.4
==============================
### Fixed
* Log4j dependency upgraded to 2.17.1 to avoid new vulnerabilities
* Some new tests
* Fixes in tests and docs
* Bugfix in LocalProteinDomainParser #1009 

BioJava 6.0.3
==============================
### Fixed
* Log4j dependency upgraded to 2.16.0, to avoid log4hshell vulnerability

BioJava 6.0.2
==============================
### Fixed
* Log4j dependency upgraded to 2.15.0, to avoid log4hshell vulnerability
* PDB and mmCIF resolution parsing fixes. #1000

BioJava 6.0.1
==============================
### Fixed
* Now actually runnable in a java-8 JRE. Previous release 6.0.0 had a java-11 dependency that made it incompatible. #996
* Switch JAXB to glassfish implementation for better java 15+ support. Now biojava should run under a java-15 JRE. #996

BioJava 6.0.0 
==============================
Note that BioJava 6.0.0 was intended as java-8 runtime compatible. However, a java-11 dependency crept in making it java-8 incompatible. Please use 6.0.1 instead.
### Removed
* All code related to All-vs-All structural alignments db calculation and access
* JFatCatClient and all code depending on it
* PDP domain providers (depended on JFatCatClient)
* Support for retrieving structure data with prefix "PDP:" (AtomCache, StructureIO)
* RemoteScopInstallation consuming data provided by source.rcsb.org
* The whole `org.biojava.nbio.structure.rcsb` package, a client for the legacy RCSB PDB APIs (disappearing in Nov 2020)
* The whole `org.biojava.nbio.structure.validation` package
* The `org.biojava.nbio.structure.domain.PDBDomainProvider` class to pull domain definitions from legacy RCSB PDB APIs
* Support for automatically fetching dssp files from RCSB (`org.biojava.nbio.structure.secstruc.DSSPParser.fetch()`)
* `org.biojava.nbio.structure.PDBStatus`: simplified `Status` enum to 3 states, with OBSOLETE now called REMOVED
* `org.biojava.nbio.structure.PDBStatus`: removed `getReplacement` and `getReplaces` 
* Removed `org.biojava.nbio.structure.io.mmcif` package
* Removed functionality to write isolated CIF headers from `FileConvert`
* Removed `org.biojava.nbio.structure.io.mmtf.MmtfUtils.setUpBioJava()`
* Removed from `org.biojava.nbio.structure.Chain` interface: `getParent()`, `setParent()`, `getAtomLigands()`, `getSwissprotId()`, `setSwissprotId()`, `getInternalChainID()`, `setInternalChainID()`, `getChainID()`, `setChainID()`
* Removed from `org.biojava.nbio.structure.Structure` interface: `findChain()`, `getId()`, `setId()`, `getChainByPDB()`, `getCompoundById()`, `getResidueRanges()`, `getRanges()`
* Removed from `org.biojava.nbio.structure.StructureTools` : `isNucleicAcid()`, `isProtein()`, `getPredominantGroupType()`, `isChainWaterOnly()`, `isChainPureNonPolymer()`, `getReducedStructure()`
* Removed `org.biojava.nbio.structure.io.SandboxStyleStructureProvider`
* In `org.biojava.nbio.structure.align.xml.MultipleAlignmentXMLParser` made all methods private except `parseXMLfile`

### Breaking API changes
* Extracted `StructureIO.StructureFiletype` enum to `org.biojava.nbio.structure.io.StructureFiletype` (supports `PDB`, `MMTF`, `CIF`, and `BCIF`)
* `org.biojava.nbio.structure.align.util.AtomCache`: removed `setUseMmCif`, `setUseMmtf`, `isUseMmCif`, and `isUseMmtf` - replaced by `setFiletype` and `getFiletype` that controls parsed content via the `StructureFiletype`
* `org.biojava.nbio.structure.io.MMCIFFileReader` is now effectively `org.biojava.nbio.structure.io.CifFileReader`
* Moved `org.biojava.nbio.structure.io.mmcif.model.DatabasePdbrevRecord` to `org.biojava.nbio.structure.DatabasePDBRevRecord.java`
* Moved all chem-comp model classes from `org.biojava.nbio.structure.io.mmcif.chem` to `org.biojava.nbio.structure.chem`
* Moved all chem-comp parsing classes from `org.biojava.nbio.structure.io.mmcif.chem` to `org.biojava.nbio.structure.io.cif`
* Moved classes in `org.biojava.nbio.structure.io.mmcif` to `org.biojava.nbio.structure.chem`
* Fixed `CRC64Checksum#public void update(byte[] b, int offset, int length)` to use the `length` argument correctly as specified in `java.util.zip.Checksum` interface.
* In `SubstructureIdentifier`, `StructureName`, `EcodDomain`, `ScopDomain` : `getPdbId()` returns `PdbId` object instead of `String`.
* Removed `DownloadChemCompProvider.useDefaultUrlLayout` with a more flexible system to provide templated URLs `DownloadChemCompProvider.setChemCompPathUrlTemplate()` and `DownloadChemCompProvider.setServerBaseUrl()`
* In `Structure` (and `StructureImple`), the accessor methods `String getPdbId()` and `setPdbId(String)` were previously depricated. They were revived in BioJava 6.0.0 but as `PdbId getPdbId()` and `setPdbId(PdbId)` instead.n
* `GeneSequence#public ExonSequence addExon(AccessionID accession, int begin, int end)` no longer declares a checked exception #966

### Added
* New `keywords` field in `PDBHeader` class, populated by PDB and mmCIF parsers #946
* OBO parsing now supports multiple altids, #960
* New class `PdbId` that wrapps a PDB Identifier and handles conversion between current short PDBID format and upcoming extended PDBID format #930

### Fixed
* Correct chain assignment to entities when parsing PDB/mmCIF without entity information (in cases with more than 3 chains per entity) #931
* Dealing with chain ids correctly when parsing bonds in PDB-format files #943 #929

BioJava 5.4.0
=============
### Added
* Minimal read support for mmCIF files with branched entities (upcoming PDB release July 2020). The new entity type is understood now but branched entities are still treated as non-polymers within BioJava. #868
* InterfaceFinder class to find interfaces of a given PDB assembly #867
* New switch in Subunit clusterer `useEntityIdForSeqIdentityDetermination` #857 #859

### Changed
* Now genbank parser will allow 5'<3' for circular DNA #855

### Fixed
* Issue in bonds between atoms of different alt locs (https://github.com/rcsb/mmtf/issues/44) #854
* Upgrade ciftools-java dependency to latest java-8 compatible release 0.7.1

BioJava 5.3.0
=============
### New features
* Support for reading structures from [binary cif format](https://github.com/dsehnal/BinaryCIF) in structure module, thanks to  [CIFTools-java library](https://github.com/rcsb/ciftools-java). Thanks @JonStargaryen 
* Reading structures from mmCIF via new parser from [CIFTools-java library](https://github.com/rcsb/ciftools-java). Much better read performance than existing parser. Both parsers still live alongside in BioJava 5.3.0, with default still being the BioJava native one. 

### Bug fixes
* 8x performance increase in reading (non-gzipped) MMTF files thanks to [mmtf-java](https://github.com/rcsb/mmtf-java) dependency upgrade to 1.0.9
* Bug fixes in MMTF file reading, #671 #850 
* Bug fix in OBO reading, where only one synonym was saved, #836 
* Bug fix in Genbank LOCUS line parsing #833 
* Bug fix in PDB file reading of MTRIX records #845 
* Bug fix in GenbankReader #800 #829 
* Dependencies and maven plugin upgrades

BioJava 7.0.0
==============================
### Breaking
* BioJava is now compiled at JDK 11 level. It will not work anymore under older JREs (e.g. JRE 8). 
This is the main reason for the major bump.

### Added
* Better handling of partial downloads 

### Fixed
* Issue with some edge cases in CIF parsing: #1054
 

BioJava 5.2.1
=============
### Bug fixes

* 2 bugfixes in ASA calculation introduced in 5.2.0: calculation would fail when an atom had no neighbors or when supplying a 0-length atom array, #824
* Fixes in bioassembly creation, where EntityInfo objects weren't correctly cloned and wired #825
* More efficient interface ASA calculation in NCS cases #823
* Add EntityInfo to reduced structure #822 

BioJava 5.2.0
=============

### New Feature
* new algorithm for ASA computation. It is much faster on large molecules. #820

### Bug fixes
* Fix broken tests #809 & ed7fb66
* Add tests for new GenBank formats (confirmed that the parser worked) #811
* Fix exceptions displaying CE-Symm results #816 #817
* Merge in bug fixes from the 4.2.x series (4.2.6 through 4.2.12)

BioJava 5.1.1
=============
Note this is the first version of BioJava that will run under 9, 10 or 11 JREs. It is still fully compatible with Java 8.

### Bug fixes
* Chain identifiers for generated bioassemblies now more explicit, #801
* Adapted BioJava to run under 9, 10 and 111 JREs, #804 

BioJava 5.1.0
=============
### New feature
* ABI tracer ported from legacy biojava, #769, thanks @MaxGreil

### Bug fixes
* Performance improvement for secondary structure calculation, #789
* Fixed issue #731
* Improved alt locs docs and some fixes, #778
* Jmol dep updated to 14.29.17
* Fixed issue #712
* Fixed issue #791
* Fixed issue #797
* Fixed issue #784

BioJava 5.0.2
=============
### Bug fixes
* Fixed issue #770
* Upgraded to latest mmtf-java 1.0.8

BioJava 5.0.1
=============
### Bug fixes
* Fixed issue #767
* Fixed issue #761
* Pom fixes for mvn site
* Some logging fixes

BioJava 5.0.0
=============

This release contains [1,170 commits](https://github.com/biojava/biojava/compare/biojava-4.2.11...biojava-5.0.0) from 19 contributors.

Requires Java 8 or newer.

### New features

#### biojava-alignment
* New utlity methods for sequence alignment objects (gap, similarity and coverage).

#### biojava-structure
* The data structures to represent 3D macromolecules now follow the mmCIF data model.
* [MMTF format](http://mmtf.rcsb.org/) support.
* Symmetry detection algorithms overhaul: better symmetry detection for tertiary and quaternary structure levels.
* New method and data structures for the clustering of protein subunits at the sequence and structure levels.
* New method to align biological assemblies, see `org.biojava.nbio.structure.align.quaternary.QsAlign`.
* New algorithms for base-pair geometry in nucleic acids.
* New SuperPosition interface for different 3D-structure superposition algorithms, see `org.biojava.nbio.structure.geometry.SuperPosition`.
* Geometry-related API now more consistently based on vecmath interfaces.

### Changed
* For short structure selections (e.g. 1abc.A:1-100), ligands within 5A will be included
* Symmetry expansion for bioassembly creation is now by default happening via adding new chains instead of new models. 
* Make objects serializable for compatibility with big data frameworks (e.g. Spark).

### Breaking API changes

* module biojava-phylo merged into biojava-alignment. The package namespace stays the same (`org.biojava.nbio.phylo`).
* module biojava-sequencing merged into biojava-genome. Package `org.biojava.nbio.sequencing.io.fastq` is now `org.biojava.nbio.genome.io.fastq`
* `org.biojava.nbio.structure.Compound` -> `org.biojava.nbio.structure.EntityInfo`
* `org.biojava.nbio.structure.io.util.FileDownloadUtils` -> `org.biojava.nbio.core.util.FileDownloadUtils`
* `org.biojava.nbio.structure.symmetry.core.AxisAligner` -> `org.biojava.nbio.structure.symmetry.axis.AxisAligner`
* `org.biojava.nbio.structure.symmetry.core.Subunits` -> refactored into several classes in `org.biojava.nbio.structure.cluster`: Subunit, SubunitCluster, SubunitClusterer
* `org.biojava.nbio.structure.align.helper.AlignTools` -> `org.biojava.nbio.structure.align.helper.AlignUtils`
* All deprecations introduced in 4.0.0 or before were removed.

### General

* Javadocs improvements across the board.
* All tests are now Junit4.
* Updated dependency versions (guava, slf4j, and log4j).

### Bug fixes
A very long list.

BioJava 4.2.11
==============

release date: January 11th 2018
This release contains [3](https://github.com/biojava/biojava/compare/biojava-4.2.10...biojava-4.2.11) commits from 1 contributor.

### Bug fixes
- Updated hmmer scan web service URL to https.

BioJava 4.2.10
==============

release date: December 11th 2017
This release contains [7](https://github.com/biojava/biojava/compare/biojava-4.2.9...biojava-4.2.10) commits from 2 contributors.

### Bug fixes
- Fixed issue #659
- Fixed issue #715

BioJava 4.2.9
=============

release date: October 19th 2017
This release contains [15](https://github.com/biojava/biojava/compare/biojava-4.2.8...biojava-4.2.9) commits from 2 contributors.

### Bug fixes
- Some fixes to PDB file parsing CONECT/LINK records
- Updated URLs for external resources


BioJava 4.2.8
=============

release date: July 6th 2017
This release contains [15](https://github.com/biojava/biojava/compare/biojava-4.2.7...biojava-4.2.8) commits from 3 contributors.

### Bug fixes
- Small additions to AlignedSequence in core module to better support pipelines that use 4.2.x
- URLs adapted to latest RCSB PDB convention #682

BioJava 4.2.7
=============

release date: March 7th 2017
This release contains [8](https://github.com/biojava/biojava/compare/biojava-4.2.6...biojava-4.2.7) commits from 4 contributors.

### Bug fixes
- Fix for hmmer web service in biojava-ws #640
- Fix in chromosome mapping tool #636

BioJava 4.2.6
=============

release date: February 17th 2017
This release contains [12](https://github.com/biojava/biojava/compare/biojava-4.2.5...biojava-4.2.6) commits from 4 contributors.

### Bug fixes
* Fix for problem in chain cloning, #631
* Several bug fixes and better error check in quaternary symmetry detection code

BioJava 4.2.5
=============

release date: December 7th 2016
This release contains [30](https://github.com/biojava/biojava/compare/biojava-4.2.4...biojava-4.2.5) commits from 7 contributors.

### Bug fixes

* Fix for new phosphositeplus.org format, #610
* org.biojava.nbio.genome.parsers.gff.Location union() and intersect() now work correctly, #355
* Minor addition of crystallographic metadata fields to handle legacy PDB entries
* Jmol interchange format is now mmCIF, allowing for multiletter chain ids
* Update to latest jmol 14.6.2_2016.08.28
* A few minor bug fixes

BioJava 4.2.4
=============

release date: July 29th 2016
This release contains over [17](https://github.com/biojava/biojava/compare/biojava-4.2.3...biojava-4.2.4) commits from 4 contributors.

### Bug fixes

* NCBI links now using https (see [NCBI's announcement](http://www.ncbi.nlm.nih.gov/news/06-10-2016-ncbi-https/) )
* CATH links redirected to new server http://release.cathdb.info/
* SCOP default location now points to the Berkeley server after demise of Scop at MRC LMB
* Fixed important bug in mmCIF writing where structures with multiple models were written with identical coordinates
* Fixed bug in Group cloning where chemical components weren't cloned
* Added utility class for Chromosome mapping

BioJava 4.2.3
=============

release date: July 28th 2016
This release contains over [13](https://github.com/biojava/biojava/compare/biojava-4.2.2...biojava-4.2.3) commits from 2 contributors.

### Bug fixes

* mmCIF file writing: special fields (e.g. containing hyphens) are now correctly written
* General improvements in mmCIF file read and write

BioJava 4.2.2
=============

release date: June 14th 2016
This release contains over [31](https://github.com/biojava/biojava/compare/biojava-4.2.1...biojava-4.2.2) commits from 5 contributors.

### Bug fixes

This is a bug-fix release

* CE-Symm features and bug fixes
 - Better data structures for symmetry axes (particularly for hierarchical symmetry)
 - Fix bug with symmetry axis positioning
 - Optimization includes all symmetry repeats for hierarchical symmetry
* Update of protein modifications to latest version,
  - including new glycans and chromophores
  - Updating naming definitions to latest conventions


BioJava 4.2.1
=============

release date: May 3rd 2016
This release contains over [31](https://github.com/biojava/biojava/compare/biojava-4.2.0...biojava-4.2.1) commits from 7 contributors.

### Bug fixes

Biojava-structure

- Nucleotide bonds are now generated
- BIO: identifiers are now correctly handled
- Several fixes for CE-Symm
- Substructures now contain seqres groups (isse #449)
- Structures containing insertion codes are now written correctly to mmCIF
- AtomCache now uses the correct default parsing parameters (issue #455)
- Fixed problem with some atom charges that weren't being added
- CATH updated to 4.0.0
- Better ECOD javadocs (issue #452)

Biojava-structure-gui
- Removed javaws dependency (issue #459)

BioJava 4.2.0
=============

release date: March 10th 2016

This release contains over [750](https://github.com/biojava/biojava/compare/6f8d796fee92edbbcd001c33cdae4f15c5480741...biojava-4.2.0) commits from 16 contributors.

BioJava 4.2.0 offers many new features, as well several bug-fixes.

-   Requires Java 7
-   Better logging with SLF4J

### New Features

#### biojava-core

-   New SearchIO framework including blast xml parser

#### biojava-structure

-   Secondary structure assignment (DSSP compatible)
-   Multiple Structure Alignments
    -   New MultipleStructureAlignment datastructure supporting flexible and order-independent alignments
    -   MultipleMC algorithm
        - Can use any pairwise StructureAlignment implementation
    -   serialize and parse multiple structure alignments as XML files, output as Text, FatCat, FASTA, Rotation Matrices, etc.
-   More complete mmCIF and cif parsing
    -   Parse bonds, sites, charges
    -   Better support for non-deposited pdb and mmcif files
-   Include CE-Symm algorithm for finding internal symmetry (Myers-Turnbull, 2014)
-   Replaced internal graph datastructures with Jgraph
-   Unified StructureIdentifier framework
-   Improved chemical component framework, now by default providing full chemical description by using DownloadChemCompProvider
-   Optimised memory usage of Residue/Atoms

#### biojava-structure-gui  

-   MultipleAlignmentGUI for visualizing Multiple Structure Alignments with Jmol
-   SymmetryDisplay for visualizing internal symmetry

#### biojava-phylo  

-   Use `Forester 1.038`
-   Significant bug fixes
-   use `SubstitutionMatrices` in the core module (instead of imported Jalview matrices)
-   use `Sequence` and `Compound` classes from the alignment module
-   provide some Wrapper methods to communicate with forester
-   decouple distance matrix calculation from tree constructor
-   provide methods for common distance matrix calculations and framework for user-defined distances
-   update the forester version to have the correct NJ tree constructor
-   correct some of the tree evaluator statistics.

BioJava 4.1.0
=============

release date: June 24th 2015

### New Features:

- New algorithm for multiple structure alignments
- Improved visualization of structural alignments in Jmol
- Support for the ECOD protein classification
- Better mmCIF support: limited write support, better parsing

BioJava 4.0.0
=============

release date: January 30th 2015

### New Features:

- General
  - Consistent error logging. SLF4J is used for logging and provides adaptors for all major 
  logging implementations. (many contributors, including @benjamintboyle and @josemduarte)
  - Improved handling of exceptions (@dmyersturnbull)
  - Removed deprecated methods
  - Expanded the BioJava tutorial (@andreasprlic, @josemduarte, and @sbliven)
  - Updated dependencies where applicable
  - Available on Maven Central (@andreasprlic and @heuermh)
- biojava3-core
  - Improved Genbank parser, including support for feature records, qualifiers, and nested 
  locations. (@paolopavan and @jgrzebyta)
- biojava3-structure
  - Better support for crystallographic information, including crystallographic operators, 
  unit cells, and protein-protein interfaces. (@josemduarte)
  - Better organization of downloaded structure files (set using the PDB_DIR and PDB_CACHE_DIR 
  environmental variables) (@sbliven)
  - Better command-line tools for structure alignment (@sbliven)
  - New algorithm for symmetry detection in biological assemblies (@pwrose)
  - New algorithm for fast contact calculation, both intra-chain and inter-chain (@josemduarte)
  - Support for Accessible Surface Area (ASA) calculation through and implementation of 
  the Shrake & Rupley algorithm, both single-thread and parallel (memory permitting) (@josemduarte)
  - Support for large structures (memory permitting) and multi-character chain IDs.
  - Default to mmCIF file format, as recommended by the wwPDB

This version is compatible with Java 6, 7, and 8.

### Upgrading 
Since we renamed all package names to be consistent across the whole project, 
there will be import errors when upgrading to this version. These can automatically get resolved 
by IDEs such as Eclipse or IntelliJ by selecting the Optimize Import menu item.

BioJava 3.1.0
=============

release date: August 25th 2014

While most development is going towards the upcoming 4.0.0 release, this release provides 
bug fixes and a few new features:

- CE-CP version 1.4, with additional parameters
- Update to SCOPe 2.04
- Improvements in FASTQ parsing
- Fix bugs in PDB parsing
- Minor fixes in structure alignments

This version is compatible with Java 6 and 7.

BioJava 3.0.8
=============

release date: March 25th 2014

New Features:

- New Genbank writer
- New parser for Karyotype file from UCSC
- New parser for Gene locations from UCSC
- New parser for Gene names file from genenames.org
- New module for Cox regression code for survival analysis
- New calculation of accessible surface area (ASA)
- New module for parsing .OBO files (ontologies)
- Improved representation of SCOP and Berkeley-SCOP classifications
 
BioJava 3.0.7
=============

release date: September 23rd 2013

New features:

- added a basic genbank parser 
- fixed a problem when translating codons with N 
- now can infer bonds in protein structures 
- added support to parse mmcif records for organism and expression system 
- many small bug fixes and improvements
