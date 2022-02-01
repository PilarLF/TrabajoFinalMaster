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
