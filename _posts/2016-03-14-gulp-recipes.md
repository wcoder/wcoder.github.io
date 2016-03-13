---
layout: post
title: Gulp сниппеты
date: 2016-03-14 01:00
original_url:
tags:
- gulp
- сниппет
- перевод
---

Это небольшая коллекция [Gulp](http://gulpjs.com/) сниппетов для различных целей.

## ES6 Babel

Компилирование JavaScript ES6 в ES5 с включенными sourcemaps. Пример взят с [сайта babel](https://babeljs.io/docs/setup/#gulp).

```
npm install gulp gulp-babel gulp-sourcemaps babel-preset-2015
```

```js
var gulp = require("gulp");
var babel = require("gulp-babel");
var sourcemaps = require("gulp-sourcemaps");

gulp.task("default", function () {
	return gulp.src("js/*.js")
		.pipe(sourcemaps.init())
		.pipe(babel({
			presets: ['es2015']
		}))
		.pipe(sourcemaps.write("."))
		.pipe(gulp.dest("dist"));
});
```

## Конвертирование изображений

Конвертация всех изображений из директории `images` в другой формат.

```
npm install gulp gulp-gm
```

```js
var gulp = require('gulp');
var gm = require('gulp-gm');

gulp.task('default', function () {
	gulp.src('./images/*.png')
		.pipe(gm(function (gmfile) {
			return gmfile.setFormat('jpg').quality(90);
		}))
		.pipe(gulp.dest('dist'));
});
```

## Изменение размеров изображения

В этом примере мы изменим размер изображений из Instagram (1080), чтобы вашему телефону не приходилось их масштабировать.

```
npm install gulp gulp-gm
```

```js
var gulp = require('gulp');
var gm = require('gulp-gm');

gulp.task('default', function () {
	gulp.src('images/*')
		.pipe(gm(function (gmfile) {
			gmfile.setFormat('jpg').quality(100);
			return gmfile.resize(1080, 1080);
		}))
		.pipe(gulp.dest('dist'));
});
```

## JShint

Проверка JS файлов с помощью [jshint](http://jshint.com/) и Gulp.

```
npm install jshint gulp-jshint gulp
```

```js
var gulp   = require('gulp');
var jshint = require('gulp-jshint');

gulp.task('jshint', function() {
	return gulp.src('./js/*.js')
		.pipe(jshint())
		.pipe(jshint.reporter('default'));
});

gulp.task('default',function() {
	gulp.start('jshint');
	gulp.watch('./js/*.js',['jshint']);
});
```

## Цепочки задач

Для создания цепочки задач, которые выполняются друг после друга, просто определить их в качестве зависимостей.

```js
var gulp = require('gulp');
var fs = require('fs');

gulp.task('one', function (cb) {
	fs.writeFile('one.txt', 'Gulp stuff in series!', function (err) {
		// если callback получают ошибку, вся цепочка останавливится
		cb(err);
	});
});

// задача 2 выполнится после задачи 1
gulp.task('two', ['one'], function() {
	console.log('задача запущена после задачи 1')
});

// определяет несколько зависимостей через ['one', 'two']
gulp.task('default', ['one', 'two']);
```

## Less

Компиляция [Less](http://lesscss.org/) и наблюдение за изменениями.

```
npm install gulp gulp-less
```

```js
var gulp = require('gulp');
var sass = require('gulp-less');

gulp.task('styles', function() {
	gulp.src('./scss/test.less')
		.pipe(gulp.dest('./css'))
});

gulp.task('default',function() {
	gulp.start('styles');
	gulp.watch('./less/*.less',['styles']);
});
```

## Sass

Компиляция [Sass](http://sass-lang.com/) и наблюдение за изменениями.

```
npm install gulp gulp-sass
```

```js
var gulp = require('gulp');
var sass = require('gulp-sass');

gulp.task('styles', function() {
	gulp.src('./scss/style.scss')
		.pipe(sass().on('error', sass.logError))
		.pipe(gulp.dest('./css'))
});

gulp.task('default',function() {
	gulp.start('styles');
	gulp.watch('./scss/*.scss',['styles']);
});
```
