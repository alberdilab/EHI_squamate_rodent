# 0_host_species_representation
Raphael Eisenhofer

- [Representativeness of the different host
  metagenomes](#representativeness-of-the-different-host-metagenomes)
- [DAMR estimates by host order and
  species](#damr-estimates-by-host-order-and-species)

## Representativeness of the different host metagenomes

    Warning: package 'lubridate' was built under R version 4.4.1

    Warning: package 'ggrepel' was built under R version 4.4.1

    Warning: package 'ggh4x' was built under R version 4.4.1

We have 20 different host taxa, 11 are squamates, 9 are rodents.

Here’s a phylogeny of the hosts (estimated using timetree.og)

![](images/timetree.svg)

**Note**: *Rattus rattus* is not displayed here (timetree doesn’t seem
to have sufficient info, or the *Rattus* genus is a mess?). Regardless,
we can expect that it’s closest ancestor is *Rattus lutreolus.*

How well represented are the host orders by metagenomic data and number
of MAGs?

``` r
set.seed(1337)

coassembly_df %>%
  ggplot(aes(x = host_order, y = n_mags, 
             colour = host_order, label = paste(host, n_samples))) +
  geom_jitter(height = 0, width = 0.1, alpha = 0.6, 
              aes(size = metagenomic_data_gbp)) + 
  geom_text_repel() + 
  theme_classic() +
  theme(axis.title = element_text(size = 16),
        axis.text = element_text(size = 14))
```

![](supp_host_species_representation_files/figure-commonmark/unnamed-chunk-2-1.png)

**My interpretation:** Looks like we have a decent number of MAGs to
work with per host species, and there is not too much variation between
either host class or species.

## DAMR estimates by host order and species

Determining how representative the MAGs are of each host species using
the Domain-Adjusted Mapping Rate (DAMR):

> **DAMR** = MAG catalogue mapping rate (%) / SingleM microbial fraction
> (%)

``` r
#Select the right view id
view <- airtable(base = 'appWbHBNLE6iAsMRV',
                 table = 'tblWDyQmM9rQ9wq57',
                 view = 'viwQ4Ow667d1lqFpf')

#wrangle
airtable_data_dmb <- read_airtable(view, 
                               id_to_col = TRUE, 
                               max_rows = 50000) %>%
  filter(DM_batch_static == "DMB0173") %>%
  select(EHI_sample_static, MAG_mapping_percentage,
         SingleM_fraction, host_species) %>%
  mutate(SingleM_fraction = as.numeric(SingleM_fraction),
         host_species = as.character(host_species),
         DAMR = MAG_mapping_percentage / SingleM_fraction) %>%
  inner_join(., coassembly_df, by = join_by(host_species == host))

#plot
airtable_data_dmb %>%
  filter(EHI_sample_static != "EHI01045" & EHI_sample_static != "EHI01812") %>%
  ggplot(aes(x = host_species, y = DAMR, 
             colour = host_order, label = host_species)) +
  geom_jitter(
    size = 3, 
    height = 0, 
    width = 0.15, 
    alpha = 0.5
  ) +
  facet_nested(~host_order, scale = "free") +
  stat_summary(
    fun = "mean", 
    geom = "crossbar", 
    colour = "red", 
    width = 0.5
  ) +
  stat_summary(
    fun = "mean",
    geom = "text",
    aes(label = round(..y.., 2)),
    vjust = -1,
    colour = "black"
  ) +
  scale_y_continuous(limits = c(0.3, 1.3)) +
  theme_classic() +
  theme(
    axis.text = element_text(size = 14),
    axis.title = element_text(size = 16),
    axis.text.x = element_text(angle = 90, face = "italic",
                               hjust = 1, vjust = 0.25),
    legend.position = "none"
    )
```

![](supp_host_species_representation_files/figure-commonmark/unnamed-chunk-3-1.png)

Note I’ve filtered out both EHI01045 (no SMF value) and EHI01812 (only
0.16% mapping), and removed *Muscardinus avellanarius* as we don’t have
enough data for this species.

**My interpretation:** There appears to be some slight overestimation of
DAMR for squamates (database-related??). Regardless, most host species
have DAMRs \>80% (i.e. MAGs capture \>80% of the estimated prokaryote
reads). I think we can say that we have sufficient data to proceed with
eco/evo questions.
