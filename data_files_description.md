# Data file descriptions

This is a detailed description of the files and types of data stored in the `data` folder.

-   `anno_urls.tsv` - a line separated file, where each line is a URL for a MAG's gene annotations.

-   `beta.Rdata` - contains hilldiv2 beta diversity outputs. created [from this .Rmd file](https://github.com/alberdilab/EHI_squamate_rodent/blob/main/code/06_beta_diversity.Rmd)

-   `data.Rdata` - contains various cleaned dataframes and matrices created from [this .Rmd file](https://github.com/alberdilab/EHI_squamate_rodent/blob/main/code/01_prepare_data.Rmd)

-   `DMB0173_Cdb.csv.gz` - Output from dRep on all MAGs. Contains clustering information for each MAG. For more info, [see here](https://drep.readthedocs.io/en/latest/advanced_use.html). Columns:

    |            |                                   |                               |                    |                          |                                 |
    |------------|-----------------------------------|-------------------------------|--------------------|--------------------------|---------------------------------|
    | **genome** | **secondary_cluster**             | **threshold**                 | **cluster_method** | **comparison_algorithm** | **primary_cluster**             |
    | MAG        | secondary cluster assigned to MAG | estimated threshold from MASH |                    |                          | primary cluster assigned to MAG |

-   `DMB0173_counts.tsv.gz` - Count table where each column is a sample, and each row is a MAG. Contains all MAGs and samples.

-   `DMB0173_coverage.tsv.gz` - Same as above, but instead of counts, contains the proportion of the MAG that was covered by reads. E.g. if a 300 bp MAG had 150 bp covered by reads, the value would be 0.5.

-   `DMB0173_mag_info.tsv.gz` - Contains basic metadata (mostly taxonomic) for each MAG. See `mag_extra_metadata.csv.gz` below for extra MAG metadata. Columns:

    | genome | domain | phylum | class | order | family | genus | species | completeness                   | contamination                   | mag_size |
    |--------|--------|--------|-------|-------|--------|-------|---------|--------------------------------|---------------------------------|----------|
    |        |        |        |       |       |        |       |         | CheckM2 estimated completeness | CheckM2 estimated contamination |          |

-   `DMB0173_merged_kegg.tsv.gz` - output from DRAM distill. each column is a module (M\*\*\*\*\*), and each row is a MAG, with the value representing the proportion of the functional pathway being covered. E.g. if a module has 4 genes, and only two are detected, the value would be 0.5

-   `DMB0173_metadata.tsv.gz` - table that contains sample-level metadata. Columns:

    <table>
    <tbody>
    <tr class="odd">
    <td><p>EHI_plaintext</p></td>
    <td><p>sample_code</p></td>
    <td><p>species</p></td>
    <td><p>region</p></td>
    <td><p>sample_type</p></td>
    <td><p>order</p></td>
    <td><p>sex</p></td>
    <td><p>country</p></td>
    <td><p>latitude</p></td>
    <td><p>longitude</p></td>
    <td><p>singlem_fraction</p></td>
    <td><p>reads_post_fastp</p></td>
    <td><p>host_reads</p></td>
    <td><p>reads_lost_fastp_percent</p></td>
    <td><p>metagenomic_bases</p></td>
    <td><p>host_bases</p></td>
    <td><p>bases_lost_fastp_percent</p></td>
    <td><p>diversity</p></td>
    <td><p>C</p></td>
    </tr>
    <tr class="even">
    <td><p>sample id</p></td>
    <td><p>sample code</p></td>
    <td><p>host species</p></td>
    <td><p>region</p></td>
    <td><p>type of sample</p></td>
    <td><p>host order</p></td>
    <td><p>host sex</p></td>
    <td><p>country derived from</p></td>
    <td></td>
    <td></td>
    <td><p>fraction of metagenome that is estimated to be prokaryotic</p></td>
    <td><p>reads after trimming/cleaning with fastp</p></td>
    <td><p>number of reads aligned to host genome</p></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td><p>nonpareil 3 estimated diversity <a href="https://journals.asm.org/doi/10.1128/msystems.00039-18">see paper</a></p></td>
    <td><p>nonpareil 3 C value, estimate of diversity captured.</p>
    <p><a href="https://journals.asm.org/doi/10.1128/msystems.00039-18">see paper</a></p></td>
    </tr>
    </tbody>
    </table>

-   `DMB0173_Ndb.csv.gz` - Output from dRep on all MAGs. Contains pairwise secondary clustering (ANI) for each genome in a primary cluster. For more info, [see here](https://drep.readthedocs.io/en/latest/advanced_use.html). Columns:

    |               |                 |                                     |                                                 |                                                                                                        |
    |---------------|-----------------|-------------------------------------|-------------------------------------------------|--------------------------------------------------------------------------------------------------------|
    | **reference** | **querry**      | **ani**                             | **alignment_coverage**                          | **primary_cluster**                                                                                    |
    | MAG           | MAG compared to | average nucleotide identity (ANImf) | fraction of MAGs that was covered in comparison | the primary cluster that the MAGs were in. primary clusters are defined as MAGs \> 90% ANI using MASH. |

-   `DMB0173.tree.gz` - tree file for all MAGs, placed on the backbone of the GTDB tree. (gtdbtk version: 2.3.0, release: r214)

-   `EHI_squamate_rodent_coassembly_info.txt` - .tsv file containing information for each host species' coassembly. Columns:

    | host             | host_order     | n_mags                        | metagenomic_data_gbp                                               | n_samples                            |
    |------------------|----------------|-------------------------------|--------------------------------------------------------------------|--------------------------------------|
    | the host species | the host order | number of mags per coassembly | how much metagenomic data was used in coassembly (giga base pairs) | number of samples used in coassembly |

-   `genome_gifts_raw.Rdata` - raw distillR output matrix. each row is a MAG, each column is a gift, value = coverage of gift. [see here](https://github.com/anttonalberdi/distillR).

-   `host_species.tsv` - line separated file of host species binomials. Used as uploaded input to timetree.org to create host phylogeny.

-   `host_species.nwk` - phylogenetic tree output of timetree.org in newick format. (using `host_species.tsv`). Note that *Rattus rattus* is no included in the final tree (timetree complained about there being insufficient data to place it). My guess is that it shares a MRCA with *Rattus lutreolus*.

-   `mag_extra_metadata.csv.gz` - extra metadata for each MAG. columns:

    |        |          |                                        |                                            |            |             |                   |                                 |                                           |                     |                     |                             |                                   |                         |                      |                                |                         |                      |                   |                   |
    |--------|----------|----------------------------------------|--------------------------------------------|------------|-------------|-------------------|---------------------------------|-------------------------------------------|---------------------|---------------------|-----------------------------|-----------------------------------|-------------------------|----------------------|--------------------------------|-------------------------|----------------------|-------------------|-------------------|
    | ID     | mag_name | host_species                           | closest_placement_ani                      | GC         | N50         | contigs           | number_genes                    | cazy_hits                                 | pfam_hits           | kegg_hits           | number_unannotated_genes    | percent_unannotated_genes         | DM_batch                | MAG_url              | taxonomy_level                 | anno_url                | gbk_url              | kegg_url          | host_order        |
    | MAG id | MAG name | host species that MAG was derived from | closest placement ANI in GTDB-tk analysis. | GC content | N50 for MAG | number of contigs | number of detected genes in MAG | number of Carbohydrate Active EnzYme hits | number of pfam hits | number of KEGG hits | number of unannotated genes | percent of MAG that is unannotaed | the dereplication batch | URL for MAG download | lowest taxonomic level for MAG | URL for annotation file | URL for genbank file | URL for KEGG file | host order of MAG |
