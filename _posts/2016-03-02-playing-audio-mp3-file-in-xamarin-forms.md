---
layout: post
title: Воспроизведение аудио-файлов в Xamarin.Forms
redirect_to: "https://ypakala.com"
date: 2016-03-02 23:20
original_url: http://err2solution.com/2016/02/playing-audio-mp3-file-in-xamarin-forms/
tags:
- c#
- xamarin forms
- ios
- android
- перевод
---

Недавно мне было необходимо проиграть mp3-файл в одном из моих Xamarin.Forms приложений, когда я начал искать решение в интернете, то узнал о том, что в настоящий момент Xamarin.Forms приложения не поддерживают проигрывание аудио-файлов из коробки. Но, как знает всякий разработчик на Xamarin.Forms, мы можем использовать любую нативную функциональность по средствам dependency-сервисов, поэтому я решил попробовать и вот что получилось.

Нам потребуется создать интерфейс, который будет отдельно реализован на каждой конкретной платформе, я назвал его `IAudio.cs`:

```csharp
using System;
namespace AudioPlayEx
{
	public interface IAudio
	{
		void PlayAudioFile(string fileName);
	}
}
```

Теперь мы должны написать класс содержащий реализацию интерфейса с использованием платформа специфичного кода. Я назвал этот класс `AudioService` на каждой платформе.

Класс для Android проекта выглядит так:

```csharp
using System;
using Xamarin.Forms;
using AudioPlayEx.Droid;
using Android.Media;
using Android.Content.Res;

[assembly: Dependency(typeof(AudioService))]
namespace AudioPlayEx.Droid
{
	public class AudioService : IAudio
	{
		public AudioService ()
		{
		}

		public void PlayAudioFile(string fileName) {
			var player = new MediaPlayer();
			var fd = global::Android.App.Application.Context.Assets.OpenFd(fileName);
			player.Prepared += (s, e) =>
			{
				player.Start();
			};
			player.SetDataSource(fd.FileDescriptor,fd.StartOffset,fd.Length);
			player.Prepare();
		}
	}
}
```

> Убедитесь что файлы, которые вы хотите воспроизвести скопированы в директорию `Assets`, тогда код будет работать правильно.

И класс для iOS проекта:

```csharp
using System;
using Xamarin.Forms;
using AudioPlayEx;
using AudioPlayEx.iOS;
using System.IO;
using Foundation;
using AVFoundation;

[assembly: Dependency (typeof (AudioService))]
namespace AudioPlayEx.iOS
{
	public class AudioService : IAudio
	{
		public AudioService ()
		{
		}

		public void PlayAudioFile(string fileName)
		{
			string sFilePath = NSBundle.MainBundle.PathForResource(Path.GetFileNameWithoutExtension(fileName), Path.GetExtension(fileName));
			var url = NSUrl.FromString (sFilePath);
			var _player = AVAudioPlayer.FromUrl(url);
			_player.FinishedPlaying += (object sender, AVStatusEventArgs e) => {
				_player = null;
			};
			_player.Play();
		}
	}
}
```

> В случае iOS проекта, вам нужно будет скопировать аудио-файлы в директорию `Resources`.

И, наконец, для воспроизведения аудио-файлов в нашем PCL/Shared проекте, воспользуемся следующим кодом:

```csharp
DependencyService.Get<IAudio>().PlayAudioFile("MySong.mp3");
```

Вы можете вызвать этот код при нажатии кнопки или любом другом событии в вашем PCL/Shared проекте. В приведенном выше примере я проигрывал МР3-файл, но вы можете использовать тот же самый код, чтобы воспроизвести любой другой аудио файл поддерживающийся платформой, например .aif (только в iOS), МР4, WAV и т.д.

Исходный код примера вы можете найти [здесь](https://github.com/srkrathore/AudioPlayEx).

Счастливого кодинга :smile:
