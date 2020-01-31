---
layout: post
title: "Github Actions with fastlane"
date: 2020-01-31 06:30:00 +0530
categories: Fastlane Script CI-CD GithubActions
---

1. Open Repository locally.
2. Add `.github` folder
3. Add `workflow` folder inside `.github` folder.
4. Add `swift.yml` file.
5. Add contents as follows in `swift.yml` file.


```yml
name: iOS Compile Check
on:
  pull_request:
    branches:
      - development
jobs:
  build:
    runs-on: macOS-latest
    steps:
    - uses: actions/checkout@v1
    - name: Build
      run: bundle install; pod install; bundle exec fastlane ios buildApp
```

#### About Above Action

- Above action does successfull compilation check.
- On pull request for given branche(here development), it executes above job.

#### About above Action's job

- It runs on macOS latest
- It first checks out branch (here development)
- It runs `bundle install` (which is necessary for fastlane execution)
- It adds all dependencies `pod install`
- `bundle exec fastlane ios buildApp` builds app & check compilation status.

- Know more about [`bundle exec fastlane ios buildApp`](/swift/fastlane/ci-cd/2020/01/31/Fastlane-build-ios-simulator.html)