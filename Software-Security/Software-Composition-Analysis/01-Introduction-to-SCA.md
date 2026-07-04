Software Composition Analysis (SCA)

Learning Objectives
By the end of this lesson, you should be able to answer:
What is Open Source Software?
What is a dependency?
Why do developers use dependencies?
What are package managers?
What are manifest files?
What are lock files?
What are direct and transitive dependencies?
Why do dependencies become vulnerable?
Why is Software Composition Analysis important?
How do attackers exploit dependency vulnerabilities?
Why is Snyk considered an SCA tool?

1. What is Open Source Software (OSS)?
Open Source Software is software whose source code is publicly available for anyone to:
Read
Modify
Improve
Redistribute
    Unlike proprietary software (such as Microsoft Office), open-source software allows developers to inspect and reuse the code.
Examples include:
Linux
Node.js
React
Django
Flask
Express.js
TensorFlow
Imagine you're building a house.
Instead of making every brick yourself, you buy ready-made bricks from a trusted company.
Those bricks are equivalent to open-source libraries.

Why Developers Love Open Source
Without open source:
A developer building a login page would have to write:
Password hashing
Database connection
Email sending
Authentication
Encryption
Logging
HTTP requests
from scratch.
That could take months.
Instead, they install trusted libraries that already solve these problems.
For example:
Instead of writing encryption yourself:
Install bcrypt
Instead of creating an HTTP client:
Install axios
Instead of writing date calculations:
Install moment.js
Result:
Development becomes much faster.

The Hidden Problem
Every library is written by another human.
Humans make mistakes.
If that library contains insecure code...
Every application using that library inherits the vulnerability.
This is one of the biggest cybersecurity problems today.

Example
Imagine you own a hotel.
You purchase electronic locks from a company.
Months later...
Researchers discover the lock can be opened with a paperclip.
Even though you never designed the lock...
Every room in your hotel becomes insecure.
Exactly the same thing happens with software dependencies.

2. What is a Dependency?
A dependency is any external code your application relies on to perform a function.
Instead of writing everything yourself...
You "depend" on someone else's code.
Example:
A Node.js application:
const express = require("express")
Your application now depends on Express.
Without Express...
Your application cannot start.
Therefore:
Express is a dependency.

Real-Life Example
Think about making Jollof Rice.
You need:
Rice
Pepper
Tomato
Vegetable oil
Seasoning
These ingredients are dependencies.
If one ingredient is contaminated...
The whole meal becomes unsafe.
Software works exactly the same way.

Types of Dependencies
There are two major types.
Direct Dependencies
These are dependencies you intentionally install.
Example:
npm install express
You installed Express yourself.
Therefore:
Express is a Direct Dependency.

Transitive Dependencies
These are dependencies installed automatically because another package requires them.
Example:
You install:
npm install express
Express internally installs:
body-parser
cookie
debug
mime
send
qs
You never requested these packages.
They were installed automatically.
These are Transitive Dependencies.

Visual Representation
Your Project

│

├── Express (Direct)

│

├── body-parser (Transitive)

│

├── debug (Transitive)

│

├── mime (Transitive)

│

└── qs (Transitive)
One direct dependency can pull in dozens—or even hundreds—of transitive dependencies.

Why Are Transitive Dependencies Dangerous?
Suppose your project contains only:
Express
Looks harmless.
However...
Express may rely on 30 packages.
Each of those packages may rely on 20 more.
Soon your application contains:
700 packages
You only installed one.
This is called the Dependency Tree.
A vulnerability deep in that tree can still compromise your application.

3. What is a Package?
A package is a reusable collection of code that solves a specific problem.
Examples:
Package
Purpose
Express
Build web servers
React
Build user interfaces
Axios
Make HTTP requests
bcrypt
Hash passwords
Lodash
Utility functions
Helmet
Improve HTTP security

Think of a package as an appliance in a kitchen. Instead of building a blender yourself, you buy one and use it.

4. What is a Package Manager?
A package manager is software that installs, updates, removes, and manages dependencies automatically.
Without package managers:
You would need to:
Search the internet
Download code manually
Resolve dependencies yourself
Update packages one by one
Package managers automate all of this.

