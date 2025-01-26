# 4.1. Version control in team work

The goal of this submodule is to learn how to use version control in team work. The submodule is divided into three parts:
the first part is about the basics of version control, the second part is about branches. The third part covers merging and merge conflicts. Finally, we discuss the commit comments, which are crucial for documenting the changes in the project.

## 4.1.1. Basics of version control

In your earlier studies, you have learned the basics of version control. You probably know how to pull, commit, and push changes to a repository. As a quick recap, here are the most important commands:
- **pull** retrieves the latest changes from the remote repository
- **commit** saves the changes to the local repository
- **push** sends the changes to the remote repository

This set of commands is often enough if you work in a small project as the only developer. However, in a larger project, you need to be able to work in a team. In this case, you need to know how to create branches, merge branches, and resolve merge conflicts.


## 4.1.2. Branches

In GitHub, branches are used to develop features isolated from each other. The `master` branch is the "default" branch when you create a repository. Use other branches for development and merge them back to the `master` branch upon completion.

Let's start with a simple example. We create a new project in IntelliJ IDEA and add one class into its `src` folder. The class is called `GreetingMachine`, and it prints out a greeting message. The code is shown below:

```java
public class GreetingMachine {

    public void greet() {
        System.out.println("Hello World!");
    }

    public static void main(String[] args) {
        GreetingMachine gm = new GreetingMachine();
        gm.greet();
    }
}
```

Out of this project, we now create a repository in GitHub. We do this by clicking the **VCS** menu and selecting **Share Project on GitHub**. After this, we can commit and push our changes to the remote repository.

As we share the project in GitHub, an initial branch called `master` is automatically created. This is the default branch in GitHub. We can see the branch in the "Branches" tab in GitHub:

![Branches](images/one_branch.png)

From now on, let's keep the production code in the `master` branch.

We now create a new branch for development. Let's call it `feature`. In IntelliJ IDEA, we can create a new branch by clicking the **Git** menu and selecting **New Branch**:

![New branch](images/new_feature.png)

In the dialog box, the checkbox **Checkout branch** is selected by default. This means that we automatically switch to the new branch after creating it. We can also create a new branch without switching to it. In this case, we need to switch to the new branch manually. Using Git terminology, The Git variable HEAD now points to the current branch. We can see the current branch in the bottom right corner of IntelliJ IDEA:

![Current branch](images/git_head.png)

As this is our development branch, let the development begin!

We add a new method to the `GreetingMachine` class. The new method is also called `greet` but it takes a parameter of type `String`. The code is shown below:

```java
public class GreetingMachine {

    public void greet() {
        System.out.println("Hello World!");
    }

    public void greet(String message) {
        System.out.println(message + "!");
    }

    public static void main(String[] args) {
        GreetingMachine gm = new GreetingMachine();
        gm.greet();
        gm.greet("Good Morning Universe");
    }
}
```

The code works just fine, so we can commit and push our changes to the remote repository:

1. Click **Git / Commit** in the main menu.
2. Type a commit message in the **Commit Message** field. For example, "Add a custom greeting".
3. Click **Commit and Push**. Check that the updated files are selected in the opening dialog box.

When you look at your source code at the GitHub website, you can see that the new method is not there. This is because the new method is in the `feature` branch, and the `master` branch does not know anything about it. You can see the branches in the **Branches** tab in GitHub. Also, when you open a file in GitHub, you can see the branch name in the top left corner of the file:

![Branch name](images/git_branch_selector.png)

Just change the branch name to `feature` and you can see the source code of the new method.

## 4.1.3. Merging

In the previous section, we created a new branch called `feature` and added a new method to the `GreetingMachine` class. Now that we are happy with the new code, we want to merge the `feature` branch to the `master` branch. This means that we want to add, or incorporate, the new method to the `master` branch. We can do this in IntelliJ IDEA:

1. Make sure you have both branches checked out and updated to the latest version. You can do this by first switching to the desired branch in the bottom right corner and performing a checkout operation. Then select **Git / Pull** in the main menu.

2.  Swith to the `master` branch. Then click **Git / Merge** in the main menu. Select the `feature` branch in the opening dialog box. Click **Merge**.  IntelliJ IDEA performs the merge operation. If there are no conflicts, the merge is successful. (If you are vigilant, you may  notice a green toaster in the bottom left corner of IntelliJ IDEA. It suggests that you can delete the `feature` branch. You can click that, or delete the branch in the next step.)

