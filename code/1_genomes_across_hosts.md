# 1_genomes_across_hosts
Raphael Eisenhofer

- [How are bacterial genomes distributed across
  hosts?](#how-are-bacterial-genomes-distributed-across-hosts)

## How are bacterial genomes distributed across hosts?

``` r
library(tidyverse)
library(rairtable)

cdb <- read_delim("../data/DMB0173_Cdb.csv.gz")
#contains high level clustering information for MAGs
ndb <- read_delim("../data/DMB0173_Ndb.csv.gz")
#contains pairwise ANI distances (fastANI) between clusters of MAGs

#filter out only genomes that have secondary clusters (MASH ANI > 90%)
primary_cluster_counts <- cdb %>%
  count(primary_cluster, name = "n_primary_cluster_members")

secondary_cluster_counts <- cdb %>%
  count(secondary_cluster, name = "n_secondary_cluster_members")

cdb_filt <- cdb %>%
  left_join(., primary_cluster_counts, by = join_by(primary_cluster)) %>%
  left_join(., secondary_cluster_counts, by = join_by(secondary_cluster)) %>%
  filter(n_primary_cluster_members > 1) %>%
  mutate(genome = str_replace(genome, '.gz', ''))

#basic stats
n_primary_clusters <- length(unique(cdb$primary_cluster))
n_secondary_clusters <- length(unique(cdb$secondary_cluster))


#Also filter for ANI > 95% in Ndb??

mag_info <- read_delim("../data/mag_info.csv") %>%
  inner_join(., cdb_filt, by = join_by(mag_name == genome)) 

## Pull data from AirTable
#Select the right view id
# view <- airtable(base = 'appWbHBNLE6iAsMRV',
#                  table = 'tblMzd3oyaJhdeQcs',
#                  view = 'viwMD6lnM4YFxkAmi')
# 
# #download mag info from AirTable (takes a while, so do once)
#  airtable_data_mag <- read_airtable(view,
#                                     id_to_col = TRUE,
#                                     max_rows = 50000)
#  airtable_data_mag <- airtable_data_mag %>%
#    unnest(cols = c(host_species, host_class, host_order))
# 
# write_delim(airtable_data_mag, file = "../data/mag_info.csv")

#Host species per cluster?

hosts_across_clusters <- mag_info %>%
  summarise(n_species_cluster = n_distinct(host_species),
            n_secondary_cluster_members = max(n_secondary_cluster_members),
            n_class_cluster = n_distinct(host_class),
            n_secondary_cluster = n_distinct(secondary_cluster),
            mean_genome_size = mean(size),
            .by = secondary_cluster)

n_cross_class <- hosts_across_clusters %>%
  filter(n_class_cluster == 2) %>%
  nrow()

n_cross_species <- hosts_across_clusters %>% 
  filter(n_secondary_cluster_members > 1 & n_species_cluster > 1) %>% 
  nrow()
```

5123 MAGs were dereplicated down to 4545 at 98% ANI (average nucleotide
identity). There were 3201 clusters of MAGs that were \>90% similar
based on MASH.

A total of 7 clusters contains MAGs \>98% ANI across host class
(i.e. squamates and rodents), and for host species this is 430. Keep in
mind that these values are raw – no evenness of coverage filtering, so
will need to be validated. Regardless, there looks like there is
sufficient comparisons here that we can explore when it comes to testing
e.g. ‘does ANI between bacterial genomes correlate with host
phylogenetic distance?’.

Possible future questions:

> 1.  Does ANI between bacterial genomes correlate with host
>     phylogenetic distance?
> 2.  MAG covariates that explain these cross-species clusters?
>     (e.g. taxonomy, genome size, GC, specific functions)
> 3.  More of these clusters in squamates vs rodents? (though possibly
>     biased by host phylogenetic distance?)