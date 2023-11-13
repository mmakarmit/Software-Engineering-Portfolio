# Project Work Week 3

## Descriptive Summary

This week the [task](https://github.com/timh1975/UNDAC-Project/issues/53) is, as an UNDAC Team Support and Logistics Manager, the focus was on developing functionalities to efficiently manage and assign resources like food, medical supplies, and fuel to operations. The aim was to align with the end-user goal of managing requests effectively and the end business goal of optimal resource utilisation.

## Code Snippets with Commentary

### Method 1: AssignEquipmentToTeam
```csharp
public async Task<bool> AssignEquipmentToTeam(int equipmentId, int teamId)
{
    var equipment = await database.Table<Equipment>().Where(i => i.ID == equipmentId).FirstOrDefaultAsync();
    if (equipment != null)
    {
        equipment.AssignedTeamId = teamId;
        await database.UpdateAsync(equipment);
        return true;
    }
    return false;
}
```
This method is designed to assign a piece of equipment to a specific team. It updates the assigned team ID of an equipment item after verifying its existence in the database.

This code block demonstrates good coding by first of all having strong adherence to the SRP. The methods assigns equipment to a team and that is all. It shows good asynchronous programming for database operations which is best practice. It is highly readable and allows for early exit if the conditions are not met. Finally, of course, it adheres to the usual suspects, DRY, KISS, and YAGNI.

### Method 2: SearchEquipmentAsync
```csharp
public async Task<List<Equipment>> SearchEquipmentAsync(string name)
{
    return await database.Table<Equipment>()
                        .Where(equipment => equipment.Name.Contains(name))
                        .ToListAsync();
}
```
This method facilitates the search for equipment by name, returning a list of equipment items that match the provided search string.

Again, this code block shows strong adherence to the DRY, KISS, and YAGNI principles. It adheres to SRP as it solely searches for equipment by name, which is great for readability and reusability.

## Test with Commentary

### Test 1: SearchEquipmentAsync_ReturnsCorrectItems
```csharp
public async Task SearchEquipmentAsync_ReturnsCorrectItems()
{
    // Arrange
    var testData = new List<Equipment>
    {
        new Equipment { Name = "Drill", Type = "Power Tool" },
        new Equipment { Name = "Hammer Drill", Type = "Power Tool" },
        new Equipment { Name = "Saw", Type = "Hand Tool" }
    };

    var mockDataService = new Mock<IEquipmentDataService>();
    mockDataService.Setup(service => service.GetAllEquipmentAsync()).ReturnsAsync(testData);

    var equipmentDB = new EquipmentDB(mockDataService.Object);
    var searchQuery = "Drill";

    // Act
    var result = await equipmentDB.SearchEquipmentAsync(searchQuery);

    // Assert
    Assert.Equal(2, result.Count); // Expecting two items containing "Drill" in their names
    Assert.All(result, item => Assert.Contains(searchQuery, item.Name));
}
```
This test ensures that the method returns the correct number of items and that all returned items contain the search query in their names.

### Test 2: AssignEquipmentToTeam_AssignsCorrectly
```csharp
public async Task AssignEquipmentToTeam_AssignsCorrectly()
{
    // Arrange
    var mockDb = new Mock<SQLiteAsyncConnection>();
    var testEquipment = new Equipment { ID = 1, Name = "Drill", AssignedTeamId = 0 };

    // Simulating database fetching and updating behavior
    mockDb.Setup(db => db.Table<Equipment>().Where(It.IsAny<Expression<Func<Equipment, bool>>>()).FirstOrDefaultAsync())
          .ReturnsAsync(testEquipment);
    mockDb.Setup(db => db.UpdateAsync(It.IsAny<Equipment>())).ReturnsAsync(1);

    var equipmentDB = new EquipmentDB(mockDb.Object);
    int equipmentId = 1;
    int teamId = 101; // Hypothetical team ID

    // Act
    bool success = await equipmentDB.AssignEquipmentToTeam(equipmentId, teamId);

    // Assert
    Assert.True(success);
    Assert.Equal(teamId, testEquipment.AssignedTeamId);
}
```
This test verifies if the `AssignEquipmentToTeam` method successfully assigns equipment to the designated team.

Both of these tests are using unit testing best practices. They are both structured to use setup, execution, and verification sections which means its using the arrange, act, and assert pattern. The use of mocking also ensures we are not reliant on a database which means these are true unit tests. Which are very, very difficult to get working!

## Reflective Summary of Changes

My code was peer reviewed by two people this week and the feedback was positive. There were actually no suggested changes this week besides to maybe put more meaningful comments in the test code as it is quite complicated and arrange, act, assert may not be enough for everyone. There was no need for comments in the methods as the code is self-explanatory.

## Descriptive Summary of Issues Found

I peer reviewed one other developer's code this week and the only feedback I gave them was to reduce their commenting. Some methods had more lines of comments than lines of code and most of it was highly unnecessary. The nature of the code was very basic, so it was very difficult to find anything else wrong with it.

## Reflection

### Realisation of New Things

This week I learned a couple of little markdown techniques to make some things look better in my repository. The first thing is when using a code block in markdown, to put the name of the language after the triple backticks. This enables syntax highlighting for the named language which I think looks considerably better and more readable than the standard black and white code block.

Another technique I learned is to put anything I want highlighted in between single backticks and this highlights it in a monospaced font. This is great for names, variables, methods, and so on.

### Common Problem with Team Development

Getting testing to work in a team environment where most people are unexperienced is unbelievably troublesome. Our team spent hours collaborating trying to get everyone up to speed with xUnit testing at least. Most of us already had it working or got it working, but in the end some simply still could not. Moreover, a few of us got together to tackle mock testing for the second time. We ran into countless errors and in the end got a couple of working tests with the help of AI.

### Comparing Practices

The only point to mention this week for comparing practices is again about testing. I and the developers I work with are trying out new testing techniques most weeks. From talking to others, I have gathered that most people are struggling with testing and can't even get xUnit tests to work. We seem to have got xUnit tests working fairly quickly and are now trying to get our heads round effective mock testing, as well as trying out a test-driven development at some point.

