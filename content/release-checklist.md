Title: Release Checklist

# OpenWebBeans Release Checklist

Before performing the release you need to configure your environment if you haven't done it before.

 1. Publishing Maven Artifacts (https://www.apache.org/dev/publishing-maven-artifacts.html)
 2. Go to the section SETUP YOUR DEVELOPMENT ENVIRONMENT and generate the pgp key signature. Don't forget to distribute the public key step.  
    Generate PGP signature: https://blog.sonatype.com/2010/01/how-to-generate-pgp-signatures-with-maven/#.Vm9Km8q22-q
    
## Prepare

 1. JIRA Release Management
    - Make sure that all tickets for the release are properly marked as 'released' in our issue tracker.
    - Create ${version.next} (if not already done)
    - Move unresolved issues from ${version} (e.g. 2.0.8) to ${version.next} (e.g. 2.0.9)
  
  
 2. Update README
    - Export changelog from JIRA
    - add it to readme/README.txt
    - also update the version numbers in the preface  
  
  
 3. Release via MVN
    - mvn clean install -Papache-release
    - mvn release:prepare -DdryRun=true
    - mvn release:prepare -Dresume=false 
    - mvn release:perform  
  
    if the release:perform fails on the last step (commit the new version in trunk) but the tag was successfully created,
    you can deploy the tag manually to nexus:

    - commit the version change in trunk
    - checkout the tag and execute: mvn clean install deploy -Papache-release  
  
  
 4. Provide the staging repository 
    - login to nexus: https://repository.apache.org/#stagingRepositories
    - select the staging repository and "close" it
    - the final url should be similar to https://repository.apache.org/content/repositories/orgapacheopenwebbeans-1047  
  

 5. Provide assembly
    - SVN https://dist.apache.org/repos/dist/dev/openwebbeans/
    - Commit following files inside the ${version} (2.0.8) directory:
      - openwebbeans-2.0.8-source-release.zip
      - openwebbeans-2.0.8-source-release.zip.asc
      - openwebbeans-2.0.8-source-release.zip.sha512
      - openwebbeans-distribution-2.0.8-binary.tar.gz
      - openwebbeans-distribution-2.0.8-binary.tar.gz.asc
      - openwebbeans-distribution-2.0.8-binary.tar.gz.sha512 (must be generated manually)
      - openwebbeans-distribution-2.0.8-binary.zip
      - openwebbeans-distribution-2.0.8-binary.zip.asc
      - openwebbeans-distribution-2.0.8-binary.zip.sha512 (must be generated manually)  
    
    OR
    
    - announce the VOTE with links to the staging repo
       In this case you MUST provide at least the sha1 of the source-release.zip!
  
 6. Send the VOTE mail  
  
  
## After successful vote 

 1. Release from the staging repository
    - login to nexus: https://repository.apache.org/#stagingRepositories
    - select the staging repository and "release" it
    - the artifacts will be synced with the maven repos now  
  
  
 2. JIRA Release Management
    - Select the version and release it
    - bulk-transition all resolved tickets of ${version} to 'closed'
  
  
 3. Upload assembly
    - SVN https://dist.apache.org/repos/dist/release/openwebbeans/
    - commit same files as in 5) under the directory for the ${version} (2.0.8)
    - delete ${version.old} (2.0.7) directory  
  
  
 4. Apache Reporter Service
    - wait for the mail
    - login to https://reporter.apache.org/addrelease.html?openwebbeans
    - add the ${version} and release date  
  
  
## After the artifacts has been synced to central maven repo

 1. Create blog
    - Login to https://blogs.apache.org/owb/
    - Add a post for the new release, this will automatically picked up by the site deployment
   
  
 2. Update site
    - Browse https://github.com/apache/openwebbeans-site/tree/main/content
    - Edit news.md
    - Edit download.md
    - Committing will cause the site to be published
  
  
 3. Send release mail  
  
  
 4. Twitter  