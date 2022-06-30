# cloudinary-flutter-image-upload

##Dependencies cloudinary_public, image_picker


```
Future<void> imageUploadUsingCloudinaryPublic() async {
    final ImagePicker picker = ImagePicker();
    final XFile? pickedImage = await picker.pickImage(
      source: ImageSource.gallery,
    );
    if (kIsWeb) {
      final image = Image.network(pickedImage!.path);
      print("image is $image");
    } else {
      //TODO:Image picker for android and ios
      // Image.file(File())
    }
    final cloudinary = CloudinaryPublic(
      'Your Upload preset here',
      'Cloud Name',
      cache: false,
    );
    try {
      CloudinaryResponse response = await cloudinary.uploadFile(
        CloudinaryFile.fromFile(
          pickedImage!.path,
          resourceType: CloudinaryResourceType.Image,
        ),
      );

      print("cloudinary success${response.secureUrl}");
    } on CloudinaryException catch (e) {
      print("cloudinary failure ${e.message}");
      print("cloudinary failure ${e.request}");
    }
  }
```

##Dependencies cloudinary_sdk, image_picker

```
Future<void> imageUploadUsingCloudinarySdk() async {
    final ImagePicker picker = ImagePicker();
              final XFile? pickedImage = await picker.pickImage(
                source: ImageSource.gallery,
              );
              if (kIsWeb) {
                final image = Image.network(pickedImage!.path);
                print("image is $image");
              } else {
                //Image picker for android and ios
                // Image.file(File())
              }
              final cloudinary = Cloudinary.full(
                apiKey: "apiKey",
                apiSecret: "apiSecret",
                cloudName: "Cloud Name",
              );

              final response = await cloudinary.uploadResource(
                CloudinaryUploadResource(
                  filePath: pickedImage!.path,
                  fileBytes: await pickedImage.readAsBytes(),
                  resourceType: CloudinaryResourceType.image,
                  folder: "user_banners",
                  fileName: pickedImage.name,
                  progressCallback: (count, total) {
                    print(
                      'Uploading image from file with progress: $count/$total',
                    );
                  },
                ),
              );

              if (response.isSuccessful) {
                print('Get your image from with ${response.secureUrl}');
              }
      }
```
