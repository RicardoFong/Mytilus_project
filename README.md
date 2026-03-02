# Hosseini Khorami et al., 2026; Unstable mitochondrial heteroplasmy in _Mytilus edulis_ primary cell cultures

## Mapping tissue and cell derived Illumina sequences of the blue mussel _Mytilus edulis_ against male and female mitochondrial genomes

This repository documents the read trimming, mapping, and quantification workflow used in Hosseini Khorami et al. (2026). The purpose of this repository is to provide a detailed and transparent description of the computational procedures applied to the Illumina NovaSeq sequencing data analyzed in the study. The results generated using this workflow are reported in the associated publication.

This repository is intended for documentation and reproducibility purposes and does not constitute a general-purpose analysis pipeline.

## Mapping and quantification process:

- Reads were single-end. Files were trimmed for adapters and sequencing artifacts (e.g. polyG sequences) using fastp v0.24.0.

```sh
$ fastp -i "infile.fastq.gz" --adapter_sequence=AGATCGGAAGAGCACACGTCTGAACTCCAGTCA \
                           --trim_poly_g \
                           --length_required 30 \
                           --trim_poly_x \
                           --cut_front \
                           --cut_tail \
                           --cut_window_size 5 \
                           --cut_mean_quality 20
```

The sequence content and mean length of both files before and after trimming is:

Comparison of reads, bases, and read lengths for **Mm-T** sample before and after trimming

#### Mm-T sample
| Metric         | Before trimming | After trimming |
|----------------|-----------------|----------------|
| Total reads    | 22.005895 M     | 21.722752 M    |
| Total bases    | 3.322890 G      | 3.170502 G     |
| Mean length    | 151 bp          | 145 bp         |



Comparison of reads, bases, and read lengths for **Mm-C** sample before and after trimming

#### Mm-C sample
| Metric         | Before trimming | After trimming |
|----------------|-----------------|----------------|
| Total reads    | 33.327738 M     | 21.722752 M    |
| Total bases    | 5.032488 G      | 3.170502 G     |
| Mean length    | 151 bp          | 148 bp         |




- Mapping was performed with the BWA algorithm with bwa v-2.2.1 and filtering for high-quality mapping reads with samtools v-2.2.1

```sh

$ bwa mem $refgen $infile | samtools sort -T $tag -O BAM -o $outfile -

```

Mapped and unmapped reads for **Mm-T**.

#### Mm-T sample
| mt Genome     | Mapped  | Total unmapped |
|---------------|---------|----------------|
| AY484747.1 (F)| 141389  | -              |
| AY823623.1 (M)| 466     | -              |
| **Total**     |         | 21581136       |



Mapped and unmapped reads for **Mm-C**.

#### Mm-C sample
| mt Genome     | Mapped  | Total unmapped |
|---------------|---------|----------------|
| AY484747.1 (F)| 6106    | -              |
| AY823623.1 (M)| 50976   | -              |
| **Total**     |         | 33161591       |


- These reads were further filtered by mapping quality 30

```sh

$ samtools view -bq 30 $bamfile > $filtered_bamfile

```

- The final number of reads mapping against AY484747.1 and AY823623.1 mitochondrial genomes are:

| Sample | AY484747.1 (F) | AY823623.1 (M) | Total reads | % AY484747.1 (F) | % AY823623.1 (M) |
| ------ | -------------- | -------------- | ----------- | ---------------- | ---------------- |
| Mm-T   | 140559         | 102            | 140661      | 99.9             | 0.1              |
| Mm-C   | 6061           | 50551          | 74831       | 10.7             | 89.3             |


The complete study and figures can be found in the original paper.
