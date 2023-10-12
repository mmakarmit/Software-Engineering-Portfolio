# Documentation

## Introduction

The entry of my personal development portfolio this week is about documentation for code.
This includes discussing if the code meets good coding rules, commenting or not commenting, and
using Doxygen to produce a HTML document of the code. I am using code that was supposed to 
be done for the [Week 3](week3_workflow.md) Workflow task but as explained in my portfolio
entry that week, due to lack of instruction, communication from my team, and general idea of what 
to do, I didn't have any code. Therefore, I pieced together what I could this week with a lot
of outside help so I could carry out this task.


## Six Rules of Clean Code and Comparing to my Code

```
using SkillsSQLite.Models;
using System;
using System.Collections.Generic;
using SkillsSQLite.Views;

namespace SkillsSQLite
{
    /**
     * \brief Main page for skill management.
     */
    public partial class SkillsMainPage : ContentPage
    {
        public SkillsMainPage()
        {
            InitializeComponents();
        }

        /**
         * \brief Clears the feedback message on the UI.
         */
        private void ClearFeedbackMessage()
        {
            feedbackMessage.Text = "";
        }

        /**
         * \brief Handles the addition of a new skill.
         *
         * This method is triggered when the user tries to add a new skill.
         * It clears any existing feedback messages, adds the skill, and then updates the feedback.
         * 
         * \param sender Source of the event.
         * \param eventArgs Event data.
         */
        private void HandleNewSkill(object sender, EventArgs eventArgs)
        {
            ClearFeedbackMessage();
            App.SkillsRepository.AddSkill(skillInput.Text);
            feedbackMessage.Text = App.SkillsRepository.StatusInfo;
        }

        /**
         * \brief Fetches and displays all the skills.
         *
         * This method retrieves all the skills from the repository and sets them for display.
         * 
         * \param sender Source of the event.
         * \param eventArgs Event data.
         */
        private void FetchAllSkills(object sender, EventArgs eventArgs)
        {
            ClearFeedbackMessage();
            List<Skill> skills = App.SkillsRepository.RetrieveAllSkills();
            skillsDisplay.ItemsSource = skills;
        }

        /**
         * \brief Navigates to the ModifySkillPage for a selected skill.
         *
         * When a skill is selected, this method is triggered to navigate to its modification page.
         * 
         * \param sender Source of the event.
         * \param eventArgs Event data.
         */
        private async void SkillSelected(object sender, EventArgs eventArgs)
        {
            if (sender is Grid selectedGrid && selectedGrid.BindingContext is Skill chosenSkill)
            {
                await Navigation.PushAsync(new ModifySkillPage(chosenSkill));
            }
        }
    }
}

```

### Rule 1 - Clear Descriptive Naming
This rule essentially means that all variables, methods, and classes should be named in a clear,
concise, succinct, and descriptive manner. The idea behind this is it should be clear to
anyone who may be looking at your code at a later date, what these things do just by the name.
I was able to improve the second draft of my code using this rule as the FetchAllSkills
method was initially called OnGetButtonClicked. The original name isn't very clear and only
really indicates that there is a button click taking place. The new name however, describes
what will happen when the click happens, which is retrieve all skills in the database. There were
examples like this throughout the code which has been implemented in the second draft.

### Rule 2 - DRY (Don't Repeat Yourself)
The DRY rule is very self explanatory. It essentially means to not repeat any unnecessary code,
therefore, every piece of logic should have one unambiguous purpose within the system. This 
means to not repeat code at all unless specifically required. A way around this is to create a 
method for any repeating code. In my code, I had the feedback message, feedbackMessage.Text = ""; initialising with an empty
string multiple times throughout the code. I placed this message into it's own method to satisfy this 
rule. This felt a bit overkill and probably unnecessary, but I done it to demonstrate the rule.

### Rule 3 - YAGNI (You Aren't Gonna Need It)
The YAGNI rule pertains to making sure you don't add any functionality until it is actually
required for the system. A developer may think of things that they think will be required and
pre-emptively code them in when they aren't actually required at all. Ensuring you only add
functionality when it is required will avoid this pitfall. I believe my code meets this rule
already as everything that I have is required to do the task I need it to do. This is a limited
piece of code and I imagine given a much larger codebase, this issue would be more apparent.

