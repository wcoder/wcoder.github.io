---
layout: post
title: Добавление AppBar в Windows Phone 8
date: 2014-05-22 12:42
tags:
- xaml
- windows phone
- сниппет
---

```xml
<!--<phone:PhoneApplicationPage ... >-->

	<!-- ... -->
	
	<phone:PhoneApplicationPage.ApplicationBar>
		<shell:ApplicationBar IsVisible="True">
			<shell:ApplicationBar.Buttons>
				<shell:ApplicationBarIconButton
					IconUri="/Assets/appbar.navigate.previous.png"
					Text="back"
					Click="ApplicationBarIconButtonBack_OnClick"
					/>
				<shell:ApplicationBarIconButton
					IconUri="/Assets/appbar.navigate.next.png"
					Text="next"
					Click="ApplicationBarIconButtonNext_OnClick"
					/>
			</shell:ApplicationBar.Buttons>
			<shell:ApplicationBar.MenuItems>
				<shell:ApplicationBarMenuItem
					Text="settings"
					Click="ApplicationBarMenuItemSettings_OnClick"
					/>
				<shell:ApplicationBarMenuItem
					Text="about"
					Click="ApplicationBarMenuItemAbout_OnClick"
					/>
			</shell:ApplicationBar.MenuItems>
		</shell:ApplicationBar>		
	</phone:PhoneApplicationPage.ApplicationBar>
	
<!--</phone:PhoneApplicationPage>-->
```

### Полезные ссылки

* [Application Bar for Windows Phone](http://developer.nokia.com/community/wiki/Application_Bar_for_Windows_Phone) 
* [ApplicationBar Controller](http://www.codeproject.com/Articles/470063/Windows-Phone-ApplicationBar-Controller)
