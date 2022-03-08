##本地发布模板说明

```
name: test_flutter_package
description: A new Flutter package.
version: 0.0.1
homepage: https://xxxxxxx/
publish_to: http://192.168.100.233:8080

```
export https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890 all_proxy=socks5://127.0.0.1:7890

flutter packages pub publish --dry-run

flutter packages pub publish --server=http://192.168.100.233:8080



##使用
```
environment: 
  sdk: >=2.15.0 < 3.0.0

dependencies:
  transmogrify:
    hosted: http://192.168.100.233:8080
    version: ^1.4.0

```



environment:
  sdk: ">=2.13.0 <3.0.0"
  flutter: ">=1.17.0"

# ARCHIVED

This repo has been archived, and is no longer maintained.

Issues and PRs will *not* be responded to.

Should there be community interest in alternate package servers for Dart,
we recommend these are handled as community projects.

## NOTE: This is package is an alpha version and is not recommended for production use.

Provides re-usable code for making a Dart package repository server.
The `package:pub_server/shelf_pubserver.dart` library provides a [shelf] HTTP
handler which provides the HTTP API used by the pub client.
One can use different backend implementations by implementing the
`PackageRepository` interface of the `package:pub_server/repository.dart`
library.

## Example pub repository server

An *experimental* pub server based on a file system can be found in
`example/example.dart`. It uses a filesystem-based `PackageRepository` for
storing packages and has a read-only fallback to the real `pub.dartlang.org`
site, if a package is not available locally. This allows one to use all 
`pub.dartlang.org` packages and have additional ones, on top of the publicly
available packages, available only locally.

It can be run as follows
```
~ $ git clone https://github.com/dart-lang/pub_server.git
~ $ cd pub_server
~/pub_server $ pub get
...
~/pub_server $ dart example/example.dart -d /tmp/package-db
Listening on http://localhost:8080

To make the pub client use this repository configure your shell via:

    $ export PUB_HOSTED_URL=http://localhost:8080
```

Using it for uploading new packages to the locally running server or downloading
packages locally available or via a fallback to `pub.dartlang.org` is as easy
as:

```
~/foobar $ export PUB_HOSTED_URL=http://localhost:8080
~/foobar $ pub get
...
~/foobar $ pub publish
Publishing x 0.1.0 to http://localhost:8080:
|-- ...
'-- pubspec.yaml

Looks great! Are you ready to upload your package (y/n)? y
Uploading...
Successfully uploaded package.
```

The fact that the `pub publish` command requires you to grant it oauth2 access -
which requires a Google account - is due to the fact that the `pub publish`
cannot work without authentication or with another authentication scheme.
*But the information sent by the pub client is not used for this local server
at the moment*.

[shelf]: https://pub.dartlang.org/packages/shelf
