<img width="1436" alt="Screenshot 2025-04-30 at 4 38 54 PM" src="https://github.com/user-attachments/assets/fa33a730-dcb6-452c-9a04-e7aa80edafe2" />





# Resolving Concurrent Changes in Git: A Practical Scenario


https://github.com/bankolejohn/mini-git-project.git


This document outlines a common scenario in Git where two developers make changes to the same file concurrently and how Git handles the merging of these changes.

## Scenario: Updating `index.html`

**Goal:** Update the `index.html` file with navigation changes and new contact information in the footer.

**Participants:**

* **Tom:** Responsible for updating the navigation.
* **Jerry:** Responsible for adding contact information to the footer.

**Steps:**

1.  **Clone the Repository:** Both Tom and Jerry start by cloning the central repository to their local machines.

    ```bash
    git clone <repository_url>
    cd <repository_name>
    ```

    * `git clone <repository_url>`: This command creates a local copy of the remote repository on your machine. Replace `<repository_url>` with the actual URL of the Git repository.
    * `cd <repository_name>`: This command changes the current directory in your terminal to the newly cloned repository. Replace `<repository_name>` with the name of the cloned repository.

2.  **Create Branches:** Each developer creates their own branch to isolate their changes.

    **Tom's actions:**

    ```bash
    git checkout main
    git checkout -b update-navigation
    ```

    * `git checkout main`: This command switches your current branch to the `main` branch, ensuring you are starting from the latest version of the main codebase.
    * `git checkout -b update-navigation`: This command creates a new branch named `update-navigation` based on the current state of the `main` branch and immediately switches to this new branch.

    **Jerry's actions:**

    ```bash
    git checkout main
    git checkout -b add-contact-info
    ```

    * `git checkout main`: This command switches your current branch to the `main` branch.
    * `git checkout -b add-contact-info`: This command creates a new branch named `add-contact-info` based on the current state of the `main` branch and immediately switches to this new branch.

3.  **Make Changes:** Both Tom and Jerry independently make their changes to the `index.html` file on their respective branches.

    * **Tom (on `update-navigation` branch):** Modifies the navigation section of `index.html`.
    * **Jerry (on `add-contact-info` branch):** Adds contact information to the footer section of `index.html`.

4.  **Commit Changes:** After making their changes, both developers commit them to their local branches.

    **Tom's actions:**

    ```bash
    git add index.html
    git commit -m "Update navigation links"
    ```

    * `git add index.html`: This command stages the changes made to the `index.html` file, preparing them for the commit.
    * `git commit -m "Update navigation links"`: This command saves the staged changes to the local `update-navigation` branch with a descriptive message.

    **Jerry's actions:**

    ```bash
    git add index.html
    git commit -m "Add contact information to footer"
    ```

    * `git add index.html`: This command stages the changes made to the `index.html` file.
    * `git commit -m "Add contact information to footer"`: This command saves the staged changes to the local `add-contact-info` branch with a descriptive message.

5.  **Push Branches:** Both developers push their local branches to the remote repository.

    **Tom's actions:**

    ```bash
    git push origin update-navigation
    ```

    * `git push origin update-navigation`: This command uploads the `update-navigation` branch and its commits to the remote repository (typically named `origin`).

    **Jerry's actions:**

    ```bash
    git push origin add-contact-info
    ```

    * `git push origin add-contact-info`: This command uploads the `add-contact-info` branch and its commits to the remote repository.

6.  **Create Pull Requests:** Both Tom and Jerry create pull requests (PRs) from their respective branches to the `main` branch on the remote repository.

    * **Tom:** Creates a PR from `update-navigation` to `main`.
    * **Jerry:** Creates a PR from `add-contact-info` to `main`.

