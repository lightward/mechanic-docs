# Contributing to the Task Library

## What is the Mechanic task library?

Our open-source task library contains 280+ ecommerce automation tasks that are ready to use out of the box or serve as excellent starting points for custom tasks. The task library consists of the task code as well as the task documentation. It's important to note that the documentation is part of the task library because building the docs will be part of the steps covered in this section.

{% hint style="info" %}
Check out the Task Library on GitHub: [github.com/lightward/mechanic-tasks](https://github.com/lightward/mechanic-tasks)
{% endhint %}

## Contributing your work to the task library

You've created a custom task, and you want to share it with the world! This brings us so much joy, and it epitomizes what the Mechanic project and this community are about. If you get stuck along the way, please hop onto the [Slack workspace](https://join.slack.com/t/usemechanic/shared_invite/zt-cq84nrs7-ggYbYTbf~CrCjTg8nmHP2A), and we'll be glad to help.

{% hint style="info" %}
The task library is hosted in a Git repository on GitHub. You'll make your contribution via a [Pull Request](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests).
{% endhint %}

We follow the same process any open-source project does when it comes to code management and code contributions. One bonus of contributing to the Mechanic task library is that once you learn the process here, you'll know how to contribute to open-source projects going forward.

### The process

* You'll [fork](https://docs.github.com/en/github/getting-started-with-github/fork-a-repo) the task library [repository](https://github.com/lightward/mechanic-tasks).
  * Forking is taking a copy of our repository, so that you can make changes to your copy.
* Make your changes in your forked repository.
* Make a [pull request](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests), which will trigger a review of your proposed changes and the merging of them into the main repository, making your task available to everyone using the app.

### Step-by-step instructions

1. You'll need a GitHub account, you can signup for one [here](https://github.com/join). 
2. Visit the [task library repository](https://github.com/lightward/mechanic-tasks) and fork it as shown below. You'll make your changes to this copy of the repository.

![](../.gitbook/assets/fork.gif)

2. The task library is made up of the [tasks](https://github.com/lightward/mechanic-tasks/tree/master/tasks) and the supporting documentation.  In these next few steps, you'll ensure you can build the [docs](https://github.com/lightward/mechanic-tasks/tree/master/docs), so that you can complete this step when you are ready to submit your contribution.

   1. Building the docs requires npm, node &gt;=12.10.0, and yarn. 
      1. Install npm and node here: [https://www.npmjs.com/get-npm](https://www.npmjs.com/get-npm)
      2. Install yarn here: [https://yarnpkg.com/getting-started/install](https://yarnpkg.com/getting-started/install)
   2. While in the project directory, run the following commands to build the docs:

   ```text
   yarn          # install dependencies
   npm run build # compile docs
   npm run test  # apply sanity checks
   ```

3. Now that you can build the docs you're are ready to [contribute](https://github.com/lightward/mechanic-tasks/blob/master/CONTRIBUTING.md)! 
4. Your task documentation, options, subscriptions, code, are done in Mechanic. If you choose to use an external editor that's great, you still need to transfer it into Mechanic, so that you can export the task in the JSON format you need for the library. Importing/Exporting tasks from Mechanic is covered [here](../core-concepts/tasks/importing-exporting-tasks.md).
5. If you're changing an existing task you export the JSON, and replace the contents of the `task/task_file_name.json` and then run the commands `npm run build` and `npm run test.`
6. If you are contributing a new task, you'll [export](../core-concepts/tasks/importing-exporting-tasks.md) the JSON from Mechanic, and save the JSON file in the `tasks/directory` of your forked repoistory, named with an appropriate handle for the task. \(For example, a task named "Hide out-of-stock products" should have its JSON export stored in `"tasks/hide-out-of-stock-products.json"`.\) And, then you'll execute the commands: 

   `npm run build` and `npm run test`.

7. If all goes well with the build, you'll see your task listed in the automatically created documentation in `docs/README.md`
8. You're now ready to make your pull request! Head over to [https://github.com/lightward/mechanic-tasks/pulls](https://github.com/lightward/mechanic-tasks/pulls)  and click New pull request, you should see the changes you committed to your fork, and you'll proceed with filling out the pull request form.
9. This process could sound confusing if you haven't done it before, but once you've done it once, it's simple and it is also pretty exciting to go through the process.  The other bonus is, you'll be ready to submit a pull request to any open-source software project in the future. If you need help please out to us in the [Slack workspace](https://join.slack.com/t/usemechanic/shared_invite/zt-cq84nrs7-ggYbYTbf~CrCjTg8nmHP2A).

