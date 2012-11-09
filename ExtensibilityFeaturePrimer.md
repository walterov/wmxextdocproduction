#	Extensibility Feature Primer

WebMatrix 2 Beta Extensions Gallery offers extensions that provide users with an constantly improving development experience. For Extensions developers, it provides the extensibility points that enable customization of how users can create, manage, and use web applications. This article covers the details of the feature and serves as a base for those who want to go further into developing WebMatrix 2 Extensions. Use the Contents box to the right of the page to move to other articles as needed. Also you can find links to other useful resources at the end of the article.

##	Core application extensibility

WebMatrix offers a number of extensibility points, such as dialogs, ribbon buttons, context menus and editor features. These extensibility points allow you to build new features for WebMatrix. The controls inherit the parent's look and feel (unless you specify otherwise), which means that they will feel natural to users.

	Available extensibility points:

*	Look up the site's local path, URL, and application identifier
*	Look up the currently active workspace (Site, Files, Databases, or Reports)
*	Add items to the Site workspace dashboard
*	Add groups and buttons to the ribbon
*	Add a dialog box
*	Add a shortcut menu for when a user right-clicks a file in the Files workspace
*	Add status information to the notification bar
*	In the editor, get and modify selected text in the active open file
*	In the editor, get the location of the insertion point and insert text in the active open file

WebMatrix enables individuals and community members to customize the experience to best fit their own unique processes and workflow. Developers can take adavantage of a set of API's to configure many aspects of the development experience, so users can become more productive by streamlining common tasks, automating monotonous ones, and simplifying difficult ones!

##	Types of Extensibility

Extensibility for WebMatrix 2 Beta takes three forms: task-based extensibility, help extensibility, and extensions. This gallery site and its corresponding documentation is focused only on the extensions type.

1.	Task-based extensibility refers to the ability of third party web applications to embed a file within the install package to customize the WebMatrix user interface for that particular application. By providing some simple XML, applications in the [Web App Gallery] can add custom links to the Ribbon or dashboard, protect core application files, provide enhanced Intellisense for PHP, and more.
2.	Help extensibility makes it possible to integrate custom content with WebMatrix's new, context-sensitive help pane. The new help system shows links to relevant content and videos based on where the user is in the application and what they are doing. Help content is drawn from a variety of sources; content providers can create custom feeds to cover new topics or provide more context on existing ones. [This article explains how to create custom help content].
3.	WebMatrix Extensions. For developers, the real power lies in the ability to write extensions that run inside WebMatrix because they're capable of far richer customization. WebMatrix extensions can be written in any .NET language, are loaded by [MEF, the Managed Extensibility Framework], and installed/uninstalled (behind the scenes) as [NuGet] packages (with a slight twist I'll explain in a different post). Similar to Visual Studio, WebMatrix has an extension gallery that allows users to browse and install extensions from a central feed - or create and share custom feeds!

In summary, WebMatrix 2 provides developers with multiple ways to create a customize enviroenement for users. The Extensions on this site are a very small sample of possibilities, there is much to build.

<!-- Urls. -->
[Web App Gallery]: http://www.microsoft.com/web/gallery/
[This article explains how to create custom help content]: http://learn.iis.net/page.aspx/1032/creating-a-content-feed-for-learning/
[MEF, the Managed Extensibility Framework]: http://msdn.microsoft.com/en-us/library/dd460648.aspx
[NuGet]: http://nuget.org/ 