3. If the `feature` branch is not needed anymore, we can delete it. Click **Git / Branches** in the main menu. Select the `feature` branch and click **Delete**.

4. Push the changes to the remote repository. Click **Git / Push** in the main menu. Check that the updated files are selected in the opening dialog box.

If you look at the source code in GitHub, you can see that the new method is now in the `master` branch.


The procedure above provides a nice, systematic way to develop features in a team. However, sometimes things go wrong. For example, you might have made changes to the same file in two different branches. In this case, you get a merge conflict. Let's see how this works in practice.


## 4.1.4 Resolving merge conflicts

Merge conflicts arise when two branches have changed the same part of the same file, and then those branches are merged together.

It is possible for a conflict to occur even if no separate branches have been created. When two developers make changes to the same file and both try to push their changes to the same `master` branch without using any branches, a merge conflict can arise.

In this situation, when both developers attempt to push their changes to the remote repository (called `origin` in Git terminology), Git detects that the changes conflict with each other. This happens because Git cannot automatically determine which changes should be accepted and which should be rejected.

When a conflict arises, Git notifies you and marks the file with a "conflict state." In the conflict state file, you can see the changes made by both developers, and you must manually resolve the conflicts.

Resolving a merge conflict requires collaboration between the developers. Typically, it involves reviewing the conflicting changes and deciding which changes to accept or combining both sets of changes. Once the conflicts are resolved, the file is marked as "conflict resolved," and you can proceed with pushing the changes forward.

Let's generate a situation where a merge conflict occurs.

As a starting point, we have the GreetingMachine class in the `master` branch. Let's pull the latest changes from the remote repository, and check out the `master` branch.

In the `master` branch, we modify the `GreetingMachine` class by changing the `Hello World` text in the `greet` method into `Hello Pluto`:

```java
    public void greet() {
        System.out.println("Hello Pluto!");
    }
```

Now, we commit the change _but do not push_.

At the same time, we modify the same file directly in the remote repository via the GitHub website. You can do this by clicking the pencil icon in the top right corner of the file:

![Edit file](images/github_edit.png)


We change the `Hello World` text in the greet method into `Hello Mars` this time:

```java
    public void greet() {
        System.out.println("Hello Mars!");
    }
```

At the GitHub website, we continue by committing the changes directly to the `master` branch.

Now, we have a situation where the same file has been modified in two different places. Let's see what happens when we try to push the changes in IntelliJ IDEA from the local repository to the remote repository.

In IntelliJ IDEA, we click **Git / Push** in the main menu. The IDE detects the discrepancy between the local and remote repositories, and we get the following dialog box:

![Merge conflict](images/push_alert.png)

Click **Merge** to proceed. Now, we get the following dialog box:

![Merge conflict](images/conflicts_dialog.png)

Here, you can click **Accept Yours** or **Accept Theirs** to resolve the conflict. **Accept Yours** means that you want to keep the changes you made in the local repository. **Accept Theirs** means that you want to keep the changes made in the remote repository. It may not be a good habit to routinely click one of these buttons without thinking. You should always carefully consider which changes to keep.

Let's assume we are not yet sure which changes are worth keeping. It may also be the case that we need to manually craft a combination of the changes.
To keep that choice, we click **Merge**. Now, we get the following dialog box:

![Merge conflict](images/conflicts_resolve.png)

Here, we can see:
- the changes made in the local repository on the left
- the changes made in the remote repository on the right
- the merged result in the middle

We can manually edit the result in the middle to resolve the conflict. For example, if we want to keep the text `Hello Pluto!`, we click the arrow button that points from the left to the middle. This means that we want to keep the changes made in the local repository.

When we are done, we can click **Accept Merge**. Now, we can **commit** and **push** the changes to the remote repository.

This is how you solve merge conflicts in IntelliJ IDEA. In practice, you should avoid merge conflicts by using branches. However, sometimes conflicts are unavoidable. In this case, you should resolve the conflicts as soon as possible. It is a messy business, but it has to be done.


## 4.1.4. Commit comments

At this point you know the basics of team work using GitHub. There are other features as well, but you can learn them as you go. However, there is one thing that you should know about: commit comments.

