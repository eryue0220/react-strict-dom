---
slug: /contribute/author-patch
---

# Author a patch

<p className="text-xl">These instructions cover the process for making contributions to the React Strict DOM repository. You will learn how to run tasks and tests locally, as well how to submit and update patches.</p>

:::info[Discuss significant changes first]

If you plan to work on a new feature or significant change, please [**open a discussion**](https://github.com/facebook/react-strict-dom/discussions) with a detailed proposal before starting on the work. We don't want you to waste your efforts on a pull request that may not be accepted.

:::

## Contributor License Agreement ("CLA")

In order to accept your pull request, we need you to [complete and submit a CLA](https://code.facebook.com/cla). You only need to do this once to work on any of Meta's open source projects.

By contributing to React Strict DOM, you agree that your contributions will be licensed under the [LICENSE](https://github.com/facebook/react-strict-dom/blob/main/LICENSE) file in the root directory of the repository.

## Create a new branch

Update the local `main` checkout with the latest changes from upstream. The `<remote>` should be `origin` or `upstream`, depending on whether you are working from a fork or the source repo.

```
git checkout main
git pull <remote> main
```

Install dependencies:

```
npm i
```

Create a new branch, usually from `main`:

```
git checkout -b <branch-name>
```

## Documentation

Documentation changes can be made to the package can be found in the `website` package within the `apps` directory. To run the documentation website locally:

```
npm run dev -w website
```

## Development tasks

Most contributions will typically be made to the `react-strict-dom` package within the `packages` directory. Each time a commit is made, the linter and code formatting tasks will automatically (auto-fixing where possible).

During development it's good practice to run and watch the unit tests in a separate terminal:

```
npm run jest -- --watch
```

Flow type checking can be run as follows:

```js
npm run flow
```

All tests can be run as follows:

```
npm test
```

If you need to update one of the patched `node_modules`, edit the file directly in `node_modules` and then run:

```
npx patch-package <package-name>
```

## Integration testing

Each patch can also be visually tested against the example web and native app built with Expo. This package can be found in the `examples` package within the `apps` directory.

First, build and automatically rebuild `react-strict-dom` on changes:

```
npm run dev -w react-strict-dom
```

In another process, start the examples app:

```
npm run dev -w examples
```

To load the app in a browser or local simulator, follow the Expo instructions in the terminal. You may need to install XCode and Android Studio to use simulators.

To open the React DevTools for native, press `Shift + M` in the terminal running Expo, and select "Open React devtools".

To test on device, install Expo Go on your device per [these instructions](https://reactnative.dev/docs/environment-setup?guide=quickstart#target-os-1). Scan the QR code with Expo Go to load the example app on your phone.

## Performance testing

If changes are being made to the native code path that may affect performance, it is worth running the performance benchmarks locally to get early signal on any improvements or regressions caused by the patch (CI always runs these tests). There are `perf` and `size` benchmarks available. This package can be found in the `benchmarks` package within the `packages` directory.

```
npm run perf -w benchmarks
npm run size -w benchmarks
```

To compare the results of a patch to the base branch, first run these tasks from `main` and then from the patch. Comparison data can be generated by running the following task with paths to the before-and-after files.

```
npm run compare -w benchmarks -- <path-to-base.json> <path-to-patch.json>
```

## Prepare a merge request

Branches should be cleaned up before review. An interactive rebase can be used to squash commits, reorder commits, and reword commits. For example, this will run an interactive rebase for the last 5 commits from `HEAD` on a branch.

```
git rebase -i HEAD^5
```

Once the branch has been prepared for review, it should be pushed to GitHub:

```
git push <remote> <branch>
```

Create a "Pull Request" against the React Strict DOM repository.

## Update a merge request

To update a branch with the latest changes to origin's `main`:

```
git checkout main
git pull <remote> main
git checkout <branch-name>
git rebase main
```

This will rebase the branch onto `main` and prompt to resolve any potential conflicts. Re-install dependencies after each update.

If changes are made to a branch after it has been put up for review, the branch can be rebased once again (e.g., to update commits with fixes), and then needs to be forced-pushed to GitHub.

```
git push -f <remote> <branch>
```

## Code review

A bot may comment on your merge request with suggestions. Generally we ask you to resolve these first before a maintainer will review your code. It's also a good idea to review your own code first, and provide contextual comments for reviewers where appropriate.

If changes are requested and addressed, please [request review](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/requesting-a-pull-request-review) to notify reviewers to take another look. Once a "code owner" has approved the merge request, it can be merged.