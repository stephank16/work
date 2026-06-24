---
tags:
topic: PlantUML
---








- 
- dkrz-in.puml

```plantuml

hide footbox

title Data pipeline flow

  

entity "Kafka\ninput" as kafka1 #99ff99

database S3

box "Data pipeline" #LightBlue

control Consumer

control Downloader

control Executor

control Producer

end box

entity "Kafka\nresults" as kafka2 #ccccff

  

kafka1 --> Consumer : Notification about new data

Consumer --> Downloader : Download data

Downloader --> S3 : Read data from S3 bucket

S3 --> Downloader : Here's required data

Downloader --> Consumer : Here's required data

Consumer --> Executor : Run executor

Executor --> Consumer : Return report

Consumer --> Producer : Results\nin JSON format

Producer --> kafka2 : Publish\nresults\nin JSON format


```
- 
- 
- 
- 
- dkrz_cat.puml
  
```plantuml

title Data catalog perspectives

note "transient data object management \ne.g. grib, zarr\ndirect data generation \n workflow integration" as trans_note
actor data_discovery

package model_data_gen #AliceBlue{
   
    actor data_producer
    collections ".."
    file input_data
    
    control model_run
    input_data -d-> model_run
}

package external_model_data_sink #AliceBlue{
    database partner_site
    cloud data_lake
}

note "analysis ready data catalog \n direct analysis wflow integration \n e.g. intake, FREVA" as da_note
package datastream_store #AliceBlue {
    actor workflow_manager
    storage parallel_file_system
    database FDB 
    cloud ZARR_backend
    database wflow_catalog  #LightSkyBlue 
    wflow_catalog .. trans_note
    parallel_file_system -d- FDB
    FDB -d- ZARR_backend
    ZARR_backend -- parallel_file_system
    storage HSM_raw
    wflow_catalog .u. workflow_manager
}

model_run --> datastream_store 

workflow_manager .. external_model_data_sink

package analysis_data_store #PaleGreen {
    actor data_analysis
    database analysis_data_catalog #LightSkyBlue
    storage analysis_file_system
    database analysis_FDB 
    cloud ZARR_cloud_ready
    storage HSM_analysis
    analysis_file_system -d- analysis_FDB
    analysis_FDB -d- ZARR_cloud_ready
    ZARR_cloud_ready -- analysis_file_system

}
analysis_data_catalog .. da_note 
datastream_store -u-> external_model_data_sink
datastream_store -d-> analysis_data_store
data_analysis .. analysis_data_catalog

note " discoverability of core ESM data collections \n e.g. in EOSC, WMO, Copernicus etc. " as earth_note

package earth_science_community #LightSeaGreen{
    collections external_data_sources
    collections external_data_sinks
    database external_data_portals
    external_data_portals --[hidden]> external_data_sinks
    external_data_portals --[hidden]> external_data_sources

}

external_data_portals .. earth_note

analysis_data_store --> community_data_store

note "data discovery \n curated stable data collections \n persistent references with tombstones \n citation \n community API e.g. STAC, ESGF, WDCC/CERA \n " as c_note
package community_data_store #LightGray{

cloud analysis_ready_data
storage curated_published_data
    
database community_data_catalog #LightSkyBlue
actor data_manager
rectangle archived_data {
    storage HSM_curated
}
archived_data <-u-> curated_published_data
archived_data <-u-> analysis_ready_data
}
community_data_catalog .. c_note


data_manager .. community_data_catalog


data_discovery -u-> community_data_catalog
community_data_store -> earth_science_community

```
```plantuml
testdot
```



