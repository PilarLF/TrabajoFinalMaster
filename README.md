# **Trabajo Final de Máster**

Máster en Bioinformática por la Escuela Técnica Superior de Ingeniería (Universidad de Valencia). Curso Académico 2021-2022. 
Centro de investigación I2SysBio (Paterna).
                                                                                            

## Objetivos del trabajo

Idea principal del proyecto: análisis transcriptómico del cuerpo graso de individuos adultos quasi-aposimbiontes (individuos a los que se les ha reducido drásticamente la población endosimbionte de  Blattabacterium). Se hará un análisis de la expresión diferencial génica con el fin de buscar qué genes se expresan o no de forma significativa entre ambos grupos como consecuencia de la presencia/ausencia del endosimbionte. Se espera encontrar genes significativos codificantes de:  
1. Proteínas que participan en el metabolismo de rutas de integración del endosimbionte    
2. transportadores de la membrana simbiosomal para el tráfico de metabolitos ensosimbionte-huésped.  
3. AMP -AMPlike relacionados con el control poblacional del endosimbionte  

El objetivo de este repositorio es proporcionar el material suplementario sobre el software y los paquetes usados, incluyendo sus parámetros, usados durante el análisis de RNAseq de individuos de *Blatella germanica*. 
Para ello, se ha realizado un control de calidad de las muestras, un ensamblaje *de novo* del transcritpoma de Blatella, una cuantificación mediante pseudo-alineador (mediante la terminal de bash) y un análisis de la expresión diferencial y anotación funcional de los transcritos (ejecutado en R, versión 4.1). Los scripts pueden en contrarse en la carpeta ./scripts/ de este repositorio. Se ha tratado de acompañarlos con una breve explicación de cada paso del análisis. 

## 1. Trinity de novo asembly
```
Trinity --seqType fq --max_memory 10G --left G2r-CG-pool-4_S32.trim.R1.fastq,G2r-CG-pool-3_S31.trim.R1.fastq,G2r-CG-pool-2_S30.trim.R1.fastq,G2r-CG-pool-1_S29.trim.R1.fastq,control-CG-pool-4_S28.trim.R1.fastq,control-CG-pool-3_S27.trim.R1.fastq,control-CG-pool-2_S26.trim.R1.fastq,control-CG-pool-1_S25.trim.R1.fastq --right G2r-CG-pool-4_S32.trim.R2.fastq,G2r-CG-pool-3_S31.trim.R2.fastq,G2r-CG-pool-2_S30.trim.R2.fastq,G2r-CG-pool-1_S29.trim.R2.fastq,control-CG-pool-4_S28.trim.R2.fastq,control-CG-pool-3_S27.trim.R2.fastq,control-CG-pool-2_S26.trim.R2.fastq,control-CG-pool-1_S25.trim.R2.fastq --CPU 32 --trimmomatic --output CG_transcriptome1

#o, de forma más eficiente:
cat *R1.fastq > reads.ALL.left.fq
cat *R2.fastq > reads.ALL.rigth.fq
Trinity --seqType fq --max_memory 100G --left reads.ALL.left.fq.gz --right reads.ALL.right.fq.gz --CPU 32 --trimmomatic
```
El resultado se recoge en ficheros/Trinity.fasta. 
Muchos de los pasos realizados a continuación se han tomado de: https://southgreenplatform.github.io/trainings/trinityTrinotate/TP-trinity/#practice-2 

### 1.1 Assessing transcriptome assembly quality
#### 1.1.1 Getting basic Assembly metrics with trinitystats.pl
Tras obtener el ensamblaje final del transcriptoma, este queda recogido en el fichero Trinity.fasta generado por Trinity (trinity_out_directory). Trinity proporciona el código TrinityStats.pl, con el cual realizar la evaluación del ensamblaje. 
````
TrinityStats.pl trinity_out_dir/Trinity.fasta > trinityStats.log
````
El resultado se recoge en ficheros/trinityStats.log

#### 1.1.2 Reads mapping back rate and abundance estimation using align_and_estimate_abundance.pl
A continuación, vamos a mapear nuestras lecturas contra el transcrito recién esnamblado. Un buen ensamblaje debería, normalmente, mapear el 80% de las reads (parejadas, entre sí), por lo menos. Para esto Trinity proporciona el script align_and_estimate_abundance.pl. Con Bowtie2, se ha seguido los siguientes pasos:

````
44071070 reads; of these:
  44071070 (100.00%) were paired; of these:
    3080056 (6.99%) aligned concordantly 0 times
    29199848 (66.26%) aligned concordantly exactly 1 time
    11791166 (26.75%) aligned concordantly >1 times
    ----
    3080056 pairs aligned concordantly 0 times; of these:
      317887 (10.32%) aligned discordantly 1 time
    ----
    2762169 pairs aligned 0 times concordantly or discordantly; of these:
      5524338 mates make up the pairs; of these:
        3189471 (57.73%) aligned 0 times
        1004700 (18.19%) aligned exactly 1 time
        1330167 (24.08%) aligned >1 times
96.38% overall alignment rate

````

````
# create a salmon_outdir and go on
mkdir salmon_outdir; cd salmon_outdir

# salmon
perl $path_to_trinity/util/align_and_estimate_abundance.pl \
--transcripts $fasta \
--seqType fq \
--samples_file $samplesfile \
--est_method salmon \
--trinity_mode \
--prep_reference > salmon_align_and_estimate_abundance.log 2>&1 &
````


### (1.1 Ensamblaje con SPAdes)
Pruebo a lanzar SPAdes para ver cómo arranca. Para ello lanzo:
````
spades.py -1 ../../Trinity/reads.ALL.left.fq -2 ../../Trinity/reads.ALL.rigth.fq --careful --cov-cutoff auto -o spades_assembly_all_illumina
````
Todos los parámetros usados son los que estaban por defecto cuando he buscado la linea de comandos en su pagina web. 


