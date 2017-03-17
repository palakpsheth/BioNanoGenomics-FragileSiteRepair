# BioNanoGenomics-FragileSiteRepair
Scripts for repairing (stitching) across fragile sites breaks in BioNano genome maps.

```
Usage: perl fragileSiteRepair.pl [OPTIONS] --fasta <ref.fa> --cmap <assembly.cmap> --output <output folder>
```
``` 
=== REQUIRED ===
--fasta <.fasta> : input reference FASTA file to be used for fragile site prediction and alignment
--cmap <.cmap> : input BioNano assembly CMAP to perform fragileSiteRepair on
--output <path/to/output> : output folder

=== KEY OPTIONS ===
--bnx <.bnx> : input BNX used for assembly. Requires --err to perform single molecule alignments. Default: NONE
--alignmolDir <path/to/assembly/output/contigs/exp_refineFinal1/alignmol/merge> : directory containing molecules aligned to assembly maps XMAPs and ERR file. Requires --bnx to perform single molecule alignment scoring. Default: NONE  
--bam <.bam> : BAM file of NGS scaffolds/contigs/reads aligned to --fasta. Used to supplement scoring of stitchPositions. Default: NONE

--break : Flag to enable breaking of maps at stitchPositions with score less than threshold value. Requires --bnx and --err. Default: OFF
--runSV : Flag to run SV module on final cmap. Default: OFF
--gaps  : N-base gaps BED file 
--force : Flag to overwrite --output folder. Default: OFF
--aggressive : Flag to calculate TypeIII and TypeIV fragile sites in addition. Default: OFF

=== OTHER OPTIONS ===
--enzyme <nickase sequence> : Nickase enzyme for in-silico digestion and fragile site prediction. Default: GCTCTTC
--ngsBuffer <basepairs> : Number of basepairs that a single NGS alignment must extend past the ends of a stitchPosition to supplement score. Default: 500
--ngsBonus <raw score value> : Score bonus for each NGS alignment supporting a fragile site repair [NOTE: see documentation for details on raw scoring for BIoNano molecules]. Default: 10
--breakNGSonly : Flag to break maps at stitchPositions that have only NGS alignment support and no BioNano single molecule support. Default: OFF
--threshold <scaled score> : Minimum stitchPoisitons scaled score below which to break maps. Scaled score=Raw Score/10. Default: 1.0
--maxlab <label count> : Maximum number of reference labels to allow between adjacent maps. Default: 1
--maxfill <basepairs> : Maximum number of basepairs to allow between adjacent maps. Default: 30000
--wobble <basepairs> : Maximum number of basepairs to allow the fragile site to vary. Default: 30000
--seq <basepairs> : Number of basepairs of reference sequence +/- predicted fragile site to output into BED file. Default: OFF
--optArgs <optArguments.xml> : optArguments.xml to use for alignment. Default: ~/scripts/optArguments_human.xml
--endoutlier <pvalue> : endoutlier penalty for single molecule alignments (see RefAligner -help for more info). Default: 1e-5
--minRatio <ratio> : minimum ratio of single molecule alignments that must end at genome maps ends to be classified as a potential fragile site. Requires --alignmolDir. Default: 0.70
--maxOverlap <bp> : maximum number of basepairs overlap between maps to allow merge. Default: 20000
--maxOverlapLabels <int labels> : maximum number of labels overlap between maps to allow merge. Default: 5

--n <CPU cores> : Maximum number of CPU cores/threads to use. Default: nproc
--j <number jobs> : Maximum number of parallel jobs. Default: nproc/6

--h : Display this help menu
```

###Installing dependencies:
1. Install BioNano Genomics de-novo assembly Linux pipeline in user's home directory (~/scripts and ~/tools) according to [instructions](http://www.bnxinstall.com/training/docs/IrysViewSoftwareInstallationGuide.pdf)  
2. login as root [or use sudo]
3. `yum install perl-CPAN.noarch perl-Config-Tiny.noarch perl-Data-Dumper.x86_64 perl-ExtUtils-CBuilder.noarch perl-ExtUtils-ParseXS.noarch perl-HTML-Parser.x86_64 perl-IPC-Run.noarch perl-Module-Build.noarch perl-Module-CoreList.noarch perl-Module-Install.noarch perl-Module-Metadata.noarch perl-Sys-MemInfo.x86_64 perl-TimeDate.noarch perl-XML-Parser.x86_64 perl-XML-Simple.noarch perl-devel.x86_64 perl-local-lib.noarch perl-threads.x86_64 perl-threads-shared.x86_64 perl-App-cpanminus.noarch perl-CPAN-Meta.noarch perl-Config-Simple.noarch perl-DateTime.x86_64 perl-ExtUtils-Install.noarch perl-ExtUtils-MakeMaker.noarch perl-ExtUtils-Manifest.noarch perl-File-Copy-Recursive.noarch perl-File-Path.noarch perl-File-Slurp.noarch perl-IPC-Run3.noarch perl-Sys-CPU.x86_64 perl-Time-HiRes.x86_64 perl-YAML.noarch`
4. `cpanm install -f IPC::System::Simple Sys::Info Bio::Seq Parallel::ForkManager DateTime::Format::Human::Duration PerlIO::Util JSON::XS File::Tee Statistics::Lite`
5. Install Bio-SamTools per https://github.com/gitpan/Bio-SamTools
  1. `git clone https://github.com/gitpan/Bio-SamTools.git ./Bio-SamTools`
  2. `cd Bio-SamTools`
  3. `perl INSTALL.pl`

