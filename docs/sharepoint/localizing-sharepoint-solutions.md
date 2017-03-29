---
title: "Localizing SharePoint Solutions | Microsoft Docs"
ms.custom: ""
ms.date: "02/02/2017"
ms.prod: "visual-studio-dev14"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "office-development"
ms.tgt_pltfrm: ""
ms.topic: "article"
f1_keywords: 
  - "VS.SharePointTools.Project.GlobalAndFeatureResource"
  - "VS.SharePoint.Project.AddResourceDialog"
dev_langs: 
  - "VB"
  - "CSharp"
  - "VB"
  - "CSharp"
helpviewer_keywords: 
  - "globalizing [SharePoint development in Visual Studio]"
  - "localizing [SharePoint development in Visual Studio]"
  - "SharePoint development in Visual Studio, localizing"
ms.assetid: 0d4cfa2b-8b48-45c7-bbee-ece9b0baffaf
caps.latest.revision: 18
author: "kempb"
ms.author: "kempb"
manager: "ghogen"
---
# Localizing SharePoint Solutions
  The process of preparing your applications so that they can be used worldwide is known as localization. Localization is translating resources to a specific culture. For more information, see [Globalizing and Localizing Applications](/visual-studio/ide/globalizing-and-localizing-applications). This topic provides an overview on how to localize a SharePoint solution.  
  
 To localize a solution, you remove hard-coded strings from the code and abstract them into resource files. A Resource file is an [!INCLUDE[TLA2#tla_xml](../sharepoint/includes/tla2sharptla-xml-md.md)]-based file with a .resx extension. The resource file contains the translated versions of the strings used in your solution. For more information, see [Resources in Applications](http://go.microsoft.com/fwlink/?LinkID=155844).  
  
> [!NOTE]  
>  Add only string resources to SharePoint solution resource files. Although the Resource Editor enables you to add non-string resources, non-string resources do not deploy to SharePoint.  
  
## Resource Files  
 There are three kinds of resource files: default, language-neutral, and language-specific.  
  
|Resource File Type|Description|  
|------------------------|-----------------|  
|Default|Also known as a fallback resource, default resource files contain strings localized for the default culture, such as English. They are used if no localized resource files for the specified language can be found. Default resources do not have separate files, they are stored in the main application assembly.|  
|Language-neutral|A resource file that contains strings localized for a language, but not a specific culture. For example, "fr" for French.|  
|Language-specific|A resource file that contains strings localized for a language and a culture. For example, "fr-CA" for French Canadian.|  
  
 For more information, see [Hierarchical Organization of Resources for Localization](http://go.microsoft.com/fwlink/?LinkId=178360).  
  
 To specify default resource files in SharePoint projects that you develop in [!INCLUDE[vsprvs](../sharepoint/includes/vsprvs-md.md)], choose **Invariant Language (Invariant Country)** in the culture list of the **Add Resource** dialog box when you add a resource file.  
  
## Localizing Visual Studio SharePoint Solutions  
 When you localize a solution, you should consider all of the textual information that your solution displays to users. Informational messages, error messages, and [!INCLUDE[TLA2#tla_ui](../sharepoint/includes/tla2sharptla-ui-md.md)] strings must be translated and those translations placed in the resource files.  
  
 Every string in a resource file has a unique identifier. Use the same identifier for the translated string in each resource file. For example, if "String1" is the identifier for the first string in the default resource file, use the same identifier for the first string in the language-specific resource files.  
  
 There are three areas you typically localize in [!INCLUDE[vsprvs](../sharepoint/includes/vsprvs-md.md)] SharePoint applications: features, ASPX page markup, and code. For purposes of illustration, the following sections assume you have a SharePoint solution that you want to localize into German and Japanese. The default language is English.  
  
### Localizing Features  
 To localize a feature, you have to replace the hard-coded title and description of the feature with an expression that references the translated title and string in the localized resources file. You make this change in the **Feature Designer** in [!INCLUDE[vsprvs](../sharepoint/includes/vsprvs-md.md)]. For more information, see [How to: Localize a Feature](../sharepoint/how-to-localize-a-feature.md).  
  
 To localize your English feature into German and Japanese, you add three Resource File project items to your project: one for English, one for German, and one for Japanese. Feature resource files cannot be used to localize ASPX markup or code; separate resource files are required for them.  
  
 After you create the feature resource files, add translated strings to them. Access the localized strings with an expression in the following format:  
  
```  
$Resources:String ID  
```  
  
 Feature resources in [!INCLUDE[vsprvs](../sharepoint/includes/vsprvs-md.md)] are always named Resources. If you select a language other than Invariant Language, then a culture [!INCLUDE[TLA2#tla_id](../sharepoint/includes/tla2sharptla-id-md.md)] is added to the resource file name. For example, if you add an invariant language (default) feature resource file, it is called Resources.resx. If you add a language-specific feature resource by selecting a culture of Japanese (Japan), the file is called Resources.ja-JP.resx. Feature resource file names are automatically assigned and cannot be changed.  
  
 The scope of feature resources is local to the feature they are added to. To create resources that can be used by any feature or element file in the solution, add a **Global Resources File** project item instead of a feature resource File. The **Global Resources File** project item is located in the **2010** folder under **SharePoint** in the **Add New Item** dialog box. Global resources files deploy to the \Resources folder of the SharePoint root folder.  
  
### Localizing ASPX Page Markup  
 To localize [!INCLUDE[vstecasp](../sharepoint/includes/vstecasp-md.md)] pages, you add three Resources File project items to your project: one for English, one for German, and one for Japanese. If you do not have to localize code in addition to the markup, you can instead add Global Resources Files.  
  
 Provide a name for the default language resource file. Give the localized resource files the same name appended with the language-specific culture [!INCLUDE[TLA2#tla_id](../sharepoint/includes/tla2sharptla-id-md.md)]. For example, MyAppResources.de-DE.resx for German and MyAppResources.ja-JP.resx for Japanese.  
  
 Set the **Deployment Type** property of each resource file to **AppGlobalResource**. This causes the resource files to deploy to the App_GlobalResources folder, where they are available to all ASPX pages and controls in the solution. The App_GlobalResources folder is located in C:\inetpub\wwwroot\wss\VirtualDirectories\\<port number\>\App_GlobalResources.  
  
> [!NOTE]  
>  If you use non-global resource files, move them into the project item folder to enable the Deployment Type property and other SharePoint-specific properties.  
  
 ASPX markup resource files can also be used to localize code. If you are using the resources to localize code in addition to ASPX markup, leave the Build Action property setting of each file as Embedded Resource to cause the resource to compile into a satellite assembly. However, if you are using the resource files only to localize markup, you can optionally change Build Action to Content to prevent the file from being compiled into the main application assembly.  
  
 Replace all hard-coded property strings in your ASPX pages and controls markup with an expression in the following format:  
  
```  
<asp:<class> runat="server" Text="<%$Resources:<Resource File Name>, <String ID>%>" />  
```  
  
 For example:  
  
```  
<asp:Button ID="btn1" runat="server" onclick="btn1_Click" Text="<%$Resources:Resource1,String7%>"></asp:Button>  
```  
  
 For ASPX as text, use an expression in the following format:  
  
```  
<asp:literal ID="<ID>" runat="server" Text="<%$Resources:<Resource File Name>, <String ID>%>" />  
```  
  
 For example:  
  
```  
<asp:literal ID="Literal1" runat="server" Text="<%$Resources:Resource1, String9%>" />  
```  
  
 For more information, see [How to: Localize ASPX Markup](../sharepoint/how-to-localize-aspx-markup.md).  
  
### Localizing Code  
 In addition to localizing Feature strings and [!INCLUDE[vstecasp](../sharepoint/includes/vstecasp-md.md)] markup, you also have to localize the message strings and error strings that appear in your solution code. Localized informational and error messages are contained in satellite assemblies. Satellite assemblies contain strings that are visible to users, such as [!INCLUDE[TLA2#tla_ui](../sharepoint/includes/tla2sharptla-ui-md.md)] text and output messages like exceptions.  
  
 [!INCLUDE[vsprvs](../sharepoint/includes/vsprvs-md.md)] uses the standard .NET Framework hub and spoke model. The hub, or main program assembly, contains the default language resources. The spokes, or satellite assemblies, contain the language-specific resources. For more information, see [Packaging and Deploying Resources](http://go.microsoft.com/fwlink/?LinkId=179280). Satellite assemblies are compiled from resource (.resx) files. When you add language-specific resource files to your project and the solution package, [!INCLUDE[vsprvs](../sharepoint/includes/vsprvs-md.md)] compiles the resource files into satellite assemblies named *Project Name*.resources.dll.  
  
 As with ASPX markup, localize SharePoint application code by adding separate Resources File project items to your project; one for the default language and one for each localized language. However, as mentioned previously, if you already have resource files for localizing ASPX markup, you can reuse them for localizing code. If you need to create resource files, give the default language resource file a name of your choice appended with a .resx extension. Name the localized resource files the same name appended with the language-specific culture [!INCLUDE[TLA2#tla_id](../sharepoint/includes/tla2sharptla-id-md.md)]. Set the Build Action property of each resource file to Embedded Resource to enable the creation of satellite resource assemblies.  
  
 To create the satellite assemblies, build the project and then add the files as additional assemblies through the **Advanced** tab of the **Package Designer**. When adding the assemblies, prepend a culture [!INCLUDE[TLA2#tla_id](../sharepoint/includes/tla2sharptla-id-md.md)] folder to the Location path, such as de-DE\\*Project Item Name*.resources.dll. This allows the package to contain files that have the same name.  
  
 In your code, replace hard-coded strings with calls to the <xref:System.Web.HttpContext.GetGlobalResourceObject%2A> method using the following syntax:  
  
```  
HttpContext.GetGlobalResourceObject("<Resource File Name>", "<String ID>")  
```  
  
 For more information, see [How to: Localize Code](../sharepoint/how-to-localize-code.md).  
  
#### Web Part Code Localization  
 Web parts include a custom property editor feature that includes code attributes that use hard-coded strings, such as WebDisplayName, Category, and WebDescription. To replace the string values for these attributes, create a separate class that derives from the attribute's class. In those classes, set the attribute's property. The attribute property depends on the base class. For example, the WebDisplayName attribute property is DisplayNameValue and the WebDescription attribute property is DescriptionValue.  
  
 In the derived class, reference the string ID from the resource file and the ResourceManager object to get the localized value for the string ID. Return this value to the property editor attribute.  
  
## See Also  
 [How to: Localize a Feature](../sharepoint/how-to-localize-a-feature.md)   
 [How to: Localize ASPX Markup](../sharepoint/how-to-localize-aspx-markup.md)   
 [How to: Localize Code](../sharepoint/how-to-localize-code.md)   
 [How to: Add a Resource File](../sharepoint/how-to-add-a-resource-file.md)   
 [How to: Use a Resource File to Specify Localized Names, Properties, and Permissions](../sharepoint/how-to-use-a-resource-file-to-specify-localized-names-properties-and-permissions.md)  
  
  