It is far too common to follow bad practices when writing commit comments. For example, you might write a comment like "Fix a bug" or "Add a feature". This is not a good practice. The commit comment should always describe what you have done in the commit. For example, "Fix a bug" could be "Fix a bug in the GreetingMachine class". "Add a feature" could be "Add a custom greeting to the GreetingMachine class".

If you ever need to revert to an older version of your code, you use the commit comments to find the version you are looking for. If you have written good commit comments, you can easily find the version you are looking for. If you have written bad commit comments, you are in trouble.

Also, each commit should just span the changes related to a single feature. If you have made changes to multiple files, you should commit the changes to each file separately. This way, you can easily revert the changes to a single file if needed. It is a very bad practice to work for an entire day, and then commit all the changes at once. If you do this, the comments are of little use.

<!---
## Assignment

This assignment is done in a small team. You can choose your team members freely. The team should consist of 2-3 members.

The goal is to practice branching, merging, and resolving merge conflicts. You should also practice writing good commit comments.

Proceed as follows:

1. Create a new repository in GitHub. Make all team members collaborators.
2. Clone the repository to your local computer.
3. Create a new Java project in IntelliJ IDEA. Make sure that the project is in the repository folder.
4. Add the following Java class into the project. Note that the class is on purpose not very well written. It is missing comments and the code is not very readable. You should fix these issues later in your own branch.

```java
public class PalindromeChecker {

    public static void main(String[] args) {
        
        Scanner scanner = new Scanner(System.in);
        String input = scanner.nextLine();
        
        String transformed = transformInput(input);
        
        boolean isPalindrome = checkPalindrome(transformed);
        if (isPalindrome) {
            System.out.println("The transformed input is a palindrome.");
        } else {
            System.out.println("The transformed input is not a palindrome.");
        }
    }

    public static String transformInput(String input) {
        input = input.toLowerCase();
        input = input.replace(",", "");
        input = input.replace("!", "");
        return input;
    }

    public static boolean checkPalindrome(String input) {
        String reversed = reverseString(input);
        if (input.equals(reversed)) {
            return true;
        } else {
            return false;
        }
    }

    public static String reverseString(String input) {
        StringBuilder reversed = new StringBuilder();
        for (int i = input.length() - 1; i >= 0; i--) {
            reversed.append(input.charAt(i));
        }
        return reversed.toString();
    }
}
```

5. Commit the changes (i.e., the adding initial code) and push them to the remote repository.

From now on, each team member should create an own branch and work there. The branch name should be the name of the team member. For example, if the team member's name is John, the branch name should be `john`.

Each team member should refactor the code in their own branch. Refactoring means that you change the code without changing its functionality. For example, you can rename variables, extract methods, add comments, and so on. Try to improve the legibility of the code as much as possible.

When you are done, commit the changes and push them to the remote repository.

When all team members are done, select the team member whose name is first in alphabetical order. This team member should merge the branches of all team members into the `master` branch. This should cause a merge conflicts. Resolve the conflicts together with the team members. Finally, push the changes to the remote repository.

When resolving the conflicts, discuss with the team members which changes to keep.

For this assignment, you get points as follows:

- the repository with the initial code was created (1 point)
- the branches were created (1 point)
- the code in each branch was refactored (1 point)
- all merge conflicts were resolved (2 points)
- the resulting code contains the best parts of the code written by each team member (1 point)

---
_This learning material has been produced with assistance from OpenAI's ChatGPT-4 and GitHub Copilot. These large language models have provided suggestions and solutions that have assisted the author in producing and supplementing the material. While their contribution has been significant, the final responsibility for the content and its correctness resides with the author._

-->


# 4.2. AI-assisted coding

GitHub Copilot is an AI-powered coding assistant developed through a collaboration between OpenAI and GitHub.
GitHub Copilot is designed to help programmers write code more efficiently and quickly. It integrates directly into programming environments such as IntelliJ Idea and automatically suggests code snippets or functions in real time as you type. Capable of understanding your intentions through the code you write, it learns from your coding style and past solutions.

In this submodule, we install GitHub Copilot and activate in in IntelliJ Idea. Then, we explore the functionality with a few coding assignments.

## 4.2.1. Installation and activation

