# 52°North SOS OSGeo-Live Contribution preparation

1. **Create SOS package**

      1. Checkout the branch:

         ```git checkout distribution/osgeolive```

      1. Merge with the latest release (replace `x.y.z` with the latest version):

         ```git merge x.y.z --no-ff```

      1. Fix conflicts.

      1. Build with maven: ```mvn clean install```.

1. **Example Data Update**

    1. Identify software versions of OSGeoLive via:

         1. tomcat version:
            ```
            user@osgeolive:~$ /usr/share/tomcat8/bin/version.sh
            ```
         1. postgreSQL and PostGIS:
            ```
            user@osgeolive:~$ sudo -u postgres psql
            postgres=# \c 52nSOS
            52nSOS0# SELECT version(), PostGIS_version();
            ```
         1. Update `SOS_REPO/webapp-osgeolive/docker/docker-compose.yml` to match
            the output of `version.sh`:
            ```
            db:
              image: mdillon/postgis:10-alpine
            [...]
            sos:
              image: tomcat:8.5-jre8-alpine
            ```

      1. **Optional**: Update the following files from the deployed 52N-SOS instance:

         * In the case of 52N-SOS configuration changes:

           ```configuration.json``` into ```webapp-osgeo-live/src/main/webapp/config```

         * In the case of datasource configuration changes:

           ```datasource.properties``` into ```webapp-osgeo-live/src/main/webapp/WEB-INF/config```

         * In the case of helgoland configuration changes:

           ```app-config.json``` into ```webapp-osgeo-live/src/main/webapp/static/client/helgoland/assets```

1. **Final Package**

    Run ```mvn clean install``` for the last time and copy the resulting ```52n-sos-osgeo-live-X.Y.tar.gz``` to the 52N webdav folder for OSGeo-Live ```https://52north.org/files/sensorweb/osgeo-live/```.

1. **Update Installer**:

    Update the properties

      * `SOS_TAR_NAME` to match the name of the new uploaded file.

      * `SOS_VERSION` to match the version of the current SOS deployed on OSGeoLive.

1. **Update Documentation**
