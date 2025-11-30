---
layout: post
title: Многопоточность в Android. Все что вам нужно знать. Часть 1 - Введение
redirect_to: "https://ypakala.com"
date: 2017-08-13 11:40
original_url: https://www.toptal.com/android/android-threading-all-you-need-to-know
tags:
- многопоточность
- android
- перевод
---

Каждый Android разработчик, в тот или иной момент сталкивается с необходимостью иметь дело с потоками в своем приложении.

Когда приложение запускается, оно создает первый поток выполнения, известный как основной поток или main thread. Основной поток отвечает за отправку событий в соответствующие виджеты пользовательского интерфейса, а также связь с компонентами из набора инструментов Android UI.

Чтобы ваше приложение сохраняло отзывчивость, важно избегать использования основного потока для выполнения любой операции, которая может привести к его блокировке.

 Сетевые операции и обращения к базе данных, а также загрузка определенных компонентов, являются типичными примерами операций, которые не следует выполнять в основном потоке. Когда они вызваны в главном потоке, они вызваны синхронно, что означает, что пользовательский интерфейс не будет ни на что реагировать до завершения операции.  По этой причине, они обычно выполняются в отдельных потоках, что позволяет избежать блокировки пользовательского интерфейса во время выполнения (т. е. они выполняются асинхронно из UI).

Android предоставляет множество способов создания и управления потоками, и множество сторонних библиотек, которые делают управление потоками гораздо более приятным.

![Многозадачность в Android](https://uploads.toptal.io/blog/image/122375/toptal-blog-image-1489079683857-fb445c27f8f0fe4d4a94d4731b1d669e.jpg)

В этой статье вы узнаете о некоторых распространенных сценариях в Android разработке, где многопоточность становится важной, и некоторые простые решения, которые могут быть применены к этим сценариям, и многое другое.

## Многозадачность в Android

 В Android вы можете классифицировать все компоненты потоков на две основные категории:
1. Потоки связанные с активностью / фрагментом.
Эти потоки привязаны к жизненному циклу активности / фрагмента и завершаются сразу после их уничтожения.

2. Потоки не связанные с активностью / фрагментом.
Эти потоки могут продолжать работать за пределами жизни активности / фрагмента (если есть), из которых они были созданы.


### Компоненты многопоточности, которые присоединяются к активности / фрагменту

#### AsyncTask

`AsyncTask` это наиболее основной Android компонент для организации потоков. Он прост в использовании и может быть хорошей основой для вашего сценария.

Пример использования:

```java
public class ExampleActivity extends Activity {

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);

		new MyTask().execute(url);
	}

	private class MyTask extends AsyncTask<String, Void, String> {

		@Override
		protected String doInBackground(String... params) {
			String url = params[0];
			return doSomeWork(url);
		}

		@Override
		protected void onPostExecute(String result) {
			super.onPostExecute(result);
			// что-то делаем с результатом
		}
	}
}
```

Однако, `AsyncTask` не подойдет, если вам нужен отложенный запуск задачи, после завершения работы вашей активности / фрагмента. Стоит отметить, что даже такая простая вещь, как вращение экрана может вызвать уничтожение активности.

#### Загрузчики

Загрузчики могут решить проблемы, упомянутые выше. Загрузчик автоматически останавливается, когда уничтожается активность и перезапускает себя, после пересоздания активности.

В основном есть два типа загрузчиков: `AsyncTaskLoader` и `CursorLoader`. О загрузчике `CursorLoader` вы узнаете далее в этой статье.

`AsyncTaskLoader` похож на `AsyncTask`, но немного сложнее.

Пример использования:

```java
public class ExampleActivity extends Activity{

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);

		getLoaderManager().initLoader(1, null, new MyLoaderCallbacks());
	}

	private class MyLoaderCallbacks implements LoaderManager.LoaderCallbacks {

		@Override
		public Loader onCreateLoader(int id, Bundle args) {
			return new MyLoader(ExampleActivity.this);
		}

		@Override
		public void onLoadFinished(Loader loader, Object data) {

		}

		@Override
		public void onLoaderReset(Loader loader) {

		}
	}

	private class MyLoader extends AsyncTaskLoader {

		public MyLoader(Context context) {
			super(context);
		}

		@Override
		public Object loadInBackground() {
			return someWorkToDo();
		}

	}
}
```

### Компоненты многопоточности, которые не присоединяются к активности / фрагменту

#### Service

`Service` это компонент, который полезен для выполнения длинных (или потенциально длительных) операций без какого-либо пользовательского интерфейса.

`Service` работает в основном потоке своего процесса; не создает свой собственный поток и не запускается в отдельном процессе, если вы это не указали.

Пример использования:
```java
public class ExampleService extends Service {

	@Override
	public int onStartCommand(Intent intent, int flags, int startId) {
		doSomeLongProccesingWork();
		stopSelf();

		return START_NOT_STICKY;
	}

	@Nullable
	@Override
	public IBinder onBind(Intent intent) {
		return null;
	}
}
```
Используя `Service` вы обязаны остановить его, когда его работа будет завершена, вызвав методы `stopSelf()` или `stopService()`.

#### IntentService

`IntentService` работает в отдельном потоке и автоматически останавливается после завершения работы.

`IntentService` обычно используется для коротких задач, которые не обязательно должны быть привязаны к какому-либо пользовательскому интерфейсу.

Пример использования:
```java
public class ExampleService extends IntentService {

	public ExampleService() {
		super("ExampleService");
	}

	@Override
	protected void onHandleIntent(Intent intent) {
		doSomeShortWork();
	}
}
```

[Часть 2. 7 шаблонов использования многопоточности в Android](https://wcoder.github.io/notes/android-threading-all-you-need-to-know-part2)
