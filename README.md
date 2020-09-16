# Bisulfite-sequencing-data-analysis
Install zlib1g-dev library
```
$ sudo apt install zlib1g-dev
```
Download latest GSL from http://ftpmirror.gnu.org/gsl/ or `ftp://ftp.gnu.org/gnu/gsl/`

Unpack archive
```
$ tar -zxvf gsl-latest.tar.gz
```
Enter the gsl-2.6 directory ```$ cd gsl-2.6/``` and execute the following codes
```
$ mkdir /home/rajan/gsl
$ ./configure --prefix=/home/rajan/gsl
$ make
$ make check
$ make install
```
Download methpipe-4.1.1.tar.gz from https://github.com/smithlabcode/methpipe/releases/download/v4.1.1/methpipe-4.1.1.tar.gz

Unpack archive
```
$ tar -zxvf methpipe-4.1.1.tar.gz
```
Enter the methpipe-4.1.1 directory ```$ cd methpipe-4.1.1/``` and execute the following codes
```
$ mkdir build && cd build
$ ../configure
$ ../configure --prefix=/home/rajan/methpipe
$ make
$ make install
```
Download walt from https://github.com/smithlabcode/walt/archive/master.zip or https://github.com/smithlabcode/walt

Extract and rename it *walt*

Enter the *walt* directory ```$ cd walt/``` and execute the following codes
```
$ make all
$ make install
$ cd bin
```
Copy and paste EXPERIMENT_1 folder (containing reads folder(containing _1 and _2 reads) and reference_genome) into the bin of *walt*

Execute the following codes to make index for mapping
```
$ ./makedb -c EXPERIMENT_1/MTBS.fa -o MTBS.dbindex
```
Enter the EXPERIMENT_1 folder `$ cd EXPERIMENT_1/` and execute the following codes
```
$ mkdir reads_split
$ for i in reads/*.fastq; do \
split -a 3 -d -l 4000000 ${i} reads_split/$(basename $i); done
$ cd ..
```
Execute the following codes to make mapped reads files ( *.mr suffix)
```
$ ./walt -i MTBS.dbindex -1 EXPERIMENT_1/reads_split/MTBS0_1.fastq -2 EXPERIMENT_1/reads_split/MTBS0_2.fastq -o MTBS0.mr
$ ./walt -i MTBS.dbindex -1 EXPERIMENT_1/reads_split/MTBS1_1.fastq -2 EXPERIMENT_1/reads_split/MTBS1_2.fastq -o MTBS1.mr
$ ./walt -i MTBS.dbindex -1 EXPERIMENT_1/reads_split/MTBS2_1.fastq -2 EXPERIMENT_1/reads_split/MTBS2_2.fastq -o MTBS2.mr
$ ./walt -i MTBS.dbindex -1 EXPERIMENT_1/reads_split/MTBS3_1.fastq -2 EXPERIMENT_1/reads_split/MTBS3_2.fastq -o MTBS3.mr
$ ./walt -i MTBS.dbindex -1 EXPERIMENT_1/reads_split/MTBS4_1.fastq -2 EXPERIMENT_1/reads_split/MTBS4_2.fastq -o MTBS4.mr
$ ./walt -i MTBS.dbindex -1 EXPERIMENT_1/reads_split/MTBS5_1.fastq -2 EXPERIMENT_1/reads_split/MTBS5_2.fastq -o MTBS5.mr
$ ./walt -i MTBS.dbindex -1 EXPERIMENT_1/reads_split/MTBS6_1.fastq -2 EXPERIMENT_1/reads_split/MTBS6_2.fastq -o MTBS6.mr
```
Merge all the mapped reads files ( *.mr suffix) and (*.mr.mapstats)
```bash
$ cat *.mr > MTBS.mr
$ cat *.mapstats > MTBS.mr.mapstats
```
