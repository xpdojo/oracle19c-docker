# Docker 테스트

- 원본 저장소를 참조해서 Oracle DB 컨테이너 이미지를 빌드해보려고 한다.
- 회사에서 Fedora 36에 설치하려 했더니 네트워크 속도가 느려서 패키지 설치가 너무 오래 걸린다.
  - 어디에 병목이 있는지는 확인하지 않음.
- 집에서 Windows 11 WSL2에서 실행해보니 빠르다.

## 컨테이너 이미지 빌드

```ps1
cd OracleDatabase/SingleInstance/dockerfiles
./buildContainerImage.sh -v 19.3.0 -e
```

아래는 정상적으로 빌드되었을 경우 출력 메시지다.

```ps1
WARNING: No blkio throttle.read_bps_device support
WARNING: No blkio throttle.write_bps_device support
WARNING: No blkio throttle.read_iops_device support
WARNING: No blkio throttle.write_iops_device support
Checking Docker version.
Checking if required packages are present and valid...
LINUX.X64_193000_db_home.zip: OK
==========================
Container runtime info:
Client:
 Context:    default
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc., v0.8.2)
  compose: Docker Compose (Docker Inc., v2.6.1)
  extension: Manages Docker extensions (Docker Inc., v0.2.7)
  sbom: View the packaged-based Software Bill Of Materials (SBOM) for an image (Anchore Inc., 0.6.0)
  scan: Docker Scan (Docker Inc., v0.17.0)

Server:
 Containers: 1
  Running: 1
  Paused: 0
  Stopped: 0
 Images: 15
 Server Version: 20.10.17
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runtime.v1.linux runc io.containerd.runc.v2
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 10c12954828e7c7c9b6e0ea9b0c02b01407d3ae1
 runc version: v1.1.2-0-ga916309
 init version: de40ad0
 Security Options:
  seccomp
   Profile: default
 Kernel Version: 5.10.16.3-microsoft-standard-WSL2
 Operating System: Docker Desktop
 OSType: linux
 Architecture: x86_64
 CPUs: 12
 Total Memory: 15.6GiB
 Name: docker-desktoop
 ID: K2EP:OX26:6SKL:MIRY:S7IZ:DKWM:SNJU:B6N4:DMVM:6FGF:Z3L6:42I5
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 HTTP Proxy: http.docker.internal:3128
 HTTPS Proxy: http.docker.internal:3128
 No Proxy: hubproxy.docker.internal
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  hubproxy.docker.internal:5000
  127.0.0.0/8
 Live Restore Enabled: false

WARNING: No blkio throttle.read_bps_device support
WARNING: No blkio throttle.write_bps_device support
WARNING: No blkio throttle.read_iops_device support
WARNING: No blkio throttle.write_iops_device support
==========================
Building image 'oracle/database:19.3.0-ee' ...
[+] Building 250.4s (16/16) FINISHED
 => [internal] load build definition from Dockerfile                                                                                                                    0.1s
 => => transferring dockerfile: 4.90kB                                                                                                                                  0.0s
 => [internal] load .dockerignore                                                                                                                                       0.1s
 => => transferring context: 2B                                                                                                                                         0.0s
 => [internal] load metadata for docker.io/library/oraclelinux:7-slim                                                                                                   9.9s
 => [auth] library/oraclelinux:pull token for registry-1.docker.io                                                                                                      0.0s
 => [base 1/4] FROM docker.io/library/oraclelinux:7-slim@sha256:99f2f7104df8f71643a5bae052c5fa839030e773f653f931d4de71f787d97acc                                        4.8s
 => => resolve docker.io/library/oraclelinux:7-slim@sha256:99f2f7104df8f71643a5bae052c5fa839030e773f653f931d4de71f787d97acc                                             0.0s
 => => sha256:99f2f7104df8f71643a5bae052c5fa839030e773f653f931d4de71f787d97acc 547B / 547B                                                                              0.0s
 => => sha256:da19b7361d378c23b1010026169c7ae6cbba00f98659b16ef945ebf8d5acbfce 529B / 529B                                                                              0.0s
 => => sha256:41388a53234b5a845ed220112cc9ea6d1eb6344f02e3ee7e74e5c1369d7a50c6 1.48kB / 1.48kB                                                                          0.0s
 => => sha256:66fb3478003364657ac7751c40e41a135e02975f9459f652b1df23e3896b5fac 48.76MB / 48.76MB                                                                        2.0s
 => => extracting sha256:66fb3478003364657ac7751c40e41a135e02975f9459f652b1df23e3896b5fac                                                                               2.3s
 => [internal] load build context                                                                                                                                      19.1s
 => => transferring context: 3.06GB                                                                                                                                    19.1s
 => [base 2/4] COPY setupLinuxEnv.sh checkSpace.sh /opt/install/                                                                                                        1.2s
 => [base 3/4] COPY runOracle.sh startDB.sh createDB.sh createObserver.sh dbca.rsp.tmpl setPassword.sh checkDBStatus.sh runUserScripts.sh relinkOracleBinary.sh /opt/o  0.1s
 => [base 4/4] RUN chmod ug+x /opt/install/*.sh &&     sync &&     /opt/install/checkSpace.sh &&     /opt/install/setupLinuxEnv.sh &&     rm -rf /opt/install          63.8s
 => [builder 1/2] COPY --chown=oracle:dba LINUX.X64_193000_db_home.zip db_inst.rsp installDBBinaries.sh /opt/install/                                                   2.7s
 => [builder 2/2] RUN chmod ug+x /opt/install/*.sh &&     sync &&     /opt/install/installDBBinaries.sh ee                                                             95.2s
 => [stage-2 1/4] COPY --chown=oracle:dba --from=builder /opt/oracle /opt/oracle                                                                                       17.1s
 => [stage-2 2/4] RUN /opt/oracle/oraInventory/orainstRoot.sh &&     /opt/oracle/product/19c/dbhome_1/root.sh                                                           0.9s
 => [stage-2 3/4] WORKDIR /home/oracle                                                                                                                                  0.2s
 => [stage-2 4/4] RUN echo 'ORACLE_SID=${ORACLE_SID:-ORCLCDB}; export ORACLE_SID=${ORACLE_SID^^}' > .bashrc                                                             0.6s
 => exporting to image                                                                                                                                                 22.9s
 => => exporting layers                                                                                                                                                22.8s
 => => writing image sha256:0c6db9c32a1ae074ccf082d225247b371ddcb98deac53e45539301265637c1c9                                                                            0.0s
 => => naming to docker.io/oracle/database:19.3.0-ee                                                                                                                    0.0s

Use 'docker scan' to run Snyk tests against images to find vulnerabilities and learn how to fix them


  Oracle Database container image for 'ee' version 19.3.0 is ready to be extended:

    --> oracle/database:19.3.0-ee

  Build completed in 251 seconds.
```

