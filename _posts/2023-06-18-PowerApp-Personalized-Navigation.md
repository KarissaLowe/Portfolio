---
layout: post
title: Power Apps - User-Specific Navigation Menus
---

### Introduction
When it comes to building dynamic and user-friendly applications, delivering a personalized experience can make all the difference. In many of the Power Apps I have built for my company, I have needed to have pages within the app that are only accessible by certain groups in my organization. 

For example, on the Staff Name Pronunciation application I wrote about here, only members of my organization's HR team have access to a page within the app where they can delete users’ profiles. This allows them to remove users just in case someone were to add something inappropriate. Another example is a page only viewable by the accounting team on an Expense Reimbursement application I built. This page allows the accounting team to review and download all submitted expenses. 

Luckily, with PowerApps and Sharepoint lists you can easily create navigation menus that display different options to different individuals within your organization.
<br>

---

### Example Navigation Menu

In the Staff Name Pronunciation app I mentioned above, our HR team has the ability to see the “Settings” page button in the navigation menu like so:

![Navigation Menu with Button Example](https://github.com/KarissaLowe/TheDataMouse/blob/master/images/NavigationMenu_1.PNG?raw=true)


But other users do not have this “Settings” navigation option visible to them:

![Navigation Menu without Button Example](https://github.com/KarissaLowe/TheDataMouse/blob/master/images/NavigationMenu_2.PNG?raw=true)
<br>

---

### How to Add

First create a sharepoint list called “Settings_Button_Access.” 
Add a Person column to this list. For each user that needs access to the button and page, create a row and add the user into the Person column. In my example above, I would create a row for each person in my company’s HR team. Add a [connection to this sharepoint in your app.](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/connections/connection-sharepoint-online)

In my App, my navigation menu is a simple square shape and multiple transparent-background buttons that link to each of their respective pages. I also add icons to each button, but this is optional.

Now, you only want the “Settings” button to be visible to the user if they are included on the ‘Settings_Button_Access’ sharepoint list. Go to the Visible property on the button that links to the Settings page and adding this If statement using the CountRows function:

```
If(CountRows(Filter(Settings_Button_Access,Person.Email=User().Email))>0, true, false)
```

With this added, only users included on the sharepoint list will be able to see the button at all. Now, in my case, I used a square as the background of my navigation menu. If there is one less button visible for some users, the square will have a lot of empty space. You can solve this with a very similar addition to the Height property of your square. If the user is on the list, make the square a few pixels longer, if not keep it shorter. You might have to play around with different heights to see what looks best.  

```
If(CountRows(Filter(Settings_Button_Access,Person.Email=User().Email))>0, 351, 290)
```



