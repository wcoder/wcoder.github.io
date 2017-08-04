---
layout: post
title: Множественные RecyclerView через ViewPager и TabLayout в Xamarin.Android
date: 2017-07-20 15:50
original_url: http://code2xamarin.net/multiple-recyclerview-with-viewpager-and-tablayout-xamarin-android/
tags:
- xamarin
- android
- xamarin.android
- c#
- перевод
---

В этой статье вы увидите:

- Работу с  RecyclerView, ViewPager  и TabLayout;
- Загрузку изображений по URL с использованием Glide;
- Использование Fragment и FragmentPagerAdapter;

Для использования примеров кода, вам необходимо подключить следующие NuGet пакеты: `Xamarin.Android.Support.v4`, `Xamarin.Android.Support.Design` и `Glide.Xamarin` для загрузки и кеширования изображений без каких-либо исключений.

## Разметка

Для этого проекта нам потребуется Material Theme, и вот почему:

- Изменение свойств TabLayout (например, цвет, вид текста)
- Чтобы иметь возможность использовать **AppCompatActivity** и **SupportFragmentManager** и др.

Чтобы использовать Material Theme, в директории ресурсов создайте поддиректорию **values-21**, а в ней новый XML-файл с именем "styles".

Скопируйте в созданный файл, следующий XML-код:
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
	<style name="MaterialTheme" parent="MaterialTheme.Base" />

	<style name="MaterialTheme.Base" parent="Theme.AppCompat.Light.NoActionBar">
		<item name="colorAccent">#3a67e0</item>
		<item name="colorPrimary">#3a67e0</item>
		<item name="colorPrimaryDark">#ffffff</item>
		<item name="android:windowDrawsSystemBarBackgrounds">true</item>
		<item name="android:statusBarColor">#0d47a1</item>
	</style>

	<style name="TabLayoutTheme" parent="Widget.Design.TabLayout">
		<item name="tabIndicatorColor">#000000</item>
		<item name="tabIndicatorHeight">2dp</item>
		<item name="tabTextAppearance">@style/TabLayoutTextAppearance</item>
	</style>

	<style name="TabLayoutTextAppearance" parent="TextAppearance.Design.Tab">
		<item name="textAllCaps">false</item>
		<item name="android:textSize">19sp</item>
		<item name="android:fontFamily">sans-serif-light</item>
	</style>
</resources>
```

Создайте новый макет (или используйте MainLayout), где будут располагаться **ViewPager** и **TabLayout**.

Скопируйте и вставьте следующий код:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	xmlns:app="http://schemas.android.com/apk/res-auto"
	android:orientation="vertical"
	android:layout_width="match_parent"
	android:layout_height="match_parent">
	<android.support.v7.widget.Toolbar
		android:id="@+id/toolbar"
		android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
		android:layout_width="match_parent"
		android:layout_height="wrap_content"
		android:background="?attr/colorPrimary"
		android:minHeight="?attr/actionBarSize" />
	<android.support.design.widget.TabLayout
		app:tabMode="scrollable"
		app:tabGravity="fill"
		style="@style/TabLayoutTheme"
		android:id="@+id/tabLayout"
		android:layout_width="match_parent"
		android:layout_height="wrap_content" />
	<android.support.v4.view.ViewPager
		android:id="@+id/viewPager"
		android:layout_width="match_parent"
		android:layout_height="match_parent"
		app:layout_behavior="@string/appbar_scrolling_view_behavior" />
</LinearLayout>
```

Для каждой страницы потребуется **RecyclerViewLayout**, поэтому вы должны создать новый макет для RecyclerView.

Скопируйте и вставьте следующий макет в RecyclerViewLayout:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.widget.RecyclerView xmlns:android="http://schemas.android.com/apk/res/android"
	xmlns:app="http://schemas.android.com/apk/res-auto"
	app:layout_behavior="@string/appbar_scrolling_view_behavior"
	android:clipToPadding="false"
	android:layout_width="match_parent"
	android:layout_height="match_parent"
	android:id="@+id/recyclerView" />
