# How to Create Extensions

This document provides the basic steps to get you started writing Extensions for WebMatrix. The simplest path to take is to use the [WebMatrix Extension Visual Studio Project Template]. The template is called **WebMatrix Extension Template** and you can find it in the Templates section of the Extension Manager in Visual Studio or as part of the [WebMatrix Extension Kit]. The template allows you to create Visual Studio projects that contain a fully functional Extension. Also, you can learn about the [WebMatrix Extensibility API] and explore the sample extension provided in the kit. Once you finish your extension and publish it in this site, it will be available to users through the WebMatrix's Extension Gallery:
	
![][0]

## Introducing the Extension Template for Visual Studio

The basic functionality of the template extension is to add a Ribbon button and handle activation of that button to open the current web site in the browser. Here is a list of the files and items implemented that you may find useful:

1. **ReadMe.txt file**: Contains the instructions for setting up the Debug properties to call WebMatrix.exe when running the extension in debug mode.
2. **WebMatrixExtension.cs file**: Contains an implementation of the abstract class Extension. This class provides the basic ingredients to create an Extension:

	1.	The abstract method “Initialize(IWebMatrixHost host, ExtensionInitData initData)” is called by WebMatrix at startup. Through ExtensionInitData you have the opportunity to setup elements such as Dashboard items and Ribbon controls. The method also allows you to store a reference to the IWebMatrixHost interface needed by your extension to access the rest of WebMatrix extensibility functionality.

	2.	The methods needed to help you implement your particular User Experience for a custom installation step: “HasInstaller”, “IsInstalled”, and “OnInstall”.

	3. You will also find other items that will help you guide the implementation of your extension; for example: usage of icon images and instantiation of classes such as RibbonGroup, RibbonItem, and RibbonButton. It will become apparent the relationship within these objects and will let you get started on more sophisticated constructs.

	4.	It also shows how you can take advantage of accepted practices such as the use of delegates when setting up event handlers.

3.	**DelegateCommand.cs**: Contains a definition of the DelegateCommand class, a simple implementation of the DelegateCommand (aka RelayCommand) pattern. It derives from the ICommand interface. It contains the basic implementations of the constructors and required ICommand methods and events (“Execute”, “CanExecute”, and “CanExecuteChanged”).

## Setting Up and Using the Extension Template
	
The recommended approach is to use the extension template found in the Visual Studio Extension Gallery. You can open the Visual Studio Extension Manager available under the Tools Menu, select "Extension Manager", then Online Gallery, click on the Templates tab and search for WebMatrix. Once it is displayed in the list, click download.


	
Alternatively, you can use the WebMatrixExtension.zip file provided in the kit by following these steps:

1.	Copy the WebMatrix extension Visual Studio project template (WebMatrixExtension.zip file) provided in the Extension Kit above and save it in your "%USERPROFILE%\Documents\Visual Studio 2010\Templates\ProjectTemplates" directory.

2. Run Visual Studio and select File Menu -> New Project. In the New Project Dialog, select the "Visual C#" node, scroll down and find the "WebMatrix Extension" list item (see image below), type a project name (ex: "MyExtension" -no spaces), and click OK.


	![][1]

	The project template sets up the project with the configured environment, all the necessary references (i.e., Microsoft.WebMatrix.Extensibility.dll), includes pre- and post-build steps to install your extension into WebMatrix, and re-enabled in case of an extension crash. Notice:
	
	* It is assumed that you have already installed WebMatrix, otherwise the API’s referenced won’t be available.
	* The project created contains a simple Ribbon-based extension that demonstrates some of the basic extensibility points.
	* The template's ReadMe.txt explains how to configure Visual Studio so that pressing F5 will automatically load the extension inside WebMatrix for a simple, seamless debugging experience with complete breakpoint support. Please make sure to follow the instructions provided in it. See image below:


	![][2]

3. Once you have followed the instructions in the ReadMe.txt file, you can build it and press F5 to run it inside WebMatrix.

	![][3]

##	Using the template with Visual C# 2010 Express

You can use this Development environment which can be downloaded for free here: http://www.microsoft.com/visualstudio/en-us/products/2010-editions/visual-csharp-express. Follow these steps to create a startup program for your dll Extension:

1.	Since Visual C# Express does not allow you to setup an external startup program. For debugging purposes you can add a command line application project to the solution, this application would invoke WebMatrix.exe. In the project’s Program.cs file add: 

	<pre>
	using System;
	using System.Collections.Generic;
	using System.Linq;
	using System.Text;
	using System.Diagnostics;
	
	namespace ConsoleApplication1
	{
		class Program
		{
			static void Main(string[] args)
			{
				Process webMatrix = new Process();
				webMatrix.StartInfo.FileName = "C:\Program Files (x86)\Microsoft WebMatrix\WebMatrix.exe";
				webMatrix.Start();
				webMatrix.WaitForExit();
			}
		}
	}
	</pre>

2.	In the Solution Explorer, right click the new added project and select “Set as StartUp Project”.

	![][4]

3.	Now you can build and press F5 to see your extension loaded inside WebMatrix.

##	Inspecting your new Extension

1.	Open the WebMatrixExtension.cs file, notice that the template generated the [ExtensionName] class which derives from Extension; this class simplifies the work of interfacing with WebMatrix. It also shields you from the work of directly using MEF to implement your Extension.

2.	Notice that in order for an extension to be loaded by WebMatrix, it needs to be located in the right directory: ("%USERPROFILE%\AppData\Local\Microsoft\WebMatrix\Extensions\20RC\[ExtensionName]"). The template already provides the pre and post build steps to place you extension in such directory.

3.	You can provide content for the Ribbon by adding instances of the relevant classes (RibbonButton, RibbonMenuButton, RibbonGroup, etc.) to the Extension’s RibbonItems collection. This is done only at initialization time through the Initialize method in the Extension class. Once these items are initialized, you cannot change them. You can show and hide Ribbon content as needed. But you can't add or remove items after initialization. There are simple, concrete subclasses for each of the relevant objects (RibbonButton, RibbonMenuButton, RibbonButtonGallery, etc.) so you don't need to spend time implementing these classes. Using the "helper implementations" is completely optional. Here is sample code from the WebMatrixExtension.cs file: 

	<pre>
	protected override void Initialize(IWebMatrixHost host, ExtensionInitData initData)
	{
		_webMatrixHost = host;	
		// Add a simple button to the Ribbon
		initData.RibbonItems.Add(
		new RibbonGroup(
			"My Group",
			new RibbonItem[]
			{
				new RibbonButton(
				"My Button",
				new DelegateCommand(HandleRibbonButtonInvoke),
				null,
				_starImageSmall,
				_starImageLarge)
			}));
	}
	</pre>

4. 	Notice that to create a RibbonButton you need a label, an ICommand implementation, and a small/large image. The template's sample includes a couple of images already configured properly to provide a working example. Wiring up the images correctly is standard WPF, but the pack URI syntax can be a little tricky and it's common to forget to change the build type of the image files to "Resource" - so that's already been done for you as a helpful reminder. For its ICommand implementation, the template sample uses a simple DelegateCommand class (also included). The sample DelegateCommand is very much in line with other implementations of DelegateCommand or RelayCommand. (Feel free to use whatever version you'd like; the sample DelegateCommand exists simply to avoid introducing a dependency on a third-party library.) As you'd expect, the ICommand's CanExecute and CanExecuteChanged methods are used to dynamically enable/disable the button and its Execute method is called when the button is clicked.

##	Submit your Extension to the Extension Gallery

Finally, once you are ready to share your extension with the WebMatrix Team you can upload it here: http://extensions.webmatrix.com

In Summary, the Extension template provides you with the foundation of a working Extension to start from. Review the Super Snippets example also included in this package. Examine it in detail, it should clarify many of the questions you may have and demonstrate other WebMatrix extensibility points. Once you are ready to share your extension, upload it to the gallery.

<!-- Images. -->
[0]: media/htiwme-WMX1.png
[1]: media/htcx-ExtTemplate.PNG
[2]: media/htcx-TempReadme.PNG
[3]: media/htcx-ExtBtn.png
[4]: media/htcx-SetAsStartUp.png
[5]: media/installingtemplate1.png
[6]: media/installingtemplate2.png

<!-- Urls. -->
[WebMatrix Extension Visual Studio Project Template]: http://visualstudiogallery.msdn.microsoft.com/f40607ae-66ba-4982-a4e5-5ea969ea43e1
[WebMatrix Extension Kit]: http://webmatrix2.blob.core.windows.net/webmatrix2/WebMatrix2ExtensionKit.zip
[WebMatrix Extensibility API]: http://msdn.microsoft.com/en-us/library/jj158462(v=vs.111).aspx