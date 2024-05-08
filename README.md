# ft-buildinfo
Dynamically transport information through a clone into the migrated solution.



<img src="./docs/assets/screenshot.png" alt="screenshot" style="zoom:50%;" />



## About this project



When you deploy a new version of your solution, you cannot use normal fields to carry information like version number or build number and timestamps. These will be overwritten in the data migration. Calculation field would normally so the trick, but setting their values manually as part of your deployment pipeline or routine is not suitable for modern CI/CD scenarios. 

## How does is work?

We use SQL to alter the default value of a field in a global table. As the built-in ExecuteSQL() function only supports SELECT statements, we need a plugin for that. You can store up to 255 characters as the field's default value.

After the the data migration, all the fields contain the 'old' values from your former production file. When opening the file, we do quick check of the field's ModCount by querying the internal FileMaker_Fields table. As the modcount changes everytime you set new build information, a new version will be detected in the script and the information will be extracted by creating a new record. The json will be processed and the record will be deleted afterwords – this happens only once for every release. 
