
### 1.How can we run TestNG test cases without Java compiler?

TestNG never depends on the Java compiler directly. It **only runs the compiled .class files**.

When running TestNG through tools like:

testng.xml

IntelliJ / Eclipse

Maven Surefire plugin

Jenkins pipeline

These tools automatically compile the code in the background. TestNG simply picks up the compiled files and executes them.
So we’re running TestNG tests without manually invoking the Java compiler.

### 2. What is the purpose of testng.xml?

The testng.xml file is used to **configure and control the execution** of TestNG test cases.

It defines which classes, methods, or packages to run, allows grouping, parallel execution, parameterization, and test suite management.

It also helps integrate tests into CI/CD tools like Jenkins.

### Why do we need testng.xml when TestNG can use annotations?

Annotations work only inside the code.

But XML provides **external control**, where we manage:

which tests to run

grouping

parallel execution

parameters

multiple classes

ordering

Without modifying Java code.


This makes XML essential in real projects and CI pipelines.

### 3.Can we run TestNG without testng.xml?

Yes, but only individually or class-wise.

We cannot run:

multiple test classes

groups

parallel execution

suite-level configurations

So in real-time project execution, XML is mandatory.

### 4.How does testng.xml help in Data-Driven Testing?

XML provides parameters externally.

We can pass URLs, usernames, passwords, environments etc.

Example:
```
<parameter name="browser" value="chrome"/>
```
Thus the same test runs on different configurations.

### 5.Explain parallel testing using testng.xml

XML allows setting:

**parallel="methods" thread-count="5"**

This runs test methods concurrently, improving execution speed.

**Used for:**

cross-browser testing

speeding up large test suites

Selenium Grid executions

### 6.What are the advantages of including/excluding tests in XML?

We can skip certain tests without touching Java code.

Example:

<exclude name="paymentTest"/>

Useful for:

failing modules

partial deployments

smoke/regression cycles

### 7. How do you run test suites in Jenkins?

We configure Jenkins to run:
```
mvn test -DsuiteXmlFile=testng.xml
```

XML becomes the entry point for automation pipeline.

### 8. What is priority in TestNG?

Priority in TestNG is used **to control the order of execution of test methods**.

**Lower** number means **higher priority**. This helps us execute tests in a logical business flow or ensure critical test cases run first.

### 9. What happens if two test methods have the same priority?

If two tests have the same priority, TestNG resolves the tie by **executing them in alphabetical order of the method name**.

### 10. What is the default priority if not specified?

The **default priority is 0**, and such tests run before tests with positive priorities—but still in alphabetical order.

### 11.Can we use negative priorities?

Yes. TestNG accepts negative values. A test with **priority -1 runs before priority 0**, which is useful when you want to enforce some test as the absolute first execution step.

### 12. Real-time scenario: How do you prioritize test cases in a workflow?

In real-time projects, we follow the business flow.

For example, in an e-commerce application:

login → search → add to cart → checkout

I assign priority in increasing order to ensure the flow is followed.

This prevents failures caused by out-of-sequence execution and makes CI runs predictable.

### 13. How is priority different from dependsOnMethods?

priority controls sequence, but does not enforce dependency.

dependsOnMethods ensures a **test will run only if its dependent test passes**.

In workflow-dependent scenarios, we often combine both.

### 14. In your project, where did you use TestNG priority?

In my previous project, we used priority for:

Running critical functional tests first

Maintaining workflow sequence for our end-to-end scenarios

Ensuring smoke tests run before full regression

Making CI execution faster by failing early if critical flows broke

This made our pipeline more stable and reduced debugging time.

### 15. What is include/exclude in TestNG?

Include & exclude allow us to **control which test methods or classes should run**. It is configured inside testng.xml.

include runs only specified tests, while exclude prevents mentioned tests from running.

### 16. Why do we need include/exclude in real-time projects?

We rarely run the entire suite every time. Include/exclude helps us run:

Smoke tests

Specific module tests

Critical regression cases

CI-friendly lightweight suites

It also helps temporarily skip failing or unstable tests without touching the code.

### 17.What will happen if both include and exclude are used together?

If both are present, **include has higher precedence**.

That means TestNG will first select included methods, then apply exclusion rules only inside that set.

### 18. Can we use regex in TestNG include/exclude?

Yes, TestNG supports regex.

Example: <include name="test.*"/>

This is extremely useful when we want to run a pattern-based set of tests.

