```plantuml
sprite $businessProcess [16x16/16] {
FFFFFFFFFFFFFFFF
FFFFFFFFFFFFFFFF
FFFFFFFFFFFFFFFF
FFFFFFFFFFFFFFFF
FFFFFFFFFF0FFFFF
FFFFFFFFFF00FFFF
FF00000000000FFF
FF000000000000FF
FF00000000000FFF
FFFFFFFFFF00FFFF
FFFFFFFFFF0FFFFF
FFFFFFFFFFFFFFFF
FFFFFFFFFFFFFFFF
FFFFFFFFFFFFFFFF
FFFFFFFFFFFFFFFF
FFFFFFFFFFFFFFFF
}

 rectangle "Message Queue" <<$businessProcess>>

package "ESGF Data Node" {
[Publisher] 
database "Model Data"
}

package "Fed Search Node 1" {

   
    publishAPI1 - [PUB_Recv1]
    [MQ_Consumer1] 
    
    package STAC_FastAPI {
      database "Index1"
      [Index1] -right- STAC_API1
   
    }

    [PUB_Recv1] -u..> [Message Queue]
    [MQ_Consumer1] ..> [STAC_API1]
}

package "Fed Search Node 2" {

    publishAPI2 - [PUB_Recv2]
    [MQ_Consumer2]

    package STAC_FASTAPI2 {
    database "Index2"
    [Index2] -right- STAC_API2
}
    [MQ_Consumer2] ..> [STAC_API2]
    [PUB_Recv2] -u..> [Message Queue]
}

[Publisher] -r- [Model Data]
[Publisher] -d..>publishAPI1

[Message Queue] ..> [MQ_Consumer1]
[Message Queue] ..> [MQ_Consumer2]

```



```plantuml
sprite $businessProcess [16x16/16] {
FFFFFFFFFFFFFFFF
FFFFFFFFFFFFFFFF
FFFFFFFFFFFFFFFF
FFFFFFFFFFFFFFFF
FFFFFFFFFF0FFFFF
FFFFFFFFFF00FFFF
FF00000000000FFF
FF000000000000FF
FF00000000000FFF
FFFFFFFFFF00FFFF
FFFFFFFFFF0FFFFF
FFFFFFFFFFFFFFFF
FFFFFFFFFFFFFFFF
FFFFFFFFFFFFFFFF
FFFFFFFFFFFFFFFF
FFFFFFFFFFFFFFFF
}

 rectangle "Message Queue" <<$businessProcess>>


package "Fed Search Node 1" {

   
    publishAPI1 - [PUB_Recv1]
    database "Index1"
    [Index1] -right- STAC_API1
    [PUB_Recv1] ..> [Index1]
    [PUB_Recv1] -u..> [Message Queue]
}

package "Fed Search Node 2" {

    rectangle "Message Queue" <<$businessProcess>>

    publishAPI2 - [PUB_Recv2]
    database "Index2"
    [Index2] -right- STAC_API2
    [PUB_Recv2] ..> [Index2]
    [PUB_Recv2] -u..> [Message Queue]
}
```


```plantuml
sprite $businessProcess [16x16/16] {
FFFFFFFFFFFFFFFF
FFFFFFFFFFFFFFFF
FFFFFFFFFFFFFFFF
FFFFFFFFFFFFFFFF
FFFFFFFFFF0FFFFF
FFFFFFFFFF00FFFF
FF00000000000FFF
FF000000000000FF
FF00000000000FFF
FFFFFFFFFF00FFFF
FFFFFFFFFF0FFFFF
FFFFFFFFFFFFFFFF
FFFFFFFFFFFFFFFF
FFFFFFFFFFFFFFFF
FFFFFFFFFFFFFFFF
FFFFFFFFFFFFFFFF
}

  

rectangle "Message Queue" <<$businessProcess>>

package "DKRZ Tape Data Node" {
	database "CERA"
	[Tape_Pub]
	database "Tape"
	[CERA] -d- Tape
	[Tape_Pub] - [Tape]
}

  
  
  

package "DKRZ ESGF Data Node" {
	database "ESGF"
	[ESGF_Pub] -r- [ESGF]
}

  

package "DKRZ Data Node" {
	[DKRZ_Pub]
	database "High Res"
	[DKRZ_Pub] - [High Res]
}

package "Fed Search Node 1" {

	[MQ_Sub1]
		package STAC_FastAPI {
		database "Index1"
		[Index1] -right- STAC_API1
	}
	[PUB_Recv1] -u..> [Message Queue]
	[MQ_Sub1] ..> [STAC_API1]
}

package "DKRZ Search Node" {
	[MQ_Consumer2]
	package STAC_FASTAPI2 {
		database "Index2"
		[Index2] -right- DKRZ_STAC_API
	}
	[MQ_Sub2] ..> [DKRZ_STAC_API]
}
 
[ESGF_Pub] ..> STAC_API1
[DKRZ_Pub] ..> DKRZ_STAC_API
[Tape_Pub] ..> DKRZ_STAC_API

[Message Queue] ..> [MQ_Sub1]
[Message Queue] ..> [MQ_Sub2]

```


```plantuml
[Publisher] ..> stac_API
[STAC] -up-> stac_API
[STAC] ..> pub
[Message Queue] -up-> pub
[Message Queue] -down-> sub
[Index] -up..> sub
note left of [Publisher]
phils diagram
ESGFconf Washington
end note

@enduml