- raido.plantuml
```plantuml
' load the C4 PlantUML template  
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

' for the database shape `ContainerDb`
!define ICONS https://raw.githubusercontent.com/tupadr3/plantuml-icon-font-sprites/master/

LAYOUT_TOP_DOWN()
LAYOUT_WITH_LEGEND()

title C4 Container diagram for Raido

Person_Ext(PublicUser, "Public User", "[anonymous]")
Person(SpUser, "Raido\nServicePoint User", "[SSO sign-in via OpenID Connect]")
System_Ext(SpInt, "Raido\nServicePoint Integrator", "[api-key authenticated]")
System_Ext(IdProvider, "ID Provider", "[Google / AAF / ORCiD]")
System_Ext(LocalHdlSvc, "Local Handle Service", "[ARDC DevOps]")
System_Ext(OrcidSvc, "ORCID Service", "[ORCID]")
System_Ext(RorSvc, "ROR Service", "[ROR]")
System_Ext(DoiSvc, "DOI Service", "[DOI]")


System_Boundary(raidC1, "Raid") {
  Container(RaidWebServer, "web server", "Cloudfront") 
  Container(RaidApiSvc, "raid-api-svc", "AWS Lambda") 
}


System_Boundary(raidoC1, "Raido") {
  Container(AppClient, "app-client", "Raido UI -\nSinglePageApp") 
  
  Container(RaidoWebServer, "web server", "Cloudfront") 
  Container(RaidoApiSvc, "raido-api-svc", "Spring REST API") 
  ContainerDb(db, "Database", "RDS PostgreSQL")
}

 

Rel(PublicUser, RaidWebServer, "Uses", "in browser")
Rel(SpUser, AppClient, "Uses", "in browser")
Rel(SpUser, IdProvider, "Identifies with", "HTTPS/OAuth2")
Rel(AppClient, RaidoWebServer, "Uses", "HTTPS")
Rel(RaidoWebServer, RaidoApiSvc, "Uses", "HTTPS")
Rel(RaidWebServer, RaidApiSvc, "Uses", "HTTPS")
Rel(RaidApiSvc, RaidoWebServer, "redirect", "HTTPS")
Rel(SpInt, RaidoWebServer, "Uses", "HTTPS")
Rel(RaidoApiSvc, db, "read/write", "postgres:5432")
Rel(RaidoApiSvc, LocalHdlSvc, "mint", "HTTPS")
Rel(RaidoApiSvc, OrcidSvc, "exists?", "HTTPS")
Rel(RaidoApiSvc, RorSvc, "exists?", "HTTPS")
Rel(RaidoApiSvc, DoiSvc, "exists?", "HTTPS")

```


- PID.puml
```plantuml

circle PID1

object PID1_Kernel_Info {
+ **URL** : "https://handle-esgf.dkrz.de/lp/21.14100/.."
+ **AGGREGATION_LEVEL** : "FILE"
+ **FIXED_CONTENT** : TRUE
+ **FILE_NAME** : drs_file_name.nc
+ **IS_PART_OF** : hdl:21.14100/481..
+ **FILE_VERSION** : 1
+ **CHECKSUM** : 8f714d056dc5857
+ **CHECKSUM_METHOD** : SHA256
+ **URL_ORIGINAL_DATA** : <locations> .. </locations>
==
+ **Metadata** : "hdl:2114100/..."
} 

object PID2_Kernel_Info {
+ **URL** : "https://handle-esgf.dkrz.de/lp/21..."
+ **AGGREGATION_LEVEL** : "DATASET"
+ **FIXED_CONTENT** : TRUE
+ **DRS_ID** : CMIP6.facet1.facet2..
+ **HAS_PARTS** : [PID1,PID3,PID4,...]
+ **VERSION_NUMBER** : 20190710
+ **HOSTING_NODE** : <locations> .. <locations>
+ **IS_PART_OF** : doi:10.22033/ESGF/CMIP6.4403
+ **COMMENT** : "Added DOI on 2022....M. Buurman, DKRZ"
==
+ **Metadata** : "hdl:2114100/..."
}


class PID_KIP {
+ **URL** : STRING
+ **AGGREGATION_LEVEL** : "FILE" or "DATASET"
+ **FIXED_CONTENT** : TRUE
+ **IS_PART_OF** : PID / 'hdl or doi'
..
+ **FILE_NAME** : STRING
+ **FILE_VERSION** : NUMBER
+ **CHECKSUM** : STRING
+ **CHECKSUM_METHOD** : STRING / SHA256 or ..
+ **URL_ORIGINAL_DATA** : XML_STRING
+ **URL_REPLICA_DATA** : XML_STRING
..
+ **DRS_ID** : STRING / .structured
+ **VERSION_NUMBER** : DATE_STRING
+ **HOSTING_NODE** : XML_STRING
+ **HAS_PARTS** : List of PIDs 
+ **REPLICA_NODE** : XML_STRING
+ **_REMOVED_ERRATA_IDS** : STRING / ?structure?
+ **_ADDED_ERRATA_IDS** : STRING
+ **COMMENT** : STRING
==
+ **Metadata** : PID
}

object M_PID_Kernel_Info {
+ **MD_SCHEMA** : STAC
+ **MD_CATALOG** : https:....
+ **MD_CATALOG_Service_Type** : STRING / e.g. OAI, REST, OGC ..
+ **MD_API** : https://
+ **MD_ID** : "a.b.c.d"
+ **external_repr** : [PID1,PID2,..]
+ **external_repr_t** : [DCAT,ESGF,DIF]
}


class M_PID_KIP { 
+ **MD_SCHEMA**: STRING
+ **MD_CATALOG**: URI
+ **MD_API**: URI
+ **MD_ID** : STRING
+ **external_repr** : Ordered LIST of PIDs
+ **external_repr_t** : Ordered LIST of STRINGS
}

M_PID_Kernel_Info --> M_PID_KIP : adheres_to Profile
PID1 --> PID1_Kernel_Info

PID1_Kernel_Info --> PID_KIP : adheres_to Profile
PID2_Kernel_Info --> PID_KIP : adheres_to Profile
PID1_Kernel_Info::IS_PART_OF ..> PID2_Kernel_Info

PID1_Kernel_Info::Metadata ..> M_PID_Kernel_Info 
PID2_Kernel_Info::Metadata ..> M_PID_Kernel_Info

M_PID_Kernel_Info::external_repr ..> M_PID_Kernel_Info

```

