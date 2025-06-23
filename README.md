# screen-time-bruteforce

Even though I refer to this as a vulnerability, this is not a vulnerability. This is a bug that can be exploited to bypasss screen time by resetting the iPhone.

The symbol "[-]" represents data that has been redacted for this repo.

---

# My report:
*Reported on 8/11/24, 5:23 PM*

<div class="entry-content"><h2 class="typography-eyebrow-elevated">What is required to reproduce the issue?</h2>
  
- Device: An iPhone with "Screen Time" controls enabled by a parent.   
- External Tool: A Flipper Zero, BadUSB/Rubber Ducky, or any similar device capable of emulating keyboard imput via Bluetooth.

<h2 class="typography-eyebrow-elevated">Summary</h2>Hello,

My name is [-], and I am a [-]-year-old student. About a year ago, I discovered a method to bypass several "Screen Time" features, including Downtime, App Limits, Communication Limits, Communication Safety settings, and Content &amp; Privacy Restrictions.

<strong>Disclaimer:</strong> My findings were tested on an iPhone 7 running iOS 15.8. I have not personally tested this on models with newer iOS versions. However, I shared the reproduction steps with a friend who confirmed that the method worked on his iPhone 13 running iOS 18 Beta 2. Based on this, I can reasonably conclude that the issue also affects devices running iOS 17.x.x, which is the most recent publicly available iOS version at the time of this report.

<strong>How the Exploit Works:</strong>
As of this report, there are only three methods to bypass iOS Screen Time restrictions on an iPhone controlled by a parent:

1. <strong>Use the Screen Time Passcode</strong>: This requires the passcode, which should be inaccessible to the child.
2. <strong>Recover the Screen Time Passcode</strong>: This can be done using the parent's Apple ID, which the child also should not have access to.
3. <strong>Log Out of the Apple ID</strong>: This method, which we intend to exploit, allows for the bypass of Screen Time restrictions by using a new Apple ID.

In theory, it should not be possible to log out of the Apple ID on a child's iPhone that is "Screen Time controlled" by a parent without the Screen Time passcode. However, I will demonstrate below how I was able to log out of the Apple ID without needing the Screen Time passcode.<h2 class="typography-eyebrow-elevated">Steps to reproduce</h2>{the (e) after a number means "explanation"}

1. <strong>Access the Device:</strong> On the child's iPhone, navigate to:

<span class="code-inline">Settings &gt; General &gt; Transfer or Reset iPhone &gt; Erase All Content and Settings &gt; Continue</span>

Enter the iPhone's passcode when prompted.


1e. <strong>Screen Time Passcode Prompt:</strong>

At this stage, you should see a page asking for the Screen Time Passcode.
If you can bypass this, you can reset the iPhone and sign out of the Apple ID without any restrictions.
<strong>Issue:</strong> This page lacks an anti-bruteforce mechanism, which can be exploited to bypass the Screen Time passcode. (See the attached video #1 for a demonstration.)


2. <strong>Create a Bruteforce Script:</strong>

Write a Python script that generates every possible 4-digit passcode combination (from 0000 to 9999) for a Rubber Ducky or BadUSB device. Example code:
```python
for i in range(10000):
    combination = f"{i:04}"
    print(f"STRING {combination}")
    print("DELAY 100")
```
Save the output to a .txt file.

2e. **Generated script explanation:**

The generated output will look like this:
```
STRING 0000
DELAY 100
STRING 0001
DELAY 100
STRING 0002
etc...
```
The STRING command simulates a keyboard input of the passcode, while the DELAY command ensures the iPhone has enough time to process each input.


3. <strong>Deploy the script:</strong>

Import the .txt file into a Flipper Zero/BadUSB device. Execute the script on the iPhone. The device will attempt multiple passcodes, cycling through combinations from 0000 to 9999.<h2 class="typography-eyebrow-elevated">Expected and actual results</h2>Once the correct passcode is identified, the iPhone can be reset, and a new Apple ID can be created, effectively bypassing the Screen Time feature.<h2 class="typography-eyebrow-elevated">How you could fix it</h2>To fix this vulnerability, implement an anti-bruteforce system on the Screen Time Passcode page, similar to what is used for the iPhone's lock screen passcode.

[-]

Have a great day!</div>

---

# Apple's answer:

Thanks again for reporting this to us. Features like Screen Time are designed to provide parents with the tools to understand and manage their children’s device usage. Screen Time is not intended to protect a device against manipulation. If you’ve found a vulnerability in the storage or transmission of Screen Time usage data, please let us know so we can pursue this as a security issue. Otherwise, we recommend reporting this via https://feedbackassistant.apple.com.

In addition, please remember physical security remains an important part of protecting the data on your iOS and iPadOS device.

[report closed]
[I couldn't verify if that bug has been fixed]

---

# My conclusion:
Ethically and practically: I'm right since kids should not be able to brute-force their way out of Screen Time, and Apple should fix it.

By Apple's "rules": They're right to classify it as "expected behavior" under their security model.
