Fork from [nst/objc_strings](https://github.com/nst/objc_strings).

I made some changes to detect more warning cases.

---

#### Goal

Helps Cocoa applications localization by detecting unused and missing keys in '.strings' files.

#### Input

Path of an Objective-C project.

#### Output

1. warnings for untranslated strings in *.m
2. warnings for unused keys in Localizable.strings
3. warnings for lost keys in some Localizable.strings
4. errors for keys defined twice or more in the same .strings file

#### Typical usage

    $ python auto_check_i18n.py -p /path/to/project
    ./MyProject/en.lproj/Localizable.strings:13: warning: unused key in en.lproj: "Misc"
    ./MyProject/ViewController.m:16: warning: missing key in fr.lproj: "World"

#### Xcode integration

1. make `auto_check_i18n.py` executable

    $ chmod +x auto_check_i18n.py

2. copy `auto_check_i18n.py` to the root of your project
3. add a "Run Script" build phase to your target
4. move this build phase in second position
5. set the script path to `"${SOURCE_ROOT}/auto_check_i18n.py"`

![settings](https://github.com/nst/objc_strings/raw/master/images/settings.png "settings")
![warnings](https://github.com/nst/objc_strings/raw/master/images/warnings.png "warnings")

#### Common Issues

Some may experience *UnicodeDecodeError* when running the script.
The problem is that the script runs through all directories to look for .strings files, which may include already compile .strings files which can not be parsed. Often you have some in Build/ or if you integrate CocoaPods ( Pods/ )

To prevent this you can add dirs which you want to have excluded like this
```
"${SRCROOT}/auto_check_i18n.py" --exclude-dirs 'Build','Pods'
```
or if you are on terminal
```
$ auto_check_i18n.py --project-path /path/to/project --exclude-dirs 'Build','Pods'
```

#### ToDo

* Scan Interface Builder (.xib) Files for localized Strings
