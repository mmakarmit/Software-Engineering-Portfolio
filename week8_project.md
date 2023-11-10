# Project Work Week 2

## Descriptive Summary

This is the second week of work for the UNDAC project, (although I am submitting it into the first week due to issues), where we have to pick another issue, implement it, and merge it into the team project. Due to the success of working with a new team last week, I officially changed teams this week. This has been a breath of fresh air with regards to team communication and collaboration. Making it far easier to learn how to use all these new tools and techniques.

The issue I chose to work on this week was a simple one that could piggyback onto the issue I done last week. It was to allow for requests to remove user access when users leave so that security is maintained. This ensures data security and prevents unwanted access to the system.

Again, the acceptance criteria is two-fold. The first is to allow for requests to be automatically approved. The second is that when access is removed, that the record is not deleted but instead set to disabled. We took a straightforward approach to this, this week again due to time limitations from last weeks never ending series of problems. I felt it appropriate to add this functionality into our issue from last week as it all fits together. This way instead of creating a whole new issue we could just add this extra functionality into the delete button, which worked out great and saved us a bunch of time.

## Code Snippets with Commentary

### Method 1

```
protected override async void OnNavigatedTo(NavigatedToEventArgs args)
    {
        base.OnNavigatedTo(args);
        var items = await database.GetItemsAsync();
        MainThread.BeginInvokeOnMainThread(() =>
        {
            Items.Clear();
            foreach (var item in items)
                Items.Add(item);

        });
    }
```
In the above code, when a user navigates to a new screen, data is fetched from the database without freezing the screen, which is an excellent feature for these kinds of applications. Afterwards, it updates the user interface with the new data.

I have made sure to adhere to KISS, DRY, and YAGNI by not repeating code, implementing unnecessary code, or over complicating the method. The code also adheres to the open/closed principle and the Liskov principle from what can be seen here as it doesn't change the behaviour of the base class.

### Method 2

```
public class TeamMemberModel
    {

        /// <summary>
        /// Attributes for Employees in the table
        /// </summary>
        [PrimaryKey, AutoIncrement]
        public int ID { get; set; }

        public string Name { get; set; }

        [Column("access")]
        public bool access { get; set; }

        [Column("status")]
        public bool status { get; set; }
    }
```
The above code defines a C# class called TeamMemberModel. This represents a user, employee, or team member within the given system. This has been designed to map to a database table. It uses auto increment to auto increment the ID which is the primary key of the database. The rest creates properties and maps them to the database.

This code shows good coding principles by first of all showing good clarity as it is easy to understand and properties are named in a way that makes their purpose obvious. It adheres to the usual KISS, DRY, and YAGNI by being non repetitive, not being over complicated, and by not implementing any code that isn't required. Finally, it adheres to SRP as it only relates to a team members data model. It shows good coding conventions by using attributes like primary key and auto increment to define scheme mappings.

## Tests with Commentary

### Test 1

```
public void ChangeAndCheckUserStatus() 
    {
        var teamMemberDB = new TeamMemberDB();

        String ID = "1";

        teamMemberDB.DeleteUserWithID(ID);

        bool result = teamMemberDB.CheckUserAccess(ID);

        Assert.False(result);
    }
```
The above test, tests the interaction with the database to alter the status of a user and verify that this has been achieved. More specifically that the access of ths user is revoked. This is done by creating an instance of TeamMemberDB and setting the string ID variable to 1 in the first two lines. The next two lines attempt to delete or disable the user and then checks to see if their access has been correctly removed. An assertion is made in the last line to confirm all of this which is typical in xUnit testing.

## Reflective Summary of Changes

I had my code peer reviewed by a team member this week and the feedback was mostly positive. It was mentioned to me that this developer had difficulty finding issues with the code due to the small size of the methods which is something I believe also. With that said it was pointed out that my summary comment at the top was redundant due to the code being self explanatory and was advised to remove that. Furthermore, it was pointed out that I had mistakenly not used correct C# naming conventions as I had named a property access instead of Access, and status instead of Status. These were easy changes to rectify and I was happy to receive feedback that wasn't just "comment more". Below is the code with changes.

```
public class TeamMemberModel
    {
        [PrimaryKey, AutoIncrement]
        public int ID { get; set; }

        public string Name { get; set; }

        [Column("access")]
        public bool Access { get; set; }

        [Column("status")]
        public bool Status { get; set; }
    }
```

## Descriptive Summary of Issues Found

Again, this week I found it very difficult to find issues with any code that I reviewed. Everyone is doing the bare minimum with code due to time constraints or using AI to help with code, and both of these things are making it very difficult to find flaws in the code.

With that said, I did notice a slight issue with one method I reviewed for someone which broke the single responsibility principle slightly. This person was updating the UI and carrying out some basic business logic in the same method which could have been placed into two methods for better adherence to this principle. I have noticed that this is probably the most common principle broken by non experienced programmers as it is tempting to do as much as possible in a single method.

## Reflection

### Realisation of New Things

1. This week I realised the importance of file paths especially when using Visual Studio. This is something I would normally take for granted as it seems simple and tools usually carry the burden here. There was a lecture on it this week and I have had my own issues with this. The main technical issue I had been having for weeks ended up being because important packages were pointing to the wrong folder for some reason. A little bit of research on this and redirecting these packages to the correct file path and now my environment works!
1. I feel like I am getting a stronger grasp of the principles of clean code due to repetition of using and looking for them in code reviews. I still have a long way to go but I am noticing them more and more and more so understanding them when people find them in my work.
1. I am starting to realise and have a better understanding of team workflow now that I am in a more positive team. There are guidelines laid out in this team which the whole team adheres too which was sort of absent in my previous team. I am also starting to further develop my own personal workflow. For instance, I am starting to organise my own development environment with packages and extensions I like. I am even starting to streamline my physical environment to how I like to work. I need further development in my personal workflow for GitHub integration and this is something I will work on.

### Common Problems with Team Development

I am finding it difficult to integrate different kinds of tests as they are difficult to implement and can mean spending a long time playing with the environment to get them working. I feel like my whole team is having this issue and is therefore just regurgitating basic tests due to this. This is something myself and the team are going to try and develop further moving forward to try and achieve better test coverage.

There is definitely a fairly big skill disparity in some groups which leads to an uneven workload which I have witnessed multiple times. This can lead to friction and issues within the group. This was weighted in my favour for a while due to issues I had but now that they are rectified I am going to make best endeavours to carry my weight.

Merge conflicts have been an issue in my previous group and this one. My previous group could not get anything to merge correctly without a plethora of problems. My new group has done this a lot better but there are still plenty of issues with merging that we need to work on moving forwards.

### Comparing Practices

I am trying to compare my tools usage to other developers to see how I can improve my environment (and therefore my personal workflow) and my coding ability. I have been doing this by communicating with other members of my group, communicating with a Microsoft mentor I have as part of a scheme, and by researching online.

My team has been researching workflow models to ensure we are using an effective model. By researching and comparing our strategy to information we have found online about effective teams we believe we are on the right track.

I have personally been looking at recommendations online for completing code reviews within a team as I feel this is an aspect my team could definitely improve on, as well as myself as an individual. This is also something my Microsoft mentor has been able to advise me on.