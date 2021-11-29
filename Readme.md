# Soft Acceleo

Soft Acceleo is a repository used to store a maven repository for the releases of [Acceleo](https://www.eclipse.org/acceleo/) that are not available in the [Eclipse Maven repository](https://repo.eclipse.org)

The main idea is to retrieve Acceleo release from its p2 update site and to convert it to a maven repository.

Acceleo update sites are located : https://download.eclipse.org/acceleo/updates/releases/

# How to install new version

1. Install Maven
2. In your Github account , create a personnal access token (PAT) with read/write packages rights and save these informations for in %User%/.m2/settings.xml file :
```
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 https://maven.apache.org/xsd/settings-1.0.0.xsd">
      <servers>
		<server>
			<id>github</id>
			<username>[GITHUB USERNAME]</username>
			<password>[GENERATED ACCESS TOKEN]</password>      
		</server>
	  </servers>
</settings>
```
3. Use one of the two following methods to clone and install the artifacts as a maven repository on github.
   * only an Windows
        1. Download from https://github.com/KinoriTech/p2mvn latest released version
        2. Unzip and Execute p2maven.exe and define the following parameters
            - p2 update site : Source URL (e.g https://download.eclipse.org/acceleo/updates/releases/3.7/R202102190929/)
            - d for deploy
            - maven repository url : https://maven.pkg.github.com/masagroup/soft.acceleo
            - maven repository id : github
        3. Execute %User%/p2mvn.org.eclipse.acceleo/deploy.bat 
   * on all platforms
        1. Clone p2 update site with eclipse via the command lines:
        ```
        eclipse -nosplash -verbose
        -application org.eclipse.equinox.p2.metadata.repository.mirrorApplication
        -source Insert Source URL (e.g https://download.eclipse.org/acceleo/updates/releases/3.7/R202102190929/)
        -destination Insert Destination URL (e.g. file:/tmp/Acceleo-Mirror/)
        ```
        ```
        eclipse -nosplash -verbose
        -application org.eclipse.equinox.p2.artifact.repository.mirrorApplication
        -source Insert Source URL (e.g https://download.eclipse.org/acceleo/updates/releases/3.7/R202102190929/)
        -destination Insert Destination URL (e.g. file:/tmp/Acceleo-Mirror/)
        ```
        2. For each jar file in the cloned mirror, execute the following command line:
        ```
        mvn deploy:deploy-file -DgroupId=org.eclipse.acceleo -DartifactId=[artifactID] -Dversion=[version] -Dpackaging=jar -Dfile=[artifactID]_[version].jar -DrepositoryId=github -Durl=https://maven.pkg.github.com/masagroup/soft.acceleo
        ```