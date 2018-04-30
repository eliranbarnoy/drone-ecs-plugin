# drone-ecs-plugin-link-duo

### Usage

#### The plugin allows handling logs with `awslogs` log driver, as well as using secrets in the designated ECS environment
#### Note that the last two parameters do not have a value so that the plugin would search them in the secrets
#### This plugin is intended to work specifically for two services that require one-way-link connection between them.
```yaml
deploy-production:
image:
    cluster: my-cluster
    region: us-east-1
    service: my-service
    family: my-service
    image_name: reg.company.com/image
    image_name_linked: reg.company.com/image
    image_tag: ${DRONE_BUILD_NUMBER}
    image_tag_linked: ${DRONE_BUILD_NUMBER}
    link_name: my-linked-service
    environment_variables:
      - BRANCH=${DRONE_BRANCH}
      - SHA1=${DRONE_COMMIT_SHA}
      - BUILD_NUMBER=${DRONE_BUILD_NUMBER}
      - AWS_ACCESS_KEY_ID=
      - AWS_SECRET_ACCESS_KEY=
    environment_variables_linked:
      - BRANCH=${DRONE_BRANCH}
      - SHA1=${DRONE_COMMIT_SHA}
    port_mappings:
      - 0 3000
    memory: 800
    log_driver: awslogs
    log_options:
      - awslogs-group=my-service
      - awslogs-region=us-east-1
    port_mappings_linked:
      - 0 3000
    memory_linked: 800
    log_driver_linked: awslogs
    log_options_linked:
      - awslogs-group=my-linked-service
      - awslogs-region=us-east-1
    secrets:
      - aws_access_key_id
      - aws_secret_access_key
    when:
      branch: [master]
```

Mounting `docker.sock` to `/var/run/docker.sock` can be done by adding the setting:
```yaml
mount_dockersock: true
```