### Rule 4 - KISS (Keep It Simple Stupid)
The KISS rule essentially means to keep everything in the code as simple as it can be to
effectively do the task that is required. If the task requires more complex code, that is 
fine but don't be a [StackOverflow](https://stackoverflow.com/) developer and make things 
complicated just to show off how good you are. It is virtually useless if only you can understand
it and future developers are baffled when they come to maintain or update your code. Again,
I feel my code adheres to this rule already as it is a small block of code that is doing a 
basic task. (If you are comfortable with C# that is!)

### Rule 5 - Comment Wisely
The comment wisely rule pertains to the use of commenting within a given code block. What 
is wise commenting? Well, as it turns out, this means to go against everything I had been previously
taught. As code is supposed to be as self explanatory as possible, commenting should only be
used to explain particularly complex pieces of code, or anything that just isn't abundantly
obvious to look at. Therefore, avoid redundant comments that state the obvious. For instance, 
having a "fetches all skills" comment above the FetchAllSkills method would be completely
redundant. I was able to avoid this pitfall that I certainly would have fell into as I have
not yet commented the code, this will be done with the next part of this portfolio entry.

### Rule 6 - Consistent Formatting
This rule as the name would suggest is to keep consistent formatting throughout all of the
code. This includes the previously mentioned naming conventions, keeping consistent indentation,
consistent placement of braces like the Allman style or K&R, consistent use of whitespace, 
line length, and commenting style. There are other aspects of consistent formatting as well, 
the list really does go on and on. I was able to improve the code due to this rule as I had
mistakenly named some methods using camelCase and others using PascalCase. Once I realised this,
I changed all methods to use PascalCase and ensured all variables used camelCase as is the convention
with C#.


## Comments and Doxygen

```
/**
 * \brief Main page for skill management.
 */
 
/**
 * \brief Clears the feedback message on the UI.
 */

/**
 * \brief Handles the addition of a new skill.
 *
 * This method is triggered when the user tries to add a new skill.
 * It clears any existing feedback messages, adds the skill, and then updates the feedback.
 * 
 * \param sender Source of the event.
 * \param eventArgs Event data.
 */

/**
 * \brief Fetches and displays all the skills.
 *
 * This method retrieves all the skills from the repository and sets them for display.
 * 
 * \param sender Source of the event.
 * \param eventArgs Event data.
 */

/**
 * \brief Navigates to the ModifySkillPage for a selected skill.
 *
 * When a skill is selected, this method is triggered to navigate to its modification page.
 * 
 * \param sender Source of the event.
 * \param eventArgs Event data.
 */
```

Doxygen comments are specially formatted comments processed by the Doxygen tool to generate
easy to understand documentation. This is used the world over by developers to provide
documentation for a myriad of reasons. Below is a short breakdown of the structure used:

\brief - This is the command that provides a concise summary of the code that follows it.
In my code it gives a short description of what the methods and classes all do. The lines that
follow up until the next command, give a more detailed description of the methods and classes.

\param - This is the command to describe the parameters of a method. In my code the parameters
of each method is explained in detail.

There are a plethora of purposes for using the Doxygen commenting system as they serve as a
bridge between humans and the code. Predominantly, they offer clarity by displaying both
brief and detailed descriptions to explain the purpose and behaviour of each component as
necessary. This then has a continuous and sequential effect by making the code more readable
and maintainable which in turn enhances collaboration between developers. Moreover, Doxygen
obviously provides structured and easy to read documentation so developers and non-developers
can more easily understand how all the pieces fit together.

I had a lot of issues trying to get my source files to work correctly with Doxygen. I had no support
from my team as no one who knows what to do communicates and therefore had to move on without being 
able to get the required screenshots of the documentation.


## Eliminated Need for Comments

In this codebase I have obviously added Doxygen style comments to everything but that is
just for the purpose of generating (or trying to) documentation for all of the methods therein.
I feel like the need for further inline comments has been eliminated pretty much throughout the
codebase by following the good coding rules. I will give three examples below.

### Meaningful Names
I have named all methods, classes, and variables in a meaningful enough way that I do not
feel like there is a need for further inline comments to explain anything. I think my naming
clearly conveys what each thing needs to do. For instance, private void ClearFeedbackMessage(),
it is obvious that this is going to clear the feedback message.

### Clear Structure
I think the methods in this code follow a clear sequence and generally speaking have a 
singular responsibility and due to this eliminates the need for any further inline comments.
For instance, private void FetchAllSkills(object sender, EventArgs eventArgs), has a single
responsibility to fetch and display skills. This makes it self-explanatory.

### Clear Event Handling
My one integral piece of event handling is dealt with in a clear way, by naming clearly, 
and ensuring it handles a single event logically. This eliminates a lot of the confusion
usually apparent with event handling and removes the need for further inline comments again.


## Reflection

I found this week to be exceptionally difficult as there was utter confusion yet again as
to what we were actually to do. This was further emphasised by even the assistant lecturer
thinking we should not have coded by this point and telling us to use a different coding task.
It unfolded that we were to use code from a previous week where near enough my whole team
were unable to complete the code due to a massive disparity in my team in experienced people. There
is no knowledge sharing going on and it is making all of these tasks far more difficult than they
need to be. I feel like when forming the teams, there should have been assurances that there
were people with the required skills in each group.

Furthermore, I had great difficulty getting Doxygen to read from my source file correctly.
It was displaying a HTML document with some basic information, but not everything, and not
correctly. I tried researching online, asking people in my group, and asking people in other
groups and could not really get the help I required. I decided after already losing several
marks to late hand in to just submit what I could. This is now only the second time ever I
have handed in late work through all my years of study and both times relate to the same task!

The one area we have touched on recently where I do comfortably feel like I have learned and
sharpened some skills on is the area of clean code. I still find it incredibly difficult 
and definitely needs a lot more work but after the past couple of weeks I have grasped a lot
of the concepts and even started to understand most of the code smells, which was an entirely
new concept to me.