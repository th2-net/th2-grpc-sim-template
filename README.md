# gRPC Generator Template

Fork this project and follow instructions to start your custom project.

This tool generates code from `.proto` files and upload built packages (`.proto` files and generated code) to specified repositories.

## How to use:
#. Create a directory with the same name as project name (use underscores instead of dashes) under `src/main/proto` directory (remove other files and directories if they exist).
#. Place your custom `.proto` files in created directory. Pay attention to `package` specifier and `import` statements.
#. Edit `vcs_url` property in gradle.properties file. It will be published onto bintray repository.
#. Edit `rootProject.name` variable in `settings.gradle` file. This will be the name of Java package.
#. Edit parameters of `setup.py` in `setup` function invocation such as: `author`, `author_email`, `url`. Do not edit the others.

Note that the name of created directory under `src/main/proto` directory is used in Python (it's a package name).

See [Parameters](#parameters) section.

### Java
If you wish to manually create and publish package for Java, run these command:
```
gradle --no-daemon clean build publish artifactoryPublish \
       -Pbintray_user=${BINTRAY_USER} \
       -Pbintray_key=${BINTRAY_KEY}
```
See [Parameters](#parameters) section.

### Python
If you wish to manually create and publish package for Python:
1. Edit `package_info.json` file in order to specify name and version for package (create file if it's absent).
2. Run these commands:
```
pip install -r requirements.txt
python setup.py generate
python setup.py sdist
twine upload --repository-url ${PYPI_REPOSITORY_URL} --username ${PYPI_USER} --password ${PYPI_PASSWORD} dist/*
```
See [Parameters](#parameters) section.
