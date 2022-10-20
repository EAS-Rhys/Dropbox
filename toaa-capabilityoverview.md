
## TO Application Automation Capability Technical Overview


### Introduction
App Automation is delivering a user consumable application deployment capability based on a number of modular ansible roles customised to deliver the capabilities required. Each of the modular ansible roles are known as ```capabilities```. Capabilities are essentially ansible roles created to simplify the consumption of ansible functionality by application owners to deliver an automated deployment process for their application.

### Capability structure
A capability is a role, that in turn may make use of multiple other internal, community created or vendor created ```roles``` or ```collections``` in order to simplify their consumption whilst still allowing the flexible use of the underlying interfaces by more advanced users and/or applications.
Roles and collections are bundles together into many task definitions within a capability and conditionally executed based on the existence and values in their associated ```directives```. Directives are used as the method to select and inform the bundled activities relevant to a deployment task within a capability.

##### Guiding Principles
In order to deliver the best business benefit the following considerations should be made when creating capabilities:

> - Directives shouldn't re-implement capability already existing in another capability or role.
> - All directives should be completely modular in nature and contained within the most suitable capability. i.e. file copy may be required for an IIS configuration, but will also be required for others so should form a fileutils capability and be consumed independently.
> - Directives should simplify and reduce the amount of inputs required by a user whilst still allowing additional inputs to be provided as accepted by the underlying interface.
> - Directives parameters should have sensible defaults for as many inputs as possible. These should be driven by group best practices or common implementation patterns wherever possible.
> - Capabilities MUST be version controlled with pinned release versions available with tagged releases. This must be accompanied by Changelog and Release notes notifiying users of breaking changes.
> - Wherever possible directives should be OS agnostic. Readme documents and meta files should both describe the supported platforms for use. Badges must be present in the README.md and CAPABILITY.md for ease of user consumption.
> - OS Native methods must always be used with Command and shell interactions used as an absolute last resort. Command/Shell usage must be clearly stated in documentation.
> - Capability directives must specify in documentation wether they support check_mode and diff_mode to assert the level of idempotency.

##### Naming
All capabilities must be named as per the following format:
>```<capabilityname>-capability```

capabilityname must be alphanumeric and contain no spaces or special characters. i.e. ```osutils-capability```

##### File/Folder Structure


📦```<name>```-capability <br>
 ┣ 📂defaults <br>
 ┃ ┗ 📜main.yml				 ```Sensible default values that can be overriden ``` <br>
 ┣ 📂handlers <br>
 ┃ ┗ 📜main.yml				```Actions that happen as a result of a task``` <br>
 ┣ 📂meta <br>
 ┃ ┗ 📜main.yml				```Role meta: name,version,license,dependencies etc``` <br>
 ┣ 📂molecule 					```Testing framework for ansible roles/playbooks``` <br>
 ┃ ┗ 📂default <br>
 ┃    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;┣ 📜converge.yml <br>
 ┃    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;┣ 📜molecule.yml <br>
 ┃    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;┗ 📜verify.yml <br>
 ┣ 📂tasks <br>
 ┃ ┣ 📜main.yml 				```Default Entrypoint: used to decided which tasks are included``` <br>
 ┃ ┣ 📜directive1.yml 	```Bundled piece of functionality i.e. iis_website``` <br>
 ┃ ┗ 📜directive2.yml <br>
 ┣ 📂tests <br>
 ┃ ┣ 📜main.yml 				```Used for pre test setup and inclusion criteria``` <br>
 ┃ ┣ 📜directive1.yml 	```Test for bundled directive``` <br>
 ┃ ┗ 📜directive2.yml <br>
 ┣ 📂vars <br>
 ┃ ┗ 📜main.yml				```Complete list of vars for testing all directives``` <br>
 ┣ 📜.gitignore <br>
 ┣ 📜CHANGELOG.md 	```Log of all release and unreleased changes to the capability``` <br>
 ┣ 📜validation.yaml 		```JSONSchema validation file for the directive structure and values``` <br>
 ┣ 📜CAPABILITY.md 		```Description of the capability and specification for all included directives``` <br>
 ┗ 📜README.md 			```Complete documentation of capability including all directives and dependencies``` <br>

##### Consumption

Each capability will have it's own complete documentation in it's README.md file. A collated ```Capability Index``` will be created in the etc Github pages repository. This will be generated dynamically from the CAPABILITY.md from each capability repository. The collated index can be found here: LINK
