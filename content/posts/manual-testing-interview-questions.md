---
title: "Manual Testing Interview Questions"
date: 2022-02-20
tags: ["testing"]
draft: true
author: "Collected from the internet"
---

(*As a fresher who is looking for opportunities to grow, I am currently learning Manual testing & mehtodologies. This post consists of a small collection of interview questions for SQA. You, who find this resource, I hope it'll be beneficial for you*)

Things to do:

- [ ] Adding proper figures for explantion.
- [ ] Adding more questions.

# About SDLC

<span style="color: red;">What is Software Development Life Cycle?</span>

<span style="color: green;"><ins>Ans:</ins></span> **SDLC** (*Software Development Life Cycle*) is the overall process of developing information system through a multi-step process from investigation of initial requirements through analysis, design, coding, testing, implementation and maintenance.

<span style="color: red;">What are the various models of SDLC?</span>

<span style="color: green;"><ins>Ans:</ins></span> There are many types of models. Among them:

- Waterfall model (*Linear Sequential Model*);
- Prototype Model;
- Rapid Application Development Model;
- Spiral Model;
- V-Model;

<span style="color: red;">What is waterfall model?</span>

<span style="color: green;"><ins>Ans:</ins></span> The **Waterfall model** is a sequential process in which progress is seen as flowing downwards (*like a waterfall*) through the phases of analysis, design, coding, testing, support and maintenance.

<span style="color: red;">What is spiral model?</span>

<span style="color: green;"><ins>Ans:</ins></span> In **Spiral Model**, activities are organized into many cycle. Each cycle begins with the

- identification of objectives for that cycle for
- different alternatives that are possible achieving the objectives.
- constraints that exist.
- different alternatives are evaluated based on the objectives and constraints.
- all the products developed during this cycle are reviewed.
- Further planning based on user comments is done.

<span style="color: red;">What is Rapid Application Development (*RAD*) Model?</span>

<span style="color: green;"><ins>Ans:</ins></span> **RAD** model is a type of incremental model. In RAD model the components or functions are developed in parallel as if they were mini projects.

The phases in RAD model are:

- **Business modeling**: The information flow is identified between various business functions.
- **Data modeling**: Information gathered from business modeling is used to define data objects that are needed for the business.
- **Process modeling**: Descriptions are created for adding, modifying, deleting or retrieving a data object.
- **Application generation**: Automated tools are used to convert process models into code and the actual system.
- **Testing and turnover**: Test new components and all the interfaces.

<span style="color: red;">What is V - model?</span>

<span style="color: green;"><ins>Ans:</ins></span> **V-model** is also called as *verification* and *validation* model.

In V model, project development and testing should go parallel. Verification phase should be carried out from SDLC where validation phase should be carried out from STLC.

# About different types of testing

<span style="color: red;">What is Unit testing?</span>

<span style="color: green;"><ins>Ans:</ins></span> This is first level of testing. It involves checking that each feature specified in the *Component Design* has been implemented in the component.

Developers do it, as they are the people who understand how a component works.

<span style="color: red;">What is Integration Testing?</span>

<span style="color: green;"><ins>Ans:</ins></span> Tested components are linked together to check if they work with each other. 

The tests are organized to check all the interfaces, until all the components have been built and interfaced to each other producing the whole system.

<span style="color: red;">What is System Testing?</span>

<span style="color: green;"><ins>Ans:</ins></span> Once the entire system has been built, it is tested against system specification.

**System testing** is not about checking the individual parts of the design, but about checking the system as a whole.

<span style="color: red;">What is Acceptance Testing?</span>

<span style="color: green;"><ins>Ans:</ins></span> **Acceptance Testing** is a level of the software testing where a system is tested for acceptability. It checks that the system delivers what was requested.

<span style="color: red;">What is Verification?</span>

<span style="color: green;"><ins>Ans:</ins></span> **Verification** is the process of confirming that software meets its specification. It involves reviews and meetings to evaluate documents, plans, code, requirements and specifications.

<span style="color: red;">What is Validation?</span>

<span style="color: green;"><ins>Ans:</ins></span> **Validation** is the process of confirming that software meets the user's requirements. Validation typically involves actual testing and takes place after verification are completed.

<span style="color: red;">What is Test Scenario?</span>

<span style="color: green;"><ins>Ans:</ins></span> **Test Scenario** is prepared before the actual testing starts.
It includes plans for testing product, number of team members, environmental condition, making test cases, making test plans and all the features that are to be tested for the product.

<span style="color: red;">What is Test Methodology?</span>

<span style="color: green;"><ins>Ans:</ins></span> **Test methodology** is a three step process

- creating a test strategy
- creating a test plan
- executing tests

<span style="color: red;">What is Test Strategy?</span>

<span style="color: green;"><ins>Ans:</ins></span> **Test strategy** documents -

- describes the roles, responsibilities and the resources required for the test and schedule.
- tells us how the software product will be tested.
- helps to review the test plan with the project team members executing tests.

<span style="color: red;">What is Test Plan?</span>

<span style="color: green;"><ins>Ans:</ins></span> **Test plan** is a document that describes the scope, approach, resources and schedules of internal testing activities.

<span style="color: red;">What is Requirement Test Matrix?</span>

<span style="color: green;"><ins>Ans:</ins></span> **Requirement Test Matrix** is a project management tool for tracking tool for tracking and managing testing efforts based on requirements throughout the project life cycle.

<span style="color: red;">What is Test Case?</span>

<span style="color: green;"><ins>Ans:</ins></span> **Test case** is a documents which contains set of test inputs, execution conditions, and expected output.

# About STLC

<span style="color: red;">What is STLC?</span>

<span style="color: green;"><ins>Ans:</ins></span> **Software Testing Life Cycle** (*STLC*) is the testing process, in which different activities are carried out to improve the quality of the product.

Steps in STLC are:

- Requirement Analysis
- Test planning
- Test Case Development
- Environment Setup
- Test Execution
- Test Cycle Closure

<span style="color: red;">What is Bug Life Cycle?</span>

<span style="color: green;"><ins>Ans:</ins></span> **Bug life cycle** (*also known as **Defect Life Cycle***) is the journey of a defect cycle, which a defect goes through during its lifetime.

It starts when defect is found and ends when a defect is closed, after ensuring it's not reproduced.
