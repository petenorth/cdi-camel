# CDI Camel QuickStart

This example shows how to work with Camel in the Java Container using CDI to configure components,
endpoints and beans.

This example is implemented using Java code with CDI injected resources such as Camel endpoints and Java beans.

### Building

The example can be built with

    mvn clean install

### Running the example locally

The example can be run locally using the following Maven goal:

    mvn clean install exec:java


### Running the example in fabric8

It is assumed that OpenShift platform is already running. If not you can find details how to [Install OpenShift at your site](https://docs.openshift.com/enterprise/3.1/install_config/install/index.html).

The example can be built and deployed using a single goal:

    mvn -Pf8-deploy

When the example runs in OpenShift, you can use the OpenShift client tool to inspect the status

To list all the running pods:

    oc get pods

Then find the name of the pod that runs this quickstart, and output the logs from the running pods with:

    oc logs <name of pod>

You can also use the OpenShift [web console](https://docs.openshift.com/enterprise/3.1/getting_started/developers/developers_console.html#tutorial-video) to manage the
running pods, and view logs and much more.


### Running the example using OpenShift S2I template

The example can also be built and run using the included S2I template quickstart-template.json.

The application can be run directly by first editing the template file and populating S2I build parameters, including the required parameter GIT_REPO and then executing the command:

    oc new-app -f quickstart-template.json

Alternatively the template file can be used to create an OpenShift application template by executing the command:

    oc create -f quickstart-template.json


### More details

You can find more details about running this [quickstart](http://fabric8.io/guide/quickstarts/running.html) on the website. This also includes instructions how to change the Docker image user and registry.

### Using with a build pipeline

    oc login <OPENSHIFT MASTER>:8443
    oc new-project development
    oc new-project testing
    oc project development
    oc create -f quickstart-template.json
    oc new-project cicd

in the new project 'add to project' by importing the pipeline/pipeline.yaml file (should see the jenkins service start)

    oc policy add-role-to-user edit system:serviceaccount:cicd:jenkins -n development
    oc policy add-role-to-user edit system:serviceaccount:cicd:jenkins -n testing
    oc policy add-role-to-group system:image-puller system:serviceaccounts:testing -n development
    oc project testing
    oc create deploymentconfig s2i-quickstart-cdi-camel --image=<DOCKER_REGISTRY_IP>:5000/development/s2i-quickstart-cdi-camel:promoteToQA

