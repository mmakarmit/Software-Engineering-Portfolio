# Project Work Week 5

## Descriptive Summary
In week 12, the last week of the UNDAC project I decided to do my own issue rather than one of the cookie cutter issues on GitHub. This allowed me to do something a bit more complex that made more sense to me. I got the skeleton code for this from ChatGPT and then myself and the developers I pair program with manipulated that code to fit our system using GeeksForGeeks extensively.

The task we decided to do ended up being very complex and we did not get it working 100% but, it was still very beneficial learning and there is enough good code to discuss. The task was a task management system which specifically focuses on prioritisation and categorisation. This should allow us to manage the other tasks more efficiently improving workflow.

## Code Snippets with Commentary

### Method 1: `CategoriseAndPrioritiseTasks`
```csharp
public void CategoriseAndPrioritiseTasks()
{
    var tasks = _taskService.GetAllTasks();
    var categorisedTasks = tasks.GroupBy(t => t.Category).ToDictionary(g => g.Key, g => g.ToList());

    foreach (var category in categorisedTasks.Keys)
    {
        categorisedTasks[category] = PrioritiseTasks(categorisedTasks[category]);
    }

    _taskService.UpdateTasks(categorisedTasks.SelectMany(kvp => kvp.Value).ToList());
}

private List<Task> PrioritiseTasks(List<Task> tasks)
{
    return tasks.OrderBy(t => t.Deadline).ThenBy(t => t.Workload).ToList();
}
```
The above code demonstrates managing tasks in a fairly complex manner. Firstly, it categorises a task, then it looks at the deadline and workloads and prioritises the task. The method makes use of LINQ for data manipulation, complex data structures, and lambda expressions.

This adheres to good coding principles in many ways. Primarily, it adheres to the SRP principle as it doesn’t do anything apart from categorise and prioritise tasks. The code is modular, clear, and has descriptive naming. It also strongly adheres to DRY and YAGNI.

### Method 2: `VisualiseTaskDistribution`
```csharp
public Dictionary<string, int> VisualiseTaskDistribution()
{
    var tasks = _taskService.GetAllTasks();
    return tasks.GroupBy(t => t.Category)
                .ToDictionary(g => g.Key, g => g.Count());
}
```
This method looks a lot simpler than the previous one but is just as important. It visualises the tasks across different categories. Again, using LINQ, it groups tasks by category and maps that to the count of tasks contained. It also uses dictionaries and key value pairs, which I had previously only ever really used in Python.

Much like the previous method, this one also adheres to SRP for the same reasoning. It also adheres to DRY, YAGNI, and even KISS. As much as it uses some complex features, it is not overly complicated. Moreover, descriptive and clear naming is once again used.

## Test with Commentary

### Test 1: `SortsTasksCorrectly`
```csharp
[Fact]
public void SortsTasksCorrectly()
{
    var mockTaskService = new MockTaskService();
    var taskManager = new TaskManager(mockTaskService);
    taskManager.CategoriseAndPrioritiseTasks();

    var tasks = mockTaskService.GetUpdatedTasks();
    foreach (var category in tasks.Select(t => t.Category).Distinct())
    {
        Assert.True(IsSortedByDeadlineAndWorkload(tasks.Where(t => t.Category == category).ToList()));
    }
}

private bool IsSortedByDeadlineAndWorkload(List<Task> tasks)
{
    for (int i = 1; i < tasks.Count; i++)
    {
        if (tasks[i].Deadline < tasks[i - 1].Deadline || 
            (tasks[i].Deadline == tasks[i - 1].Deadline && tasks[i].Workload < tasks[i - 1].Workload))
        {
            return false;
        }
    }
    return true;
}
```
This unit test is particularly complicated and way beyond my current skill level with tests. Therefore, we leaned heavily on AI for help constructing this, our part mostly came from trying to get it to integrate into our environment correctly. Which, took a considerable amount of time adjusting settings. The test, tests to see if method 1 sort tasks correctly within each category. It uses the mock service to simulate data retrieval and a method to validate the logic.

The test case adheres to most of the common good coding principles as you would expect as it was mostly constructed by AI. DRY, KISS, YAGNI, and SRP are all apparent here as well as highly readable and maintainable code.

## Reflective Summary of Changes
As the code was intentionally more complex this week, it was mostly beyond any one developers solitary skills. Therefore, there was no feedback with changes to make, largely due to the AI influence as well. One developer mentioned no comments as an issue but, I had intentionally left them out as they don't feel overly useful when snippets of code are being used anyway.

## Descriptive Summary of Issues Found
I only done one code review this week and found the code to be of a much higher standard than the earlier weeks. I could not find any evidence of SRP being broken for probably the first time during this project. The only issue I could find with the code was the developer had abbreviated naming which wasn't overly intuitive, therefore, I recommended that they rethink their naming to adhere more closely to good naming conventions.

## Reflection

### Realisation of New Things
1. Lambda expressions were a new concept to me this week. I had seen and heard of them before but had never studied them. I had to research quite a bit this week to try and understand them for this task. It is something that still needs a lot more of my attention as they seem extremely useful.
1. During the implementation of this week's task I realised the importance of performance when working with algorithms. This is something that had always been in the back of my mind when attempting to write code, but it is especially apparent when dealing with more complex systems with large datasets.
1. LINQ queries were a new concept to me this week, which ties in with the lambda expressions point. Again, this needs a lot more of my attention in the future, but I am glad to have started the process of learning these two new things at this point in the project.

### Common Problem with Team Development
Merging code into the main branch continues to be a challenge, even with my new team. Even with good communication, this has caused a myriad of issues and much like my last team it has come to a point where we cannot all integrate into the same branch. I feel at this point this is something that could only be rectified with everyone in a team receiving training on this point. Or, at least study all together to achieve this singular purpose.

### Comparing Practices
As this is the final week of the project, comparing my practices to that of my peers, I have noticed an increased focus in efficiency and performance in my approach. Moreover, I continue to focus on SRP, clear readable code, and naming conventions more than most others, from what I can see from our exchange of code reviews. I am now much more mindful of these things and believe I will take them forward with me through my career.