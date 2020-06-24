Overview of the CI/CD Process of the Swiss Proximity Tracing System (PT-S)
==========================================================================
This document gives an overview over the CI/CD process used to deploy the red and black backends and additional services plus the black frontend. Tp produce reproducible builds either the io.github.zlika maven plugin is used or a hash is computed over the static files (see the respective repositories for more details).


Federal IT Steering Unit Network Policies
-----------------------------------------
As described in the [system overview](overview.md) the architecture is based on the network zone policy concept of the Federal IT Steering Unit (FITSU, https://www.isb.admin.ch/) - [Official Document](https://www.isb.admin.ch/dam/isb/de/dokumente/ikt-vorgaben/sicherheit/si003/Si003-Netzwerksicherheit_in_der_Bundesverwaltung_V2-0-d.pdf.download.pdf/Si003-Netzwerksicherheit_in_der_Bundesverwaltung_V2-0-d.pdf) - as well as the access matrix - [Official Document](https://www.isb.admin.ch/isb/de/home/ikt-vorgaben/sicherheit/si002-ikt-grundschutz_in_der_bundesverwaltung.html). As a result of these policies it is not possible to deploy directly from GitHub (Internet zone) to the SSZ (Shared Server Zone) zone.

Consequently we cannot directly deploy the artifacts in the public GitHub repositories to the Federal Office of Information Technology, Systems and Telecommunication (FOITT) PaaS platform but we have to pull either the code or the build artifacts and then deploy it from our internal infrastructure.

Build Pipeline for Black Backend
--------------------------------
For the black backend we have a step build and deployment process with a pipeline on GitHub and other open build components (Travis-CI and SonarCloud) bound togteher with Github Actions. For security reasons - so that nobody on GitHub side can temper with the build artifacts before they are deployed to the FOITT PaaS - we clone the sourcecode from GitHub and build the artifacts again internally and also run security, regression tests and quality scans. 

<img src="images/cicd_black_backend.png" width="600">
*Fig 1: CI/CD Process Black Backend*

Build Pipeline for Red Backend and Config Service
-------------------------------------------------
The DP3T artifacts are (red side) developed and build by Ubique in their repositories and these are used directly for the deplyoment on the FOITT PaaS as depictured on the following diagram: 

![CI/CD Process Red Backend and Config Service](/images/cicd_red_backend_config.png)
*Fig 2: CI/CD Process Red Backend and Config Service*
