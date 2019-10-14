## Cancer Predisposition Sequencing Reporter (CPSR)

### Overview

The *Cancer Predisposition Sequencing Reporter (CPSR)* is a computational workflow that **interprets germline variants** identified from next-generation sequencing **in the context of cancer predisposition**. The workflow is integrated with the framework that underlies the [Personal Cancer Genome Reporter (PCGR)](https://github.com/sigven/pcgr), utilizing the Docker environment for encapsulation of code and software dependencies. While *PCGR* is intended for reporting and analysis of somatic variants detected in a tumor, *CPSR* is intended for reporting and ranking of germline variants in protein-coding genes that are implicated in cancer predisposition and inherited cancer syndromes.

*CPSR* accepts a query file with raw germline variant calls encoded in the [VCF](https://samtools.github.io/hts-specs/VCFv4.2.pdf) format (i.e. analyzing SNVs/InDels). Furthermore, through the use several different _virtual cancer predisposition gene panels_ harvested from the [Genomics England PanelApp](https://panelapp.genomicsengland.co.uk/), the user can flexibly put a restriction on which genes and findings are displayed in the cancer predisposition report.

The software performs extensive variant annotation on the selected geneset and produces an interactive HTML report, in which the user can investigate four types of variants:

1. __ClinVar variants__ - pre-classified variants according to a five-level tier scheme in ClinVar (Pathogenic to Benign)
2. __Non-ClinVar variants__ - classified by CPSR through ACMG criteria (variant frequency levels and functional effects) into to a five-level tier scheme (Pathogenic to Benign)
3. __Secondary findings (optional)__ - pathogenic ClinVar variants in the [ACMG recommended list for reporting of incidental findings](https://www.ncbi.nlm.nih.gov/clinvar/docs/acmg/)
4. __GWAS hits (optional)__ - variants overlapping with previously identified hits in genome-wide association studies (GWAS) of cancer phenotypes (i.e. low to moderate risk conferring alleles), using [NHGRI-EBI Catalog of published genome-wide association studies](https://www.ebi.ac.uk/gwas/) as the underlying source.

The variant sets can be interactively explored and filtered further through different types of filters (phenotypes, genes, variant consequences, population MAF etc.). Importantly, the unclassified non-ClinVar variants are assigned a *pathogenicity score* based on the aggregation of scores according to previously established [ACMG criteria](https://www.ncbi.nlm.nih.gov/pubmed/25741868). The ACMG criteria includes cancer-specific criteria, as outlined and specified in several previous studies ([Huang et al., *Cell*, 2018](https://www.ncbi.nlm.nih.gov/pubmed/29625052); [Nykamp et al., *Genet Med.*, 2017](https://www.ncbi.nlm.nih.gov/pubmed/28492532); [Maxwell et al., *Am J Hum Genet.*, 2016](https://www.ncbi.nlm.nih.gov/pubmed/27153395); [Amendola et al., *Am J Hum Genet.*,  2016](https://www.ncbi.nlm.nih.gov/pubmed/27181684)). See also [*Related work*](https://github.com/sigven/cpsr#related-work) below).

##### Cancer predisposition genes

The cancer predisposition report can show variants found in a number of well-known cancer predisposition genes, and the specific set of genes can be customized by the user by choosing any of the following __virtual gene panels (0 - 36)__:

  * **Panel 0 (default)** is a comprehensive, research-based gene panel assembled through known sources on cancer predisposition:
	* A list of 152 genes that were curated and established within TCGA’s pan-cancer study ([Huang et al., *Cell*, 2018](https://www.ncbi.nlm.nih.gov/pubmed/29625052))
	* A list of 107 protein-coding genes that has been manually curated in COSMIC’s [Cancer Gene Census v90](https://cancer.sanger.ac.uk/census),
	* A list of 148 protein-coding genes established by experts within the Norwegian Cancer Genomics Consortium (http://cancergenomics.no)

	The combination of the three sources resulted in a non-redundant set of [213 protein-coding genes](https://github.com/sigven/cpsr/blob/master/predisposition.md) of relevance for predisposition to tumor development.

  * **Panels 1 - 36** are panels for inherited cancer syndromes and cancer predisposition assembled within the [Genomics England PanelApp](https://panelapp.genomicsengland.co.uk/):
	  * [1 = Adult solid tumours cancer susceptibility](https://panelapp.genomicsengland.co.uk/panels/245/)
	  * [2 = Adult solid tumours for rare disease](https://panelapp.genomicsengland.co.uk/panels/391/)
	  * [3 = Bladder cancer pertinent cancer susceptibility](https://panelapp.genomicsengland.co.uk/panels/208/)
	  * [4 = Brain cancer pertinent cancer susceptibility](https://panelapp.genomicsengland.co.uk/panels/166/)
	  * [5 = Breast cancer pertinent cancer susceptibility](https://panelapp.genomicsengland.co.uk/panels/55/)
	  * [6 = Childhood solid tumours cancer susceptibility](https://panelapp.genomicsengland.co.uk/panels/259/)
	  * [7 = Colorectal cancer pertinent cancer susceptibility](https://panelapp.genomicsengland.co.uk/panels/244/)
	  * [8 = Endometrial cancer pertinent cancer susceptibility](https://panelapp.genomicsengland.co.uk/panels/271/)
	  * [9 = Familial Tumours Syndromes of the central & peripheral Nervous system](https://panelapp.genomicsengland.co.uk/panels/167/)
	  * [10 = Familial breast cancer](https://panelapp.genomicsengland.co.uk/panels/158/)
	  * [11 = Familial melanoma](https://panelapp.genomicsengland.co.uk/panels/522/)
	  * [12 = Familial prostate cancer](https://panelapp.genomicsengland.co.uk/panels/318/)
	  * [13 = Familial rhabdomyosarcoma](https://panelapp.genomicsengland.co.uk/panels/290/)
	  * [14 = GI tract tumours](https://panelapp.genomicsengland.co.uk/panels/254/)
	  * [15 = Haematological malignancies cancer susceptibility](https://panelapp.genomicsengland.co.uk/panels/59/)
	  * [16 = Head and neck cancer pertinent cancer susceptibility](https://panelapp.genomicsengland.co.uk/panels/115/)
	  * [17 = Inherited non-medullary thyroid cancer](https://panelapp.genomicsengland.co.uk/panels/171/)
	  * [18 = Inherited ovarian cancer (without breast cancer)](https://panelapp.genomicsengland.co.uk/panels/143/)
	  * [19 = Inherited pancreatic cancer](https://panelapp.genomicsengland.co.uk/panels/524/)
	  * [20 = Inherited renal cancer](https://panelapp.genomicsengland.co.uk/panels/521/)
	  * [21 = Inherited phaeochromocytoma and paraganglioma](https://panelapp.genomicsengland.co.uk/panels/97/)
	  * [22 = Melanoma pertinent cancer susceptibility](https://panelapp.genomicsengland.co.uk/panels/133/)
	  * [23 = Multiple endocrine tumours](https://panelapp.genomicsengland.co.uk/panels/36/)
	  * [24 = Multiple monogenic benign skin tumours](https://panelapp.genomicsengland.co.uk/panels/558/)
	  * [25 = Neuroendocrine cancer pertinent cancer susceptibility](https://panelapp.genomicsengland.co.uk/panels/183/)
	  * [26 - Neurofibromatosis Type 1](https://panelapp.genomicsengland.co.uk/panels/255/)
	  * [27 = Ovarian cancer pertinent cancer susceptibility](https://panelapp.genomicsengland.co.uk/panels/117/)
	  * [28 = Parathyroid Cancer](https://panelapp.genomicsengland.co.uk/panels/86/)
	  * [29 = Prostate cancer pertinent cancer susceptibility](https://panelapp.genomicsengland.co.uk/panels/17/)
	  * [30 = Renal cancer pertinent cancer susceptibility](https://panelapp.genomicsengland.co.uk/panels/154/)
	  * [31 = Rhabdoid tumour predisposition](https://panelapp.genomicsengland.co.uk/panels/600/)
	  * [32 = Sarcoma cancer susceptibility](https://panelapp.genomicsengland.co.uk/panels/217/)
	  * [33 = Sarcoma susceptibility](https://panelapp.genomicsengland.co.uk/panels/734/)
	  * [34 = Thyroid cancer pertinent cancer susceptibility](https://panelapp.genomicsengland.co.uk/panels/421/)
	  * [35 = Tumour predisposition - childhood onset](https://panelapp.genomicsengland.co.uk/panels/243/)
	  * [36 = Upper gastrointestinal cancer pertinent cancer susceptibility](https://panelapp.genomicsengland.co.uk/panels/273/)

### Example report

* [Cancer predisposition sequencing report](http://folk.uio.no/sigven/example.cpsr.grch37.html)

### Annotation resources included in _cpsr - 0.5.1_

* [VEP](http://www.ensembl.org/info/docs/tools/vep/index.html) - Variant Effect Predictor v98.2 (GENCODE v31/v19 as the gene reference dataset), includes [gnomAD r2.1](http://gnomad.broadinstitute.org/), [dbSNP build 151/150](http://www.ncbi.nlm.nih.gov/SNP/), [1000 Genomes Project - phase3](ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/release/20130502/)
* [ClinVar](http://www.ncbi.nlm.nih.gov/clinvar/) - Database of variants with clinical significance (October 2019)
* [CIViC](http://civic.genome.wustl.edu) - clinical interpretations of variants in cancer (October 13th 2019)
* [Cancer Hotspots](http://cancerhotspots.org) - Resource for statistically significant mutations in cancer (v2 - 2017)
* [dBNSFP](https://sites.google.com/site/jpopgen/dbNSFP) - Database of non-synonymous functional predictions (v4.0, May 2019)
* [UniProt/SwissProt KnowledgeBase](http://www.uniprot.org) - Resource on protein sequence and functional information (2019_08, September 2019)
* [Pfam](http://pfam.xfam.org) - Database of protein families and domains (v32, Sep 2018)
* [CancerMine](https://zenodo.org/record/3472758#.XZjCqeczaL4) - Literature-derived database of tumor suppressor genes/proto-oncogenes (v17, October 2019)
* [GenomicsEngland PanelApp](https://panelapp.genomicsengland.co.uk) - panels as of October 13th 2019
* [NHGRI-EBI GWAS catalog](https://www.ebi.ac.uk/gwas/) - GWAS catalog for cancer phenotypes (September 24th 2019)

### Documentation

[![Documentation Status](https://readthedocs.org/projects/cpsr/badge/?version=latest)](https://cpsr.readthedocs.io/en/latest/?badge=latest)

### News
* *October 13th 2019*: **0.5.1 release**
  * Updated software (VEP 98.2)
  * Updated bundle (ClinVar, CancerMine, Genomics England PanelApp (36 panels))
* *October 4th 2019*:
  * By mistake, the recently updated grch38 data bundle (20190927) is missing a critical part for CPSR processing. Please download the missing files using [this link](https://drive.google.com/open?id=13mOn1WXEXz_XO3N57LC415gPO0kz3oRA), and put the contents underneath `data/grch38/gnomad_cpsr/`
* *September 29th 2019*: **0.5.0 release**
  * Updated software (VEP 97.3, LOFTEE 1.0.3)
  * Data bundle
	  * ClinVar, UniProt KB, CancerMine, Genomics England PanelApp (35 panels), NHGRI-EBI GWAS catalog
  * New option: _diagnostic_grade_only_ - for virtual panels from Genomics England PanelApp, screen only genes that are marked with a GREEN status
  * Addition of germline biomarkers from CIViC
	  * Report will display clinical evidence items of prognostic/diagnostic/therapeutic/predisposing nature that overlap with variants found in the query sample
* *May 22nd 2019*: **0.4.1 release**
  * Minor bugfixes (PCGR update)
* *May 20th 2019*: **0.4.0 release**
  * Major upgrade
	  * Re-implementation of classification scheme for novel variants (*ACMG*)
	  * Simplified report structure (class 1 - 5) and new summary of findings
	  * Updated resources (ClinVar, VEP, Genomics England, UniProt, dbNSFP, DisGeNET, NHGRI-EBI GWAS catalog)
	  * Optional display of known pathogenic variants in the [ACMG-recommended list of genes for incidental findings](https://www.ncbi.nlm.nih.gov/clinvar/docs/acmg/)
* *November 19th 2018*: **0.3.0 pre-release**
  * Bug fixing and bundle update
* *November 12th 2018*: **0.2.1 pre-release**
  * Improved ACMG classification transparency


### Getting started

#### STEP 0: Install PCGR (version 0.8.3)

Make sure you have a working installation of PCGR (**version 0.8.3**) and the accompanying data bundle(s) (walk through [steps 0-2](https://github.com/sigven/pcgr#getting-started)).

#### STEP 1: Download the latest release

Download the [0.5.1 release](https://github.com/sigven/cpsr/releases/tag/v0.5.1) of *cpsr* (run script and configuration file)

#### STEP 2: Configuration

A few elements of the workflow can be figured using the *cpsr* configuration file, encoded in [TOML](https://github.com/toml-lang/toml). The following can be configured:

* Choice of gnomAD control population
* Upper MAF limit for unclassified variants to be included in the report
* Inclusion of GWAS hits
* Inclusion of CPSR-based ACMG classifications for ClinVar variants
* Inclusion of secondary findings
* VEP/_vcfanno_ options

See section on [Input](https://cpsr.readthedocs.io/en/latest/input.html) for more details wrt. default configuration.

#### STEP 3: Run example

	Cancer Predisposition Sequencing Reporter (CPSR) - report of cancer-predisposing germline variants

	positional arguments:
	  query_vcf             VCF input file with germline query variants (SNVs/InDels).
	  pcgr_base_dir         Directory that contains the PCGR data bundle directory, e.g. ~/pcgr-0.8.3
	  output_dir            Output directory
	  {grch37,grch38}       Genome assembly build: grch37 or grch38
	  virtual_panel_id      Identifier for choice of virtual cancer predisposition gene panels,
	                         choose any between the following identifiers:
	                        0 = CPSR cancer predisposition panel (n = 213, TCGA + Cancer Gene Census + NCGC)
	                        1 = Adult solid tumours cancer susceptibility (Genomics England PanelApp)
	                        2 = Adult solid tumours for rare disease (Genomics England PanelApp)
	                        3 = Bladder cancer pertinent cancer susceptibility (Genomics England PanelApp)
	                        4 = Brain cancer pertinent cancer susceptibility (Genomics England PanelApp)
	                        5 = Breast cancer pertinent cancer susceptibility (Genomics England PanelApp)
	                        6 = Childhood solid tumours cancer susceptibility (Genomics England PanelApp)
	                        7 = Colorectal cancer pertinent cancer susceptibility (Genomics England PanelApp)
	                        8 = Endometrial cancer pertinent cancer susceptibility (Genomics England PanelApp)
	                        9 = Familial Tumours Syndromes of the central & peripheral Nervous system (Genomics England PanelApp)
	                        10 = Familial breast cancer (Genomics England PanelApp)
	                        11 = Familial melanoma (Genomics England PanelApp)
	                        12 = Familial prostate cancer (Genomics England PanelApp)
	                        13 = Familial rhabdomyosarcoma (Genomics England PanelApp)
	                        14 = GI tract tumours (Genomics England PanelApp)
	                        15 = Haematological malignancies cancer susceptibility (Genomics England PanelApp)
	                        16 = Head and neck cancer pertinent cancer susceptibility (Genomics England PanelApp)
	                        17 = Inherited non-medullary thyroid cancer (Genomics England PanelApp)
	                        18 = Inherited ovarian cancer (without breast cancer) (Genomics England PanelApp)
	                        19 = Inherited pancreatic cancer (Genomics England PanelApp)
	                        20 = Inherited renal cancer (Genomics England PanelApp)
	                        21 = Inherited phaeochromocytoma and paraganglioma (Genomics England PanelApp)
	                        22 = Melanoma pertinent cancer susceptibility (Genomics England PanelApp)
	                        23 = Multiple endocrine tumours (Genomics England PanelApp)
	                        24 = Multiple monogenic benign skin tumours (Genomics England PanelApp)
					    25 = Neuroendocrine cancer pertinent cancer susceptibility (Genomics England PanelApp)
	                        26 = Neurofibromatosis Type 1 (Genomics England PanelApp)27 = Ovarian cancer pertinent cancer susceptibility (Genomics England PanelApp)
	                        28 = Parathyroid Cancer (Genomics England PanelApp)
	                        29 = Prostate cancer pertinent cancer susceptibility (Genomics England PanelApp)
	                        30 = Renal cancer pertinent cancer susceptibility (Genomics England PanelApp)
	                        31 = Rhabdoid tumour predisposition (Genomics England PanelApp)
	                        32 = Sarcoma cancer susceptibility (Genomics England PanelApp)
					    33 = Sarcoma susceptibility (Genomics England PanelApp)
	                        34 = Thyroid cancer pertinent cancer susceptibility (Genomics England PanelApp)
	                        35 = Tumour predisposition - childhood onset (Genomics England PanelApp)
	                        36 = Upper gastrointestinal cancer pertinent cancer susceptibility (Genomics England PanelApp)

	  configuration_file    Configuration file (TOML format)
	  sample_id             Sample identifier - prefix for output files

	optional arguments:
	  -h, --help            show this help message and exit
	  --force_overwrite     By default, the script will fail with an error if any output file already exists.
	                         You can force the overwrite of existing result files by using this flag
	  --version             show program's version number and exit
	  --basic               Run functional variant annotation on VCF through VEP/vcfanno, omit report generation (STEP 4)
	  --no_vcf_validate     Skip validation of input VCF with Ensembl's vcf-validator
	  --diagnostic_grade_only
	                        For Genomics England virtual predisposition panels - consider genes with a GREEN status only
	  --docker-uid DOCKER_USER_ID
	                        Docker user ID. Default is the host system user ID. If you are experiencing permission errors,
	                         try setting this up to root (`--docker-uid root`)
	  --no-docker           Run the CPSR workflow in a non-Docker mode (see install_no_docker/ folder for instructions




The *cpsr* software bundle contains an example VCF file. It also contains a configuration file (*cpsr.toml*).

Report generation with the example VCF, using the [Adult solid tumours cancer susceptibility](https://panelapp.genomicsengland.co.uk/panels/245/) virtual gene panel, can be performed through the following command:

`python ~/cpsr-0.5.1/cpsr.py ~/cpsr-0.5.1/example.vcf.gz ~/pcgr-0.8.3`
`~/cpsr-0.5.1 grch37 1 ~/cpsr-0.5.1/cpsr.toml example`

Note that the example command also refers to the PCGR directory (*pcgr-0.8.3*), which contains the data bundle that are necessary for both *PCGR* and *CPSR*.

This command will run the Docker-based *cpsr* workflow and produce the following output files in the _cpsr_ folder:

  1. __example.cpsr.grch37.pass.vcf.gz (.tbi)__ - Bgzipped VCF file with relevant annotations appended by CPSR
  2. __example.cpsr.grch37.pass.tsv.gz__ - Compressed TSV file (generated with [vcf2tsv](https://github.com/sigven/vcf2tsv)) of VCF content with relevant annotations appended by CPSR
  3. __example.cpsr.grch37.html__ - Interactive HTML report with clinically relevant variants in cancer predisposition genes organized into tiers
  4. __example.cpsr.grch37.json.gz__ - Compressed JSON dump of HTML report content
  5. __example.cpsr.snvs_indels.tiers.grch37.tsv__ - TSV file with most important annotations of tier-structured SNVs/InDels

### Related work

* [CharGer - Characterization of Germline variants](https://github.com/ding-lab/CharGer)
* [PathoMan - Pathogenicity of Mutation Analyzer (Beta)](https://pathoman.mskcc.org/)
* [SherLoc - Variant classification](https://www.invitae.com/en/variant-classification/)

### Contact

sigven AT ifi.uio.no
