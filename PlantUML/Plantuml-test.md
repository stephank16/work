

```plantuml

left to right direction
skinparam shadowing false
skinparam defaultTextAlignment center
skinparam linetype ortho
skinparam packageStyle rectangle
skinparam componentStyle rectangle
skinparam ArrowThickness 1.2
skinparam ArrowColor #666666
skinparam package {
  BackgroundColor #F7F9FC
  BorderColor #AAB7C4
}
skinparam node {
  BackgroundColor #FFF5E6
  BorderColor #D98E04
}
skinparam cloud {
  BackgroundColor #F2F7FF
  BorderColor #6C8EBF
}
skinparam database {
  BackgroundColor #EAF3FF
  BorderColor #6C8EBF
}
skinparam queue {
  BackgroundColor #FDEDEC
  BorderColor #C0392B
}
skinparam artifact {
  BackgroundColor #EAF7EA
  BorderColor #4CAF50
}
skinparam rectangle {
  BackgroundColor #FFFFFF
  BorderColor #B0BEC5
}
skinparam legend {
  BackgroundColor #FFFFFF
  BorderColor #D0D7DE
}

title AWS-like Deployment View

legend right
|= Service |= Meaning |
| <back:EAF7EA>   </back> | Compute |
| <back:EAF3FF>   </back> | Data |
| <back:FDEDEC>   </back> | Metadata |
endlegend

cloud "EDSC Platform" as EDSC {

  package "ESGF Federation" as ESGF {

    package "EVES RI" as EVES {

      node "DKRZ" as DKRZ {
        artifact "Compute Services" as DKRZ_COMPUTE
        database "Data Services" as DKRZ_DATA
        queue "Metadata Services" as DKRZ_META
      }

      node "EU Climate Archive" as EUCA {
        artifact "Compute Services" as EUCA_COMPUTE
        database "Data Services" as EUCA_DATA
        queue "Metadata Services" as EUCA_META
      }
    }
  }

  package "Other EU RIs" as OTHER_RIS {
    artifact "Compute Services" as OR_COMPUTE
    database "Data Services" as OR_DATA
    queue "Metadata Services" as OR_META
  }
}

rectangle "Research Projects / User Communities" as USERS {
  component "EERIE / C4P7 / UDAG" as P1
  component "EVES RISE / Futura / Expect" as P2
  component "IRISCC / RI-Scale" as P3
}

DKRZ <--> EUCA : federation / exchange
EUCA <--> OTHER_RIS : interoperability

P1 ..> DKRZ : access
P2 ..> EUCA : access
P3 ..> OTHER_RIS : access



```




