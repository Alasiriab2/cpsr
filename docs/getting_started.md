## Getting started

### STEP 0: Python

An installation of Python (version _3.6_) is required to run CPSR. Check that Python is installed by typing `python --version` in your terminal window.

**IMPORTANT NOTE**: STEP 1 & 2 below outline installation guidelines for running CPSR with Docker. If you want to install and run CPSR without the use of Docker (i.e. through Conda), follow [these instructions](https://github.com/sigven/cpsr/tree/master/conda_pkg/README.md)

### STEP 1: Install PCGR (version 0.9.2)

Make sure you have a working installation of PCGR (**version 0.9.2**) and the accompanying data bundle(s) (walk through [steps 1-2](https://github.com/sigven/pcgr#getting-started)).

### STEP 2: Download the latest release

Download the [0.6.2 release](https://github.com/sigven/cpsr/releases/tag/v0.6.2) of *cpsr* (run script)

### STEP 4: Run example

	usage: cpsr.py -h [options]  --input_vcf <INPUT_VCF> --pcgr_dir <PCGR_DIR> --output_dir <OUTPUT_DIR> --genome_assembly  <GENOME_ASSEMBLY> --sample_id <SAMPLE_ID>

	Cancer Predisposition Sequencing Reporter - report of clinically significant cancer-predisposing germline variants

	Required arguments:
	--input_vcf INPUT_VCF
				    VCF input file with germline query variants (SNVs/InDels).
	--pcgr_dir PCGR_DIR   Directory that contains the PCGR data bundle directory, e.g. ~/pcgr-0.9.2
	--output_dir OUTPUT_DIR
				    Output directory
	--genome_assembly {grch37,grch38}
				    Genome assembly build: grch37 or grch38
	--sample_id SAMPLE_ID
				    Sample identifier - prefix for output files

	Panel options:
	--panel_id VIRTUAL_PANEL_ID
				    Comma-separated string with identifier(s) of predefined virtual cancer predisposition gene panels,
					choose any combination of the following identifiers:
				    0 = CPSR exploratory cancer predisposition panel
					(n = 335, Genomics England PanelApp / TCGA Germline Study / Cancer Gene Census / Other)
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
				    15 = Genodermatoses with malignancies (Genomics England PanelApp)
				    16 = Haematological malignancies cancer susceptibility (Genomics England PanelApp)
				    17 = Haematological malignancies for rare disease (Genomics England PanelApp)
				    18 = Head and neck cancer pertinent cancer susceptibility (Genomics England PanelApp)
				    19 = Inherited MMR deficiency (Lynch syndrome) - Genomics England PanelApp
				    20 = Inherited non-medullary thyroid cancer (Genomics England PanelApp)
				    21 = Inherited ovarian cancer (without breast cancer) (Genomics England PanelApp)
				    22 = Inherited pancreatic cancer (Genomics England PanelApp)
				    23 = Inherited polyposis (Genomics England PanelApp)
				    24 = Inherited predisposition to acute myeloid leukaemia (AML) - Genomics England PanelApp
				    25 = Inherited predisposition to GIST (Genomics England PanelApp)
				    26 = Inherited renal cancer (Genomics England PanelApp)
				    27 = Inherited phaeochromocytoma and paraganglioma (Genomics England PanelApp)
				    28 = Melanoma pertinent cancer susceptibility (Genomics England PanelApp)
				    29 = Multiple endocrine tumours (Genomics England PanelApp)
				    30 = Multiple monogenic benign skin tumours (Genomics England PanelApp)
				    31 = Neuroendocrine cancer pertinent cancer susceptibility (Genomics England PanelApp)
				    32 = Neurofibromatosis Type 1 (Genomics England PanelApp)
				    33 = Ovarian cancer pertinent cancer susceptibility (Genomics England PanelApp)
				    34 = Parathyroid Cancer (Genomics England PanelApp)
				    35 = Prostate cancer pertinent cancer susceptibility (Genomics England PanelApp)
				    36 = Renal cancer pertinent cancer susceptibility (Genomics England PanelApp)
				    37 = Rhabdoid tumour predisposition (Genomics England PanelApp)
				    38 = Sarcoma cancer susceptibility (Genomics England PanelApp)
				    39 = Sarcoma susceptibility (Genomics England PanelApp)
				    40 = Thyroid cancer pertinent cancer susceptibility (Genomics England PanelApp)
				    41 = Tumour predisposition - childhood onset (Genomics England PanelApp)
				    42 = Upper gastrointestinal cancer pertinent cancer susceptibility (Genomics England PanelApp)

	--custom_list CUSTOM_LIST
				    Provide custom list of genes from virtual panel 0 (single-column txt file with Ensembl gene identifiers),
					alternative to predefined panels provided with --panel_id)
	--custom_list_name CUSTOM_LIST_NAME
				    Set name for custom made panel/list (single word - no whitespace), will be displayed in the report
	--diagnostic_grade_only
				    For panel_id's 1-42 (Genomics England PanelApp) - consider genes with a GREEN status only, default: False

	VEP options:
	--vep_n_forks VEP_N_FORKS
				    Number of forks (option '--fork' in VEP), default: 4
	--vep_buffer_size VEP_BUFFER_SIZE
				    Variant buffer size (variants read into memory simultaneously, option '--buffer_size' in VEP)
				    - set lower to reduce memory usage, default: 500
	--vep_pick_order VEP_PICK_ORDER
				    Comma-separated string of ordered transcript properties for primary variant pick
					( option '--pick_order' in VEP), default: canonical,appris,biotype,ccds,rank,tsl,length,mane
	--vep_no_intergenic   Skip intergenic variants during processing (option '--no_intergenic' in VEP), default: False

	vcfanno options:
	--vcfanno_n_proc VCFANNO_N_PROC
				    Number of vcfanno processes (option '-p' in vcfanno), default: 4

	Other options:
	--force_overwrite     By default, the script will fail with an error if any output file already exists.
					You can force the overwrite of existing result files by using this flag, default: False
	--version             show program's version number and exit
	--basic               Run functional variant annotation on VCF through VEP/vcfanno, omit Tier assignment/report generation (STEP 4), default: False
	--no_vcf_validate     Skip validation of input VCF with Ensembl's vcf-validator, default: False
	--docker_uid DOCKER_USER_ID
				    Docker user ID. Default is the host system user ID. If you are experiencing permission errors,
					try setting this up to root (`--docker_uid root`), default: None
	--no_docker           Run the CPSR workflow in a non-Docker mode, default: False
	--preserved_info_tags PRESERVED_INFO_TAGS
				    Comma-separated string of VCF INFO tags from query VCF that should be kept in CPSR output TSV
	--report_theme {default,cerulean,journal,flatly,readable,spacelab,united,cosmo,lumen,paper,sandstone,simplex,yeti}
				    Visual report theme (rmarkdown), default: default
	--report_nonfloating_toc
				    Place the table of contents (TOC) at the top of the HTML report, not floating at the left, default: False
	--report_table_display {full,light}
				    Set the level of detail/comprehensiveness in interactive datables of HTML report, very comprehensive (option 'full') or slim/focused ('light')
	--ignore_noncoding    Do not list non-coding variants in HTML report, default: False
	--secondary_findings  Include variants found in ACMG-recommended list for secondary findings (v3.0), default: False
	--gwas_findings       Report overlap with low to moderate cancer risk variants (tag SNPs) identified from genome-wide association studies, default: False
	--gwas_p_value GWAS_P_VALUE
				    Required p-value for variants listed as hits from genome-wide association studies, default: 5e-06
	--pop_gnomad {afr,amr,eas,sas,asj,nfe,fin,global}
				    Population source in gnomAD used for variant frequency assessment (ACMG classification), default: nfe
	--maf_upper_threshold MAF_UPPER_THRESHOLD
				    Upper MAF limit (gnomAD global population frequency) for variants to be included in the report, default: 0.9
	--classify_all        Provide CPSR variant classifications (TIER 1-5) also for variants with exising ClinVar classifications in output TSV, default: False
	--clinvar_ignore_noncancer
				    Ignore (exclude from report) ClinVar-classified variants reported only for phenotypes/conditions NOT related to cancer, default: False
	--debug            Print full docker commands to log, default: False



The *cpsr* software release contains an example VCF file.

Report generation with the example VCF, using the [Adult solid tumours cancer susceptibility](https://panelapp.genomicsengland.co.uk/panels/245/) virtual gene panel, can be performed through the following command:

	python ~/cpsr-0.6.2/cpsr.py
	 --query_vcf ~/cpsr-0.6.2/example.vcf.gz
	 --pcgr_dir ~/pcgr-0.9.2
	 --output_dir ~/cpsr-0.6.2
	 --genome_assembly grch37
	 --panel_id 1
	 --sample_id example
	 --secondary_findings
	 --gwas_findings
	 --classify_all
	 --maf_upper_threshold 0.2
	 --no_vcf_validate

Note that the example command also refers to the PCGR directory (*pcgr-0.9.2*), which here contains the data bundle that are necessary for both *PCGR* and *CPSR*.

This command will run the Docker-based *cpsr* workflow and produce the following output files in the _output_ folder:

1. __example.cpsr.grch37.vcf.gz (.tbi)__ - Bgzipped VCF file with relevant annotations appended by CPSR
2. __example.cpsr.grch37.pass.vcf.gz (.tbi)__ - Bgzipped VCF file with relevant annotations appended by CPSR (PASS variants only)
3. __example.cpsr_config.rds__ - CPSR configuration object (RDS format), mostly for debugging purposes
4. __example.cpsr.grch37.pass.tsv.gz__ - Compressed TSV file (generated with [vcf2tsv](https://github.com/sigven/vcf2tsv)) of VCF content with relevant annotations appended by CPSR
5. __example.cpsr.grch37.html__ - Interactive HTML report with clinically relevant variants in cancer predisposition genes organized into tiers
6. __example.cpsr.grch37.json.gz__ - Compressed JSON dump of HTML report content
7. __example.cpsr.grch37.snvs_indels.tiers.tsv__ - TSV file with key annotations of tier-structured SNVs/InDels
