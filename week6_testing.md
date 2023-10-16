# Testing

## Introduction

This weeks entry into my portfolio focuses on testing. Primarily xUnit test cases for a method
that I write to be integrated into a group project for a Hangman game. Due to the same team
based issues I have faced every week, I decided to this week tackle the task with a member of my team I can
communicate with easily and do it pair programming style. I found this to be more beneficial 
than I initially thought.

## Code Summary

The purpose of the code I was testing was to implement functionality into the UpdateDisplay()
method. This is a method in the GamePage class for my group Hangman style game. This method
has two main priorities which both involve updating labels. The first is to update the 
RemainingAttemptsLabel with the number of remaining attempts the player has left. The second
is to update the WordLabels to show visibility and letters to represent correct guesses and the
letters that have been guessed correctly.

## Test Code 1

```
[Fact]
public void TestCorrectGuess()
{
     // Setup
     var game = new GamePage("Easy")
     {
         RemainingAttemptsLabel = new Label(),
         WordLabels = new List<Label>
         {
             new Label{ IsVisible = false },
             new Label{ IsVisible = false },
             new Label{ IsVisible = false }
         }
     };
     string word = "cat";
     char guessedLetter = 'a';

     // Call
     game.UpdateDisplay(true, word, guessedLetter, 3);

     // Assert
     Assert.Equal("Remaining Attempts: 3", game.RemainingAttemptsLabel.Text);
     Assert.False(game.WordLabels[0].IsVisible);
     Assert.Equal("a", game.WordLabels[1].Text);
     Assert.True(game.WordLabels[1].IsVisible);
     Assert.False(game.WordLabels[2].IsVisible);
}
```

### Explanation

The main purpose of this test is to verify when a correct letter is guessed which is ultimately
the whole point in the game. While doing this it also tests that the remaining attempts label
is updated correctly as well as ensuring the correct letter label is displayed for each letter.

### Why is it Important?

This is vitally important as this test case ensures the feedback the user receives is accurate.
If this did not work correctly and did not acknowledge a correct case then the game is essentially
unplayable. It is vital that the system recognises and correctly displays a letter when a 
correct guess has taken place. This is essential for keeping players engaged and enhancing
the player experience.

## Test Code 2

```
[Fact]
public void TestIncorrectGuess()
{
     // Setup
     var game = new GamePage("Easy")
     {
         RemainingAttemptsLabel = new Label(),
         WordLabels = new List<Label>
         {
             new Label{ IsVisible = false },
             new Label{ IsVisible = false },
             new Label{ IsVisible = false }
         }
     };
     string word = "cat";
     char guessedLetter = 'z';

     // Call
     game.UpdateDisplay(false, word, guessedLetter, 2);

     // Assert
     Assert.Equal("Remaining Attempts: 2", game.RemainingAttemptsLabel.Text);
     Assert.False(game.WordLabels[0].IsVisible);
     Assert.False(game.WordLabels[1].IsVisible);
     Assert.False(game.WordLabels[2].IsVisible);
}
```

### Explanation

The main purpose of this test is to identify and confirm when an incorrect letter has been 
guessed. Which is arguably, just as important as the previous test. This is done by updating
the remaining attempts label and showing there is one less attempt than the previous guess.
Moreover, no changes are made to the visibility of letter labels.

### Why is it Important?

This test ensures that this aspect of the game works correctly and helps to maintain the
challenge of the game. If it incorrectly showed letter labels when someone guessed wrong,
it would diminish the challenge of the game and in essence, ruin it. Just like the previous
test, it also helps provide clear feedback to the user which will reduce confusion and enhance
the experience.

## Why Was This Method Important to Test?

The reasons for testing this method are much the same as the reasons for the test cases. Everything
I have tested here ensures this aspect of the game works correctly and therefore, enhances
the experience for the user. Providing immediate feedback to the user about whether the guess
is correct or not is a must. It also ensures that only correct answers display a letter so as
not to ruin the game for the user. Moreover, it gives the user a sense of game progress so
they know where they are, and how much farther to go with this word.

## Limitations of Test Cases

I decided to go ahead and complete this part in the portfolio before the test battles 
as I don't think my team are going to have a working test class due to a myriad of issues
the night before. Due to this I have had to think abstractly as to why they may be limited.

The first limitation that comes to mind is that I am testing with very limited test cases here.
I only check for one correct and one guess, which leaves a plethora of possibilities untested.
This is the first thing I would rectify given more time. 
Another issue is the tests do not test for possible side effects occurring from other methods
in the class. Due to the difficult nature of communicating with my team, I have little knowledge
of what anyone is working on and how their methods look.


## Reflection

Yet again just like week 3 and 5, I found this to be an exceptionally difficult week. I do not
really feel it is clear what to do and don't feel enough people in the class have the skills to do
it, never mind teach others. It doesn't feel like it works as a group project at this level. Moreover,
my group only tends to communicate on a Sunday night, not leaving enough time to fix issues, and then
everyone rushes to submit whatever they can.
On a more positive note I tried pair programming with the only team member I can get in touch with
easily this week and found it exceptionally beneficial. Neither of us have strong skills in any
of these areas but by putting our heads together we get there in the end.

