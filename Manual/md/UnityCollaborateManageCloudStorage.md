# Managing cloud storage

The Collaborate service uses cloud storage to backup your entire Project in the cloud and make it accessible anywhere.

本主题介绍：

* [Determining how much storage your Projects are using](#howmuchstorage)
* [Adding more storage](#addingstorage)
* [Cloud storage data retention](#retention)
* [Archiving Projects](#archivingprojects)
* [Archiving Projects for Collaborate](#archiveforcollab)

<a name="howmuchstorage"></a>
## Determining how much storage your Projects are using

Cloud storage is calculated on a per Organization basis. Unity calculates the amount of cloud storage that your Organization is using by summing the size of the latest versions of all your Organization’s Collaborate Projects. Only the latest version of your project counts towards your storage limit.  Historical (non-head) versions of your Projects do not count towards cloud storage.

To view your current storage usage:

1. Sign into the [Unity Developer Dashboard](https://developer.cloud.unity3d.com/).
2. Select a Project in the Organization whose storage you want to view.
3. In the left navigation window, click __Settings__, then click __Usage__.

In the main Project window there are tiles that show:

* __Cloud storage__ - How much storage the current Project is using compared to the rest of the Projects in your Organization,  and your entire cloud storage space.
* __Percent of storage per project__ - How much storage is used by each Project in your Organization.
* __Storage used by &lt;Project name&gt; over time__ - How much storage the current Project has used since it was created, until the current time.

<a name="addingstorage"></a>
## Adding more storage

You can obtain more cloud storage in two ways:  

* Upgrade to Unity Teams Advanced, which also provides 25GB of cloud storage.
* Purchase Storage Packs on the Unity ID dashboard. Each storage Pack adds 25GB of cloud storage to an Organization.  

__Upgrading to Unity Teams Advanced__

To upgrade to Unity Teams Advanced, visit the Unity Teams page at [unity3d.com/teams](https://unity3d.com/teams). On the Unity Teams page you can learn more about the features of Unity Teams Advanced and then upgrade.   

__Purchase storage packs__

To add storage packs:

1. Sign in to the [Unity ID](https://id.unity.com/) dashboard. 
2.在左侧导航栏中，单击 __Organizations__。
3. In the main __Organizations__ window, select an Organization.
4. In the left navigation bar, click __Service Usage__.
5. In the __Usage overview__ window, click the __Manage storage__ button.
6. On the __Add cloud storage__ dialog, select the amount of storage that you need, and then click the __Continue to purchase__ button. If you have only one Organization,  proceed to step 6.
7. In the __Checkout__ window, enter your payment information and then click __Pay now__.

__Modifying your contracted amount of storage__

1. Sign in to the [Unity ID](https://id.unity.com/) dashboard. 
2. In the left navigation window, click __Organizations__.
3. In the main __Organizations__ window, select an Organization.
4. In the left navigation bar, click __Service Usage__. The Service Usage window displays your contract period and the number of storage packs that you are contracted for.
5. In the __Usage overview__ window, click the __Manage storage__ button.
6. On the __Add cloud storage__ tab, enter the number of storage packs to add.
7. Click the __Next__ button.
8. In the __Review order__ window, review the order information and then click the __Confirm purchase__ button.

You can reduce the number storage packs on the __Manage subscription__ tab:

* To remove all contracted storage packs, turn off __Automatic Renewal__. When automatic renewal is turned off, all storage packs are removed on the renewal date.
* To reduce the number of storage packs that are renewed, enter the number of storage packs to renew in the __Select the number of storage packs you want to renew upon renewal__ field. 

Then click the __Save__ button.

<a name="retention"></a>
## Cloud storage data retention

If you're using Unity Teams Advanced, Unity does not delete any version history for your Projects.

If you're using Unity Teams Basic, Unity stores your data in cloud storage for at least 1 year. Unity reserves the right to delete Project history older than 1 year. You always have access to the latest version of your Project, but you only have access to the last 90 days of your version history. If you're using Unity Teams Basic and want full access to the last 1 year of your history, you must upgrade to Unity Teams Advanced. When you upgrade to Unity Teams Advanced, you have access to your full history going forward.

## Reducing your cloud storage usage

You have three options to reduce your cloud storage usage:

* Delete or reduce the size of Assets in your Project. It is important to remember that Project history does not count toward your cloud storage usage.
* Archive unused Projects. See [Archiving Projects](#archivingprojects).
* Archive Projects for Collaborate only. Archiving a Project for Collaborate only allows you to free cloud storage space, while still being able to use other Unity services such as Ads, Analytics, and Performance Reporting.  See [Archiving Projects for Collaborate](#archiveforcollab).

<a name="archivingprojects"></a>
## Archiving Projects

Archiving a Project automatically archives any Collaborate cloud storage associated with the Project and Unity services are no longer accessible.

To archive a Project:

1. Sign into the [Unity Developer Dashboard](https://developer.cloud.unity3d.com/).
2.选择要存档的项目。
3. In the left navigation bar, click __Settings__, then click __General__.
4. At the bottom of the main window, read the important note and, if you are certain you want to archive the Project, click the __Archive my Project for ALL Unity services__ button. 
5. In the __Are you sure you want to archive this Project__ dialog, enter the name of the Project and then click the __Yes, archive it__ button.

To unarchive a Project:

1. Sign into the [Unity Developer Dashboard](https://developer.cloud.unity3d.com/).
2. At the top right of the main window, under the __CREATE NEW PROJECT__ button, click the __View Archived Projects__ link.
3. Locate the Project to reactivate, click the vertical ellipsis to the right of the Project name  and then select __Unarchive Project__.

<a name="archiveforcollab"></a>
## Archiving Projects for Collaborate

To archive a Project for Collaborate only:

1. Sign into the [Unity Developer Dashboard](https://developer.cloud.unity3d.com/).
2.选择要存档的项目。
3. In the left navigation window, click __Collaborate__, then click __Storage__.
4. In the main window, click the __ARCHIVE__ button.

To unarchive a Project for Collaborate:

1. Sign into the [Unity Developer Dashboard](https://developer.cloud.unity3d.com/).
2. Select the Project to unarchive.
3. In the left navigation window, click __Collaborate__, then click __Storage__.
4. In the main window, click the __UNARCHIVE__ button.


----------------------
<span class="page-edit">2018-07-16Page published with [editorial review](DocumentationEditorialReview.html)
</span>
