```dart
import 'dart:io';
import 'package:hooks_riverpod/hooks_riverpod.dart';
import 'package:image_picker/image_picker.dart';
import 'package:profile/service/provider.dart';

final appStateProvider = StateNotifierProvider<AppState, dynamic>((ref) {
  // Riverpod2.0はここの引数にrefを書かなければエラーになる!
  return AppState(ref);
});

class AppState extends StateNotifier<dynamic> {
  final Ref _ref;
  AppState(this._ref) : super([]);

  Future<void> fileUpload() async {
    // imagePickerで画像を選択する
    final pickerFile =
        await ImagePicker().pickImage(source: ImageSource.gallery);
    if (pickerFile == null) {
      return;
    }
    File file = File(pickerFile.path);
    try {
      final url = "images/upload-pic.png";
      final snapshot = await _ref.read(firebaseStorageProvider).ref(url);
      snapshot.putFile(file);
      final imageUrl = await snapshot.storage.ref(url).getDownloadURL();
      print(imageUrl);
    } catch (e) {
      print(e);
    }
  }

  Future<void> sendImage() async {
    final imageRef = _ref
        .read(firebaseStorageProvider)
        .ref()
        .child("images")
        .child("upload-pic.png");
    String imageUrl = await imageRef.getDownloadURL();
    Map<String, dynamic> data = {
      "name": "cat33",
      "image": imageUrl,
    };
    final _reference = _ref.read(firebaseStoreProvider).collection('profile');
    _reference.add(data);
  }
}
```
