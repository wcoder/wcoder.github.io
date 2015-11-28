---
layout: post
title: Проверка доступности сети в универсальных приложениях Windows
date: 2015-09-13 13:12
tags:
- сниппет
- C#
- windows
- windows phone
- UWP
---

Если вы создаете универсальное приложение для Windows Phone 8.1/Windows 8.1 следующий класс поможет вам проверить доступность сети.

## Класс NetworkAvailabilty
Это синглтон класс, что означает, что только один его экземпляр будет существовать на протяжении всего жизненного цикла приложения.

``` csharp
public class NetworkAvailabilty
{
	private static NetworkAvailabilty _networkAvailabilty;
	public static NetworkAvailabilty Instance
	{
		get { return _networkAvailabilty ?? (_networkAvailabilty = new NetworkAvailabilty()); }
		set { _networkAvailabilty = value; }
	}
	
	private bool _isNetworkAvailable;
	public event Action<bool> OnNetworkAvailabilityChange = delegate { };
	
	public bool IsNetworkAvailable
	{
		get
		{
			return _isNetworkAvailable;
		}
		protected set
		{
			if (_isNetworkAvailable == value) return;
			_isNetworkAvailable = value;
			OnNetworkAvailabilityChange(value);
		}
	}
	
	private void CheckInternetAccess()
	{
		var connectionProfile = NetworkInformation.GetInternetConnectionProfile();
		IsNetworkAvailable = (connectionProfile != null &&
							 connectionProfile.GetNetworkConnectivityLevel() ==
							 NetworkConnectivityLevel.InternetAccess);
		Debug.WriteLine("has network changed: " + IsNetworkAvailable);
	}
	
	private void NetworkInformationOnNetworkStatusChanged(object sender)
	{
		CheckInternetAccess();
		Debug.WriteLine("network status changed");
	}
	
	private NetworkAvailabilty()
	{
		NetworkInformation.NetworkStatusChanged += NetworkInformationOnNetworkStatusChanged;
		CheckInternetAccess();
	}
}
```

Метод `NetworkInformationOnNetworkStatusChanged` вызывается каждый раз, когда изменяется состояние сети. Этот метод, в свою очередь, обновляет свойство `IsNetworkAvailable` класса.

## Использование

Всякий раз, когда вы должны проверить, если ли сеть, просто используйте:

``` csharp
if (NetworkAvailabilty.Instance.IsNetworkAvailable)
{
	// ...
}
```
