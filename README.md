# Pagination in Firestore



[![pub package](https://img.shields.io/pub/v/paginate_firestore.svg)](https://pub.dev/packages/paginate_firestore)
[![style: effective dart](https://img.shields.io/badge/style-effective_dart-40c4ff.svg)](https://github.com/tenhobi/effective_dart)
[![License: MIT](https://img.shields.io/badge/license-MIT-purple.svg)](https://opensource.org/licenses/MIT)


## Setup

Use the same setup used for `cloud_firestore` package (or follow [this](https://pub.dev/packages/cloud_firestore#setup)).

## Usage

In your pubspec.yaml

```yaml
dependencies:
  paginate_firestore: # latest version
```

Import it

```dart
import 'package:paginate_firestore/paginate_firestore.dart';
```

Implement it

```dart
      PaginateFirestore(
        //item builder type is compulsory.
        itemBuilder: (context, documentSnapshots, index) {
          final data = documentSnapshots[index].data() as Map?;
          return ListTile(
            leading: CircleAvatar(child: Icon(Icons.person)),
            title: data == null ? Text('Error in data') : Text(data['name']),
            subtitle: Text(documentSnapshots[index].id),
          );
        },
        // orderBy is compulsory to enable pagination
        query: FirebaseFirestore.instance.collection('users').orderBy('name'),
        //Change types accordingly
        itemBuilderType: PaginateBuilderType.listView,
        // to fetch real-time data
        isLive: true,
      ),
```

To use with listeners:

```dart
      PaginateRefreshedChangeListener refreshChangeListener = PaginateRefreshedChangeListener();

      RefreshIndicator(
        child: PaginateFirestore(
          itemBuilder: (context, documentSnapshots, index) => ListTile(
            leading: CircleAvatar(child: Icon(Icons.person)),
            title: Text(documentSnapshots[index].data()['name']),
            subtitle: Text(documentSnapshots[index].id),
          ),
          // orderBy is compulsary to enable pagination
          query: Firestore.instance.collection('users').orderBy('name'),
          listeners: [
            refreshChangeListener,
          ],
        ),
        onRefresh: () async {
          refreshChangeListener.refreshed = true;
        },
      )
```

## Getting Started

This project is a starting point for a Dart
[package](https://flutter.dev/developing-packages/),
a library module containing code that can be shared easily across
multiple Flutter or Dart projects.

For help getting started with Flutter, view our
[online documentation](https://flutter.dev/docs), which offers tutorials,
samples, guidance on mobile development, and a full API reference.