7.  **Review and Merge (Assuming Tom's PR is merged first):**

    * Tom's pull request (`update-navigation` to `main`) is reviewed and merged into the `main` branch. This updates the `main` branch on the remote repository with Tom's navigation changes.

8.  **Jerry Updates Local `main` Branch:** Before merging his own pull request, Jerry needs to update his local `main` branch to include the changes that were just merged from Tom's branch.

    ```bash
    git checkout main
    git pull origin main
    ```

    * `git checkout main`: Switches to the local `main` branch.
    * `git pull origin main`: Fetches the latest changes from the remote `main` branch and merges them into the local `main` branch.

9.  **Jerry Merges `add-contact-info` into Local `main` (Potential Conflict):** Now, Jerry switches back to his feature branch and merges the updated `main` branch into it. This is where a conflict might arise if both developers modified the same lines in `index.html`.

    ```bash
    git checkout add-contact-info
    git merge main
    ```

    * `git checkout add-contact-info`: Switches back to Jerry's feature branch.
    * `git merge main`: Integrates the changes from the `main` branch into the current `add-contact-info` branch.

    **Conflict Resolution (if necessary):**

    If Git detects conflicting changes (i.e., both Tom and Jerry modified the same lines in `index.html`), it will mark those sections in the `index.html` file with conflict markers:

    ```html
    <<<<<<< HEAD
    <nav>
      <ul>
        <li><a href="#">Home</a></li>
        <li><a href="#">Products</a></li>
      </ul>
    </nav>
    =======
    <nav>
      <ul>
        <li><a href="/">Home</a></li>
        <li><a href="/about">About</a></li>
        <li><a href="/services">Services</a></li>
        <li><a href="/contact">Contact</a></li>
      </ul>
    </nav>
    >>>>>>> main
    <footer>
      <p>Contact us at: info@example.com</p>
    </footer>
    ```

    Jerry needs to manually edit the `index.html` file to resolve the conflict. This involves deciding which changes to keep (Tom's, the original, or a combination of both) and removing the conflict markers (`<<<<<<< HEAD`, `=======`, `>>>>>>> main`).

    After resolving the conflict, Jerry needs to stage and commit the changes:

    ```bash
    git add index.html
    git commit -m "Resolve merge conflict: Update navigation and add contact info"
    ```

10. **Jerry Pushes Updated Branch and Creates Pull Request (if not already created):** If Jerry hadn't created a pull request before the merge, he would do so now. If he had, he would need to push the merged and potentially conflict-resolved changes to his remote branch.

    ```bash
    git push origin add-contact-info
    ```

11. **Review and Merge Jerry's Pull Request:** Jerry's pull request (`add-contact-info` to `main`) is reviewed. If there were conflicts, the reviewers can see how they were resolved. Once approved, the pull request is merged into the `main` branch on the remote repository.

12. **Tom Updates Local `main` Branch:** Finally, Tom should also update his local `main` branch to include all the changes.

    ```bash
    git checkout main
    git pull origin main
    ```

## Summary

This scenario demonstrates how Git allows multiple developers to work on the same file concurrently using branches. While Git often handles merges seamlessly, conflicts can arise when changes are made to the same lines. Understanding how to resolve these conflicts is a crucial skill for collaborative software development. The key commands used in this process are:

* `git clone`: Creates a local copy of a remote repository.
* `git checkout`: Switches between branches or creates a new branch.
* `git branch`: Lists, creates, or deletes branches.
* `git add`: Stages changes for a commit.
* `git commit`: Saves staged changes with a message.
* `git push`: Uploads local commits to a remote repository.
* `git pull`: Fetches changes from a remote repository and merges them into the current branch.
* `git merge`: Integrates changes from one branch into another.


## git commands 

<img width="610" alt="Screenshot 2025-04-30 at 4 47 17 PM" src="https://github.com/user-attachments/assets/1d78651f-5d40-427e-9542-64b86cdd4a61" />

<img width="599" alt="Screenshot 2025-04-30 at 4 48 35 PM" src="https://github.com/user-attachments/assets/7686602b-8e8c-4706-b813-29ca358627fb" />




