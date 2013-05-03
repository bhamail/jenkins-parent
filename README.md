jenkins-parent
==============

common parent of test jenkins app (a and b), defines distributionManagement info


Setup
-----
1. Change the pom.xml property *repoURLBase* to point to your repo root
2. Edit your .m2/settings.xml, adding an entry with the userId and password required to access your protected repositories:

        <servers>
            <server>
                <id>releasesId</id>
                <!-- @todo Edit the two lines below with your userId and password for maven release repo. -->
                <username>myuid</username>
                <password>mypwd</password>
            </server>

            <server>
                <id>snapshotsId</id>
                <!-- @todo Edit the two lines below with your userId and password for maven snapshot repo. -->
                <username>myuid</username>
                <password>mypwd</password>
            </server>
        </servers>

        <profiles>
            <profile>
                <id>repoProfileId</id>

                <activation>
                    <activeByDefault>true</activeByDefault>
                </activation>

                <repositories>
                    <repository>
                        <id>releasesId</id>
                        <!-- @todo Edit with url to maven release repo. -->
                        <url>http://yourdomain/yourrepohost/libs-releases-local/</url>
                        <snapshots>
                           <enabled>false</enabled>
                       </snapshots>
                   </repository>

                    <repository>
                        <id>snapshotsId</id>
                        <!-- @todo Edit with url to maven snapshot repo. -->
                        <url>http://yourdomain/yourrepohost/libs-snapshots-local/</url>
                        <releases>
                            <enabled>false</enabled>
                        </releases>
                   </repository>
                </repositories>
            </profile>
        </profiles>
3. Deploy this parent pom to your repo via: mvn deploy
        This will validate your configuration, as well as ensure the parent pom is available in your repo for use by project a and b.
4. After this project is deployed, you can setup two new jenkins jobs for project jenkins-a and jenkins-b (be sure to use
"maven" jobs). Verify the projects can build individually.
