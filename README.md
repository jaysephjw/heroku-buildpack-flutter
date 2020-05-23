# Heroku Buildpack: Flutter (_WIP_)

This is a Heroku buildpack for [Flutter for web][].

## Features

* Builds and runs a flutter web project on [Heroku](https://heroku.com)
* Installs the Flutter tool on the host machine
* Builds the project
* Hosts it with (TODO: webserver)

## Getting Started

(These instructions assume you have the
[heroku tools](https://toolbelt.heroku.com/) and
[git](http://git-scm.com/) installed, and that you have a heroku
account.)

Create a Heroku app, and specify both this buildpack and a
[URL that points to a compiled Flutter SDK][download].

Here is a set of commands that deploys a minimal HTTP server built with Dart
to Heroku:

```bash
$> git clone https://github.com/igrigorik/heroku-buildpack-dart.git
$> mkdir myfirstdartappforheroku
$> cp -R heroku-buildpack-dart/test-app/* myfirstdartappforheroku
$> cd myfirstdartappforheroku
$> git init
$> git add -A .
$> git commit -am "first commit"
$> heroku create myfirstdartappforheroku
$> heroku config:set DART_SDK_URL=<archive url>
$> heroku config:add BUILDPACK_URL=https://github.com/igrigorik/heroku-buildpack-dart.git
```

Push the app to Heroku. Learn more about [deploying to Heroku with git][deploy].

```bash
$> git push heroku master
```

You may need to scale to one web dyno (aka server):

```bash
$> heroku ps:scale web=1
```

Test your app! The URL is printed at the end of the `git push` step.

## Configuration

### Location of Dart SDK

You must specify a URL that points to a .zip that contains the Dart SDK.
Links to Dart SDKs built for Linux are [available][download].

```bash
$> heroku config:set DART_SDK_URL=<archive url>
```

### Specifying the script

Create the file `Procfile` in the root of your package to define the file to run when the application starts. We
recommend to put the server script into your application's `bin/` directory.

The sample app's `Procfile` looks like:

```
web: ./dart-sdk/bin/dart bin/basic_http_server.dart
```

### Specifying the pubspect.yaml

By default every pubspec.yaml will be processed.

You can define the environment variable PUBSPEC_PATH to specify which pubspec.yaml to process.

### Overriding the build command

By default `pub build` is launched after `pub get`, it can be useful to use
another command: for instance `pub build --mode=debug` or 
`/app/dart-sdk/bin/dart build.dart`:

```bash
$> heroku config:set DART_BUILD_CMD="/app/dart-sdk/bin/dart build.dart"
```

## Example 

See `test-app` directory for the world simplest Dart web app running on
Heroku.

## Learning more

Dart VM can access files, directories, sockets, HTTP, web sockets, SSL, and
more. See the [dart:io library][io] for core functionality.

Dart's package repository, [pub][], hosts lots of options for more
functionality, such as database drivers, HTTP server frameworks, templates,
and more.

## License

Original license / copyright - The MIT License - Copyright (c) 2012 Ilya Grigorik

[pub]: http://pub.dartlang.org
[flutter for web]: https://flutter.dev/web
[deploy]: https://devcenter.heroku.com/articles/git
[envcompile]: https://devcenter.heroku.com/articles/labs-user-env-compile
