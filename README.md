# GRPC Generator Template

Fork this project and follow instructions.

This tool generates code from `.proto` files and upload constructed packages (`.proto` files with generated code) to desired repositories.

## How to use:
1. Edit `rootProject.name` variable in `settings.gradle` file. This will be the name of Java package.
2. Edit `package_info.json` file in order to specify package name and version for Python package (create file if it's absent).
3. Edit parameters of `setup.py` in `setup` function invocation such as: `author`, `author_email`, `url`. Do not edit the others.
4. Create a directory with the name `package_name` (as in Python) under `src/main/proto` directory (remove example files `Foo.proto` and `Bar.proto` if present).
5. Place your own `.proto` files in created directory.
6. Edit imports in your `.proto` files so that they look like <br>
`import "{package_name}/{proto_file_name}.proto"`
7. Edit paths in `python-service-generator` stage in Dockerfile. They should correspond to the project structure.

### Parameters
- `IMAGE_NAME` - name of Docker image
- `IMAGE_VERSION` - version of Docker image
- `APP_VERSION` - version of Java package
- `ARTIFACTORY_USER` - user for Java artifactory
- `ARTIFACTORY_PASS` - password for Java artifactory
- `ARTIFACTORY_REPO` - repository for Java artifactory
- `ARTIFACTORY_URL` - URL for Java artifactory
- `NEXUS_URL` - URL for Nexus (Java)
- `NEXUS_USER` - user for Nexus (Java)
- `NEXUS_PASS` - password for Nexus (Java)
- `PYPI_REPOSITORY_URL` - URL for Python package repository
- `PYPI_USER` - user for Python package repository
- `PYPI_PASSWORD` - password for Python package repository
- `APP_NAME` - name of Python package
- `APP_VERSION` - version of Python package

### Docker
You can run everything via Docker:
```
docker build --tag {IMAGE_NAME}:{IMAGE_VERSION} . --build-arg release_version=${APP_VERSION}
                                                  --build-arg artifactory_user=${ARTIFACTORY_USER}
                                                  --build-arg artifactory_password=${ARTIFACTORY_PASS}
                                                  --build-arg artifactory_deploy_repo_key=${ARTIFACTORY_REPO}
                                                  --build-arg artifactory_url=${ARTIFACTORY_URL}
                                                  --build-arg pypi_repository_url=${PYPI_REPOSITORY_URL}
                                                  --build-arg nexus_url=${NEXUS_URL}
                                                  --build-arg nexus_user=${NEXUS_USER}
                                                  --build-arg nexus_password=${NEXUS_PASS}
                                                  --build-arg pypi_user=${PYPI_USER}
                                                  --build-arg pypi_password=${PYPI_PASSWORD}
                                                  --build-arg app_name=${APP_NAME}
                                                  --build-arg app_version=${APP_VERSION}
```

### Java
If you wish to manually create and publish package, run these command:
``` 
gradle --no-daemon clean build publish artifactoryPublish \
       -Prelease_version=${RELEASE_VERSION} \
       -Partifactory_user=${ARTIFACTORY_USER} \
       -Partifactory_password=${ARTIFACTORY_PASSWORD} \
       -Partifactory_deploy_repo_key=${ARTIFACTORY_DEPLOY_REPO_KEY} \
       -Partifactory_url=${ARTIFACTORY_URL} \
       -Pnexus_url=${NEXUS_URL} \
       -Pnexus_user=${NEXUS_USER} \
       -Pnexus_password=${NEXUS_PASSWORD}
```

### Python
If you wish to manually create and publish package, run these commands:
```
pip install -r requirements.txt
python setup.py generate
python setup.py sdist
twine upload --repository-url ${PYPI_REPOSITORY_URL} --username ${PYPI_USER} --password ${PYPI_PASSWORD} dist/*
```
