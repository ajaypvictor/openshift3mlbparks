# Sample application for JBoss EAP on OpenShift v4.5

This sample application will create and deploy a Java EE application server as well as a MongoDB database.  The sample application will display a map and perform geospatial queries to populate the map with all Major League Baseball stadiums in the United States.

There are two options for this sample application depending on what you have available in your environment.  The options are to use JBoss EAP latest or Wildfly latest.  If you are using the openshift all-in-one image, use Wildfly.  If you are using OpenShift v4.5, Dedicated, or Enteprise, use EAP.


## Instructions for OCP environment on IBM Z

### Prerequisites

1. Deploy the JBoss Eap-7.3 image as well as the runtime.
```
oc replace --force -f \
https://raw.githubusercontent.com/jboss-container-images/jboss-eap-openshift-templates/eap73/eap73-openj9-image-stream.json

for resource in   eap73-amq-persistent-s2i.json   eap73-amq-s2i.json   eap73-basic-s2i.json   eap73-https-s2i.json   eap73-sso-s2i.json; do oc replace --force -f https://raw.githubusercontent.com/jboss-container-images/jboss-eap-openshift-templates/eap73/templates/${resource}; done
```
Reference: [Redhat JBoss EAP for OCP](https://access.redhat.com/documentation/en-us/red_hat_jboss_enterprise_application_platform/7.3/html/getting_started_with_jboss_eap_for_openshift_container_platform/build_run_java_app_s2i#deploy_eap_s2i)


2. Import mongo based image
```
oc import-image rhscl/mongodb-36-rhel7 --from=registry.access.redhat.com/rhscl/mongodb-36-rhel7 --confirm
```
Reference: [RH Source of Mongo 3.6](https://catalog.redhat.com/software/containers/rhscl/mongodb-36-rhel7/5aa62427ac3db95f19608637?architecture=s390x&container-tabs=gti&gti-tabs=unauthenticated)


3. Creating the environment using the mlbparks template.
```
oc create -f https://raw.githubusercontent.com/ajaypvictor/openshift3mlbparks/master/mlbparks-template-eap.json
oc new-app mlbparks-eap
```



## Quick instructions to just get this working on an OpenShift v4.5 deployment as a normal user

````
$ oc login https://yourOpenShiftServer
$ oc new-project mlbparks
````
If your environment (all-in-one) has Wildfly, use this:
`````
$ oc create -f https://raw.githubusercontent.com/gshipley/openshift3mlbparks/master/mlbparks-template-wildfly.json
$ oc new-app mlbparks-wildfly
`````
If your environment (Online 3, Dedicated, OSE) has EAP, use this:
`````
$ oc create -f https://raw.githubusercontent.com/gshipley/openshift3mlbparks/master/mlbparks-template-eap.json
$ oc new-app mlbparks-eap
``````
## Install template as cluster-admin for everyone to use

Load the template with cluster-admin user:

`````
# oc create -f https://raw.githubusercontent.com/gshipley/openshift3mlbparks/master/mlbparks-template-wildfly.json -n openshift
`````


