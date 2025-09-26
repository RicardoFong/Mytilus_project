# Hosseini Khorami et al., 2025; Unstable mitochondrial heteroplasmy in _Mytilus edulis_ primary cell cultures

## Mapping tissue and cell derived Illumina sequences of the blue mussel _Mytilus edulis_ against male and female mitochondrial genomes

The proces described in this file is relative to the work of Hosseini Khorrami et al., 2025. The aim is to share the detailed mapping process of Illumina NovaSeq sequencing data described in the main paper. You can refer to the publication in the header of this file for the results on this workflow.

## Mapping and quantification process:

- Reads were single end. files were trimmed from adapters and sequencing artifacts such as polyG sequences using fastp v-0.24.0

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
