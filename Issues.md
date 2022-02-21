Intento organizarme los problemas que voy teniendo (muchísimos). 

Trinity falla tras Inchworm; el problema parece residir en conflictos entre librerías de samtools y openssl. La  versión de openssl que tengo (con conda) 1.11 parece no tener la libreria libcrypto.so.1.0.0 que requiere samtools para su funcionamiento. Al lanzar Trinity, tengo el siguiente error:
```
samtools: error while loading shared libraries: libcrypto.so.1.0.0: cannot open shared object file: No such file or directory
samtools: error while loading shared libraries: libcrypto.so.1.0.0: cannot open shared object file: No such file or directory
Trinity run failed. Must investigate error above.
```
Por la información que he encontrado, la solución más repetida es downgradear la versión de openssl a la 1.0, que sí cuenta con la librería correspondiente. Por eso lo más repetido es:
https://github.com/bioconda/bioconda-recipes/issues/12100
```
conda install openssl=1.0
##I've also tried installing them in the same command:
conda install samtools=1.8 openssl=1.0
##As well as downgrading samtools itself:
conda install samtools=1.8 openssl=1.0
##El mismo error persiste :c
```
Sin embargo, hay fallos en conda por conflictos en la instalación. Cuando intento descargar la versión anterior de openssl, parece haber conflictos con la versión de python (3.9). He encontrado información sobre resolverlo downgradeando también la versión de python:
https://www.codetd.com/es/article/12296435
```
conda install python=3.5
```
Sin embargo, consume muchísimo tiempo y no acaba de instalarse (parece quedarse permanentemente en 'Solving enviroment'. De nuevo, busco una posible solución para este error, que parece tener su raíz en la versión de conda.
https://github.com/conda/conda/issues/8051
Algunas de las soluciones apuntan a eliminar el canal -forge de conda:
```
##Removing the conda-forge channel appears to remove this behavior and it continues to work after add conda-forge.
##The following steps worked to resolve the issue.
    conda config --remove channels conda-forge
    conda config --add channels conda-forge
##No other changes were required to fix this on my system.
```
A mí, personalmente, tampoco parece funcionarme. 