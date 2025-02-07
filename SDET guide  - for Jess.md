#### 1) SDLC and its types (*THEORY) 
Basics and Fundamentals: * 
  - Understanding the Requirements
  - Test Planning
  - Test Case Design
  - Test Environment Setup
  - Test Execution
  - Test Reporting and Metrics
  - Test Closure
  - Post-Release Monitoring
  - Agile Model (!important)
*Core java*
- Basic (variables , function , Scope , methods)
- oops (!important)
- operators , operands
- Unit Testing (Java) - Tool: Junit
- Automating Functional Testing - Tool: Selenium
*SQL (Basic)*
**Resources:** https://youtu.be/HXV3zeQKqGY?si=A_BtsGeardWGNfPW
- DQL (Query extraction) 
- DML , DDL (Query creation)

---
#### 2.Application Testing (manual/automation) 
**Resources:** Any java basics course and Testing with java tools - Maven, Gradle

***White box testing and types* (testing all the code, application internals)** 
> *CODE - (Jest- all stack, Junit - java, React Unit - react for Unit Testing), -API - Postman, JMeter, Junit*
- path,
- condition,
- loop,
- memory pov (api only)
- performance pov (api only).  (!imp - dev)

 ***Black box testing -  (!imp - tester)***
- functionality - testing functionalities of the apps (manual / automation - selenium, playwright - supported .jar) (!BOTH)
- integration - internal service to service integration (manual / automation - selenium, playwright) (!BOTH)
- system - end to end (manual / automation - selenium, playwright) (!BOTH)
- acceptance - UAT / End user testing (beta) - (manual - end user, automation - selenium for presentation) *(!MANUAL)*
- performance - (JMeter for performance benchmarking, Postman to create units, JUnit - test unit wise) (!AutoOnly)
- usability - Load testing (traffic stress testing) - Tool: Load runner (!AutoOnly)
- smoke - Code Error check (targets one single unit/patch) - (!BOTH)
- sanity test - same as smoke (but targets a units associated/connected sub or other units) - Reference errors (!BOTH)
- ad-hoc testing - of three methods: *(!MANUAL)*
1) monkey/guerilla testing - manual (- for specially challenged feature accessibility testing)
2) out of the box testing - manual as same as monkey/guerilla method but procedure differs (- for specially challenged feature accessibility testing) 
3) regression (!BOTH)
-Ishikawa method - having different phases containing different set of errors (taking a phase and testing it one by one) (!imp)
-Fishbone method - as same as Ishikawa method (procedure differs)
and a lot more
- compatibility - testing hardware compatibility for the applications (tools: JMeter (for desktop apps), Playwright(web, browser, server)) *(!MANUAL)*
- accessibility - (- for specially challenged feature accessibility testing (color blindness oriented, touch oriented- for mobile, key oriented - for desktop)) - *(!MANUAL)*
- exploratory - ease of access to all features given, ease of use to explore features (manual, automatic - playwright (internal)) *(!MANUAL)*
- I18N (International) - (*(!MANUAL)*) - language, culture and race relevance. 
- i10n (regional/domestic) - *(!MANUAL)* same as above but regional 

---
#### 3) ****Unit Testing
all of these tools can be used for Unit testing, just the platform and the language differs:
- Code related specify unit test
- Catch Bugs Early , Easier Debugging
###### 1) *Jest* (library - multi language)
###### 2) *JUnit* (java - .jar)
###### 3) *React Testing Library* (React Script) 

---
#### 4)  ***API Testing*** 
###### 1) *Postman (Create, work and test APIs )* 
**Resources:**
link - https://www.youtube.com/watch?v=vCJVFnepECc&list=PLUDwpEzHYYLs3DYFqm79fIj2QOzPke_fW (Beginner)
link - https://www.youtube.com/watch?v=CGX6DRNfS-E  (Soo many diff of execution)

