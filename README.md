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
        This will validate your configuration, as well as ensure the parent pom is available in your repo for use by project A and B.
4. After this project is deployed, you can setup two new jenkins jobs for project jenkins-a and jenkins-b (be sure to use
"maven" jobs). Use maven goal: clean deploy. Verify the projects can build individually. After you have built both jobs in Jenkins, make sure project B
appears as a "downstream" project from project A. Similarly, project A should appear as "upstream" when viewed from project B.
Verify when you manually trigger a build of project A, that project B automatically builds also.
5. To reproduce the permissions problem, run on project A:

        mvn release:prepare
        mvn release:perform

 This will increment the version number of A (and publish a release version of a.jar).
For example, project A started at version 1.0-SNAPSHOT, was released as version 1.0, and is now at version 1.1-SNAPSHOT.
(Note: Ensure no build of project A is triggered during the next steps, for example due to scm polling)

Now edit the pom.xml of Project B to reference the new SNAPSHOT version of a.jar (for example: 1.1-SNAPSHOT).
Commit this change to Project B's pom.xml to the source bank.
Notice that at this point, the new SNAPSHOT version of a.jar has never been published to the repository. It will only be published when Jenkins runs project A.
 Now you can manually trigger a build of Project A.

The build will likely fail during the downstream build of Project B. This is because of the delay in Artifactory
asynchronously applying permissions to newly published a.jar (thus Project B fails when trying to use the new a.jar).