### 19. How do you exclude flaky tests?

In our CI pipeline, we maintain a separate testng file where we list flaky tests under <exclude>.

This allows execution to continue without modifying code and keeps the suite stable.

### 20. Real-time scenario: How did you use include/exclude in your project?

In my previous project, we used include/exclude extensively.

For example, during nightly regression we included all modules.

But during deployment smoke tests, we included only critical flows like Login, Checkout, and Payment.

We also excluded unstable tests during high-priority releases so the pipeline wouldn't block.

This gave us flexibility, saved execution time, and improved CI reliability.

### 21.Explain regex at package level

TestNG supports executing test cases at the package level using the <packages> tag.

We can also use regex patterns with the <class> tag to run only selected classes inside a package, such as classes ending with Test or starting with Login.

This is extremely useful in large frameworks because we don’t have to manually maintain long lists of test classes.

In my project, we used package-level execution and regex to run UI modules, API modules, and smoke suites efficiently in CI pipelines.

### 22. How do you exclude a group from execution?

```

<groups>
    <run>
        <exclude name="regression"/>
    </run>
</groups>
```

### 23. Difference between @Parameters and @DataProvider?

@Parameters → Values from XML, good for environment configs.

@DataProvider → Provides multiple sets of data, good for data-driven tests.

### 24.Why do we need testng.xml when we can run tests from IDE?

IDE runs tests individually, but testng.xml allows:

Batch execution of multiple classes.

Grouping tests (Smoke, Regression).

Parameterization (browser, environment).

Parallel execution for faster runs.


It’s essential for CI/CD pipelines.

### 25. How do you run only specific methods using testng.xml?

```

<classes>
    <class name="tests.LoginTests">
        <methods>
            <include name="testValidLogin"/>
        </methods>
    </class>
</classes>
```

### 26. Your suite has 100 tests, but you need to run only Smoke tests on Chrome. How will you configure it?

Create a Smoke group in code:
```

@Test(groups = {"smoke"})
public void testLogin() { ... }
```

**in testng.xml**

```

<suite name="Smoke Suite">
    <parameter name="browser" value="chrome"/>
    <test name="Smoke Tests">
        <groups>
            <run>
                <include name="smoke"/>
            </run>
        </groups>
        <classes>
            <class name="tests.LoginTests"/>
        </classes>
    </test>
</suite>
```

### 27. What is Data-Driven Testing?

Definition: Running the same test multiple times with different sets of data.

Why: Avoid hardcoding values, improve coverage, and make tests reusable.

Where used: Login tests, form submissions, search functionality, etc.

### 28.  How do you handle dynamic data from Excel in TestNG?

Use Apache POI to read Excel.

Return Object[][] from DataProvider.

Pass it to @Test(dataProvider="excelData").

### 29. Run login tests with 50 different credentials from Excel on Chrome and Firefox in parallel."

Use DataProvider for credentials.

Use @Parameters for browser.

Configure parallel="tests" in testng.xml.

```

<suite name="Parallel Suite" parallel="tests" thread-count="2">
    <parameter name="browser" value="chrome"/>
    <test name="Login Chrome">
        <classes>
            <class name="tests.LoginTests"/>
        </classes>
    </test>
    <parameter name="browser" value="firefox"/>
    <test name="Login Firefox">
        <classes>
            <class name="tests.LoginTests"/>
        </classes>
    </test>
</suite>
```

### 30.Where do you find TestNG reports after execution?

In test-output folder:

index.html → HTML report.

testng-results.xml → XML report.

### 31. Scenario:
"You need to validate 10 fields on a checkout page. If one fails, others should still be checked. How do you implement this?"

```
SoftAssert softAssert = new SoftAssert();
softAssert.assertEquals(actualPrice, expectedPrice);
softAssert.assertEquals(actualQty, expectedQty);
// ...
softAssert.assertAll();
```

### 32.Why do we use Listeners in TestNG?

To customize test execution behavior:

Capture screenshots on failure

Generate custom reports

Log test events

### 33.How do you register a listener?

In testng.xml using <listeners> tag.

Or using @Listeners annotation:

```

@Listeners(CustomListener.class)
public class TestClass { ... }
```

### 34. Your manager wants a custom report showing only failed tests with screenshots. How do you implement this?"

Implement ITestListener:

Capture screenshot in onTestFailure().


Implement IReporter:

Generate HTML report with failed test details and screenshot links.
