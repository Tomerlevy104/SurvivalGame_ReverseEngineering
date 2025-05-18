# 🎮 Survive Game - APK Reverse Engineering Project

This project involves reverse engineering a decompiled Android game application. The goal was to analyze the code structure, resolve various errors, and successfully run the game according to its intended logic.

## 📱 Project Overview

Starting with an APK file, I performed a complete reverse engineering process to understand and fix a survival game application that tests user coordination through directional button presses.

## 🔄 Reverse Engineering Process

I downloaded the APK file and uploaded it to http://www.javadecompilers.com/apktool to extract the project files. After obtaining the decompiled code, I began a systematic analysis and repair process.

The first step involved examining the AndroidManifest to identify the necessary activities for the game functionality. After this analysis, I added the required activities: ActivityMenu and ActivityGame, along with their corresponding layout files.

Once I integrated these components into my project, several errors became apparent that needed immediate attention. The analysis revealed missing drawable resources including various directional arrow icons (@drawable/ic_left, @drawable/ic_right, @drawable/ic_up, @drawable/ic_down).

## ⚠️ Error Resolution

### Improper Toast Usage
While reviewing the Activity_Game.java file, I encountered an error related to Toast implementation. The code contained calls to Toast.makeText() with hardcoded numeric values (1) as the duration parameter. Although 1 equals Toast.LENGTH_LONG, the Android API requires using proper constants (Toast.LENGTH_SHORT or Toast.LENGTH_LONG) rather than raw numeric values.

**Before fix:**
```java
Toast.makeText(this, "Survived in " + state, 1).show();
Toast.makeText(this, "You Failed ", 1).show();
```

**After fix:**
```java
Toast.makeText(this, "Survived in " + state, Toast.LENGTH_SHORT).show();
Toast.makeText(this, "You Failed ", Toast.LENGTH_SHORT).show();
```

### Missing String Resource
During the integration process, I discovered that activity_Menu was missing a crucial string resource. The application required a URL string that wasn't defined in the strings.xml file. I added the following entry:
```xml
<string name="url">https://pastebin.com/raw/T67TVJG9</string>
```

### Hidden Character Issue
When copying the URL from the original file, invisible characters were inadvertently included in the string. These zero-width characters are undetectable to the naked eye but cause server connection failures. I resolved this by manually deleting these hidden characters and retyping the URL.

### Network URL Content
The URL contains an array of US states that the game uses for its logic. This external resource is essential for the game's state selection mechanism.

### Internet Permission Error
After reviewing the code logic, I attempted to run the application but encountered a network permission error in the logcat. The AndroidManifest.xml was missing the internet permission declaration. I resolved this by adding:
```xml
<uses-permission android:name="android.permission.INTERNET" />
```

## 🎯 Game Logic Analysis

After successfully running the application, I analyzed the code to understand the game mechanics. The core logic involves performing a modulo 4 operation on each digit of the user's ID number.

Through careful examination, I determined the directional arrow mappings and their corresponding numerical values:
- Left = 0
- Right = 1  
- Up = 2
- Down = 3

## 🏆 Game Mechanics

The game consists of two main activities:

**Menu Activity**: Users input a 9-digit ID number. The application makes a server call to retrieve a comma-separated string of US states.

**Game Activity**: Features four directional buttons (left, right, up, down). Each digit in the user's ID undergoes a modulo 4 calculation to determine the required button sequence. Success leads to a "Survived in [state]" message displaying the selected destination.

## 🎊 Successful Completion

After understanding the complete code structure and resolving all identified errors, I successfully solved the challenge and reached **Texas** as my destination state. The application now runs smoothly with all functionality restored.