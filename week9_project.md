# Project Work Week 1

## Descriptive Summary

This is the first week of the UNDAC project where each member of the team has to take an issue from the team repository project board and complete it and then integrate it into the group system. Due to a plethora of issues I have decided to work with a couple members of my team as well as some from another team with the goal of joining their team upon manager approval.

The issue myself and the developers I am pair programming with this week chose is to reinstate access to returning users so that they have the required and appropriate access. This access is controlled to control access to mission systems and ensure data security.

The acceptance criteria for this issue is two fold. The first is to allow the details of the users current system privileges to be viewed. It was decided amongst the team to take a very simple approach to this as it could get quite out of hand code wise implementing this. Due to time constraints, and time wasted trying to fix technical and group issues, this was not feasible. Therefore, we implemented this by adding a simple tick or cross next to the user to show whether they had access or not. We felt although basic, this was acceptable to meet this criteria.
The second criteria was to request access from the Deputy Team Leader. Again, this could have snowballed code wise quite easily. Given we only have a week to do this and that we ran into loads of problems this week, we decided to keep it simple. We implemented this by adding a pop up screen that requests a password which would have to have been provided by the team leader. The user can't access the system without this password.

## Code Snippets with Commentary

### Method 1

```
async void OnSaveClicked(object sender, EventArgs e)
    {
        //test that the Name field is not empty and display alert
        if (string.IsNullOrWhiteSpace(Item.Name))
        {
            await DisplayAlert("Name Required", "Please enter a name for the todo item.", "OK");
            return;
        }
       
        await database.SaveItemAsync(Item);
        await Shell.Current.GoToAsync("..");
    }
```
The above method is an even handler method for a save button on click event. The method firstly checks if the item object is null, empty, or consists of white space characters. If it is empty it will run the DisplayAlert method, if not it will save the item to the database.

I have noticed it is difficult NOT to follow good design practice for most of the methods for these issues as they all tend to be quite small with very little code. If given more time on each issue, we could evolve these issues into more complex issues but I don't feel there is time for that and the code isn't marked so feels redundant to spend so much time on code. With that said, there are no obvious code smells that have made their way into this code. Moreover, the code adheres to DRY, YAGNI, KISS, and the Single Responsibility Principle. I also think it provides good code clarity and promotes readability.

### Method 2

```
async void AccessToggled(object sender, ToggledEventArgs e)
    {
        if(e.Value == true)
        {
            string password = await DisplayPromptAsync("Admin Verification", "Enter admin password:");

            if (password != "ABC")
            {
                // Incorrect password, revert switch and inform the user
                ((Switch)sender).IsToggled = false;
                await DisplayAlert("Access Denied", "Incorrect password.", "OK");
            }
        }
    }
```
This method was pair programmed with a couple of other programmers this week as we struggled to come up with an easy way to implement this criteria with limited time. This is essentially creating a password system so that only users who have been given this password from the team leader can get access. Simple but effective given the time constraints.

Initially, I thought this method was great as it adheres to the common principles DRY, KISS, and YAGNI. However, upon closer inspection I noticed some issues. Primarily this method doesn't adhere to the Single Responsibility Principle as it is handling both UI interaction and business logic, it would be best to place these into separate concerns. It also fails the open/closed principle as the code is not closed for modification if the validation logic becomes more complex. Moreover, there are a few code smells apparent due to the hardcoded password, lack of scalability, and improper abstraction. I don't have time this week to fix these issues but I will take note and take this forward with me.

## Tests with Commentary

My team tried to get mock tests working this week. The mock tests were to create a "mock" entry into the database instead of actually adding useless data to the database. However, this was to no avail as it just kept throwing error after error. Therefore, we decided to implement some basic xUnit tests.

### Test 1

```
public void GetUserList()
    {
        var teamMemberDB = new TeamMemberDB();
        var expectedList = new List<int> { 1, 2, 3 };

        var result = teamMemberDB.getUserList();

        Assert.Equal(expectedList, result);
    }
```
The above test tests the ability of a database class to retrieve a list of user identifiers. The first line creates an instance of the database class TeamMemberDB. The second line creates a list of integers that represents the outcome of the getUserList call. The next line calls the method on the database instance and expects a list of IDs from the database. Finally, the last line asserts that the list matches the actual result. The assert.equal is used to compare the two lists, if they are not equal then the test will fail.

### Test 2

```
public void GetUserAccess()
    {
        var teamMemberDB = new TeamMemberDB();
        int anyId = 123; 

        var result = teamMemberDB.getUserAccess(anyId);

        Assert.False(result);
    }
```
The above test tests the functionality of the GetUserAccess method on the database class. This method takes an integer ID and returns a boolean to represent access to something. This is done by creating an instance of the TeamMemberDB on the first line. The second line defines and assigns a placeholder ID. Next, the method is called with the placeholder ID and a boolean is returned to show if the user has access or not. On the final line, if the boolean is true the test has failed due to the usage of assert.false.

