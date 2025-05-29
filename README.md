# New-Project-Lap-Diagnostic
Code for Job search
// === LabCorp UI Test in C# with Selenium and Reqnroll === // Directory: LabCorpJobAutomation/
 
// Reqnroll Feature File - Features/JobSearch.feature Feature: Job Search and Validation on LabCorp Careers Page
 
Scenario: Search and validate job posting details Given I open the LabCorp home page When I navigate to the Careers page And I search for "QA Test Automation Developer" And I select the job posting Then I validate the Job Title, Location, and Job ID And I validate the job description paragraph and requirements When I click Apply Now Then I verify job details on the application page And I return to Job Search
 
// Step Definitions - Steps/JobSearchSteps.cs using OpenQA.Selenium; using OpenQA.Selenium.Chrome; using NUnit.Framework; using Reqnroll; using LabCorpJobAutomation.Pages; using LabCorpJobAutomation.Drivers;
 
[Binding] public class JobSearchSteps { private IWebDriver _driver; private HomePage _homePage; private CareersPage _careersPage; private JobDetailsPage _jobDetailsPage; private ApplicationPage _applicationPage;
 
[BeforeScenario]
public void Setup() {
    _driver = WebDriverSupport.CreateDriver();
}
 
[AfterScenario]
public void TearDown() {
    _driver.Quit();
}
 
[Given("I open the LabCorp home page")]
public void GivenIOpenTheLabCorpHomePage() {
_driver.Navigate().GoToUrl("https://www.labcorp.com");
    _homePage = new HomePage(_driver);
}
 
[When("I navigate to the Careers page")]
public void WhenINavigateToTheCareersPage() {
    _homePage.ClickCareers();
    _careersPage = new CareersPage(_driver);
}
 
[When("I search for \"(.*)\"")]
public void WhenISearchFor(string jobTitle) {
    _careersPage.SearchJob(jobTitle);
}
 
[When("I select the job posting")]
public void WhenISelectTheJobPosting() {
    _careersPage.SelectFirstResult();
    _jobDetailsPage = new JobDetailsPage(_driver);
}
 
[Then("I validate the Job Title, Location, and Job ID")]
public void ThenIValidateTheJobDetails() {
    Assert.IsTrue(_jobDetailsPage.GetTitle().Contains("QA Test Automation Developer"));
    Assert.IsTrue(_jobDetailsPage.GetLocation().Contains("USA"));
    Assert.IsTrue(_jobDetailsPage.GetJobId().StartsWith("24"));
}
 
[Then("I validate the job description paragraph and requirements")]
public void ThenIValidateParagraphsAndRequirements() {
    Assert.IsTrue(_jobDetailsPage.GetIntroParagraph().Contains("participate in the test automation technology development"));
    Assert.IsTrue(_jobDetailsPage.GetSecondBulletUnderManagement().Contains("Prepare test plans"));
    Assert.IsTrue(_jobDetailsPage.GetThirdRequirement().Contains("5+ years"));
    Assert.IsTrue(_jobDetailsPage.GetTools