*Pros:*
- Functional Testing
-get , post , put , delete  (Viewing - Request/response )
- Load Testing - traffic Setup
- Security Testing - (encryption, different authentication mechanisms)
- Reliability and Stability Testing (dynamic components)
- Data Validation Testing ( Browser Status Code -> request OK or NOT OK Testing)
- Can create and test APIs
- Shows live report
*Cons:*
- Paid
- Limited for free tier (For security and load testing)


###### 2) *JMeter (only For Testing)*
**Resources:**
link - https://www.youtube.com/watch?v=817zU_bXh9Y&list=PLUDwpEzHYYLs33uFHeIJo-6eU92IoiMZ7
*Pros:* 
- Functional Testing
-get , post , put , delete  (Viewing - Request/response )
- Load Testing - traffic Setup
- Security Testing - (encryption, different authentication mechanisms)
- Data Validation Testing (Request/response GET POST viewer)
- Detailed reports and graphs (provides html and graph reports.
- Full stack test APIs
*Cons:*
- No live report (gui but viable
- can create APIs, only can test. 

---
#### 5) *Automation Testing*
these are all the testing concept which can be performed using automation testing. 
- Automated Functional Testing
- Cross-Browser Testing
- Regression Testing
- Smoke Testing
- Sanity Testing
- Load Testing (with integration)
- API Testing (combined with other tools)
- Screen Capture and Visual Testing
- Data-Driven Testing
- Handling Pop-ups/Alerts
- E2E Testing 

###### *Tools:* (used for automation testing)
###### 1) *Selenium*(java) (!IMP)
**Resources:**
Selenium
https://youtube.com/playlist?list=PLacgMXFs7kl8e8xIdMDQEi2c6eQnO1toK&si=Vp-ie0nJNL6afBhl
Selenium - In depth 
https://www.youtube.com/live/9p6NNapsUvQ?si=LpoaAeFgz7J2ZnO7

- Headed mode only 
- specify the browser
- parallel testing (TestNG)
- WebDriver(Download Driver)

###### 2) *Playwright* (java APIs ,JS & TS officially support for web automation)
**Resources:**
link - https://www.youtube.com/watch?v=yOuElUSfAs8&list=PLUDwpEzHYYLsw33jpra65LIvX1nKWpp7- (Early pickup  for JS)
link - https://www.youtube.com/watch?v=EfRAjmsEQ3k&list=PL6flErFppaj0pscPIPA_Cxhil6ZDeFuJJ (Java)
link - https://www.youtube.com/watch?v=zF3ftXEj5Aw&list=PL6flErFppaj0iQG2_Dd72Jz0bfrzZwMZH (Java Script)
link - 

- Headless mode / headed mode 
- specify the browser
- parallel testing
- Api automation 

###### 3) *Cypress* (Js) (!optional)
**Resources:**
link - https://www.youtube.com/watch?v=69SFwgWHUig&list=PLUDwpEzHYYLvA7QFkC1C0y0pDPqYS56iU
link - https://www.youtube.com/watch?v=bpuEAzYDTy4&list=PLL34mf651faP_cOOErNUi33GeRHhr2QsP
 -  Headless mode  
 - Specify the browser    
 - Runs inside the browser
 
###### 4) *TestNG Framework* 
**Resources:**
link - https://www.youtube.com/watch?v=K6ET456rQnQ&list=PL699Xf-_ilW4VpISC5etvNNbHquV5ZrKu
- Annotations
- TestNG XML Configuration
- Grouping Tests
- Test Execution Workflow
- Parallel Test Execution
- Dependency Testing
- Data-Driven Testing (Parameterized Tests)
- TestNG Reports (HTML) (imp)
Mainly some people are using TestNG For Flexible Execution ,Reports and Logging

---
#### 6) *Report Generation :*  
**Resources:**
link - https://www.youtube.com/watch?v=ZcdJP0cos8o&list=PL6SXxvjnlkaRA2S2t9NTWIFrYIjEx3FJt
   - Detailed Test Information
   - Test Organization
   - Beautiful, Interactive Reports
   - Step-Level Reporting
   - Support for Multiple Languages and Frameworks
   - Customizable Reports
###### *Tools:*
 - *Allure Report*  (!VERY VERY IMPORTANT)
---



