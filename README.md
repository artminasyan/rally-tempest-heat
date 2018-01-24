# Rally-tempest
Repo for build rally image with Tempest

Description:
1. keystonercv3 should be mounted to container
2. Destination folder for all logs should be mounted to container
3. You can use variable CUSTOM or SET or nothing to run all tests:
4. The image is available via following links: 
   * docker-prod-local.docker.mirantis.net/mirantis/oscore/rally-tempest (internal) 
   * docker-prod-local.artifactory.mirantis.com/mirantis/oscore/rally-tempest (external)

## Example start the container:

```
docker run --rm --net=host \
    -v /root/keystonercv3:/home/rally/keystonercv3 \
    -v /root/log_dir:/home/rally/rally_reports/ \
    -e SET=full \
    docker-prod-local.docker.mirantis.net/mirantis/oscore/rally-tempest:latest
```


## Used variables:

* **SOURCE_FILE** - Full path to *keystonercv3* file. By default `SOURCE_FILE=/home/rally/keystonercv3`
* **LOG_DIR** - Destination folder for uploading tempest's logs and results. By default `LOG_DIR /home/rally/rally_reports/`. If the folder is absent, it will be created automaticaly.
* **SET** - Predefined set for tempest tests. Can be *smoke*, *full*, *compute*, *identity*, *image*, *network*, *object_storage*, *orchestration*, *volume*, and *scenario*. By default `SET=smoke`
* **CUSTOM** - Custom parameters for tempest, for example `CUSTOM='--pattern <pass_to_tests>'` . If **CUSTOM** doesn't exist, a container uses **SET**
* **CONCURRENCY** - How many processes to use to run Tempest tests.  The default value `CONCURRENCY=0` auto-detects CPU count
* **TEMPEST_CONF** - A tempest.conf's file name. It is possible to use predefine tempest.conf or a custom file. In this case the file should be available in the `LOG_DIR`. The current image contains and supports the follow predefine tempest config files:
    1. `mcp.conf` supports latest feature and can be used with the most MCP model or like a template
    2. `aio_mcp.conf` doesn't support live migration and can be used with All-In-One model
    3. `lvm_mcp.conf` is used with most virtual models (LVM storage)
    4. `ceph_mcp.conf` is used with ceph-ha model (CEPH storage)
    
    By default `TEMPEST_CONF=aio_mcp.conf`
* **SKIP_LIST** - A skip.list's file name. It is possible to use predefine tempest.conf (`mcp_skip.list`) or a custom file. In this case the file should be available in the `LOG_DIR`. By default `SKIP_LIST=mcp_skip.list`
* **REPORT_SUFFIX** - Empty by default. If **REPORT_SUFFIX** is empty or not passed to docker, report name will use **SET** variable. Else, this variable will be a part of image name

## SSL/TLS support:

If the environment uses a self-signed certificate, please mount certificate's folder to the container:
`-v /etc/ssl/certs/:/etc/ssl/certs/`
