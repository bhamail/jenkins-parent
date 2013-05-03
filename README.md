jenkins-parent
==============

common parent of test jenkins app (a and b), defines distributionManagement info


Setup
-----
1. Change the pom.xml property *repoURLBase* to point to your repo root
2. Edit your .m2/settings.xml, adding an entry with the userId and password required to access your protected repositories:

        <profiles>
            <profile>
                <id>repoProfileId</id>

                <activation>
                    <activeByDefault>true</activeByDefault>
                </activation>

                <repositories>
                    <repository>
                        <id>releasesId</id>
                        <url>http://yourdomain/yourrepohost/libs-releases-local/</url>
                        <snapshots>
                           <enabled>false</enabled>
                       </snapshots>
                   </repository>

                    <repository>
                        <id>snapshotsId</id>
                        <url>http://yourdomain/yourrepohost/libs-snapshots-local/</url>
                        <releases>
                            <enabled>false</enabled>
                        </releases>
                   </repository>
                </repositories>

            </profile>
        </profiles>
3. Deploy this parent pom to your repo via: mvn deploy
        This will validate you configuration, as well as ensure the parent pom is available for use by project a and b.