```

RecyclerView нуждается в разметке для каждого элемента, поэтому вы должны ее создать.

В этом примере есть ImageView и TextView:
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	android:orientation="vertical"
	android:layout_width="match_parent"
	android:layout_height="wrap_content">
	<ImageView
		android:layout_margin="10dp"
		android:layout_alignParentLeft="true"
		android:layout_width="120dp"
		android:layout_height="120dp"
		android:id="@+id/imageView"
		android:adjustViewBounds="true" />
	<TextView
		android:layout_toEndOf="@id/imageView"
		android:layout_marginStart="30dp"
		android:id="@+id/textView"
		android:layout_width="wrap_content"
		android:layout_height="wrap_content"
		android:fontFamily="sans-serif"
		android:textSize="21sp"
		android:layout_centerVertical="true" />
</RelativeLayout>
```

## Код

На первым шаге, создадим класс фрагмента. Поскольку ViewPager всегда будет иметь единое представление для всех данных, приложение будет следовать определенной схеме, возвращая фрагменты.

`Func<T>` предопределенный тип делегата для метода, который возвращает некоторое значение типа `T`, и мы можем использовать этот тип для ссылки на метод, возвращающий некоторое значение, которое будет представлять собой View.

```csharp
public class RecyclerViewFragment : Android.Support.V4.App.Fragment
{
	readonly Func<LayoutInflater, ViewGroup, Bundle, View> view;

	public RecyclerViewFragment(Func<LayoutInflater, ViewGroup, Bundle, View> view)
	{
		this.view = view;
	}

	public override void OnCreate(Bundle savedInstanceState)
	{
		base.OnCreate(savedInstanceState);
	}

	public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
	{
		base.OnCreateView(inflater, container, savedInstanceState);

		return view(inflater, container, savedInstanceState);
	}
}
```

**FragmentPagerAdapter** реализует **PagerAdapter**, представляет каждую страницу как фрагмент, который постоянно хранится в менеджере фрагментов, пока пользователь может вернуться на страницу. В этом примере **FragmentPagerAdapter** будет содержать объекты RecyclerView.

Метод `addFragmentView` будет добавлять фрагменты в поле `fragmentList` нашей Activity.
```csharp
public class RecyclerViewFragmentPagerAdapter : FragmentPagerAdapter
{
	readonly List<Fragment> fragmentList = new List<Fragment>();
	readonly ICharSequence[] titles;

	public RecyclerViewFragmentPagerAdapter(FragmentManager fragmentManager, ICharSequence[] titles) : base(fragmentManager)
	{
		this.titles = titles;
	}

	public override int Count => fragmentList.Count;

	public override Fragment GetItem(int position)
	{
		return fragmentList[position];
	}

	public override ICharSequence GetPageTitleFormatted(int position)
	{
		return titles[position];
	}

	public void addFragmentView(Func<LayoutInflater, ViewGroup, Bundle, View> fragmentView)
	{
		fragmentList.Add(new RecyclerViewFragment(fragmentView));
	}
}
```

RecyclerView нуждается в некоторой модели данных для каждого элемента. В этом примере есть два свойства для TextView и ImageView.

```csharp
public class RecyclerViewDataModel
{
	public string someString { get; set; }
	public string imageUrl { get; set; }
}
```

Как и в случае с ListView, для вывода сложных объектов в RecyclerView необходимо определить свой адаптер. Поэтому определим собственный класс `RecyclerViewAdapter`, который должен наследоваться от абстрактного класса *RecyclerView.Adapter*.

Этот пример адаптера имеет возможность обработки клика по элементу.

```csharp
public class RecyclerViewAdapter : RecyclerView.Adapter
{
	public List<RecyclerViewDataModel> dataModelList;
	public Context context;
	public event EventHandler<int> eventHandler;

	public RecyclerViewAdapter(List<RecyclerViewDataModel> dataModelList, Context context)
	{
		this.dataModelList = dataModelList;
		this.context = context;
	}

	public override int ItemCount => dataModelList.Count;

	public override void OnBindViewHolder(RecyclerView.ViewHolder holder, int position)
	{
		var item = dataModelList[position];
		var viewHolder = holder as RecyclerViewHolder;

		if (holder == viewHolder && viewHolder != null)
		{
			viewHolder.textView.Text = item.someString;

			Glide.With(context).Load(item.imageUrl).Into(viewHolder.imageView);
		}
	}

	public override RecyclerView.ViewHolder OnCreateViewHolder(ViewGroup parent, int viewType)
	{
		var view = LayoutInflater.From(parent.Context).Inflate(Resource.Layout.RecyclerViewItemLayout, parent, false);
		var viewHolder = new RecyclerViewHolder(view, clickEvent);
		return viewHolder;
	}

	public void clickEvent(int position)
	{
		if (eventHandler != null)
			eventHandler(this, position);
	}
}

public class RecyclerViewHolder : RecyclerView.ViewHolder
{
	public View view { get; set; }
	public ImageView imageView { get; set; }
	public TextView textView { get; set; }

	public RecyclerViewHolder(View view, Action<int> eventHandler) : base(view)
	{
		this.view = view;
		imageView = view.FindViewById<ImageView>(Resource.Id.imageView);
		textView = view.FindViewById<TextView>(Resource.Id.textView);
		view.Click += (sender, e) => eventHandler(AdapterPosition);
	}

}
```

