# Web HMI Infrastructure Enhancement

* Proposal: [SDL-NNNN](NNNN-web-hmi-infrastructure-enchancement.md)
* Author: [Maksym Ked, Mykola Korniichuk]()
* Status: **Draft**
* Impacted Platforms: [HMI]

## Introduction

 This proposal discusses approaches to make Web HMI great again.

## Motivation

Web HMI started off as a tool for manual testing only, often compromising on quality standards common for SDL core or SDL test scripts in favor of the bare minumum functionality needed to test certain feature or verify fix for a defect. This proposal addresses the need to introduce practices that will negate technical debt on the application, will diminish codebase degradation, will  help track regression that may be introduced with new fatures, will make establishing the environment for manual testing easier as well as less time consuming, will make the Web HMI application overall more stable and reliable.


## Proposed solution

To achieve aforementioned goals, it is proposed to implement the following steps: 

- Upgrade Ember JS version to 3.x
As of the moment of writing of this proposal, current version of Ember JS framework used by Web HMI is 1.0, which is before the first stable version of this framework. This constitutes a great technical debt and application is bound to stop working correctly once major browsers discontinue support of certain JS features. 
Another point advocating the upgrade to newer version of Ember JS is the ability to access the filesystem. During the implementation of certain features the need to read or write to filesystem may arise, resulting in a problem that cannot be solved by a classic web application. However, since version 2.x Ember JS applications are deployed as web servers which can use a component granting an access to filesystem.

- Cover basic functionality with Unit Tests as well as require new UTs to be added for each feature that is being merged into develop.
The aim of this point is to stop codebase degradation and introduce the ability to track possible regression. Newer versions of Ember JS do have unit testing facilities at developer's disposal, giving the ability to implement coverage of main web hmi components.

- Move to ES6 standard as well as introducing coding style.
As of the moment of writing of this proposal, there is no established standard of coding style in Web HMI application. Standartizing can be achieved by agreeing on certain style to be maintained in Web HMI repository as well as introducing a linting script that checks the style's validity and is able to fix it on demand.
Moreover, it is proposed to require using ES6 features during development, code reviews, etc.

- Configure Continious Integration jobs to run checks on coding style as well as unit test on pushes and pull requests to develop.
The need to use CI is thoroughly described in [CI proposal]. As of now, there is no way to track possible regression besides manual testing. 


- Introduce a Dockerfile that can be build into the manual testing environment for each feature.
During the delivery of features that require non-trivial environment for manual testing, it became apparent that team members that did not directly participated in the development of the feature had troubles in configuring the environment. Despite the fact that as a rule a how-to is included, it may become outdated, be misleading or plain unclear. To address this problem, it is proposed to include a Dockerfile in the repositpry, which can be built into an image containing the environment. Dockerfile should be modified to accomodate a certain feature, and have a simple enough interface to grant the ability to be easily built and run by an engineer with no experience with a particular SDL feature.


## Potential downsides
N/A

## Impact on existing code

Since the difference between Ember JS 1.0 and 3.x is significant, the impact on Web HMI code is expected to be quite substantial. Moreover, the effort on full manual regression testing also should be considered.

## Alternatives considered

N/A