Common Package Managers
Language
Package Manager
JavaScript
npm, Yarn, pnpm
Python
pip
Java
Maven, Gradle
Go
Go Modules
Rust
Cargo
PHP
Composer


What Happens When You Install a Package?
Suppose you run:
npm install express
Behind the scenes:
npm reads your project.
It contacts the package registry.
It downloads Express.
It checks Express's dependencies.
It downloads those dependencies.
It builds the dependency tree.
It records versions in lock files.
It places everything into the node_modules directory.
A simple command can result in hundreds of packages being installed automatically.

5. Manifest Files
A manifest file tells the package manager what your project needs.
Think of it as a shopping list.
For Node.js:
package.json
Example:
{
 "name": "myapp",
 "dependencies": {
   "express": "^4.18.2",
   "axios": "^1.7.0"
 }
}
This file specifies the packages your application depends on.

Manifest Files in Different Languages
Language
Manifest File
Node.js
package.json
Python
requirements.txt / pyproject.toml
Java
pom.xml / build.gradle
Go
go.mod
Rust
Cargo.toml
PHP
composer.json


6. Lock Files
If the manifest file is a shopping list, the lock file is the receipt showing exactly what was purchased.
A lock file records:
Exact package versions
Exact transitive dependencies
Checksums (integrity hashes)
Download locations
For Node.js:
package-lock.json
Example:
express 4.18.2
debug 2.6.9
mime 1.6.0
This ensures that every developer and every deployment installs the exact same dependency versions.

Why Lock Files Matter
Imagine two developers clone the same project.
Without a lock file:
Developer A installs Express 4.18.2
Developer B installs Express 4.19.0
Their applications may behave differently.
A lock file eliminates this inconsistency by pinning exact versions.

7. Why Do Dependencies Become Vulnerable?
Dependencies become vulnerable because:
Developers make coding mistakes.
New attack techniques emerge.
Software evolves, exposing old flaws.
Bugs remain undiscovered for years.
Security research uncovers hidden weaknesses.
Common vulnerability types include:
SQL Injection
Cross-Site Scripting (XSS)
Remote Code Execution (RCE)
Directory Traversal
Command Injection
Denial of Service (DoS)

8. Why is Software Composition Analysis (SCA) Important?
Modern applications rarely consist solely of code written by the development team.
In many cases:
Around 80–90% of a modern application is composed of third-party open-source components.
The remaining 10–20% is custom business logic.
This means the security of your application depends heavily on the security of the libraries you include.
Software Composition Analysis (SCA) is the automated process of:
Identifying all open-source components in a project.
Building a complete dependency tree.
Matching package versions against vulnerability databases.
Reporting known security issues.
Suggesting upgrades or patches.
Helping organizations maintain a secure software supply chain.

9. How Attackers Exploit Vulnerable Dependencies
Attackers don't always target your custom code. They often look for known vulnerabilities in widely used libraries.
A typical attack workflow is:
Identify the technologies a target uses.
Check whether vulnerable dependency versions are present.
Find publicly disclosed exploits.
Exploit the vulnerable package to gain unauthorized access, execute code, or steal data.
This is why keeping dependencies updated is a critical security practice.

10. Where Does Snyk Fit In?
Snyk is an SCA tool.
When you run:
snyk test
Snyk performs several actions:
Reads your manifest file (such as package.json).
Builds the dependency tree, including transitive dependencies.
Identifies package names and versions.
Compares them against known vulnerability databases.
Reports vulnerabilities, their severity, and available fixes.
Suggests remediation steps such as upgrading to a secure version.
This allows developers and security teams to discover and fix vulnerable dependencies before they are exploited.

Key Terms to Memorize
Term
Definition
Open Source Software (OSS):
Software with publicly available source code.

Dependency:
External code your application relies on.

Direct Dependency:
A package you explicitly install.

Transitive Dependency:
A package installed automatically because another dependency requires it.

Package:
Reusable software that provides specific functionality.

Package Manager:
Tool that installs and manages dependencies.

Manifest File:
File declaring the project's dependencies.

Lock File:
File recording the exact installed dependency versions.

Dependency Tree: 
A hierarchical map of all direct and transitive dependencies.

Software Composition Analysis (SCA):
The automated process of identifying, analyzing, and securing third-party software components in an application.

