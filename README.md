# FT Build Info
Dynamically transport information through a clone into the migrated solution.



<img src="./docs/assets/screenshot.png" alt="screenshot" style="zoom:80%;" />



## About This Project

When you deploy a new version of your solution, you cannot use normal fields to store information such as version numbers, build numbers and timestamps or even normal record data. These will be overwritten during data migration. Normally, a calculation field would do the trick; however, manually setting their values as part of your deployment pipeline or routine is not suitable for modern CI/CD scenarios.



## How Does It Work?

We use a text field in a global table to store json in the next serial value. On inserting a new record, the 'default' field will be set using the 'next value'. In version 2 this works natively without any plugins, and you can store as much data as you'll need, we have tested this with up to 1MB of random text, the example file contains 100k of Lorem Ipsum sample text.

After data migration, all fields retain the 'old' values from your previous production file. Upon opening the file, we perform a quick check of the field’s ModCount by querying the internal FileMaker_Fields table. As the ModCount changes every time you set new build information, any new version will be detected by the script, and the information will be extracted by creating a temporary new record. This process occurs only once for each release.



## Requirements

You need a plugin to perform the ALTER statement, but only on the build side. The demo uses the MBS Plugin because of its support for execution on-idle, but any plugin with SQL functions will do. 



## Usage Examples

Besides providing version information there are plenty of situations, where storing data in a clone could come in handy:

- Patches: adding new or modifying existing records after data migration. 
- Libraries and Value Lists: like the country codes in the example file.



## Feedback and Contributions

You are welcome to comment, send suggestions, or submit pull requests. Please also share how you are using this technique in your solutions.

Big thanks to **Christian Schmitz** for the initial solution that utilized his Swiss Army knife (aka the [MBS Plugin](https://www.monkeybreadsoftware.com/filemaker/)) to store data in the default value of the field. Special thanks also to **[Russell Watson](https://www.fmworkmate.com/)** for his improvements in utilizing the next value. 