```ps1
> docker images
oracle/database                      19.3.0-ee           0c6db9c32a1a   8 minutes ago   6.53GB
```

```ps1
docker tag oracle/database:19.3.0-ee markruler/oracledb:19.3.0-ee
docker push markruler/oracledb:19.3.0-ee
```

- [markruler/oracledb](https://hub.docker.com/repository/docker/markruler/oracledb)

## 실행

```ps1
# docker run --name oracledb -p 1521:1521 -p 5500:5500 -e ORACLE_PDB=orcl -e ORACLE_PWD=password -e INIT_SGA_SIZE=3000 -e INIT_PGA_SIZE=1000 -d markruler/oracledb:19.3.0-ee
docker-compose up -d
# docker-compose logs -f
docker logs oracledb -f
```

```sh
docker exec -it oracledb bash
```

```ps1
docker exec -it oracledb cat /opt/oracle/product/19c/dbhome_1/network/admin/listener.ora

LISTENER =
(DESCRIPTION_LIST =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1))
    (ADDRESS = (PROTOCOL = TCP)(HOST = 0.0.0.0)(PORT = 1521))
  )
)

DEDICATED_THROUGH_BROKER_LISTENER=ON
DIAG_ADR_ENABLED = off
```

## SQL*Plus

- DBeaver에서는 CDB로 잘 연결되지만 DataGrip은 연결되지 않음.
- GUI Client에서 PDB로 연결 방법 모름.

```sh
sqlplus / as sysdba
SQL>
```

```sh
sqlplus /nolog
SQL> connect / as sysdba
```

### User 생성

```sql
-- C## -> CDB: ORCL
-- https://stackoverflow.com/questions/33330968/error-ora-65096-invalid-common-user-or-role-name-in-oracle
create user c##username identified by password;
grant connect, resource, dba to username;
```

```sql
connect username/password;

-- 현재 사용자 조회
select sys_context('userenv', 'current_user') from dual;
```

### 모든 User 조회

```sql
set linesize 200
col username for a20 word_wrapped
col default_collation for a20 word_wrapped
select * from all_users;
```

### CDB:PDB

```sql
show con_name
-- orclcdb

show pdbs
-- orclpdb1

alter session set container=orclpdb1;
-- Session altered.
```