## Reflective Summary

I had my code peer reviewed by a couple of the developers I worked with this week. It was refreshing this week to work with people who were easy to get in touch with and jump into meetings with! The first developer mentioned that I should implement DoxyGen style comments into my code every week. I will inherit this into future weeks but did not find it necessary this week due to the basic nature of the code.

The second developer went a little more in depth. They mentioned that I should not use "async void" really unless it was an event handling method as it makes error handling difficult. The way to do this is to extract this code into two methods and add error handling. I decided not to do this as it is not something we have focused on in the rest of the week. However, the code snippet below shows how it would look.

```
private async Task SaveItemAsync()
{
    if (string.IsNullOrWhiteSpace(Item.Name))
    {
        await DisplayAlert("Name Required", "Please enter a name for the todo item.", "OK");
        return;
    }
    
    await database.SaveItemAsync(Item);
    await Shell.Current.GoToAsync("..");
}

async void OnSaveClicked(object sender, EventArgs e)
{
    try
    {
        await SaveItemAsync();
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", "An error occurred while saving the item.", "OK");
    }
}

```

## Descriptive Summary

I find it very difficult with my lack of experience and the lack of substance to the code we are reviewing, to find any obvious issues in other developers' code at the minute. I think we need more time to develop the methods to allow for these mistakes to creep in. However, after reviewing a couple of developers code I noticed similar issues to my own code. There is a lack, or complete disregard for comments it seems. This could however be due to the basic nature of all the code we are creating right now or the unfamiliar nature of DoxyGen at this point. The other thing I have noticed is the overuse of async void in non error handling methods, however, I don't feel this is too important at this point. I don't feel like good coding principles are being broken too often at this stage due to the simplistic nature of all the methods.

## Refelection

### Realisation of New Things

1. I had been struggling the past couple weeks trying to understand how people knew what to code just by looking at the issues provided. It turns out clicking on an issue is supposed to provide more information, something that the member of my team who posted the issues did not provide. I didn't know they were posted elsewhere and this caused a lot of confusion. After talking to lecturers this issue was made clear and has made understanding the issues much better.
1. I had several issues trying to get a working SQLite database working due to the inadequacy of the tutorial I followed. After working with a new team this week they provided me with the code to attach a database and I can see how this pieces together now. This allows me to complete these issues. I still feel the need for a better tutorial though.
1. My main realisation this week is that some people and/or some teams just don't work together. With no financial incentive it feels very difficult to get some people to engage and even harder to get knowledgeable people to take the time to teach others. Finding a group of like minded people who will communicate is absolutely key. This week was infinitely better than all previous weeks for this reason.
1. The important of communication tools is apparent for me this week. My previous team solely used Discord in a text format to communicate. This is adequate for some things but not all. Sometimes, a conversation is needed over voice or video. The new team I am working with are far more willing to jump in a call together and hash out problems. This was invaluable this week.

### Common Problems with Team Development

1. Communication is a huge issue with the team development. That has been the number one issue with my previous team for so many reasons. Mistakenly pushing to the main and causing issues, not helping others with problems, not communicating the issues correctly, the list goes on. I feel this was actually at a breaking point with my team which is why I requested to move.
1. Merge conflicts have been a huge issues with the team development as well. As developers work on different parts of the code and try to merge them together, errors are occurring constantly. Good communication could mitigate this but in my teams case the communication is terrible therefore the problems stack up.
1. Timeliness due to flexitime working has been a huge issue. Obviously, different people work better at different times. I myself work best in the middle of the night and this doesn't coexist well with people who work well in the morning. But, my old team seemed to leave everything till late the night before work was expected to be complete. This was useless to people who needed a bit more assistance and caused loads of issues and late submissions.

### Comparing Practices

I feel like a lot of the developers doing this prefer to work alone for the most part. This makes the teamwork side of this very difficult to achieve. I myself prefer to work collaboratively with a few developers and bounce ideas off of each other. Even coding with others can be very beneficial as all of this is new. It can be hard to get people to join in with this but I have found it much easier in my new team to make this happen.

Code reviewing practices seem to differ slightly as well. I have noticed others like to work together and constantly code review each other. While I find this okay, I would rather work together to produce the code and then get someone to do one thorough code review at the end. This is due to liking to be able to fully concentrate on developing the code and then on editing the code once a thorough review has taken place. Thoroughness over speed.

With regards to testing, I much prefer to take the approach of writing tests after implementation. I have noticed some people like to create a test driven development environment, but I would generally avoid that if I could. I usually like to cover a wide range of tests but have found this difficult with this project so I have had to settle for mostly assert tests.

** Due to technical and group issues, this is the first project week I have completed so therefore can't develop further on the week 8 reflection. I have been given an extension and will therefore do this in reverse order**