- PID_Next1.puml
```plantuml
title CMIP6 PID Kernel Information Profile


object File_or_Dataset_PID1 

object Dataset_or_DOI_PID1

object File_PIDs {
    List of PIDS
} 


object Metadata_PID1

class PID_KIP {
+ **URL** : STRING
+ **AGGREGATION_LEVEL** : "FILE" or "DATASET"
+ **FIXED_CONTENT** : TRUE
+ **IS_PART_OF** : PID / 'hdl or doi'
..
+ **FILE_NAME** : STRING
+ **FILE_VERSION** : NUMBER
+ **CHECKSUM** : STRING
+ **CHECKSUM_METHOD** : STRING / SHA256 or ..
+ **URL_ORIGINAL_DATA** : XML_STRING
+ **URL_REPLICA_DATA** : XML_STRING
..
+ **DRS_ID** : STRING / .structured
+ **VERSION_NUMBER** : DATE_STRING
+ **HOSTING_NODE** : XML_STRING
+ **HAS_PARTS** : List of PIDs 
+ **REPLICA_NODE** : XML_STRING
+ **_REMOVED_ERRATA_IDS** : STRING / ?structure?
+ **_ADDED_ERRATA_IDS** : STRING
+ **COMMENT** : STRING
==
+ Metadata : PID
}



class M_PID_KIP { 
+ **MD_SCHEMA**: STRING
+ **MD_CATALOG**: URI
+ **MD_API**: URI
+ **MD_ID** : STRING
+ **external_repr** : Ordered LIST of PIDs
+ **external_repr_t** : Ordered LIST of STRINGS
}

File_or_Dataset_PID1 --> PID_KIP : adheres_to Profile


PID_KIP::IS_PART_OF ..> Dataset_or_DOI_PID1
PID_KIP::HAS_PARTS ..> File_PIDs


PID_KIP::Metadata ..> Metadata_PID1

Metadata_PID1 --> M_PID_KIP : adheres_to Profile

note right of PID_KIP
  currently no PID for 
  community specific metadata object 
  associated to data object
  (indicated by "Metadata" attribute)

  community specific info
  important to work with data
  should go there !!
 
  --> Kernel Info Profile for this also necessary ?!
end note


```

