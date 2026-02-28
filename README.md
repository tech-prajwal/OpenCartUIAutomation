# OpenCartUIAutomation 🛒

> **End-to-End UI Test Automation Framework** for the [OpenCart](https://www.opencart.com/) e-commerce platform, built with Java, Selenium WebDriver, TestNG, and the Page Object Model (POM) design pattern — with CI/CD integration via Jenkins.

---

## 📋 Table of Contents

- [About the Project](#about-the-project)
- [Tech Stack](#tech-stack)
- [Design Pattern — Page Object Model](#design-pattern--page-object-model)
- [Project Structure](#project-structure)
- [Test Coverage](#test-coverage)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
  - [Clone the Repository](#clone-the-repository)
  - [Configure the Test Environment](#configure-the-test-environment)
  - [Run Tests via Maven](#run-tests-via-maven)
  - [Run Tests via TestNG XML Suite](#run-tests-via-testng-xml-suite)
- [CI/CD Integration](#cicd-integration)
  - [Jenkins Pipeline](#jenkins-pipeline)
  - [Jenkins Sample Pipeline](#jenkins-sample-pipeline)
- [Test Reports](#test-reports)
- [Best Practices Used](#best-practices-used)
- [Contributing](#contributing)
- [Author](#author)

---

## 📖 About the Project

**OpenCartUIAutomation** is a production-ready Selenium-based test automation framework targeting the popular open-source e-commerce platform, OpenCart. The project is structured around the **Page Object Model (POM)** design pattern, ensuring clean separation between test logic and UI element definitions. This makes the framework highly maintainable, scalable, and easy to extend as the application evolves.

The framework is designed to automate core functional user journeys across the OpenCart web application, including user registration, login, product browsing, cart management, and checkout — the exact scenarios that matter most in an e-commerce context.

### Key Highlights

- 🧩 **Page Object Model** — Clean separation of UI elements and test logic
- 🔄 **TestNG-powered** — Flexible test execution, grouping, and parameterization
- ⚙️ **Maven** — Streamlined dependency management and build lifecycle
- 🚀 **Jenkins CI/CD** — Automated test execution on every code change
- 🌐 **Cross-browser Ready** — Supports Chrome, Firefox, and Edge via WebDriver
- 📊 **Rich Reporting** — Detailed test execution reports via TestNG / ExtentReports

---

## 🛠 Tech Stack

| Technology           | Role                                                        |
|----------------------|-------------------------------------------------------------|
| **Java**             | Primary programming language (100% of codebase)            |
| **Selenium WebDriver** | Browser automation and UI interaction                    |
| **TestNG**           | Test runner — annotations, grouping, parallel execution      |
| **Maven**            | Build tool and dependency management (`pom.xml`)            |
| **Page Object Model**| Design pattern for scalable test architecture               |
| **Page Factory**     | Selenium utility for lazy initialization of WebElements     |
| **Jenkins**          | CI/CD pipeline automation                                   |
| **WebDriverManager** | Automatic browser driver management                         |
| **ExtentReports / TestNG Reports** | HTML test execution reports               |

---

## 🧩 Design Pattern — Page Object Model

This project strictly follows the **Page Object Model (POM)** design pattern, which is the industry-standard approach for Selenium test automation.

### How It Works

| Layer             | Responsibility                                              |
|-------------------|-------------------------------------------------------------|
| **Page Classes**  | Contain WebElement locators and page-specific action methods |
| **Test Classes**  | Contain test logic — call methods from Page Classes         |
| **Base Class**    | Handles WebDriver initialization, teardown, and shared config |
| **Utilities**     | Reusable helpers — waits, screenshots, data readers         |

### Benefits

- **Single source of truth** — UI element locators are defined once per page class
- **Easy maintenance** — A UI change only requires updating one page class, not every test
- **Readable tests** — Test methods read like user stories (`loginPage.enterEmail()`, `cartPage.addToCart()`)
- **Reusability** — Page methods can be reused across multiple test cases

### Example Pattern

```java
// Page Class — defines elements and actions
public class LoginPage {

    WebDriver driver;

    @FindBy(id = "input-email")
    WebElement emailField;

    @FindBy(id = "input-password")
    WebElement passwordField;

    @FindBy(xpath = "//input[@value='Login']")
    WebElement loginButton;

    public LoginPage(WebDriver driver) {
        this.driver = driver;
        PageFactory.initElements(driver, this);
    }

    public void enterEmail(String email) {
        emailField.sendKeys(email);
    }

    public void enterPassword(String password) {
        passwordField.sendKeys(password);
    }

    public void clickLogin() {
        loginButton.click();
    }
}

// Test Class — uses the page class
public class LoginTest extends BaseTest {

    @Test
    public void verifySuccessfulLogin() {
        LoginPage loginPage = new LoginPage(driver);
        loginPage.enterEmail("test@example.com");
        loginPage.enterPassword("Password@123");
        loginPage.clickLogin();
        Assert.assertTrue(driver.getTitle().contains("My Account"));
    }
}
```

---

## 📁 Project Structure

```
OpenCartUIAutomation/
├── src/
│   ├── main/
│   │   └── java/
│   │       └── utils/               # Reusable utility classes (waits, config reader)
│   └── test/
│       └── java/
│           ├── base/                # BaseTest — WebDriver setup/teardown
│           ├── pages/               # Page Object classes for each OpenCart page
│           │   ├── HomePage.java
│           │   ├── LoginPage.java
│           │   ├── RegisterPage.java
│           │   ├── SearchPage.java
│           │   ├── ProductPage.java
│           │   ├── CartPage.java
│           │   └── CheckoutPage.java
│           └── tests/               # TestNG test classes
│               ├── LoginTest.java
│               ├── RegisterTest.java
│               ├── SearchTest.java
│               ├── CartTest.java
│               └── CheckoutTest.java
├── src/test/resources/
│   ├── testng.xml                   # TestNG suite configuration
│   └── config.properties            # Base URL, browser, credentials
├── .gitignore                       # Git ignore rules
├── Jenkins_sample                   # Jenkins pipeline template/starter
├── Jenkinsfile                      # Jenkins CI/CD pipeline definition
└── pom.xml                          # Maven build configuration & dependencies
```

---

## ✅ Test Coverage

The framework covers the following OpenCart functional workflows:

| Module              | Test Scenarios Covered                                                    |
|---------------------|---------------------------------------------------------------------------|
| **User Registration** | New account creation, form validation, duplicate email handling         |
| **Login / Logout**  | Valid login, invalid credentials, empty field validation, logout flow     |
| **Home Page**       | Page load, navigation menu, featured products display                     |
| **Product Search**  | Search by keyword, search results validation, no results handling         |
| **Product Detail**  | Product page load, image gallery, add to wishlist, add to cart            |
| **Shopping Cart**   | Add product to cart, update quantity, remove item, cart total validation  |
| **Checkout**        | Guest checkout, billing/shipping details, order placement                 |
| **Account Dashboard** | My Orders, address book, account settings                              |

---

## ✅ Prerequisites

| Tool            | Minimum Version | Notes                                             |
|-----------------|-----------------|---------------------------------------------------|
| **Java JDK**    | 8 or higher     | `JAVA_HOME` must be configured                    |
| **Maven**       | 3.6+            | `mvn` must be available on PATH                   |
| **Chrome / Firefox / Edge** | Latest | Browser must be installed on test machine   |
| **OpenCart AUT** | 3.x or 4.x     | Running locally or accessible at a test URL       |
| **Jenkins**     | 2.x+            | Required only for CI/CD pipeline execution        |
| **Git**         | Any             | For cloning the repository                        |

> **Note:** If using **WebDriverManager**, browser drivers (chromedriver, geckodriver) are downloaded automatically — no manual driver installation needed.

---

## 🚀 Getting Started

### Clone the Repository

```bash
git clone https://github.com/tech-prajwal/OpenCartUIAutomation.git
cd OpenCartUIAutomation
```

---

### Configure the Test Environment

Edit `src/test/resources/config.properties` to point to your OpenCart instance:

```properties
# Application Under Test
base.url=http://localhost/opencart/

# Browser Configuration (chrome | firefox | edge)
browser=chrome

# Test User Credentials
valid.email=testuser@example.com
valid.password=Password@123

# Timeouts
implicit.wait=10
explicit.wait=20
```

---

### Run Tests via Maven

```bash
# Run the full test suite
mvn clean test

# Run a specific test class
mvn clean test -Dtest=LoginTest

# Run tests with a specific TestNG XML suite
mvn clean test -DsuiteXmlFile=src/test/resources/testng.xml

# Run tests on a specific browser
mvn clean test -Dbrowser=firefox

# Run tests against a specific environment
mvn clean test -Dbase.url=https://staging.youropencart.com
```

---

### Run Tests via TestNG XML Suite

You can also run directly from the TestNG XML configuration file in your IDE (Eclipse / IntelliJ IDEA):

1. Open the project in your IDE.
2. Right-click on `src/test/resources/testng.xml`.
3. Select **Run As > TestNG Suite**.

---

## ⚙️ CI/CD Integration

### Jenkins Pipeline

**File:** `Jenkinsfile`

The primary CI/CD pipeline definition for Jenkins. It automates the full lifecycle — checking out the code, installing dependencies, running the Selenium test suite, and publishing test results.

```groovy
// Typical pipeline stages:
// 1. Checkout — pull latest code from Git
// 2. Build    — compile the project via Maven
// 3. Test     — execute all TestNG tests via Maven
// 4. Reports  — publish TestNG/Surefire HTML reports
// 5. Notify   — (optional) Slack / email notification on failure
```

**Setup in Jenkins:**

1. Create a new **Pipeline** job in Jenkins.
2. Under **Pipeline**, set **Definition** to `Pipeline script from SCM`.
3. Set the **SCM** to Git and enter the repository URL:
   ```
   https://github.com/tech-prajwal/OpenCartUIAutomation
   ```
4. Set the **Script Path** to `Jenkinsfile`.
5. Ensure the Jenkins agent has Java and Maven configured in **Global Tool Configuration**.
6. Save and click **Build Now**.

---

### Jenkins Sample Pipeline

**File:** `Jenkins_sample`

A starter/boilerplate pipeline template. Use this as a reference to:
- Bootstrap a new pipeline configuration
- Add stages such as environment-specific deployments
- Extend with Slack notifications, artifact archiving, or email alerts
- Customize browser and test suite selection via Jenkins parameters

---

### Triggering the Pipeline

| Trigger Type      | Configuration                                              |
|-------------------|------------------------------------------------------------|
| **Git Push**      | Configure GitHub/GitLab webhook → Jenkins build trigger    |
| **Pull Request**  | Use GitHub Branch Source plugin or Multibranch Pipeline    |
| **Scheduled Run** | Add `cron` trigger in the Jenkinsfile `triggers` block     |
| **Manual**        | Click **Build Now** in the Jenkins job dashboard           |

---

## 📊 Test Reports

After test execution, reports are generated in the `target/` directory:

| Report Type              | Location                                    |
|--------------------------|---------------------------------------------|
| **TestNG HTML Report**   | `target/surefire-reports/index.html`        |
| **TestNG XML Results**   | `target/surefire-reports/*.xml`             |
| **Maven Site Report**    | `target/site/index.html` (run `mvn site`)   |
| **Screenshots on Failure** | `target/screenshots/` (if configured)    |

### View TestNG Report Locally

```bash
# Generate and open the site report
mvn site
open target/site/index.html   # macOS
start target/site/index.html  # Windows
```

### Publish Reports in Jenkins

In Jenkins, use the **JUnit Plugin** to publish XML test results:

```groovy
post {
    always {
        junit 'target/surefire-reports/*.xml'
        publishHTML([
            reportDir: 'target/surefire-reports',
            reportFiles: 'index.html',
            reportName: 'TestNG Report'
        ])
    }
}
```

---

## 💡 Best Practices Used

This framework incorporates industry-standard automation best practices:

| Practice                        | Implementation                                               |
|---------------------------------|--------------------------------------------------------------|
| **Page Object Model**           | Each page has a dedicated Java class with locators + methods |
| **Page Factory**                | `@FindBy` annotations with `PageFactory.initElements()`      |
| **Base Test Class**             | Centralized WebDriver setup and teardown via `@BeforeMethod` / `@AfterMethod` |
| **Explicit Waits**              | `WebDriverWait` for dynamic elements; avoids `Thread.sleep()`|
| **TestNG Annotations**          | `@Test`, `@BeforeMethod`, `@AfterMethod`, `@DataProvider`    |
| **Data-Driven Testing**         | Parameterized via `@DataProvider` or config.properties       |
| **Screenshot on Failure**       | Captured automatically using TestNG listeners                |
| **No hardcoded values**         | Base URL, credentials, and timeouts stored in properties file |

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. **Fork** the repository.
2. **Create** a feature branch:
   ```bash
   git checkout -b feature/add-checkout-tests
   ```
3. **Commit** your changes with a clear message:
   ```bash
   git commit -m "feat: add guest checkout test coverage"
   ```
4. **Push** to your forked repository:
   ```bash
   git push origin feature/add-checkout-tests
   ```
5. Open a **Pull Request** against the `master` branch.

### Contribution Guidelines

- Follow the existing Page Object Model structure — add new page classes under `src/test/java/pages/`
- Add corresponding test classes under `src/test/java/tests/`
- Ensure all tests pass locally before raising a PR
- Avoid hardcoded test data — use `config.properties` or `@DataProvider`
- Write descriptive test method names that reflect the scenario being tested

---

## 👤 Author

**tech-prajwal**
- GitHub: [@tech-prajwal](https://github.com/tech-prajwal)
- Repository: [OpenCartUIAutomation](https://github.com/tech-prajwal/OpenCartUIAutomation)

---

## 📄 Related Projects

| Repository             | Description                                         |
|------------------------|-----------------------------------------------------|
| [RestAssuredE2E](https://github.com/tech-prajwal/RestAssuredE2E) | REST API E2E test automation with REST Assured, Docker & Jenkins |

---

> ⭐ If you find this project useful, please consider giving it a star on [GitHub](https://github.com/tech-prajwal/OpenCartUIAutomation)!
