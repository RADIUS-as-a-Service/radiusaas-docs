---
description: >-
  NOTE: We use RADIUS-as-a-Service as an example, but you can use this as a
  generic guide to make links available via myapps.
---

# Add Link to MyApps

## Create the app <a href="#create-the-app" id="create-the-app"></a>

First you have to create a Enterprise app. To do this via the Azure portal, follow this steps:

1. Login to your [https://portal.azure.com/](https://portal.azure.com/) account
2. Go to “Azure Active Directory”
3. Select “Enterprise applications”
4.  Click “+ New Application”

    <figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>click on "New application"</p></figcaption></figure>
5.  Click “+ Create your own application”

    <figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p>click on "Create your own application"</p></figcaption></figure>
6. Give a name for the app (e.g. RADIUSaaS Portal)
7.  Choose “Integrate any other application you don’t find in the gallery” and click “Create”

    <figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption><p>Choose "Integrate any other application you don't find in the gallery" and click "Create"</p></figcaption></figure>

After that the app is set up, we now need to add users to it and configure the logo and link

## Add users and a logo <a href="#add-users-and-a-logo" id="add-users-and-a-logo"></a>

1.  Under “Manage” go to “User and groups” - Add all users/groups who should be able to view/use your new URL tile and save

    <figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption><p>Click "User and groups"</p></figcaption></figure>
2.  Click “Properties” - Upload an image logo of your choice and save

    <figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption><p>Click "Properties"</p></figcaption></figure>
3.  Click “Single Sign-on” - Select “Linked” mode - Then enter the URL you want and save.

    <figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption><p>Click Single Sign-on - Select "Linked" mode</p></figcaption></figure>

    <figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption><p>enter the URL you want</p></figcaption></figure>

## Access myapps <a href="#access-myapps" id="access-myapps"></a>

Your users should now be able to access the newly created link tile via [myapps](https://myapps.microsoft.com/)