- KIT_PID.puml
```plantuml
title Helmholtz PID Kernel Information Profile


object File_or_Dataset_PID 


class Helmholtz_KIP {
+ **kernelInformationProfile** : PID :: 1 (mandatory)
+ **digitalObjectType** : PID :: 1 (mandatory)
+ **digitalObjectLocation** : URL :: 1+ (m and repeatable) 
+ **digitalObjectLocationAccessProtocol** : String :: 0/1 (optional)
+ **dateCreated** : ISO Date/Time :: 1 (mandatory) 
+ **dateModified** : ISO Date/Time :: 1 (mandatory) 
+ **underEmbargoUntil** : ISO Date/Time :: 0/1 (optional)
+ **version** : Integer or String :: 0/1 (optional,mandatory if predecessor) 
+ **license** : URL :: 1r (recommended)
+ **checksum** : Formatted String (datatype for this) :: 1 (mandatory if applicable) 
+ **signature** : analog mail standard :: 0+ (opt and repeatable)
+ **topic** : controlled vocabulary :: 0+ (opt and repeatable) 
+ **locationPreview/Sample** : URL :: 0+ (opt and repeatable)
+ **contact** : URL :: 0+ (opt and repeatable)
+ **hasMetadata** : PID :: 0+ (opt and repeatable)
+ **isMetadataFor** : PID :: 0/1 (optional) 
+ **wasGeneratedBy** : PID :: 0/1 (optional) --> Activity !!
+ **specializationOf** : PID :: 0+ (opt and repeatable)
+ **wasRevisionOf** : PID :: 0+ (opt and repeatable)
+ **hadPrimarySource** : PID :: 0+ (opt and repeatable)
+ **wasQuotedFrom** : PID :: 0+ (opt and repeatable) --> Subset !!
+ **alternateOf** : PID :: 0+ (opt and repeatable) --> Replica? !!
+ **provenanceGraph** : PID :: 0/1 (optional)

}


File_or_Dataset_PID --> Helmholtz_KIP : adheres_to Profile

center footer derived from https://oceanrep.geomar.de/id/eprint/57942/7/2022-12_HMC-Paper_2_PID-KIP_V1.pdf/n (S.Kindermann 20.07.2023)
```
- catalog.puml
```plantuml
cloud cloud
rectangle catalogs {
file intake_catalog
}
control gen
control ingest
storage file_system
actor data_manager
actor data_analysis
actor data_discovery


database global_index
interface catalog_API


file_system -d-> gen
cloud -d-> gen
gen -d-> intake_catalog
intake_catalog -> ingest
ingest -> global_index
global_index - catalog_API
gen - data_manager
data_manager - ingest
data_analysis -u-> catalog_API
data_analysis -u-> intake_catalog
data_discovery -u-> catalog_API

```

- FDO.puml

```plantuml
skinparam component {
    BackgroundColor<<**FDOF Typing System**>> LightBlue
    BackgroundColor<<**Metadata Record**>> LightGray 
    BackgroundColor<<**Identifier**>> LightGray
    BackgroundColor<<**Digital Object**>> LightGreen
}


[type] <<**FDOF Typing System**>> as type
[metadata] <<**Metadata Record**>> as metadata 
[pid] <<**Persistent ID**>> as pid
[do] <<**digital object**>> as do


pid --> do : Identifies
metadata --> do : Describes
type --> do : Characterises

```
- dask.puml
```plantuml
skinparam DefaultTextAlignment Center
skinparam ranksep 10

frame site1 {

interface dask_gw1
interface wps1
component "compute\ncluster" as cluster1
database "database includes\ndata from\nensemble\nA" as data1

dask_gw1 -- cluster1
wps1 -- cluster1
cluster1 -- data1

}
frame site2 {

interface dask_gw2
interface wps2
component "computer\ncluster" as cluster2
database "database includes\ndata from\nensemble\nB" as data2

dask_gw2 -- cluster2
wps2 -- cluster2
cluster2 -- data2

}

frame site3 {
    interface subset
    component "Server\nSystems" as cluster3
    database "Earth Observation\nData" as eodata
    subset -- cluster3
    cluster3 -- eodata
}

actor user1

rectangle dask [
    f(A,B) = g(A)-h(B)
]
user1 -- dask
dask -- dask_gw1
dask -- dask_gw2

actor user2

rectangle script [
    extract observations from site3
    extract pseudo-obs from ensemble B
    compare 
]

user2 -- script
script -- wps2 
script -- subset

```

