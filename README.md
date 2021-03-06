# Humble Bundle Key Redeemer

Automatically redeems Humble Bundle keys visible in your browser's active tab using the Steam client installed on your computer.

Inspired by [steamkeyactivator](https://github.com/google/steamkeysactivator).

## What problems does this solve?

Firstly, no-one likes to manually copy and paste keys over and over from their web browser, particularly given how many clicks it takes to activate just one key in Steam!

However, what sets this project apart from similar projects is that we do our best to avoid prematurely hitting Steam's key redemption rate-limit.

## What is Steam's key redemption rate limit?

Basically Steam only allows a certain number of key redemption _attempts_ per hour, this is designed to stop people brute-force guessing Steam keys.

Steam only allows you to attempt to redeem a certain number of keys each hour. However, Steam locks you out much quicker if your key redemption attempts fail. That doesn't seem unreasonable, however Steam considers two common scenarios as failures:

 1. Accidentally attempting to redeem a key you've already redeemed.
 2. Redeeming a key that has never before been redeemed, _but_ is for a game you already own.

The former is annoying, but the latter is a real issue, particularly with Humble Bundles, as it's fairly common for the one game to appear in multiple bundles.

## What does this key redeemer do that's special?

Aside from being a bit more clever than most (displaying things like game names), this redeemer does several things to try mitigate the two issues describes above. It does this two ways.

Firstly, by keeping a record of all the keys that have previously been redeemed, or failed to be redeemed.

Secondly, by looking at your Steam accounts list of activations, and cross-referencing the names of the games (and DLC etc.) on the Humble Bundle website against them. Unfortunately, Humble Bundle doesn't use the exact same titles as Steam, so we try to be clever about it and look for _similarly_ named items that you already own, and skip activating those keys.

## How does it work?

We presently require that you use OS X, and Safari as your browser.

You navigate to a Humble Bundle page with some keys visible that you'd like activated. Then you run the Humble Bundle Key Activator. It'll automatically detect your browser and find the Humble Bundle keys visible in your active tab. Then as described above, we automatically cross-reference these keys/titles against titles you already own - you'll be prompted to login to Steam via your browser if you aren't already.

We then use your local Steam client to activate these keys in a similar fashion to how you would do so manually. You'll literally see the Steam user interface automatically being interacted with.

__Don't touch your keyboard or mouse while this is happening.__

When all the keys have been activated you'll be notified (it's _much_ quicker than doing it by hand) and a record will be kept with regards to which keys were redeemed, which ones were unredeemed (possibly suitable for gifting to friends) and which keys failed to be redeemed (ideally none).

## How to run the Humble Bundle Key Redeemer

At the moment it's just a simple Ruby script. So you'll need to install some dependencies.

### Installing Ruby

OS X does come with a Ruby interpreter, but it's the "system Ruby" and you can't easily install dependencies (Gems).

There's a few solutions, ideally you'll use a Ruby version manager. However, if you don't know what that is (or care), then the simplest solutions is just to install Ruby from [Homebrew](https://brew.sh/):

```
brew install ruby
```

### Installing the script dependencies

```
gem install bundler
bundle install
```

### Allow us to read your browser contents

We need to be able to read the contents of your browser window.

First, if you haven't already, enable the Develop menu in Safari:

1. In the menu bar, select "Safari" -> "Preferences"
2. Click on the “Advanced” tab
3. Enable the checkbox “Show Develop menu in menu bar”
4. Close the Preferences window

Now, enable JavaScript access AppleScript:

1. From the menu bar, select "Develop" -> "Enable JavaScript from Apple Events"

*__Note:__ You will be prompted when an application tries to execute JavaScript in your browser. However, if you're particularly security conscious, you can turn this feature off whenever you're not using the Humble Bundle Key Redeemer.*

### Allow our script to interact with other applications

The terminal (where this Ruby script will be run) needs to be able to use AppleScript to interact with other applications on your computer (Steam and your browser). This is achieved using the accessibility APIs built into macOS.

To give terminal access to the accessibility APIs please do the following:

1. Launch macOS "System Preferences"
2. Select "Security and Privacy"
3. Select the "Privacy" tab.
4. Click the lock icon bottom left to make changes (you'll be prompted for your password).
5. Select "Accessibility" in the left pane.
6. In the right pane, enable the checkbox next to "Terminal.app"

*__Note:__ Feel free to disable this when you're not using Humble Bundle Key Redeemer.*

### Run the script

From terminal, navigate to Humble Bundle Key Redeemer's directory, then execute:

```
./humble-bundle-key-redeemer.rb
```

You'll receive prompts along the way guiding you through the process.
