
ETL : Extract, Transform, Load


## Cloning ETL project

```
git clone https://gitlab.com/jonathanlsn/etl_planet.git

or 

git clone git clone https://jonathanlsn:glpat-CZfwCoxTiBSwsJtLCxeR@gitlab.com/jonathanlsn/etl_planet.git
```

then switch branch
```
git checkout Jonathan_ETL
```


## Prerequisites

- Docker : https://www.docker.com/products/docker-desktop/

## Run docker images 
load 'etl_img' from  etl_planet/ETL_Planet/etl_img.tar
```
cd etl_planet/ETL_Planet

docker load -i etl_img.tar
```

Then run etl_ctnr from bash
```
docker run -d -t -v $(pwd):/home/ETL_Planet --name etl_ctnr etl_img
```

Then enter into etl_ctnr from bash
```
docker exec -it etl_ctnr /bin/bash
```

To exit out of the docker container bash shell just run *exit* or hit *ctrl-D*


## Run ETL process
```
cd /home/ETL_Planet

python3 ETL.py 
```


## Documentation

### Inputs

For clinical data : ETL.py looks into exports folder
    - exports/20221020_132044_V_BLOOD_SAMPLES.xls (BLOOD_SAMPLES.xls) : Blood samples information from Ennov export "V_BLOOD_SAMPLES"
    - exports/20221020_132106_V_TUMOR_SAMPLES.xls (TUMOR_SAMPLES.xls) : Tumor samples information from Ennov export "V_TUMOR_SAMPLES"

For genomic data : ETL.py looks into seqruns/test/patients (as in gnomic) to get SNV, CNV and Fusions files.
    - SNV : PLANET_01-001_TUMOR_0/mutect2/vep/*-pass.vep.vcf.gz
    - CNV : PLANET_01-001_TUMOR_0/facets/*segments.bed
    - Fusion : PLANET_01-001_TUMOR_0/rnafusions/*.fusions-summary.tsv

For configuation : ETL.py look into conf.py to get parameters necessary for ETL process and OSIRIS data transformation

### Configuration

The conf.py file allows the configuration of OSIRIS data transformation and set default parameters required to locate inputs/outputs, files :


### Outputs 

Outputs are located in "Output" directory. It contains :
    - blood_samples.csv : This is a table is created from exports/BLOOD_SAMPLES.xls file. Columns has been rename/cretaed/modified to meet OSIRIS models nomenclature
    - tumor_samples.csv : This is a table is created from exports/TUMOR_SAMPLES.xls file. Columns has been renamed/deleted/created/modified to meet OSIRIS model's nomenclature
    - clinical_data.csv : This is the result of blood_samples and tumor_samples concatenation. A column "folder" has been added to save the information of genomic data location.
    - Patients_data : This subfolder contains OSIRIS model json files for each patient in clinical_data.

Those files are reloaded at each execution of ETL.py


### Dependencies 

They are all located into "bin" directory. There are several dependencies files dealing with defferents steps of ETL process
    - get_patientfolder.py : get the name of folder in /seqruns/test/patients using BiologicalSample_Timing, BiologicalSample_Nature, BiologicalSample_CollectDate
    - get_clinicaldata.py : allows to get blood and tumor samples data from BLOOD_SAMPLES.xls and TUMOR_SAMPLES.xls 
    - get_genomicdata.py : get genomicdata using 
    - json_transformation.py : 
    