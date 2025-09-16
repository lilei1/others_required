# Brachypodium GWAS Analysis Notes

## Background

This analysis is based on the original VCF file processing tutorial documented in:
[Brachy_mutant/analysis/Natural_accessions/filtering_variants.md](https://github.com/lilei1/Brachy_mutant/blob/master/analysis/Natural_accessions/filtering_variants.md)

The workflow follows the variant filtering procedures established for Brachypodium natural accessions analysis.

## Email Request

**From:** Mingqin Shao  
**Date:** Aug 21, 2025, 10:48 AM  
**To:** Li

Hi Li,
Could you please help us to get two more sets of filtered SNPs with the minor allele frequency by 0.05 and 0.02 for the Brachy GWAS panel? 
It is not in a rush and take your time to get them at your convenient time in the next few weeks. 
Thanks

## Initial Data Files and Statistics

### Original Data File
```
/global/u2/l/llei2019/plantbox/bscratch/llei2019/Bd21_3_mutant/natural_lines/data/maf0.05_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.recode.vcf
```

### Data Statistics
- **After filtering:** kept 6376145 out of a possible 6924193 Sites
- **Average variants:** 6376145/114 = 55931.0965

### HMP File
```
onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.recode.recode.hmp.txt
```

### Base VCF File
```
/global/u2/l/llei2019/plantbox/bscratch/llei2019/Bd21_3_mutant/natural_lines/data/onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.recode.vcf
```

## VCF Filtering Commands

### MAF 0.05 Filtering
```bash
vcftools --vcf onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.recode.vcf --maf 0.05 --recode --recode-INFO-all --out maf0.05_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044
```

### MAF 0.02 Filtering
```bash
vcftools --vcf onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.recode.vcf --maf 0.02 --recode --recode-INFO-all --out maf0.02_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044
```

## Resource Allocation
```bash
salloc --nodes 1 --qos interactive --time 02:00:00 --constraint cpu --account=m342
```

## VCF File Sorting

### Note
I have to sort vcf file first:

### MAF 0.05 Sorting Command
```bash
cat /global/u2/l/llei2019/plantbox/bscratch/llei2019/Bd21_3_mutant/natural_lines/data/maf0.05_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.recode.vcf | awk '$1 ~ /^#/ {print $0;next} {print $0 | "sort -k1,1 -k2,2n"}' > /global/u2/l/llei2019/plantbox/bscratch/llei2019/Bd21_3_mutant/natural_lines/data/sorted_maf0.05_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.recode.vcf
```

### MAF 0.02 Sorting Command
```bash
cat /global/u2/l/llei2019/plantbox/bscratch/llei2019/Bd21_3_mutant/natural_lines/data/maf0.02_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.recode.vcf | awk '$1 ~ /^#/ {print $0;next} {print $0 | "sort -k1,1 -k2,2n"}' >/global/u2/l/llei2019/plantbox/bscratch/llei2019/Bd21_3_mutant/natural_lines/data/sorted_maf0.02_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.recode.vcf
```

## TASSEL Pipeline Execution

### MAF 0.05 TASSEL Command
```bash
./run_pipeline.pl -Xmx80g -fork1 -vcf /global/u2/l/llei2019/plantbox/bscratch/llei2019/Bd21_3_mutant/natural_lines/data/sorted_maf0.05_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.recode.vcf -export -exportType Hapmap -fork1c
```

#### MAF 0.05 TASSEL Output Log
```
./lib/phg.jar:./lib/kotlin-stdlib-1.6.10.jar:./lib/jfreesvg-3.2.jar:./lib/sshj-0.32.0.jar:./lib/slf4j-api-1.7.10.jar:./lib/jfreechart-1.0.19.jar:./lib/gs-core-1.3.jar:./lib/jackson-databind-2.13.2.2.jar:./lib/itextpdf-5.1.0.jar:./lib/biojava-phylo-4.2.12.jar:./lib/postgresql-42.6.0.jar:./lib/sqlite-jdbc-3.39.2.1.jar:./lib/jackson-core-2.13.2.jar:./lib/protobuf-java-util-3.23.0.jar:./lib/biojava-core-6.0.4.jar:./lib/kotlinx-coroutines-core-jvm-1.6.0.jar:./lib/jcommon-1.0.23.jar:./lib/jhdf5-14.12.5.jar:./lib/commons-codec-1.10.jar:./lib/forester-1.039.jar:./lib/gs-ui-1.3.jar:./lib/jackson-annotations-2.13.2.jar:./lib/colt-1.2.0.jar:./lib/kotlin-stdlib-jdk7-1.6.10.jar:./lib/ejml-core-0.41.jar:./lib/json-simple-1.1.1.jar:./lib/protobuf-java-3.23.0.jar:./lib/scala-library-2.10.1.jar:./lib/biojava-genome-6.0.4.jar:./lib/error_prone_annotations-2.19.1.jar:./lib/ejml-ddense-0.41.jar:./lib/kotlin-stdlib-jdk8-1.6.10.jar:./lib/biojava-alignment-6.0.4.jar:./lib/protobuf-kotlin-3.23.0.jar:./lib/junit-4.10.jar:./lib/ini4j-0.5.4.jar:./lib/guava-22.0.jar:./lib/log4j-api-2.21.1.jar:./lib/kotlin-reflect-1.6.10.jar:./lib/jackson-module-kotlin-2.13.2.jar:./lib/log4j-core-2.21.1.jar:./lib/commons-io-2.11.0.jar:./lib/snappy-java-1.1.8.4.jar:./lib/javax.json-1.0.4.jar:./lib/trove-3.0.3.jar:./lib/mail-1.4.jar:./lib/htsjdk-2.24.1.jar:./lib/slf4j-simple-1.7.10.jar:./lib/fastutil-8.2.2.jar:./lib/ahocorasick-0.2.4.jar:./lib/commons-math3-3.4.1.jar:./sTASSEL.jar

Memory Settings: -Xms512m -Xmx80g
Tassel Pipeline Arguments: -fork1 -vcf /global/u2/l/llei2019/plantbox/bscratch/llei2019/Bd21_3_mutant/natural_lines/data/sorted_maf0.05_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.recode.vcf -export -exportType Hapmap -fork1c

[main] INFO net.maizegenetics.tassel.TasselLogging - Tassel Version: 5.2.94  Date: August 20, 2024
[main] INFO net.maizegenetics.tassel.TasselLogging - Max Available Memory Reported by JVM: 81920 MB
[main] INFO net.maizegenetics.tassel.TasselLogging - Java Version: 11.0.26
[main] INFO net.maizegenetics.tassel.TasselLogging - OS: Linux
[main] INFO net.maizegenetics.tassel.TasselLogging - Number of Processors: 256
[main] INFO net.maizegenetics.tassel.TasselLogging - Tassel Citation: Bradbury PJ, Zhang Z, Kroon DE, Casstevens TM, Ramdoss Y, Buckler ES. (2007) TASSEL: Software for association mapping of complex traits in diverse samples. Bioinformatics 23:2633-2635.
[main] INFO net.maizegenetics.tassel.TasselLogging - 
[main] INFO net.maizegenetics.tassel.TasselLogging - Tassel Using Library: Practical Haplotype Graph (PHG): Version: 1.10 Date: August 20, 2024
[main] INFO net.maizegenetics.tassel.TasselLogging - PHG Citation: Bradbury PJ, Casstevens T, Jensen SE, Johnson LC, Miller ZR, Monier B, Romay MC, Song B, Buckler ES. The Practical Haplotype Graph, a platform for storing and using pangenomes for imputation. Bioinformatics. 2022 Aug 2;38(15):3698-3702. doi: 10.1093/bioinformatics/btac410. PMID: 35748708; PMCID: PMC9344836.
[main] INFO net.maizegenetics.pipeline.TasselPipeline - Tassel Pipeline Arguments: [-fork1, -vcf, /global/u2/l/llei2019/plantbox/bscratch/llei2019/Bd21_3_mutant/natural_lines/data/sorted_maf0.05_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.recode.vcf, -export, -exportType, Hapmap, -fork1c]
[pool-2-thread-1] INFO net.maizegenetics.plugindef.AbstractPlugin - Starting net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:30:0
[pool-2-thread-1] INFO net.maizegenetics.plugindef.AbstractPlugin - 
FileLoadPlugin Parameters
format: VCF
sortPositions: false
keepDepth: false

[pool-2-thread-1] INFO net.maizegenetics.analysis.data.FileLoadPlugin - Start Loading File: /global/u2/l/llei2019/plantbox/bscratch/llei2019/Bd21_3_mutant/natural_lines/data/sorted_maf0.05_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.recode.vcf time: Sep 16, 2025 15:30:0
[pool-2-thread-1] INFO net.maizegenetics.pipeline.TasselPipeline - net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:33:6: progress: 0%
[pool-2-thread-1] INFO net.maizegenetics.pipeline.TasselPipeline - net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:33:6: progress: 10%
[pool-2-thread-1] INFO net.maizegenetics.pipeline.TasselPipeline - net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:33:6: progress: 20%
[pool-2-thread-1] INFO net.maizegenetics.pipeline.TasselPipeline - net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:33:6: progress: 30%
[pool-2-thread-1] INFO net.maizegenetics.pipeline.TasselPipeline - net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:33:6: progress: 40%
[pool-2-thread-1] INFO net.maizegenetics.pipeline.TasselPipeline - net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:33:6: progress: 50%
[pool-2-thread-1] INFO net.maizegenetics.pipeline.TasselPipeline - net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:33:6: progress: 60%
[pool-2-thread-1] INFO net.maizegenetics.pipeline.TasselPipeline - net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:33:6: progress: 70%
[pool-2-thread-1] INFO net.maizegenetics.pipeline.TasselPipeline - net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:33:6: progress: 80%
[pool-2-thread-1] INFO net.maizegenetics.pipeline.TasselPipeline - net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:33:6: progress: 90%
[pool-2-thread-1] INFO net.maizegenetics.pipeline.TasselPipeline - net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:33:7: progress: 100%
[pool-2-thread-1] INFO net.maizegenetics.plugindef.AbstractPlugin - net.maizegenetics.analysis.data.FileLoadPlugin  Citation: Bradbury PJ, Zhang Z, Kroon DE, Casstevens TM, Ramdoss Y, Buckler ES. (2007) TASSEL: Software for association mapping of complex traits in diverse samples. Bioinformatics 23:2633-2635.
BuilderFromVCF data timing 187.769s 
[pool-2-thread-1] INFO net.maizegenetics.analysis.data.FileLoadPlugin - Finished Loading File: /global/u2/l/llei2019/plantbox/bscratch/llei2019/Bd21_3_mutant/natural_lines/data/sorted_maf0.05_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.recode.vcf time: Sep 16, 2025 15:33:7

Genotype Table Name: sorted_maf0.05_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044
Number of Taxa: 114
Number of Sites: 2614247
Sites x Taxa: 298024158

Chromosomes...
BD1: start site: 0 (3768) last site: 681732 (75535479) total: 681733
BD2: start site: 681733 (111) last site: 1245471 (58852926) total: 563739
BD3: start site: 1245472 (31740) last site: 1848646 (59998619) total: 603175
BD4: start site: 1848647 (2259) last site: 2329048 (48740533) total: 480402
BD5: start site: 2329049 (5108) last site: 2614164 (28886683) total: 285116
SCAFFOLD_14: start site: 2614165 (25522) last site: 2614172 (64363) total: 8
SCAFFOLD_17: start site: 2614173 (28322) last site: 2614174 (32280) total: 2
SCAFFOLD_20: start site: 2614175 (26686) last site: 2614177 (31199) total: 3
SCAFFOLD_28: start site: 2614178 (5522) last site: 2614189 (26387) total: 12
SCAFFOLD_35: start site: 2614190 (25136) last site: 2614196 (29600) total: 7
SCAFFOLD_36: start site: 2614197 (11904) last site: 2614198 (13318) total: 2
SCAFFOLD_39: start site: 2614199 (1143) last site: 2614199 (1143) total: 1
SCAFFOLD_7: start site: 2614200 (48463) last site: 2614214 (100280) total: 15
SCAFFOLD_8: start site: 2614215 (26148) last site: 2614231 (96952) total: 17
SCAFFOLD_9: start site: 2614232 (29241) last site: 2614246 (82904) total: 15

[pool-2-thread-1] INFO net.maizegenetics.plugindef.AbstractPlugin - Finished net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:33:7
[pool-2-thread-1] INFO net.maizegenetics.plugindef.AbstractPlugin - Starting net.maizegenetics.analysis.data.ExportPlugin: time: Sep 16, 2025 15:33:7
[pool-2-thread-1] INFO net.maizegenetics.plugindef.AbstractPlugin - 
ExportPlugin Parameters
saveAs: sorted_maf0.05_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044
format: Hapmap
keepDepth: true
includeTaxaAnnotations: true
includeBranchLengths: true

[pool-2-thread-1] INFO net.maizegenetics.analysis.data.ExportPlugin - performFunction: wrote dataset: sorted_maf0.05_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044 to file: sorted_maf0.05_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.hmp.txt
[pool-2-thread-1] INFO net.maizegenetics.plugindef.AbstractPlugin - Finished net.maizegenetics.analysis.data.ExportPlugin: time: Sep 16, 2025 15:33:31
```

### MAF 0.02 TASSEL Command
```bash
./run_pipeline.pl -Xmx80g -fork1 -vcf /global/u2/l/llei2019/plantbox/bscratch/llei2019/Bd21_3_mutant/natural_lines/data/sorted_maf0.02_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.recode.vcf -export -exportType Hapmap -fork1c
```

#### MAF 0.02 TASSEL Output Log
```
./lib/phg.jar:./lib/kotlin-stdlib-1.6.10.jar:./lib/jfreesvg-3.2.jar:./lib/sshj-0.32.0.jar:./lib/slf4j-api-1.7.10.jar:./lib/jfreechart-1.0.19.jar:./lib/gs-core-1.3.jar:./lib/jackson-databind-2.13.2.2.jar:./lib/itextpdf-5.1.0.jar:./lib/biojava-phylo-4.2.12.jar:./lib/postgresql-42.6.0.jar:./lib/sqlite-jdbc-3.39.2.1.jar:./lib/jackson-core-2.13.2.jar:./lib/protobuf-java-util-3.23.0.jar:./lib/biojava-core-6.0.4.jar:./lib/kotlinx-coroutines-core-jvm-1.6.0.jar:./lib/jcommon-1.0.23.jar:./lib/jhdf5-14.12.5.jar:./lib/commons-codec-1.10.jar:./lib/forester-1.039.jar:./lib/gs-ui-1.3.jar:./lib/jackson-annotations-2.13.2.jar:./lib/colt-1.2.0.jar:./lib/kotlin-stdlib-jdk7-1.6.10.jar:./lib/ejml-core-0.41.jar:./lib/json-simple-1.1.1.jar:./lib/protobuf-java-3.23.0.jar:./lib/scala-library-2.10.1.jar:./lib/biojava-genome-6.0.4.jar:./lib/error_prone_annotations-2.19.1.jar:./lib/ejml-ddense-0.41.jar:./lib/kotlin-stdlib-jdk8-1.6.10.jar:./lib/biojava-alignment-6.0.4.jar:./lib/protobuf-kotlin-3.23.0.jar:./lib/junit-4.10.jar:./lib/ini4j-0.5.4.jar:./lib/guava-22.0.jar:./lib/log4j-api-2.21.1.jar:./lib/kotlin-reflect-1.6.10.jar:./lib/jackson-module-kotlin-2.13.2.jar:./lib/log4j-core-2.21.1.jar:./lib/commons-io-2.11.0.jar:./lib/snappy-java-1.1.8.4.jar:./lib/javax.json-1.0.4.jar:./lib/trove-3.0.3.jar:./lib/mail-1.4.jar:./lib/htsjdk-2.24.1.jar:./lib/slf4j-simple-1.7.10.jar:./lib/fastutil-8.2.2.jar:./lib/ahocorasick-0.2.4.jar:./lib/commons-math3-3.4.1.jar:./sTASSEL.jar

Memory Settings: -Xms512m -Xmx80g
Tassel Pipeline Arguments: -fork1 -vcf /global/u2/l/llei2019/plantbox/bscratch/llei2019/Bd21_3_mutant/natural_lines/data/sorted_maf0.02_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.recode.vcf -export -exportType Hapmap -fork1c

[main] INFO net.maizegenetics.tassel.TasselLogging - Tassel Version: 5.2.94  Date: August 20, 2024
[main] INFO net.maizegenetics.tassel.TasselLogging - Max Available Memory Reported by JVM: 81920 MB
[main] INFO net.maizegenetics.tassel.TasselLogging - Java Version: 11.0.26
[main] INFO net.maizegenetics.tassel.TasselLogging - OS: Linux
[main] INFO net.maizegenetics.tassel.TasselLogging - Number of Processors: 256
[main] INFO net.maizegenetics.tassel.TasselLogging - Tassel Citation: Bradbury PJ, Zhang Z, Kroon DE, Casstevens TM, Ramdoss Y, Buckler ES. (2007) TASSEL: Software for association mapping of complex traits in diverse samples. Bioinformatics 23:2633-2635.
[main] INFO net.maizegenetics.tassel.TasselLogging - 
[main] INFO net.maizegenetics.tassel.TasselLogging - Tassel Using Library: Practical Haplotype Graph (PHG): Version: 1.10 Date: August 20, 2024
[main] INFO net.maizegenetics.tassel.TasselLogging - PHG Citation: Bradbury PJ, Casstevens T, Jensen SE, Johnson LC, Miller ZR, Monier B, Romay MC, Song B, Buckler ES. The Practical Haplotype Graph, a platform for storing and using pangenomes for imputation. Bioinformatics. 2022 Aug 2;38(15):3698-3702. doi: 10.1093/bioinformatics/btac410. PMID: 35748708; PMCID: PMC9344836.
[main] INFO net.maizegenetics.pipeline.TasselPipeline - Tassel Pipeline Arguments: [-fork1, -vcf, /global/u2/l/llei2019/plantbox/bscratch/llei2019/Bd21_3_mutant/natural_lines/data/sorted_maf0.02_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.recode.vcf, -export, -exportType, Hapmap, -fork1c]
[pool-2-thread-1] INFO net.maizegenetics.plugindef.AbstractPlugin - Starting net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:43:6
[pool-2-thread-1] INFO net.maizegenetics.plugindef.AbstractPlugin - 
FileLoadPlugin Parameters
format: VCF
sortPositions: false
keepDepth: false

[pool-2-thread-1] INFO net.maizegenetics.analysis.data.FileLoadPlugin - Start Loading File: /global/u2/l/llei2019/plantbox/bscratch/llei2019/Bd21_3_mutant/natural_lines/data/sorted_maf0.02_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.recode.vcf time: Sep 16, 2025 15:43:6
[pool-2-thread-1] INFO net.maizegenetics.pipeline.TasselPipeline - net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:47:53: progress: 0%
[pool-2-thread-1] INFO net.maizegenetics.pipeline.TasselPipeline - net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:47:53: progress: 10%
[pool-2-thread-1] INFO net.maizegenetics.pipeline.TasselPipeline - net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:47:53: progress: 20%
[pool-2-thread-1] INFO net.maizegenetics.pipeline.TasselPipeline - net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:47:53: progress: 30%
[pool-2-thread-1] INFO net.maizegenetics.pipeline.TasselPipeline - net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:47:53: progress: 40%
[pool-2-thread-1] INFO net.maizegenetics.pipeline.TasselPipeline - net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:47:53: progress: 50%
[pool-2-thread-1] INFO net.maizegenetics.pipeline.TasselPipeline - net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:47:53: progress: 60%
[pool-2-thread-1] INFO net.maizegenetics.pipeline.TasselPipeline - net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:47:53: progress: 70%
[pool-2-thread-1] INFO net.maizegenetics.pipeline.TasselPipeline - net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:47:53: progress: 80%
[pool-2-thread-1] INFO net.maizegenetics.pipeline.TasselPipeline - net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:47:53: progress: 90%
[pool-2-thread-1] INFO net.maizegenetics.pipeline.TasselPipeline - net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:47:54: progress: 100%
[pool-2-thread-1] INFO net.maizegenetics.plugindef.AbstractPlugin - net.maizegenetics.analysis.data.FileLoadPlugin  Citation: Bradbury PJ, Zhang Z, Kroon DE, Casstevens TM, Ramdoss Y, Buckler ES. (2007) TASSEL: Software for association mapping of complex traits in diverse samples. Bioinformatics 23:2633-2635.
BuilderFromVCF data timing 288.459s 
[pool-2-thread-1] INFO net.maizegenetics.analysis.data.FileLoadPlugin - Finished Loading File: /global/u2/l/llei2019/plantbox/bscratch/llei2019/Bd21_3_mutant/natural_lines/data/sorted_maf0.02_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.recode.vcf time: Sep 16, 2025 15:47:55

Genotype Table Name: sorted_maf0.02_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044
Number of Taxa: 114
Number of Sites: 3984563
Sites x Taxa: 454240182

Chromosomes...
BD1: start site: 0 (3632) last site: 1061769 (75535479) total: 1061770
BD2: start site: 1061770 (111) last site: 1940982 (58862897) total: 879213
BD3: start site: 1940983 (31740) last site: 2854661 (59998619) total: 913679
BD4: start site: 2854662 (1023) last site: 3567215 (48740533) total: 712554
BD5: start site: 3567216 (5099) last site: 3984441 (28897323) total: 417226
SCAFFOLD_14: start site: 3984442 (22873) last site: 3984453 (64363) total: 12
SCAFFOLD_17: start site: 3984454 (28322) last site: 3984456 (32365) total: 3
SCAFFOLD_20: start site: 3984457 (26686) last site: 3984460 (32176) total: 4
SCAFFOLD_28: start site: 3984461 (5522) last site: 3984472 (26387) total: 12
SCAFFOLD_32: start site: 3984473 (9351) last site: 3984474 (11680) total: 2
SCAFFOLD_35: start site: 3984475 (25136) last site: 3984485 (29753) total: 11
SCAFFOLD_36: start site: 3984486 (11835) last site: 3984493 (14074) total: 8
SCAFFOLD_39: start site: 3984494 (1143) last site: 3984494 (1143) total: 1
SCAFFOLD_7: start site: 3984495 (48463) last site: 3984517 (101869) total: 23
SCAFFOLD_8: start site: 3984518 (19332) last site: 3984540 (96952) total: 23
SCAFFOLD_9: start site: 3984541 (29241) last site: 3984562 (82904) total: 22

[pool-2-thread-1] INFO net.maizegenetics.plugindef.AbstractPlugin - Finished net.maizegenetics.analysis.data.FileLoadPlugin: time: Sep 16, 2025 15:47:55
[pool-2-thread-1] INFO net.maizegenetics.plugindef.AbstractPlugin - Starting net.maizegenetics.analysis.data.ExportPlugin: time: Sep 16, 2025 15:47:55
[pool-2-thread-1] INFO net.maizegenetics.plugindef.AbstractPlugin - 
ExportPlugin Parameters
saveAs: sorted_maf0.02_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044
format: Hapmap
keepDepth: true
includeTaxaAnnotations: true
includeBranchLengths: true

[pool-2-thread-1] INFO net.maizegenetics.analysis.data.ExportPlugin - performFunction: wrote dataset: sorted_maf0.02_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044 to file: sorted_maf0.02_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.hmp.txt
[pool-2-thread-1] INFO net.maizegenetics.plugindef.AbstractPlugin - Finished net.maizegenetics.analysis.data.ExportPlugin: time: Sep 16, 2025 15:48:28
```

## Summary of Results

### MAF 0.05 Results
- **Number of Taxa:** 114
- **Number of Sites:** 2,614,247
- **Sites x Taxa:** 298,024,158
- **Output File:** `sorted_maf0.05_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.hmp.txt`
- **Processing Time:** BuilderFromVCF data timing 187.769s

### MAF 0.02 Results
- **Number of Taxa:** 114
- **Number of Sites:** 3,984,563
- **Sites x Taxa:** 454,240,182
- **Output File:** `sorted_maf0.02_onlySNP_filtered_treatmissing_biallilic_filtered_passs_genotype_gvcfs.f1.bf=g10-G3-Q40-QD5.anno_mafgt0.0044.hmp.txt`
- **Processing Time:** BuilderFromVCF data timing 288.459s

## Software Information
- **TASSEL Version:** 5.2.94 (Date: August 20, 2024)
- **Java Version:** 11.0.26
- **OS:** Linux
- **Number of Processors:** 256
- **Max Available Memory:** 81920 MB
