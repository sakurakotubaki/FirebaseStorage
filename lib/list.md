```dart
import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:flutter/material.dart';
import 'package:hooks_riverpod/hooks_riverpod.dart';
import 'package:profile/service/provider.dart';

class ListPage extends HookConsumerWidget {
  const ListPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    // AsyncValueを使わないと、赤いエラーがでた!
    final AsyncValue<QuerySnapshot> list = ref.watch(listProvider);

    return Scaffold(
      appBar: AppBar(
        title: Text('ListPage'),
      ),
      body: list.when(
        // データがあった（データはqueryの中にある）
        data: (QuerySnapshot query) {
          // post内のドキュメントをリストで表示する
          return ListView(
            // post内のドキュメント１件ずつをCard枠を付けたListTileのListとしてListViewのchildrenとする
            children: query.docs.map((document) {
              return ListTile(
                // postで送った内容を表示する
                title: Text(document['name']),
                leading: Container(
                    height: 50,
                    width: 50,
                    // Image.networkで画像を画面に表示する
                    child: Image.network('${document['image']}')),
              );
            }).toList(),
          );
        },

        // データの読み込み中（FireStoreではあまり発生しない）
        loading: () {
          return const Text('Loading');
        },

        // エラー（例外発生）時
        error: (e, stackTrace) {
          return Text('error: $e');
        },
      ),
    );
  }
}
```
