# Student-friendly pipeline documentation

## Quality assessment tools

### Metrics
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
| Total Line of Code (TLOC) | Size | 0..N | The total lines of code without comments and whitespaces. | High value: a method that can be hard to understand and to maintain. May be violating the Single Responsibility Principle. It may include duplicated code. |
| Number of Parameters (NOP) | Readability | 0..N | The number of parameters | High value: a method that can be hard to understand and to maintain. The parameters can be misinterpreted or given in the wrong order. It may be a sign of low cohesion. |
| Nested Block Depth (NBD) | Complexity | 1..N | The number of the maximum depth of nested block. If there is no nested block, the depth equals to 1. | High value: a method that is more likely to be complex |
| McCabe Cyclomatic Complexity | Complexity | 1..N | Representation of the control flow as a graph (control flow graph)<br>The control flow contains nodes (entry point, exit point, decision points)<br>Nodes are connected by directed edges<br>M = E - N +2P<br>number of edges E<br>number of nodes N<br>number of connected components P (graph theory)<br>Cyclomatic complexity of Representation of the control flow as a graph (control flow graph)<br>The control flow contains nodes (entry point, exit point, decision points)<br>Cyclomatic complexity of a linear control flow is always one. | High value: a method that is complex. Harder to test all the cases of a method |

---
### Code Quality
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

Code smells are warnings

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
- Complex Conditional
- Complex Method
- Empty catch clause
- Long Identifier
- Long Method
- Long Parameter List
- Long Statement
- [Magic Number](https://tusharma.in/smells/IMN.html)
- [Missing default](https://tusharma.in/smells/IMD.html)

---

## Action and Pipeline explanation
 See Action and Pipeline explanation [here](https://github.com/tupac-rocher/mvn-student-friendly-report)
 
 Files available on the current repository:
 - report.yml: the workflow configuration file to add to your Maven project 
 (path from the root of your project so it gets recognized by Github: ./.github/workflows)
 - checkstyle.xml: the configuration file for the checkstyle Maven plugin (to add to the root of your project)
