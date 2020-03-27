# Setting up Git

We'll be using Git and GitHub in this class to save and share our code. It will be an **INTEGRAL** part of everything we do.

* You should already have Git installed on your computer.
* You already have a GitHub username!

* [Let's set up Git!](https://help.github.com/articles/set-up-git/)

* [Setting up your Username in Git](https://help.github.com/articles/setting-your-username-in-git/)

* [Setting your commit Email address in Git](https://help.github.com/articles/setting-your-commit-email-address-in-git/)

## Setting up SSH Keys

SSH keys are a way to identify trusted computers, without involving passwords. The steps below will walk you through generating an SSH key and then adding the public key to your GitHub account. Doing this will make the process of using Github more seamless and more secure.

__Note:__ The dollar sign (`$`) is commonly used to represent the command line. Anything to the right of it is what you enter (ex. `$ ls` means you enter `ls`). The tilde (`~` not Swinton) is used to represent the home directory. If your system username is Felix for example, `~` will translate to `/Users/Felix`.

_This guide was taken in huge part from [Github's SSH key guide](https://help.github.com/articles/generating-ssh-keys/). Feel free to refer to that guide directly, but be aware that there are differences._

### Step 1: Open your Terminal

The terminal is an application provides text-based access to the operating system.


### Step 2: Check for SSH keys

Since we will be using new Macbooks, there shouldn't be any pre-existing SSH keys. Let's continue and generate one!

### Step 3: Generate a new SSH key

To generate a new SSH key, copy and paste the text below, making sure to substitute in your email address. The default settings are preferred, so when you're prompted to "Enter a file in which to save the key", just press __Enter__ to continue.

```
$ ssh-keygen -t rsa -C "your_email@example.com"
# Creates a new ssh key, using the provided email as a label
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]
```

Next, you'll be asked to enter a passphrase.

```
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
```

A passphrase makes your SSH key more secure, but if you use one make sure you don't forget it. We recommend you let your Mac remember your passphrase when prompted. Feel free to not use any passphrase.

Which should give you something like this:

```
Your identification has been saved in /Users/you/.ssh/id_rsa.
Your public key has been saved in /Users/you/.ssh/id_rsa.pub.
The key fingerprint is:
01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db your_email@example.com
```

Then add your new key to the ssh-agent:

```
# start the ssh-agent in the background
$ eval "$(ssh-agent -s)"
Agent pid 59566
$ ssh-add ~/.ssh/id_rsa
```

## Step 4: Add your SSH key to your account

Run the following command to copy the key to your clipboard.

```
$ pbcopy < ~/.ssh/id_rsa.pub
# Copies the contents of the id_rsa.pub file to your clipboard
```

1. In the top right corner of any page, click the settings icon (looks like a gear).

2. In the user settings sidebar, click __SSH keys__.

3. Click Add SSH key.

4. In the Title field, add a descriptive label for the new key. For example, if you're using a personal Mac, you might call this key "Personal MacBook Air".

5. Paste your key into the "Key" field.

6. Click __Add key__.

7. Confirm the action by entering your GitHub password.

### Step 5: Test everything out

To make sure everything is working, you'll now try SSHing to GitHub. When you do this, you will be asked to authenticate this action using your password, which was the passphrase you created earlier.

Open up your Terminal and type:

```
$ ssh -T git@github.com
# Attempts to ssh to GitHub
```

You may see this warning:

```
The authenticity of host 'github.com (207.97.227.239)' can't be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)?
```

Don't worry! This is supposed to happen. Verify that the fingerprint in your terminal matches the one we've provided up above, and then type "yes."

```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

If that username is yours, you've successfully set up your SSH key! Don't worry about the "shell access" thing, you don't want that anyway.

If you receive a message about "access denied," you can [read these instructions for diagnosing the issue](https://help.github.com/articles/error-permission-denied-publickey).

### Congrats, you generated your SSH key!

========

Follow these instructions to install the basic software you will need to write programs in Java. You will use the _IntelliJ IDE_ and the _Java8 JDK_.


Installing IntelliJ
==
IntelliJ is an **IDE**, or Integrated Development Environment.  An IDE makes programming much easier by providing a visual tool for writing, managing, running, and debugging your code.

To install IntelliJ:

1. Go to the [IntelliJ download page](https://www.jetbrains.com/idea/download/).
2. Select the `Download Community` button on the right, to download the free version of the program.  You will download a file named `ideaIC-2016.2.1.dmg`.
3. Once it's finished downloading it, click on it. When the window opens, drag the IntelliJ icon to the Applications folder.
4. Drag the `IntelliJ IDEA CE` drive icon from your desktop to the trash can.

Test that you can open IntelliJ by opening Spotlight (press `⌘-Space`) and typing the first few  letters of IntelliJ.

Installing the JDK
==
The **JDK** is the Java Development Kit, which contains the tools you need to compile and run Java programs.  There are multiple versions of Java; in this course, we will use version 8, otherwise known as **Java8**.  Make sure you are using Java8, and not Java6 or Java7.

To install the Java7 JDK:

1. Go to the [Oracle Java8 JDK page](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
1. Accept the license agreement.
1. Download `jdk-8u101-macosx-x64.dmg`.
1. Double-click on the .dmg once it downloads. This will mount the .dmg file as a drive on your computer.
1. When the .dmg mounts, a new window will open. Double-click on the .pkg package file in the window that comes up.
1. In the installer, click `OK`, then `Install`.
1. It will prompt you for your login password.
1. When it's done, find the `jdk update 101` drive icon on your desktop and drop it on the trash.  This unmounts the .dmg file.

Installing the JDK in IntelliJ
--
You'll have to set up the JDK in IntelliJ before you can use it.  You only have to do this once.

1. In the lower right click on `Configure > Project Defaults > Project Structure`.
1. In the second column, click the plus button at the top and select `JDK`.
1. A file dialog box will open.  In the top-middle, dropdown box, select `Macintosh HD`.
1. Navigate to `Library > Java > JavaVirtualMachines`.
1. There should be an entry named `jdk1.8.0_77.jdk`.  Select (single-click) this and press the `Choose` button in the lower-right.
1. Click `OK`.

Creating an IntelliJ project
==
IntelliJ organizes your work into _projects_.  In IntelliJ, you'll always work on one project at a time.  To test that you have installed IntelliJ and the JDK correctly, we'll create a simple Java project, and you'll create your first Java program.

1. Click `Create New Project`.
1. Make Sure `Java` is selected in the left column.
1. In the `Project SDK` box, there should be a `1.8` entry with version 1.8.0_77.  (If this is not there, please ask for help.)
1. Click `Next`.
2. Check the `Create project from template`. Select `Command Line App` and hit `Next`.  (A "command line" app is one that you run from the terminal.)
3. Give your project the name "helloworld". For the base package, use `nyc.c4q.(username).helloworld` where (username) is your GitHub username.  

IntelliJ creates the project for you.  In fact, it writes most of the Java code for you!  You'll see a file named `Main.java` that contains the skeleton of your first program.  Find the line that says,

    // write your code here

and delete the entire line. Replace it with this line, typed exactly as it appears below (including punctuation).

    System.out.println("Hello, world!");

Press `⌘-S`
to save.

Now run it!  Select `Run > Run Main` from the menu.  IntelliJ compiles your program and then executes it.  A box appears at the bottom and your program prints out the line "Hello, world!".

Congratulations, you've written your first Java program!

Install Google Code Style
https://github.com/google/styleguide/blob/gh-pages/intellij-java-google-style.xml
