# Sample README.md File

# selenium-automation-template

## Project Overview
This automation framework is designed to test the e-commerce web application using Selenium WebDriver with Java. The framework implements Page Object Model design pattern and supports cross-browser testing for Chrome, Firefox, and Edge browsers.

**Application Under Test:** E-commerce Shopping Platform 
**Test Types:** Smoke, Regression, and End-to-End tests 
**Framework:** Selenium WebDriver + TestNG + Maven

## Prerequisites

### Required Software
- **Java JDK:** Version 11 or higher
- **Maven:** Version 3.6 or higher
- **Git:** Latest version
- **IDE:** IntelliJ IDEA or Eclipse (recommended)

### Browser Requirements
- **Chrome:** Version 120+ (ChromeDriver auto-managed by WebDriverManager)
- **Firefox:** Version 118+ (GeckoDriver auto-managed)
- **Edge:** Version 120+ (EdgeDriver auto-managed)


## Setup Instructions

### 1. Clone Repository
```bash
git clone https://github.com/your-org/selenium-automation-template.git
cd selenium-automation-template
```

### 2. Install Dependencies
```bash
mvn clean install
```

### 3. Configure Test Environment

```
If Applicable, add steps for setting up the env
```

### 4. Set Up Test Credentials
Create `src/test/resources/testdata/user-credentials.json`:
```json
{
  "validUser": {
    "username": "test.user@example.com",
    "password": "TestPassword123"
  },
  "adminUser": {
    "username": "admin@example.com", 
    "password": "AdminPass456"
  }
}
```

## Test Execution

### Run All Tests
```bash
mvn clean test
```

### Run Specific Test Suites


```bash
mvn clean test -Dsuite=smoke-tests
```

**Cross-Browser Testing:**
```bash
mvn clean test -Dbrowser=firefox
mvn clean test -Dbrowser=edge
```

**Headless Execution:**
```bash
mvn clean test -Dheadless=true
```

### Run Tests in Parallel
```bash
mvn clean test -Dparallel=methods -DthreadCount=3
```

### Run Single Test Class
```bash
mvn clean test -Dtest=LoginTests
```

### Run Specific Test Method
```bash
mvn clean test -Dtest=LoginTests#testValidUserLogin
```

## Configuration

### Environment Configuration
| Property | Description | Default Value |
|----------|-------------|---------------|
| `base.url.staging` | Staging environment URL | Required |
| `default.browser` | Browser for test execution | chrome |
| `headless.mode` | Run tests without GUI | false |
| `implicit.wait` | Global wait time (seconds) | 10 |
| `explicit.wait` | Element wait time (seconds) | 20 |

### Test Data Configuration
- **User Credentials:** `src/test/resources/testdata/user-credentials.json`
- **Product Data:** `src/test/resources/testdata/product-data.csv`
- **Test Scenarios:** `src/test/resources/testdata/test-scenarios.xlsx`

### Reporting Configuration
Reports are generated in `target/surefire-reports/` directory:
- **HTML Report:** `emailable-report.html`
- **XML Report:** `testng-results.xml`
- **Screenshots:** `screenshots/` folder (on test failures)

## Test Structure

### Project Organisation
```
src/
├── test/
│   ├── java/
│   │   ├── pages/          # Page Object classes
│   │   ├── tests/          # Test classes organised by feature
│   │   ├── utils/          # Utility classes and helpers
│   │   └── config/         # Configuration and setup classes
│   └── resources/
│       ├── testdata/       # Test data files
│       ├── config/         # Configuration files
│       └── testng.xml      # TestNG suite files
```

### Page Object Model Implementation
```java
// Example: LoginPage.java
public class LoginPage extends BasePage {
    @FindBy(id = "email")
    private WebElement emailField;
    
    @FindBy(id = "password") 
    private WebElement passwordField;
    
    @FindBy(css = "button[type='submit']")
    private WebElement loginButton;
    
    public void loginWithCredentials(String email, String password) {
        enterEmail(email);
        enterPassword(password);
        clickLoginButton();
    }
}
```

### Test Class Example
```java
// Example: LoginTests.java
public class LoginTests extends BaseTest {
    
    @Test(groups = {"smoke", "regression"})
    public void testValidUserLogin() {
        // Arrange
        LoginPage loginPage = new LoginPage(driver);
        String email = testData.getValidUser().getEmail();
        String password = testData.getValidUser().getPassword();
        
        // Act
        loginPage.loginWithCredentials(email, password);
        
        // Assert
        DashboardPage dashboard = new DashboardPage(driver);
        Assert.assertTrue(dashboard.isUserLoggedIn(), 
            "User should be successfully logged in");
    }
}
```

## Reporting

### Test Reports Location
- **HTML Reports:** `target/surefire-reports/emailable-report.html`
- **Screenshots:** `screenshots/failure-screenshots/`

### Report Features
- Test execution summary with pass/fail counts
- Detailed failure reasons with stack traces
- Screenshots captured automatically on test failures
- Test execution time and browser information
- Environment and configuration details

### Viewing Reports
1. Open `target/surefire-reports/emailable-report.html` in browser
2. Check `logs/automation.log` for detailed execution logs
3. Review screenshots in `screenshots/` folder for failed tests

## Troubleshooting

### Common Issues and Solutions

**Issue: ChromeDriver not found**
- **Solution:** Ensure WebDriverManager is properly configured in `BaseTest.java`
- **Verification:** Check Chrome browser version matches driver version

**Issue: Element not found exceptions**
- **Solution:** Increase explicit wait times in `config/environment.properties`
- **Check:** Verify element locators are correct for current application version

**Issue: Tests fail in headless mode**
- **Solution:** Some elements may behave differently; test in headed mode first
- **Debug:** Add screenshots before interactions in headless mode

**Issue: Parallel execution failures**
- **Solution:** Ensure test data isolation and no shared resources
- **Fix:** Use ThreadLocal for WebDriver instances

**Issue: Slow test execution**
- **Solution:** Optimise wait strategies and reduce unnecessary waits
- **Check:** Monitor network and application response times

### Debug Mode
Enable debug logging by setting in `log4j2.xml`:
```xml
<Logger name="com.yourpackage" level="DEBUG"/>
```

### Environment-Specific Issues
- **Staging Environment:** Check VPN connection and credentials
- **Production Environment:** Ensure read-only test data usage
- **Local Environment:** Verify application is running on correct port

## Framework Extension Guide

### Adding New Page Objects
1. Create new class in `src/test/java/pages/`
2. Extend `BasePage` class
3. Define page elements using `@FindBy` annotations
4. Implement page-specific methods

### Adding New Test Data
1. Add data files in `src/test/resources/testdata/`
2. Update `TestDataManager.java` to load new data
3. Create data model classes if needed
4. Reference data in test classes

### Adding New Test Categories
1. Update TestNG groups in test methods
2. Create new TestNG suite files in `src/test/resources/`
3. Configure Maven profiles for new categories
4. Update documentation with execution commands

### Cross-Browser Configuration
1. Add browser configuration in `BrowserFactory.java`
2. Update environment properties with new browser options
3. Test with new browser and update documentation
4. Configure CI/CD pipeline for additional browsers
