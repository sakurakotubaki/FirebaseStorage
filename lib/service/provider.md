```dart
import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:firebase_storage/firebase_storage.dart';
import 'package:hooks_riverpod/hooks_riverpod.dart';
// FireStore用のProvider
final firebaseStoreProvider =
    Provider<FirebaseFirestore>((ref) => FirebaseFirestore.instance);
// FirebaseStorage用のProvider
final firebaseStorageProvider =
    Provider<FirebaseStorage>((ref) => FirebaseStorage.instance);
// FireStoreの値を画面に描画するProvider
final listProvider = StreamProvider.autoDispose((ref) {
  return ref.watch(firebaseStoreProvider).collection('profile').snapshots();
});
```
