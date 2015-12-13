---
layout: post
title: Реализация OnBoarding процесса в приложении Xamarin.iOS
date: 2015-12-13 19:10
tags:
- xamarin
- ios
- перевод
---

## Требование

Недавно мне пришлось реализовывать процесс адаптации в iOS приложении JustGiving, который состоит из серии экранов, по которым можно преходить вперед и назад.

![Как это выглядит](https://raw.githubusercontent.com/wcoder/blog/master/2015-12-13-xamarin-ios/onboarding-ios.png)

## Решение

Я использовал `UIPageViewController` с `UIPageViewControllerDataSource` для навигации шагов и `UIPageControl` для отображения процесса.

## Результат

<video controls preload="false" width="500">
   <source src="http://blog.lombaard.co.uk/video/onboarding.mp4" type="video/mp4">
</video>

## Шаги

Каждый шаг это `UIViewController`, который реализует следующий интерфейс.

``` csharp
public interface IMultiStepProcessStep : IDisposable
{
	int StepIndex { get; set; }
	event EventHandler<MultiStepProcessStepEventArgs> StepActivated;
	event EventHandler<MultiStepProcessStepEventArgs> StepDeactivated;
}

public class MultiStepProcessStepEventArgs
{
	public int Index { get; set; }
}
```

## Шаг UIViewController

Шаг публикует свой индекс при активации или деактивации, как активный шаг. Это делают методы `ViewDidAppear` и `ViewWillDisappear`.

```csharp
public class MakeGoodthingsHappenStep : UIViewController, IMultiStepProcessStep
{
	public override void ViewDidAppear(bool animated)
	{
	    base.ViewDidAppear(animated);
	    StepActivated?.Invoke(this, new MultiStepProcessStepEventArgs { Index = StepIndex });
	}
	
	public override void ViewWillDisappear(bool animated)
	{
	    base.ViewWillDisappear(animated);
	    StepDeactivated?.Invoke(this, new MultiStepProcessStepEventArgs { Index = StepIndex });
	}
	
	public int StepIndex { get; set; }
	public event EventHandler<MultiStepProcessStepEventArgs> StepActivated;
	public event EventHandler<MultiStepProcessStepEventArgs> StepDeactivated;
}
```

## UIPageViewControllerDataSource

Источник данных представляет собой `UIPageViewControllerDataSource`, который строится из списка шагов `IMultiStepProcessStep`.

```csharp
public class MultiStepProcessDataSource : UIPageViewControllerDataSource
{
	private readonly List<IMultiStepProcessStep> _steps;
	
	public MultiStepProcessDataSource(List<IMultiStepProcessStep> steps)
	{
		if (steps == null)
		{
			throw new ArgumentNullException(nameof(steps));
		}
		if (!steps.Any())
		{
		    throw new ArgumentException("steps cannot be empty.", nameof(steps));
		}
		if (steps.Any(s => !(s is UIViewController)))
		{
		    throw new ArgumentException("all steps must be a UIViewController", nameof(steps));
		}
		
		_steps = steps;
		
		for (int i = 0; i < _steps.Count; i++)
		{
			var step = _steps[i];
			step.StepIndex = i;
		}
	}
	
	public List<IMultiStepProcessStep> Steps => _steps;
	
	public override UIViewController GetPreviousViewController(UIPageViewController pageViewController,
		UIViewController referenceViewController)
	{
		var step = referenceViewController as IMultiStepProcessStep;
		if (step == null)
		{
			return null;
		}
		
		var index = _steps.IndexOf(step);
		if (index <= 0)
		{
			return null;
		}
		
		return   _steps[index - 1] as UIViewController;
	}
	
	public override UIViewController GetNextViewController(UIPageViewController pageViewController, 
	                                                       UIViewController referenceViewController)
	{
		var step = referenceViewController as IMultiStepProcessStep;
		if (step == null)
		{
			return null;
		}
		var index = _steps.IndexOf(step);
		if (index + 1 == _steps.Count)
		{
			return null;
		}
		
		return _steps[(step.StepIndex + 1)] as UIViewController;
	}
}   
```

## UIPageViewController

`UIPageViewController` построен из источника данных.

```csharp
public sealed class MultiStepProcessHorizontal : UIPageViewController
{
	public MultiStepProcessHorizontal(MultiStepProcessDataSource dataSource) 
	    :base(UIPageViewControllerTransitionStyle.Scroll, 
	         UIPageViewControllerNavigationOrientation.Horizontal)
	{
		DataSource = dataSource;
		SetViewControllers(new[] {dataSource.Steps.FirstOrDefault() as UIViewController}, 
		                  UIPageViewControllerNavigationDirection.Forward, 
		                  false, 
		                  null);
	}
} 
```

## UIPageControl

`UIPageControl` используется, чтобы указать, какой шаг активен.

![Индикатор](https://raw.githubusercontent.com/wcoder/blog/master/2015-12-13-xamarin-ios/indicator.png)

## Собираем все вместе в OnBoardingViewController

Обработчики событий для случаев, когда шаг активируется и деактивируется используются, чтобы установить индекс текущей страницы и обновить любые другие части пользовательского интерфейса если это будет нужно.

```csharp
private void HandleStepActivated(object sender, MultiStepProcessStepEventArgs args)
{
	_pageControl.CurrentPage =  args.Index;
}

private void HandleStepDeactivated(object sender, MultiStepProcessStepEventArgs args)
{
	//update the UI as required while transitioning between steps
}
```

Получаем все шаги, которые будут являться частью процесса адаптации и навешиваем обравотчики на события `StepActivated` и `StepDeactivated`.

```csharp
private List<IMultiStepProcessStep> GetSteps()
{
    var steps = new List<IMultiStepProcessStep>()
    {
        new MakeGoodthingsHappenStep(),
        new FundraiseStep(),
        new ConnectStep(),
        new DiscoverStep(),
        new GetStartedStep()
    };
    
    steps.ForEach(s => 
    {
        s.StepActivated += HandleStepActivated;
        s.StepDeactivated += HandleStepDeactivated;
    });
    
    return steps;
}
```

Настраиваем и добавляем элементы управления `UIPageViewController` и `UIPageControl` к основному представлению.

```csharp
private MultiStepProcessHorizontal _pageViewController;
private UIPageControl _pageControl;

private List<IMultiStepProcessStep> _steps;
public List<IMultiStepProcessStep> Steps => _steps ?? (_steps = GetSteps());

public override void LoadView()
{
    View = new UIView();
    
    _pageViewController = new MultiStepProcessHorizontal(new MultiStepProcessDataSource(Steps));
    
    _pageControl = new UIPageControl
    {
        CurrentPage = 0,
        Pages = Steps.Count
    };
        
    View.Add(_pageViewController.View);
    View.Add(_pageControl);
}
```

На этом все!

[Оригинал](http://blog.lombaard.co.uk/blog/2015/10/27/xamarin-ios-swipable-multiple-step-process-used-for-onboarding/)