Так же, не забудем добавить определение следующих переменных:

```csharp
[Activity(Theme = "@style/MaterialTheme", MainLauncher = true, Icon = "@mipmap/icon")]
public class MainActivity : AppCompatActivity
{
	Android.Support.V7.Widget.Toolbar toolbar;
	ViewPager viewPager;
	TabLayout tabLayout;
	RecyclerView recyclerView;
	RecyclerViewAdapter recyclerViewAdapter;
	RecyclerViewFragmentPagerAdapter recyclerViewFragmentPagerAdapter;
	ICharSequence[] titles;
	LinearLayoutManager linearLayoutManager;
	List<RecyclerViewDataModel> dataModelList;

	// . . .
}
```
Переменные должны быть инициализированы в методе `OnCreate`

```csharp
toolbar = FindViewById<Android.Support.V7.Widget.Toolbar>(Resource.Id.toolbar);
SetSupportActionBar(toolbar);
SupportActionBar.SetDisplayHomeAsUpEnabled(false);

tabLayout = FindViewById<TabLayout>(Resource.Id.tabLayout);
viewPager = FindViewById<ViewPager>(Resource.Id.viewPager);

dataModelList = new List<RecyclerViewDataModel>();

loadData();

viewPager.Adapter = recyclerViewFragmentPagerAdapter;
tabLayout.SetupWithViewPager(viewPager);
```
Метод `loadData` заполняет dataModelList и возвращает фрагмент для каждого элемента из dataModelList

```csharp
void loadData()
{
	for (int i = 0; i <= 7; i++)
	{
		dataModelList.Add(new RecyclerViewDataModel
		{
			someString = "SomeString " + i,
			imageUrl = "https://blog.xamarin.com/wp-content/uploads/2015/03/RDXWoY7W_400x400.png"
		});
	}

	var sArray = dataModelList.Select(x => x.someString).ToArray();

	titles = CharSequence.ArrayFromStringArray(sArray);

	recyclerViewFragmentPagerAdapter = new RecyclerViewFragmentPagerAdapter(SupportFragmentManager, titles);

	foreach (var item in dataModelList)
	{
		createFragment(dataModelList);
	}
}
```

Метод `createFragment` вернет RecyclerView, после настройки адаптера, менеджера макетов и обработчика событий.

```csharp
void createFragment(List<RecyclerViewDataModel> list)
{
	recyclerViewFragmentPagerAdapter.addFragmentView((arg1, arg2, arg3) =>
	{
		var view = arg1.Inflate(Resource.Layout.RecyclerViewLayout, arg2, false);

		recyclerView = view.FindViewById<RecyclerView>(Resource.Id.recyclerView);
		linearLayoutManager = new LinearLayoutManager(this, LinearLayoutManager.Vertical, false);
		recyclerViewAdapter = new RecyclerViewAdapter(list, this);

		recyclerView.SetLayoutManager(linearLayoutManager);
		recyclerView.SetAdapter(recyclerViewAdapter);

		recyclerViewAdapter.NotifyDataSetChanged();

		recyclerViewAdapter.eventHandler += (sender, e) =>
		{
			// Доступ к данным выбранного элемента
			var item = list[e];
			Console.WriteLine(item.someString);
		};

		return view;

	});
}
```

## Результат

![Результат](http://code2xamarin.net/content/images/2016/11/YdT0kygyYS.gif)

[Исходный код проекта на GitHub](https://github.com/beraybentesen/Xamarin.MultipleRecyclerView)

