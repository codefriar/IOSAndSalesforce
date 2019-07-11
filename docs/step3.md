# Setting up your Project

## Prerequisites <a name="pre"></a>

In order to complete this challenge you'll need:

1. A working installation of Git.
2. A Mac with Xcode 10.2+ installed.
3. If you want to install the sample app on a physical iOS device, you'll need an Apple Developer Account.
4. A Salesforce developer edition org (Signup at [developer.salesforce.com](https://developer.salesforce.com/signup))

## Source Control Setup <a name="download"></a>

This project makes use of [git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules), in addition to Xcode build dependencies to incorporate the SDK. This means you must not only clone _this_ repository, but the submodule repositories as well. If you have not yet cloned this repository, this clone command will clone not only this repo, but the submodules as well.

```console
git clone --recurse-submodules https://github.com/trailheadapps/tdx19-swift-challenge.git
```

If you've already cloned this repository, please initialize the submodules using this command

```console
git submodule update --init --recursive
```
