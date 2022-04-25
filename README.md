This repository contains the cray-nexus helm chart. The chart creates the sonatype nexus pod and sets the pod up for use.


## Image Upgrade Procedure

Refer to the container images repository for (nexus3)[https://github.com/Cray-HPE/container-images/tree/main/docker.io/sonatype/nexus3] for information on how to create a new conatiner image.

After the new image version is created in this repository the chart needs to be updated. To update the chart to use the latest version there is two lines that reference the image pulled for nexus. In (values.yaml)[https://github.com/Cray-HPE/cray-nexus/blob/master/kubernetes/cray-nexus/values.yaml] both sonatype-nexus.nexus.imageTag and sonatype-nexus.deployment.initContainers.image refence the image version to be pulled and should be updated for the new version. To edit the chart two pull requests must be made. The first pull request should only update the chart while leaving the chart version the same. The second pull request should update the chart version while leaving everything else. Then a github release should be made from the version change commit. This will then build and upload the chart to (artifactory)[https://artifactory.algol60.net/ui/repos/tree/General/csm-helm-charts%2Fstable%2Fcray-nexus] which can be pulled by helm.