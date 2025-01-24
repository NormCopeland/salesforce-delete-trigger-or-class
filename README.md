# An Easier Way to Delete Apex Classes From Production

> **Attribution**: This README is from a SalesforceBen article: [An Easier Way to Delete Apex Classes From Production | Salesforce Ben](https://www.salesforceben.com/way-to-delete-apex-classes-from-production/)
> I wanted to keep a copy of the xml files and instructions here so that I don't have to hunt down that article.

When attempting to delete Apex classes & triggers from production, you can be faced with a number of issues. This is due to the fact you cannot modify Apex code directly in production.

Within this article, I will go through a lightweight, and flexible solution, that enables you to delete Apex classes and triggers from production, quickly, and easily.

## The Problem

The most common approach to deleting Apex classes and triggers in a Salesforce production environment is to leverage either the [Force.com IDE](https://developer.salesforce.com/page/Force.com_Migration_Tool) or the [Force.com Migration tool](https://developer.salesforce.com/page/Force.com_Migration_Tool). These tools have a number of downsides, namely:

- The Force.com IDE is very 'heavyweight' and is known for being quite buggy sometimes and [unpleasant](http://sebastien-arbogast.com/2009/07/18/why-do-i-hate-eclipse/) to use
- They have a number of [dependencies](http://salesforce.stackexchange.com/questions/108101/force-ide-installation-problem) (a compatible version of Java, different IDE version etc)
- Connectivity to Salesforce via the Force.com IDE may be an [issue](http://salesforce.stackexchange.com/questions/89193/force-com-ide-eclipse-connection-issues)
- The Force.com migration tool (ANT) can take some time to learn how to use properly

## The Solution

Let's say that you have a Salesforce production org that has two Apex classes that need to be deleted.

1. Create a folder on your desktop. I will call my folder 'deleteClasses'

2. Go to Notepad (or another text editor) and copy and paste the below and save as the file with 'package.xml' and 'All files (*.*)'

3. Then create a new file in Notepad (or another text editor) and copy the below into it:

```xml
<?xml version="1.0" encoding="utf-8"?>
     <types>
          <members>SomeClass</members>
          <name>ApexClass</name>
     </types>
<version>30.0</version>
</Package>
```

Replace **SomeClass** with the name of your class that you wish to delete. If you have two classes that need to be deleted at the same time, you can simply add another <members> row into the file with the name of the other class, for example:

```xml
<?xml version="1.0" encoding="utf-8"?>
     <types>
          <members>SomeClass</members>
          <members>SomeOtherClass</members>
          <name>ApexClass</name>
     </types>
<version>30.0</version>
</Package>
```

4. Save this file name as destructiveChanges.xml (note the capital 'C' in 'changes'). Make sure the file is saved as 'All files (*.*)'

5. Now there are two files in your folder. Open the folder, select both the XML files, right-click and select 'Send To > Compressed Folder'. Keeping the default name of 'package' for the folder is fine

6. Go to [Workbench](https://workbench.developerforce.com/login.php?startUrl=%2Fquery.php) and login with your credentials. It is recommended that you login to a Sandbox instance before you deploy the file to production

7. Select Migration > Deploy

8. Click 'Browse' and select the .zip package file. Then check 'Rollback on Error', 'Single Package', and select Test Level with 'RunLocalTests'

9. Finally, select 'Next' and then you will notice that the success or error results will be displayed in Workbench once the deployment process has been completed

## Summary

Deleting Apex classes and triggers can be tricky, but it certainly doesn't have to be! There are tools at your disposal to help you out, and once you get the hang of it, you'll be smooth sailing.