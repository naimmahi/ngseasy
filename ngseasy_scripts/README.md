NGSeasy NOTES
===============
- Stephen Newhouse <stephen.j.newouse@gmail.com>

*******************

**Get the Docker Imahge**
*****************************

```sh
sudo docker pull snewhouse/ngseasy-alignment-public:v1.2
```

**Get the pipeline scripts**
****************************

Get ``ngseasy_scripts``

```sh
# make ngseasy directory
mkdir ngseasy
cd ngseasy

# get latest ngseasy_scripts code
git clone https://github.com/KHP-Informatics/ngs.git
cd ngs
git checkout dev2
git pull origin dev2
mv -v ngseasy_scripts ../

# move up to ngseasy
cd ../

# make ngs_projects
mkdir ngs_projects

# copy scripts to ngs_projects
cp -v ngseasy_scripts/run_ngseasy_dockers.sh ./ngs_projects/
cp -v ngseasy_scripts/ngs.config.file.tsv ./ngs_projects/

# get ngseasy_resources (Dropbox for a limited time only)
wget --no-check-cert https://www.dropbox.com/s/9pw3ml75pdnufjl/ngseasy_resources.tar.gz?dl=0
tar xvf ngseasy_resources.tar.gz

# move contents to ngseasy/
mv -v ngseasy_resources/** ../

# un pack them
tar xvf reference_genomes_b37.tgz
tar xvf gatk_resources.tar.gz

# clean up
rm -rf ngseasy_resources

# permissions
chmod -R 755 ./
```

**You should have the following directories**

- fastq_raw
- gatk_resources
- reference_genomes_b37
- ngs_projects
- ngseasy_scripts
- ngs

**Set Up Config File**
************************

In Excel make config file and save as [TAB] Delimited file with ``.tsv`` extenstion.  
See Example.

**Inside run_ngseasy_dockers.sh**
********************

```bash
#!/bin/bash
config_tsv=${1} 
sudo docker run -P \
-v /media/D/docker_ngs/ngseasy/fastq_raw:/home/pipeman/fastq_raw \
-v /media/D/docker_ngs/ngseasy/reference_genomes_b37:/home/pipeman/reference_genomes_b37 \
-v /media/D/docker_ngs/ngseasy/gatk_resources:/home/pipeman/gatk_resources \
-v /media/D/docker_ngs/ngseasy/ngs_projects:/home/pipeman/ngs_projects \
-v /media/D/docker_ngs/ngseasy/ngseasy_scripts:/home/pipeman/ngseasy_scripts \
-i -t snewhouse/ngseasy-alignment-public:v1.2 /sbin/my_init -- /bin/bash  /home/pipeman/ngseasy_scripts/run_ea-ngs.sh /home/pipeman/ngs_projects/${config_tsv};
```
Need **FULL PATHS** so ``../DIR`` should be ``/HOME/DIR``

Edit ``run_ngseasy_dockers.sh``

**Call the pipeline**
********************
```sh
bash run_ngseasy_dockers.sh ngs.config.file.tsv
```

**GCE**
**************
```
https://console.developers.google.com/project/apps~secure-electron-631/compute/instancesDetail/zones/europe-west1-b/instances/bigdockerbox
```

```bash
gcloud compute --project "secure-electron-631" ssh --zone "europe-west1-b" "bigdockerbox"
```



### Misc



**Software Versions**
************************
- Trimmomatic-0.32
- bwa-0.7.10
- bowtie2-2.2.3
- novocraftV3.02.07.Linux3.0
- stampy-1.0.23
- samtools-0.1.19
- picard-tools-1.115
- GenomeAnalysisTK-3.2-2
- Platypus_0.7.4
- fastqc_v0.11.2


**Trimmomatic Log**
********************
Specifying a trimlog file creates a log of all read trimmings, indicating the following details:

- the read name
- the surviving sequence length
- the location of the first surviving base, aka. the amount trimmed from the start
- the location of the last surviving base in the original read
- the amount trimmed from the end


```
206B4ABXX100825:7:1:7466:34224/1 69 0 69 7
206B4ABXX100825:7:1:7466:34224/2 62 0 62 14
206B4ABXX100825:7:1:7466:35353/1 69 0 69 7
206B4ABXX100825:7:1:7466:35353/2 76 0 76 0
206B4ABXX100825:7:1:7466:35395/1 54 0 54 22
206B4ABXX100825:7:1:7466:35395/2 76 0 76 0
206B4ABXX100825:7:1:7466:38189/1 52 0 52 24
206B4ABXX100825:7:1:7466:38189/2 76 0 76 0
206B4ABXX100825:7:1:7466:39218/1 76 0 76 0

```