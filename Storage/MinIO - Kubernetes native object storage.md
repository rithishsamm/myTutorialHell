
**Lite stable half:**  
```
services:  
  minio:  
    image: minio/minio:RELEASE.2025-01-18T00-31-37Z  
    container_name: minio  
    ports:  
      - "9998:9000"  
      - "9999:9001"  
    environment:  
      MINIO_ROOT_USER: hexradmin  
      MINIO_ROOT_PASSWORD: hexradmin  
    command: server /data --console-address ":9001"  
    volumes:  
      - "F:/rithishsamm/data/volumes/minio/:/data/"  
    restart: unless-stopped  
```
  
---
  
**Full final stable version:**  
Image:  
```
docker pull minio/minio:RELEASE.2023-07-07T07-13-57Z  
```
  
Run:  
```
docker run -d --name minio \  
  -p 9000:9000 -p 9001:9001 \  
  -e MINIO_ROOT_USER=admin \  
  -e MINIO_ROOT_PASSWORD=Admin@123 \  
  -v /mnt/minio-data:/data \  
  minio/minio:RELEASE.2023-07-07T07-13-57Z server /data --console-address ":9001"
```

---

