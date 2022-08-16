# Student-friendly pipeline documentation

This repository shows an example of a Github workflow that evaluates the quality of a Java project and reports:
- [Design Metrics](#metrics)
- [Code level style checks](#code-quality)
- [Code smells](#code-smells)

The workflow works by creating a report for pull request submitted to the repository. For example, see the [pull request in this repository](https://github.com/tupac-rocher/student-friendly-pipeline-example/pull/3).

## Usage
 
 In order to get this feedback report you need to add configurations to your Maven project:
 - [Workflow configuration](#workflow-configuration): The workflow configuration file for Github Actions is report.yml
 - [Maven configuration](#maven-configuration): Add 2 plugins to you pom.xml configuration file
 - [Checkstyle configuration](#checkstyle-configuration): Add the Checkstyle configuration file for the Checkstyle Maven plugin from this repository: checkstyle.xml
 
### **Workflow configuration**

The report.yml file needs to be placed at this path from the root of your project so it gets recognized by Github: ./.github/workflows)

This file is available at the same path on this repository.

Here is the explanation of the report.yml file

The pipeline is divided into 3 jobs:
- [build](#build)
- [upload-designite-artifact](#upload-designite-artifact)
- [report](#report)

---
#### **build**
**actions used**: actions/checkout@v3, actions/setup-java@v3

**description**: 

 
The purpose of this job is to allow the user to know directly if the code builds, tests pass and a package can be created before doing the other jobs.

---
#### **upload-designite-artifact**
**actions used**: GuillaumeFalourd/clone-github-repo-action@v2, actions/upload-artifact@v3

**description**:

The purpose of this job is to upload the designite jar to execute the tool in the last job

---
#### **report**
**actions used**: actions/checkout@v3, actions/setup-java@v3, actions/download-artifact@v3, robinraju/release-downloader@v1.4, montudor/action-zip@v1, tupac-rocher/mvn-student-friendly-report@v2.2, thollander/actions-comment-pull-request@v1

**description**: 

This job is the core of the pipeline. It will execute the Maven goals of the Maven plugins (jacoco-maven-plugin, maven-checkstyle-plugin) in order to generate the analytic files. 

It will download the Designite exectuable artifact and execute it against the project to generate the analytic files.

Regarding the metrics, to use CK it will clone its repository, since it is a Maven project it will install it, and finally execute its goal on the current project, this process is due to the way described by the CK README file to use the tool, eventually the analytic files are generated. 
To use the JaSoMe tool, the process is to download the release, unzip it and execute it against the project to generate the analytic files.

Now that all the files have been generated. The action corresponding to this repository will be used to aggregate and format the information into a single output that will represent a report in a Markdown formatted String variable.

The Markdown formatted String is used with an action that will comment the pull request.

### **Maven configuration**
 
 
 Your project should include 2 Maven plugins in the build tag of the pom.xml file. 
 
 You can refer to the pom.xml file on this repository.
 Here is a look at the configuration of this pom.xml file:
- jacoco-maven-plugin (verion: 0.8.7, group-id: org.jacoco)
```xml
<plugin>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <version>0.8.7</version>
    <executions>
        <execution>
            <id>prepare-agent</id>
            <goals>
                <goal>prepare-agent</goal>
            </goals>
        </execution>
        <execution>
            <id>report</id>
            <phase>test</phase>
            <goals>
                <goal>report</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```
The first goal, "prepare-agent", will prepare the JaCoCo runtime agent to record the execution data.<br>
The second goal, "report", will use the execution data recorded to generate code coverage reports.<br>
This second goal is tied to the "test" phase of the Maven lifecycle, this means that the goal will be trigger of the compilation of this phase.<br>
The test coverage report can be found at target/site/jacoco/index.html.

- maven-checkstyle-plugin (version: 3.1.2, group-id: org.apache.maven.plugins)

```xml
<plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-checkstyle-plugin</artifactId>
      <version>3.1.2</version>
      <configuration>
        <configLocation>checkstyle.xml</configLocation>
        <encoding>UTF-8</encoding>
      </configuration>
</plugin>
```

### **Checkstyle configuration**

The checkstyle.xml needs to be placed at the root of your project.
This file is available at the same path on this repository.

---


## Feedback Report

The feedback report provides insights upon the code quality of the project implemented the workflow. You will find insights about encapsulation, inheritance, coupling, cohesion among other aspects of Object Oriented Programming.

### Metrics

Metrics are quantitave measures that give you insights upon different aspects of your code quality.
The metric section is divided into 2 categories, class-level metrics and method-level metrics.


#### Class Level
| Metric | Category | Range | Description | Interpretation
| ------ | -------- | ----------- | - | - |
| FAN-IN | Coupling | 0..N | The number of input dependencies a class has | High value: a class that is depended on a lot by other classes |
| FAN-OUT | Coupling | 0..N | The number of output dependencies a class has | High value: a class that depends a lot on other classes |
| TCC | Cohesion | 0..1 | TCC measures the cohesion of a class via direct connections between visible methods. A direct connection between happens when these methods or their invocation trees access the same class variable. | High value: a class that is cohesive. |
| Method Iheritance Factor (MIF) | Inheritance | 0..1 | MD = Methods defined<br>PMI = Public methods inherited<br>MIF = PMI / (PMI + MD) | High value: a class that inherits more methods than it defines. It suggests that the class reuses functionalities of its parent. |
| Public Attributes | Encapsulation | 0..N | The number of fields that are visible (e.g. public) | High value: a class for which its attributes are not encapsulated |
| Method Hiding Factor (MHF) | Encapsulation | 0..N | TNM = Total Number of Methods<br>NVM = Non-Visible Methods<br>MHF = NVM/TNM | High value: a class that is really encapsulated with regards to its functionalities |


#### Method Level
| Metric | Category | Range | Description | Interpretation |
| ------ | -------- | ----------- | - | - |
| FAN-IN | Coupling | 0..N | The number of input dependencies a method has | High value: a method that is depended on a lot by other method (responsibilities) |
| FAN-OUT | Coupling | 0..N | The number of output dependencies a method has | High value: a class that depends a lot on other method |
| Total Lines of Code (TLOC) | Size | 0..N | The total lines of code without comments and whitespaces. | High value: a method that can be hard to understand and to maintain. May be violating the Single Responsibility Principle. It may include duplicated code. |
| Number of Parameters (NOP) | Readability | 0..N | The number of parameters | High value: a method that can be hard to understand and to maintain. The parameters can be misinterpreted or given in the wrong order. It may be a sign of low cohesion. |
| Nested Block Depth (NBD) | Complexity | 1..N | The number of the maximum depth of nested block. If there is no nested block, the depth equals to 1. | High value: a method that is more likely to be complex |
| McCabe Cyclomatic Complexity | Complexity | 1..N | Representation of the control flow as a graph (control flow graph)<br>The control flow contains nodes (entry point, exit point, decision points)<br>Nodes are connected by directed edges<br>M = E - N +2P<br>number of edges E<br>number of nodes N<br>number of connected components P (graph theory)<br>Cyclomatic complexity of Representation of the control flow as a graph (control flow graph)<br>The control flow contains nodes (entry point, exit point, decision points)<br>Cyclomatic complexity of a linear control flow is always one. | High value: a method that is complex. Harder to test all the cases of a method |

---
### Code Quality

This list are all the issues selected to be reported by Checkstyle, a tool that automates the process of checking Java code. This selection is based on a paper that describes all Checkstyle code quality issues considered rekevant for computing undergraduates. See the reference below.<br>
Oscar Karnalim, Simon, William Chivers, "Work-In-Progress: Code Quality Issues of Computing Undergraduates", [ref](https://ieeexplore.ieee.org/document/9766807)

Code quality issues reported are split into 7 categories:

- Naming
    - [AbstractClassName](https://checkstyle.org/config_naming.html#AbstractClassName)
    - [ConstantName](https://checkstyle.org/config_naming.html#ConstantName)
    
- Sizes
    - [ExecutableStatementCount](https://checkstyle.org/config_sizes.html#ExecutableStatementCount)
    - [LineLength](https://checkstyle.org/config_sizes.html#LineLength)
    
- Whitespace
    - [EmptyLineSeparator](https://checkstyle.org/config_whitespace.html#EmptyLineSeparator)
    
- Blocks
    - [EmptyBlock](https://checkstyle.org/config_blocks.html#EmptyBlock)
    - [EmptyCatchBlock](https://checkstyle.org/config_blocks.html#EmptyCatchBlock)
    - [NeedBraces](https://checkstyle.org/config_blocks.html#NeedBraces)
    
- Coding
    - [AvoidInlineConditionals](https://checkstyle.org/config_coding.html#AvoidInlineConditionals)
    - [DeclarationOrder](https://checkstyle.org/config_coding.html#DeclarationOrder)
    - [EmptyStatement](https://checkstyle.org/config_coding.html#EmptyStatement)
    - [MultipleVariableDeclarations](https://checkstyle.org/config_coding.html#MultipleVariableDeclarations)
    - [NestedForDepth](https://checkstyle.org/config_coding.html#NestedForDepth)
    - [NestedIfDepth](https://checkstyle.org/config_coding.html#NestedIfDepth)
    - [NestedTryDepth](https://checkstyle.org/config_coding.html#NestedTryDepth)
    - [OneStatementPerLine](https://checkstyle.org/config_coding.html#OneStatementPerLine)
    - [RequireThis](https://checkstyle.org/config_coding.html#RequireThis)
    - [SimplifyBooleanExpression](https://checkstyle.org/config_coding.html#SimplifyBooleanExpression)
    - [SimplifyBooleanReturn](https://checkstyle.org/config_coding.html#SimplifyBooleanReturn)
    - [StringLiteralEquality](https://checkstyle.org/config_coding.html#StringLiteralEquality)
    - [UnnecessaryParentheses](https://checkstyle.org/config_coding.html#UnnecessaryParentheses)
    - [UnnecessarySemicolonAfterTypeMemberDeclaration](https://checkstyle.org/config_coding.html#UnnecessarySemicolonAfterTypeMemberDeclaration)
    
- Miscellaneous
    - [ArrayTypeStyle](https://checkstyle.org/config_misc.html#ArrayTypeStyle)
    
- Metrics
    - [BooleanExpressionComplexity](https://checkstyle.org/config_metrics.html#BooleanExpressionComplexity)
---
### Code Smells

Code smells are code structures that may suggest a refactoring. They are only warnings and it is up to the developer to interpret and decide if a refactoring is necessary.

You can check the definition of the Design smells at [https://www.designite-tools.com/faq/](https://www.designite-tools.com/faq/)

#### Design smells

There are 4 categories

Abstraction:

- [Imperative Abstraction](https://tusharma.in/smells/IA.html)
- [Multifaceted Abstraction](https://tusharma.in/smells/MAC.html)
- [Unnecessary Abstraction](https://tusharma.in/smells/UA.html)
- [Unutilized Abstraction](https://www.tusharma.in/smells/UA2.html)

Encapsulation

- [Deficient Encapsulation](https://tusharma.in/smells/DE.html)
- [Unexploited Encapsulation](https://tusharma.in/smells/UE.html)

Modularization

- [Broken Modularization](https://tusharma.in/smells/BM.html)
- [Cyclic-Dependent Modularization](https://tusharma.in/smells/CM.html)
- [Insufficient Modularization](https://tusharma.in/smells/IM.html)
- [Hub-like Modularization](https://tusharma.in/smells/HM.html)

Hierarchy

- [Broken Hierarchy](https://tusharma.in/smells/BH.html)
- [Cyclic Hierarchy](https://tusharma.in/smells/CH.html)
- [Deep Hierarchy](https://tusharma.in/smells/DH.html)
- [Missing Hierarchy](https://tusharma.in/smells/MH.html)
- [Multipath Hierarchy](https://tusharma.in/smells/MH2.html)
- [Rebellious Hierarchy](https://tusharma.in/smells/RH.html)
- [Wide Hierarchy](https://tusharma.in/smells/WH.html)

#### Implementation smells

- Abstract Function Call From Constructor
- [Complex Conditional](https://tusharma.in/smells/ICC.html)
- [Complex Method](https://tusharma.in/smells/ICM.html)
- [Empty catch clause](https://tusharma.in/smells/IECB.html)
- [Long Identifier](https://tusharma.in/smells/ILI.html)
- [Long Method](https://tusharma.in/smells/LM.html)
- [Long Parameter List](https://tusharma.in/smells/LPL.html)
- [Long Statement](https://tusharma.in/smells/ILS.html)
- [Magic Number](https://tusharma.in/smells/IMN.html)
- [Missing default](https://tusharma.in/smells/IMD.html)

---
