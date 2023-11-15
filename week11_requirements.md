# Project Work Week 4

## Descriptive Summary
This week's task was to enhance the volunteer management system to allow for effective volunteer recruitment. The end-user goal is to efficiently identify appropriate team members, while the end business goal focuses on ensuring that the team possesses the necessary capacity and skills for the mission. The key features implemented include browsing and filtering volunteer lists by skill, location, and status, flagging volunteers for recruitment requests, and tracking the date of status changes along with arrival and departure dates for confirmed volunteers.

## Code Snippets with Commentary

### Method 1: `OnAppearing`
```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    PopulatePickers();
    PopulateFieldsIfEditing();
}

private void PopulatePickers()
{
    StatusPicker.ItemsSource = EnumToList<VolunteerStatus>();
    SkillPicker.ItemsSource = _dataService.GetSkills();
    LocationPicker.ItemsSource = _dataService.GetLocations();
}

private void PopulateFieldsIfEditing()
{
    if (_existingVolunteer == null) return;

    FirstNameEntry.Text = _existingVolunteer.FirstName;
    LastNameEntry.Text = _existingVolunteer.LastName;
    SkillPicker.SelectedItem = _existingVolunteer.Skill;
    LocationPicker.SelectedItem = _existingVolunteer.Location;
    StatusPicker.SelectedItem = _existingVolunteer.Status.ToString();
}

private List<string> EnumToList<TEnum>() where TEnum : Enum
{
    return Enum.GetValues(typeof(TEnum)).Cast<TEnum>().Select(e => e.ToString()).ToList();
}
```
The above method is a good example of clean code that follows good coding principles. It illustrates SRP by segregating different functionalities into distinct methods (PopulatePickers and PopulateFieldsIfEditing). This makes the code more readable, maintainable, and testable. Each sub-method performs a specific task, reducing complexity and improving modularity.

The method also demonstrates good abstraction by using EnumToList, a generic method, to convert enum values to a list. This improves code reusability and makes the method more adaptable to changes. The usage of clear and descriptive method names enhances the readability and understandability of the code, making it easier for other developers to grasp the purpose and functionality of each part of the code.

### Method 2: `LoadData`
```csharp
private void LoadData()
{
    Volunteers.Clear();
    var allVolunteers = _dataService.GetAllVolunteersWithDetails();
    Volunteers.AddRange(allVolunteers);
}

public class DataService
{
    public List<Volunteer> GetAllVolunteersWithDetails()
    {
        return GetAllVolunteers().Include(v => v.Skill).Include(v => v.Location).ToList();
    }
}
```
LoadData, adheres to SRP and it also adheres to Separation of Concerns (SoC). The method is tasked solely with updating the view model's volunteer list. This clear outline of responsibility makes the code easier to understand, maintain, and debug. The DataService class, used within this method, encapsulates all data access operations, thereby separating the concerns of data fetching and UI updating.

## Tests with Commentary

### Test 1: `OnAppearing_InitializesPickersCorrectly`
```csharp
[Fact]
public void OnAppearing_InitializesPickersCorrectly()
{
    var viewModel = new VolunteerViewModel();
    viewModel.OnAppearing();

    Assert.NotEmpty(viewModel.StatusPicker.ItemsSource);
    Assert.NotEmpty(viewModel.SkillPicker.ItemsSource);
    Assert.NotEmpty(viewModel.LocationPicker.ItemsSource);
}
```
This unit test checks if the OnAppearing method correctly initialises the pickers in the ViewModel. It follows good testing practices by focusing on a single aspect of the method's functionality. The use of clear assertions makes the test easy to understand and ensures that the UI will be populated correctly, which is vital for a smooth user experience.

### Test 2: `LoadData_PopulatesVolunteersCorrectly`
```csharp
[Fact]
public void LoadData_PopulatesVolunteersCorrectly()
{
    var viewModel = new VolunteerViewModel(new MockDataService());
    viewModel.LoadData();

    Assert.NotEmpty(viewModel.Volunteers);
}
```
This test ensures that the LoadData method in the ViewModel correctly populates the Volunteers collection. It employs a MockDataService to simulate data retrieval, isolating the test from external data dependencies. This isolation is a best practice in unit testing, ensuring reliability and consistency of the test results. The test follows the Arrange-Act-Assert pattern that I have used in previous weeks, clearly organising the setup, execution, and verification steps, which improves the readability and maintainability of the test code.

## Reflective Summary of Changes
This week, the feedback from my peer reviews was that there was too much going on inside my larger methods that were definitely not adhering to SRP. Due to this, I focused on refactoring code for better readability and adherence to design principles. I did use AI to help with this to make sure I got it right and to deepen my learning of it along the way. The refactoring led to cleaner and more maintainable code.

## Descriptive Summary of Issues Found
During code reviews, I noticed a common issue where methods were doing more than they should, violating the Single Responsibility Principle. This is the main principle that I am seeing being violated by most people, including myself. It is difficult to get out of this style of coding as we have done it for years unchecked. I advised splitting larger methods into smaller, more focused ones, which was well-received by the other developers who were in agreement.

## Reflection

### Realisation of New Things
1. The power of refactoring in enhancing code readability and maintainability became evident this week. Breaking down complex methods into smaller, more focused ones makes the code easier to understand and debug. This is something I have noticed in my own code and other peoples code most weeks, but the issue keeps rearing it's head every week!
2. I realised the importance of mock data services in unit testing. By using mock services, we can isolate tests from external dependencies, leading to more reliable and faster tests.
3. I learned the effectiveness of extending lists or collections directly instead of iterating and adding items one by one.

### Common Problem with Team Development
The main problem that is apparent week on week is merging our work into the main. We are still at a stage where most people break the whole repository when we try to merge to main and are not really sure why. This is an extremely difficult obstacle to overcome when no one in the team is technically proficient with Visual Studio and GitHub.

### Comparing Practices
Comparing my current practices with those of my peers, I’ve noticed an increased emphasis on clean code principles in my work. This focus has led to more efficient and less error-prone code, but I am still needing to get my hand held a bit through this process with the aid of AI or other team members. 