GitHub CoPilot requires a paid or free plan. The use of GitHub Copilot is included in the GitHub Education package, which is available to all students in Metropolia.

To sign up for GitHub Education, proceed as follows:

1. Make sure that you have your real name set in your GitHub profile. If you have not done so, click on your profile thumbnail in GitHub, then click **Settings** and **Profile**. Enter your real name in the **Name** field and click **Save**.
2. Go to https://education.github.com/pack and verify your student status.
As a consequence of the verification procedure, you should receive an approval notification by email. The following ways to prove one's identity are known to work for students in Metropolia (In verification, GitHub seems to prefer documents showed via the webcam to scanned documents):
   - the student card from the Tuudo app displayed via the webcam
   - a transcript of records from Oma
2. Log in to GitHub. Click your profile thumbnail in the upper right corner and select **Settings**.
3. Choose **Billing and Plans / Plans and usage**. Enable GitHub Copilot.
4. In the **Settings** menu click **Copilot**. Choose **Allow** under the **Suggestions matching public code** section. Also, tick **Allow GitHub to use my code snippets...** if you want to expose your own code as data for other users´ recommendations.

Next, open IntelliJ Idea.
1. Choose **File / Settings**.
2. Click **Plugins**.
3. Type **GitHub Copilot** in the search box and click **Install**. After the installation, restart your IDE if the system so prompts.
4. Go to **Tools / GitHub Copilot**, and select **Log into GitHub**. Enter your GitHub credentials when requested and log in.
5. Click **Tools  / GitHub Copilot** and select **Show completions**. If this option is already turned on, you can skip this step.
6. Click **Tools  / GitHub Copilot** and select **Show suggestions**. If this option is already turned on, you can skip this step.

Now you should be ready to use GitHub Copilot. Play with it a bit to get a feeling of how it works. Note that you can choose between different copilot suggestions with `ctrl`+`.` and `ctrl`+`,` shortcuts. Then complete the following assignments to further explore the functionality.


## Assignments

Submit the answer of this assignment as a pdf file. The file should contain the source codes of the relevant classes as well as an explanation of how you used GitHub Copilot to write the code. For each assignment, evaluate the applicability of GitHub Copilot for your own coding style. What are the benefits and drawbacks of using GitHub Copilot?

**Task 1: Using GitHub Copilot to write methods**

In a new project, write a `Calculator` class that has the ability to sum positive integers. A negative integer should throw an exception.

The class acts as the model in the MVC pattern. It should have the following methods:
- A method that resets the calculator to zero.
- A method that adds an integer to the calculator.
- A method that returns the current value of the calculator.

In addition, write a temporary main method that creates an instance of the `Calculator` class and uses it to calculate the sum of a few integers.

Explore various way in which you can use GitHub Copilot to write the sum method. For example, try the following:

- Write the method signature and let GitHub Copilot write the method body.
- Write the method signature and the first line of the method body, and let GitHub Copilot write the rest. Then, modify the code to use a different loop structure (while instead of for, or vice versa) in the body.
- Write a comment where you describe the method in English, letting GitHub Copilot write the method body based on the comment.


**Task 2: Using GitHub Copilot to explain code**

With GitHub Copilot, you can also write comments and explanations of code. In this assignment, you will write a textual explanation of the `Calculator` class and its methods. Use GitHub Copilot to write the explanations. Then, modify the explanations to make them more readable and understandable.

Proceed as follows:

1. Create a Markdown file in your project. Name it `README.md`. If you are not familiar with Markdown, you can read about it here: https://www.markdownguide.org/basic-syntax/
2. Write a short description of the idea of the program in the beginning of the file. You can edit the md document in IntelliJ Idea.
3. Copy the code of the `Calculator` class to the md file. Be sure to mark the code as code by using three backticks before and after the code block:
4. Below the code, explain how the class works. Use GitHub Copilot to write the explanation. Then, modify the explanation to make it more readable and understandable.

For this assignment, you get points in the following way:

1. Task 1 completed: 3 points
2. Task 2 completed: 3 points

---
_This learning material has been produced with assistance from OpenAI's ChatGPT-4 and GitHub Copilot. These large language models have provided suggestions and solutions that have assisted the author in producing and supplementing the material. While their contribution has been significant, the final responsibility for the content and its correctness resides with the author._




