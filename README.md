# TechnicalTestDemo
Automated BDD Approach with Cucumber and Selenium WebDriver
This project is an example of UI automated functional test for Flight booking portal  and search using Selenium and Cucumber Java.

Test scenarios are described in the feature files located here ./src/test/java/Features.
Methods Implementations are described in the Step Definitions located here .src\test\java\stepDefinitions
Junit Test Runner are located here .src\test\java\Runner


Installations 

JDK 8 
Apache Maven -version 3.6.1 https://www.mkyong.com/maven/how-to-install-maven-in-windows/
Environment Variables 

To run the tests locally with Chrome, install ChromeDriver from here and add its location to your system PATH.
To run the tests locally with Firefox, install GeckoDriver from here and add its location to your system PATH.
To install all cucumber dependencies, 

cucumber-core-2.4.0.jar
cucumber-java-2.4.0.jar
cucumber-junit-2.4.0.jar
cucumber-jvm-deps-1.0.6.jar
gherkin-5.0.0.jar
junit-4.12.jar
mockito-all-1.10.19.jar

To install all dependencies, run
mvn clean install

Running tests
To run all the tests, use the command
mvn test

Project Description
Now that you know all about installing and running the test for this project, let’s begin the explanation on how it works by having a look at its structure.
The Feautures folders contains the files describing the test scenarios in Gherkin syntax, also known as feature files.
Feature: Flight Search

1)   Scenario Outline: User one way domestic trip search and Add Baggage
    Given user navigates the url
    When the home page title is displayed
    Then user enters the from "<From>" and "<To>"
    Then user selects redplan
    Then user add <Bags> in checked baggage and seats
    Then Application should be closed
     Examples:
       | From   |  To        | Bags |
       | Sydney | Brisbane   |  1   |
       | Sydney | Melbourne  |  2   |
       | Sydney | Perth      |  3   |
       | Sydney | Canberra   |  4   |
       | Sydney | Queensland |  5   |

2) For Instance , When the home page title is displayed , the feature file step will be mapped like below 
The StepDefinition folders contain the contains the implementation of these test scenarios. 

Table Code Base
 @When("the home page title is displayed")
    public void the_home_page_title_is_displayed() {
       homePage = new HomePage();
       String pageTitle= homePage.verifyHomePageTitle();
       Assert.assertEquals(true, pageTitle.toLowerCase().contains("qantas"));
    }
    
3) The steps of the test scenarios are defined in the steps classes such as HomePageSteps. 
The page elements and possible actions (click, input text…) are in the Page classes such as HomePage. 
This basically follows the PageObject Design pattern.

4) Test Runner are the executor of all the scenarios
Package Runner;

Code Snippet:
import io.cucumber.junit.Cucumber;
import io.cucumber.junit.CucumberOptions;
import org.junit.runner.RunWith;

@RunWith(Cucumber.class)
@CucumberOptions(
        features = "//src//test//java//Features//search.feature",
        glue={"stepDefinitions"},
        plugin = {"pretty","html:target/cucumber-reports"},
        monochrome = true,
        dryRun = false

        )
public class TestRunner {
}

5) Configuration Properties
browser=chrome
url=https://www.qantas.com
driverPath=//driver//chromedriver.exe

ConfigReader - This file retrieves the values stored in the configuration property file
 public String getBrowser(){
        String browserName = properties.getProperty("browser");
        if(browserName!= null) return browserName;
        else throw new RuntimeException("browsername not specified in the Configuration.properties file.");
    }
    
6) POM.xml Snapshot: 

Selenium Settings
 <properties>
    <!-- project dependency configured versions -->
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    <selenium.java.version>3.141.59</selenium.java.version>
    </properties>
    
    
Maven compiler and surefire plugins      
 <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.0</version>
                <configuration>
                    <source>8</source>
                    <target>8</target>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.22.0</version>
                <configuration>
                    <testFailureIgnore>true</testFailureIgnore>
                    <includes>
                        <exclude>**/*Runner.java</exclude>
                    </includes>
            </configuration>
            </plugin>
        </plugins>
    </build>

Cucumber Dependencies

 <!-- https://mvnrepository.com/artifact/io.cucumber/cucumber-jvm-deps -->
    <dependency>
        <groupId>io.cucumber</groupId>
        <artifactId>cucumber-jvm-deps</artifactId>
        <version>1.0.6</version>
        <scope>provided</scope>
    </dependency>

    <!-- https://mvnrepository.com/artifact/io.cucumber/cucumber-core -->
    <dependency>
        <groupId>io.cucumber</groupId>
        <artifactId>cucumber-core</artifactId>
        <version>4.7.0</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/io.cucumber/cucumber-java -->
    <dependency>
        <groupId>io.cucumber</groupId>
        <artifactId>cucumber-java</artifactId>
        <version>4.7.0</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/io.cucumber/cucumber-junit -->
    <dependency>
        <groupId>io.cucumber</groupId>
        <artifactId>cucumber-junit</artifactId>
        <version>4.7.0</version>
    </dependency>

7) Reports will be stored in the html:target/cucumber-reports

Resources to assist with this work:

https://blog.codeship.com/cucumber-best-practices/
https://saucelabs.com/blog/write-great-cucumber-tests
https://automationpanda.com/2017/01/30/bdd-101-writing-good-gherkin/
https://www.foreach.be/blog/9-tips-improving-cucumber-test-readability
https://cucumber.io/docs/bdd/better-gherkin/






