---
layout: post
title: "Asynchronous Auto Complete Text Box in WPF"
date: 2013-09-21 -0800
comments: true
tags: [WPF, WPF Auto Complete, WPF AutoCompleteBox]
redirect_from:
  - index.php/asynchronous-auto-complete-text-box-in-wpf/
---
* Download and Install WPFToolkit from [WPFToolkit](http://wpf.codeplex.com/releases/view/40535)
*  Drag and Drop “AutoCompleteBox” from toolbox, if for any reason it is not appearing in the toolbox you can create a tab in toolbox and add choose items System.Windows.Controls.*.Toolkit or you can create the control following steps
   * Add as a reference following dlls, actual location may vary according to location C:\Program Files (x86)\WPF Toolkit\v3.5.50211.1\WPFToolkit.dll              C:\Program Files (x86)\WPF Toolkit\v3.5.50211.1\System.Windows.Controls.Input.Toolkit.dll
   * Add reference to assembly in XAML window tag
 
```csharp
        xmlns:my="clr-namespace:System.Windows.Controls;assembly=System.Windows.Controls.Input.Toolkit"
```

   * Add the control
  
```csharp
        <my:AutoCompleteBox Name="autoCompleteBox1"  Width="213" Height="25" Populating="autoCompleteBox1_Populating" />
```

* In populating event get the desired options from service/database and bind to the AutoCompleteBox. Calling PopulateComplete() is important as it tells the box that items that need to be populated have been binded.

```csharp
      private void autoCompleteBox1_Populating(object sender, PopulatingEventArgs e)
        {
            var options = new List() { "one", "two", "three" };
            autoCompleteBox1.ItemsSource = options;
            autoCompleteBox1.PopulateComplete();
        }
```

