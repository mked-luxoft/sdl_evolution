# Web HMI Infrastructure Enhancement

* Proposal: [SDL-NNNN](NNNN-web-hmi-infrastructure-enchancement.md)
* Author: [Maksym Ked](https://github.com/mked-luxoft), [Mykola Korniichuk](https://github.com/mkorniichuk)
* Status: **Draft**
* Impacted Platforms: [HMI]

## Introduction

 This proposal discusses approaches to improve Web HMI app's stability and long-term viability via introducing several common software engineering practices.

## Motivation

Web HMI started off as a tool for manual testing only, often compromising on quality standards common for SDL core or SDL test scripts in favor of the bare minumum functionality needed to test certain feature or verify fix for a defect. This proposal addresses the need to introduce practices that will negate technical debt on the application, will diminish codebase degradation, will  help track regression that may be introduced with new fatures, will make establishing the environment for manual testing easier as well as less time consuming, and make the Web HMI application overall more stable and reliable.


## Proposed solution

To achieve aforementioned goals, it is proposed to implement the following steps: 

- **Upgrade Ember JS version to 3.x**  
As of the moment of writing of this proposal, current version of Ember JS framework used by Web HMI is 1.0, which is before the first stable version of this framework. This constitutes a great technical debt and application is bound to stop working correctly once major browsers discontinue support of certain JS features. 
Another point advocating the upgrade to newer version of Ember JS is the increased versatility of current version. As a result, new functionality will be available for developers including: 
  - Ability to access the filesystem. During the implementation of certain features the need to read or write to filesystem may arise, resulting in a problem that cannot be solved by a classic web application. However, since version 2.x Ember JS applications are deployed as web servers which can use a component granting an access to filesystem.
  - Ability to establish Websocket connection. This may be used as alternative transport type for Web HMI.
  - Abaility to send OS signals which are handled by SDL, e.g. low voltage, ign off, etc

- **Cover basic functionality with Unit Tests as well as require new UTs to be added for each feature that is being merged into develop.**  
The aim of this point is to stop codebase degradation and introduce the ability to track possible regression. Newer versions of Ember JS do have unit testing facilities at developer's disposal, giving the ability to implement coverage of main web hmi components.

- **Move to ES6 standard as well as introducing coding style.**  
As of the moment of writing of this proposal, there is no established standard of coding style in Web HMI application. Standartizing can be achieved by agreeing on certain style to be maintained in Web HMI repository as well as introducing a linting utility that checks the style's validity and is able to fix it on demand. It is proposed to adopt [Google JS coding style](https://google.github.io/styleguide/jsguide.html) and use [ESLint](https://eslint.org/) as a tool to check validity of coding style.
Moreover, authors propose to use ES6 features during development, code reviews, etc.

- **Configure Continious Integration jobs to run checks on coding style as well as unit test on pushes and pull requests to develop.**  
Web HMI app presence on Continious Integration is crucial for long term viability of its codebase. Utilising style and unit test checks on pull request or push to develop branch will help to tackle issues that may arise during feature development of stabilization phases. Merge of a pull request should not be permitted unless all the checks were run successfully. 

- **Introduce a Dockerfile that can be build into the manual testing environment for each feature.**  
During the delivery of features that require non-trivial environment for manual testing, it became apparent that team members that did not directly participated in the development of the feature had troubles in configuring the environment. Despite the fact that as a rule a how-to is included, it may become outdated, be misleading or plain unclear. To address this problem, it is proposed to include a Dockerfile in the repositpry, which can be built into an image containing the environment. Dockerfile should be modified to accomodate a certain feature, and have a simple enough interface to grant the ability to be easily built and run by an engineer with no experience with a particular SDL feature.


## Potential downsides

Upgrading Ember JS version may result in inability to reuse certain portions of Web HMI code.
Moreover, new approach will rely on user having Docker on his system, which is an extra dependency comparing to old practice.

## Impact on existing code

Since the difference between Ember JS 1.0 and 3.x is significant, the impact on Web HMI code is expected to be quite substantial. Moreover, the effort on full manual regression testing also should be considered.

## Alternatives considered

- Gaining access to host's filesystem may be achieved by utilising Electron framework.
- Do not perform any enchancement to Web HMI app infrastructure.
