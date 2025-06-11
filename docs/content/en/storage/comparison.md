| Feature                | Cinder ("rbd-fast")                  | LUKS (based on Cinder)                    | S3 (Object Storage)                                |
|------------------------|--------------------------------------|-------------------------------------------|----------------------------------------------------|
| **Type**               | Block storage                        | Encrypted block storage                   | Object-based cloud storage                         |
| **Provisioning**       | StorageClass: `rbd-fast`             | StorageClass: `luks`                      | OpenStack API, web console, CLI                    |
| **Access Pattern**     | ReadWriteOnce (RWO)                  | ReadWriteOnce (RWO)                       | HTTP(S), REST API, global, similar to RWX          |
| **Availability**       | Within a single zone                 | Within a single zone                      | Global                                             |
| **Redundancy**         | Zone-internal                        | Zone-internal                             | Cross-zone, geo-redundant                          |
| **Encryption**         | No                                   | Default, strong encryption                | Optional (SSE-C)                                   |
| **Access Control**     | OpenStack/Kubernetes roles           | OpenStack/Kubernetes roles                | IAM, ACLs, bucket policies                         |
| **Backup**             | Managed by customer                  | Managed by customer                       | Managed by customer                                |
| **Typical Use Cases**  | Persistent volumes for VMs/containers| Encrypted persistent volumes              | Backup, archiving, web applications, data lakes    |
