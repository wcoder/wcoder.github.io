---
layout: post
title: Отслеживание угла наклона и силы нажания Apple Pencil с помощью Xamarin
date: 2016-03-05 23:10
original_url: http://www.knowing.net/index.php/2016/03/03/tracking-apple-pencil-angles-and-pressure-with-xamarin/
tags:
- xamarin
- F#
- ios
- перевод
---

Ходят слухи, что Apple будет поддерживать Apple Pencil в новых iPad. Если это так, то многие разработчики захотят использовать новые возможности `UITouch` - силу, угол, высоту - поддерживаются невероятно точным стилусом.

В основном это просто:

* сила - `UITouch.Force`
* угол - `UITouch.GetAzimuthAngle(UIView)`
* угол над горизонтом - `UITouch.AltitudeAngle`

Вот часть кода на F#:

```
namespace UITouch0

open System
open UIKit
open Foundation
open System.Drawing
open CoreGraphics

type ContentView(color : UIColor) as this =
   inherit UIView()
   do this.BackgroundColor <- color

   let MaxRadius = 200.0
   let MaxStrokeWidth = nfloat 10.0

   //Mutable!
   member val Circle : (CGPoint * nfloat * nfloat * nfloat ) option = None with get, set

   member this.DrawTouch (touch : UITouch) =
      let radius = (1.0 - (float touch.AltitudeAngle) / (Math.PI / 2.0)) * MaxRadius |> nfloat
      this.Circle <- Some (touch.LocationInView(this), radius, touch.GetAzimuthAngle(this), touch.Force)
      this.SetNeedsDisplay()


   override this.Draw rect =

      match this.Circle with
      | Some (location, radius, angle, force) ->
         let rectUL = new CGPoint(location.X - radius, location.Y - radius)
         let rectSize = new CGSize(radius * (nfloat 2.0), radius * (nfloat 2.0))
         use g = UIGraphics.GetCurrentContext()
         let strokeWidth = force * MaxStrokeWidth
         g.SetLineWidth(strokeWidth)
         let hue = angle / nfloat (Math.PI * 2.0)
         let color = UIColor.FromHSB(hue, nfloat 1.0, nfloat 1.0)
         g.SetStrokeColor(color.CGColor)
         g.AddEllipseInRect <| new CGRect(rectUL, rectSize)
         g.MoveTo (location.X, location.Y)
         let endX = location.X + nfloat (cos(float angle)) * radius
         let endY = location.Y + nfloat (sin(float angle)) * radius
         g.AddLineToPoint (endX, endY)
         g.StrokePath()
      | None -> ignore()

type SimpleController() =
   inherit UIViewController()
   override this.ViewDidLoad() =
      this.View <- new ContentView(UIColor.Blue)

   override this.TouchesBegan(touches, evt) =
     let cv = this.View :?> ContentView

     touches |> Seq.map (fun o -> o :?> UITouch) |> Seq.iter cv.DrawTouch

   override this.TouchesMoved(touches, evt) =
      let cv = this.View :?> ContentView
      touches |> Seq.map (fun o -> o :?> UITouch) |> Seq.iter cv.DrawTouch



[<Register("AppDelegate")>]
type AppDelegate() =
   inherit UIApplicationDelegate()
   let window = new UIWindow(UIScreen.MainScreen.Bounds)

   override this.FinishedLaunching(app, options) =
      let viewController = new SimpleController()
      viewController.Title <- "F# Rocks"
      let navController = new UINavigationController(viewController)
      window.RootViewController <- navController
      window.MakeKeyAndVisible()
      true


module Main =
   [<EntryPoint>]
   let main args =
      UIApplication.Main(args, null, "AppDelegate")
      0
```

И выглядит это так:

<video controls width="584" height="778" preload="none" controls="controls">
  <source src="https://github.com/wcoder/blog/raw/master/tracking-apple-pencil-angles-and-pressure-with-xamarin/ScreenFlow-2.mp4">
  Элемент video не поддерживается вашим браузером.
   <a href="https://github.com/wcoder/blog/raw/master/tracking-apple-pencil-angles-and-pressure-with-xamarin/ScreenFlow-2.mp4">Скачайте видео</a>.
</video>
