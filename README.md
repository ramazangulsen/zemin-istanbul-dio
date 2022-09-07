
# Flutter'da Dio'nun Etkili Kullanımı

Dio Metotları ve Dio Interceptor sınıfının kullanımı.

![Screenshot](https://storage.cloud.google.com/githubzeministanbul/Ekran%20Resmi%202022-09-07%2023.47.44.png](https://00f74ba44b62912ba01dccabeeefe9d8533b370f12-apidata.googleusercontent.com/download/storage/v1/b/githubzeministanbul/o/Ekran%20Resmi%202022-09-07%2023.47.44.png?jk=AFshE3WhEsDeI8wvKBc3L6Vx8KLDDxKnnaMTDnCYnbXNZYFbImLLyw-n19-4B9eAKB--XCchecsN6vvksGQsZfWNKOUOrrcSaOuZ6sVGW3M2TEnqc2HYHiC7ZT_d4SXiiSkImCGKI0lgoVjDeLgDvejI7FXrqpjMYuRGGXg7I4WtpRj5KPBpwue0fPl3BT6JEceCFc_sRaPg7AhOrrNVWz3_0o98oT_MYcEAFWFLbXuLI_1ZjOe_jotx5Z-eP6FmJR02ZEbtF03YAHEijklbz3P3DWpmCgbSWWmvYdGDkbJAYBE7zpRhw3bVc2ZAXLYcI3PqBQUPuwpEK3_jjf7ylupCH93NU_kHfeMag5_T7qOVTvJW3Xsl8Ag6V6JQZ9tBPd3lipCoGb6umvIRgIRPj8bdMBJhB0eIbP0j_MsztOemmcKGfg6xLLiLuS-dz7eRCta-gi-VuRGfYJPC79QBY2fgPClzrgtBoj14kTn6I_JiZBk0tmNJJkDGMEA-WThiVDYADJIyBntXQtEaYZ6bYCcH8acSOW6al7uwYVfeHyovq1vKeZsCJe2pzKGB-lL4HepbNYplyEp57qZHppJo2JVhp5tjCRe_a8LW_q3K_3rR4OpEAHHUdyBX4TsVHFtPDiCwq1DszJGGS5ZZ561tQiK5JjXUc7CxAkDxICKlp-MuphJbmXtiBsYiwgUhuwEmkhwy8UVvPl0Q_YLY0V46isA2RhDXrH7LCcQs5Xe46X9rJVasRwlHWpiCsYY_0DlOHHLiItnTsREoZi2uBY-uOOwbyNx1O4yRF47DcqwxgMwsLaCHrQRZ-FcwpnrWxjTTeksnfzUvS6ztOmagQtAXNBAVDSdkI7WDdRHpFW5UNV_KLN509TuzaaqMY9zXeq6ChAVjFZ4z-3vC3C1cUAWRzQFECYRIXCbAAryTPS_yemkEGE8SV9vQN4eJb13uaafkyR9rweIsKOPY2FvNEBv0WC7YXxqQhN14JoH3SrH5-MRHiJLFAesj0ygHV_yJvhWtBjzdqts_cVj1u-gY_vutPo7Fvw&isca=1)


![Screenshot](https://storage.cloud.google.com/githubzeministanbul/Ekran%20Resmi%202022-09-07%2023.48.04.png)

![Screenshot](https://storage.cloud.google.com/githubzeministanbul/Ekran%20Resmi%202022-09-07%2023.48.21.png)


## İçerik


- Paket Kurulumu
- API Kaynağı
- Dio Get Metotu
- Dio Post Metotu
- Dio Put Metotu
- Dio Delete Metotu
- Dio Error Handling
- Dio Interceptors Sınıfı
- Kaynaklar

## Paket Kurulumu

```
dependencies:
  dio: ^4.0.6
```

- Projeye import edilmesi

```
import 'package:dio/dio.dart';

```
  

## API Kaynağı

#### Gerekli Dio işlemleri için kullanılan API kaynağı.



```http
  https://jsonplaceholder.typicode.com/
```





  

## Dio Get Metotu

- Get Metotu okuma işlemleri için kullanılır. Yapılan istek sonunda bize dönen verileri baz alarak gerekli widget çizimlerini gerçekleştiririz.


  


```bash
  Future<Post> fetchPost (int postId) async {
 try {
  final response = await dio.get(postsEndpoint + "/$postId");
  debugPrint(response.toString());
  return Post.fromJson(response.data);
 } on DioError catch (e){
  debugPrint("Status code ${e.response?.statusCode.toString()}");
  throw Exception("Failed to load post : $postId");
 }
}
```

  

Yapılan Get isteği sonrasında alınan datalar 'fromJson' metotu ile Dart programlama dilinin widgetları doğru şekilde oluşturulabilmesi için tanımlandı

## Dio Post Metotu

- Post Metotu oluşturm işlemleri için kullanılır. Gerekli endpointe yapılan istek sonucunda gönderilen veriler veritabanında oluşturulur ve saklanır.




```
  Future<Post> createPost(int userId, String title, String body) async {
  try{
    final response = await dio.post(postsEndpoint,data: {
      "userId": userId,
      "title": title,
      "body": body,
    },);
    debugPrint(response.toString());
    return Post.fromJson(response.data);
  } on DioError catch (e) {
    debugPrint("Status code ${e.response?.statusCode.toString()}");
    throw Exception("Failed to create Post");
  }
}
  
  
```

Json formatında gönderilen veriler uygun endpointe istekte bulunarak gerekli veri oluşturma işlemlerini tamamladık.




## Dio Put Metotu

- Put Metotu veri güncelleme işlemleri için kullanılır. Güncellenmesi istenen veriler veritabanında mevcut değilse put metotu çalıştığında o veriler oluşturulur ve işlem tamamlanır




```
  Future<Post> updatePost(
  int postId, int userId, String title, String body
) async {
  try{
    final response = await dio.put(postsEndpoint + "/$postId",data: {
      "userId": userId,
      "title": title,
      "body": body,
    },);
    debugPrint(response.toString());
    return Post.fromJson(response.data);
  } on DioError catch (e) {
    debugPrint('Status code: ${e.response?.statusCode.toString()}');
  throw Exception("Failed to update Post");
  }
}
  
  
```

Kullanıcı arayüzünden güncellenmesi istenen 'Post'un verisi alınır ve put işlemi endpointine gönderilir. Geçerli Post'un userId, title, body parametreleri güncellenerek işlem tamamlanır.



## Dio Delete Metotu

- Delete Metotu veri silme işlemleri için kullanılır. 




```
  Future<void> deletePost (int postId) async {
  try {
    await dio.delete(postsEndpoint+ "/$postId");
    debugPrint("Delete success");
  } on DioError catch (e){
    debugPrint("Status code: ${e.response?.statusCode.toString()}");
    throw Exception("Failed to delete post : $postId");
  }
}
```

Kullanıcı arayüzünden silinmesi istenen 'Post'un verisi alınır ve delete işlemi endpointine gönderilir. 

## Dio Error Handling

- Dio paketi sayesinde istek sırasında yaşanan hataları kolay şekilde yakalayabiliriz. 
- Try-catch kullanarak istekler yapılır. Try bölümünde yapılacak istekler ve isteğin sonucu alınır. Try bloğunda yaşanan hatalar catch bloğunda yakalanır. Bu durumda Dio bize DioError sınıfını kullanmamıza olanak sağlar. Bu sayede alınan hatanın detaylarını alarak bunu kullanıcı arayüzüne bildiriziz

```
try {
    await dio.delete(postsEndpoint+ "/$postId");
    debugPrint("Delete success");
  } on DioError catch (e){
    debugPrint("Status code: ${e.response?.statusCode.toString()}");
    throw Exception("Failed to delete post : $postId");
  }
```


## Dio Interceptors

- Bazı durumlarda sunucuya istek gönderilirken daha kompleks özelliklere sahip olmak isteriz. Örneğin header'da bulunan veriler dinamik olarak değişebilir. Sunucuya attığımız bir istekten hemen sonra hazırda bulunan 'token'i yenilemek isteyebiliriz. Ya da istek gönderildikten sonra sunucu tarafından gelen hatanın çeşitlerine göre farklı işlemler yapmak isteyebiliriz. Dio paketi bu tür kompleks işlemleri halledebilmemiz için bir sınıf oluşturur. Bu sınıfın adı Interceptor'dır.

```
dynamic responseInterceptor(Response options) async {
  if (options.headers.value("verifyToken") != null) {
    //Local DB'den aktif tokenin alınması
    SharedPreferences prefs = await SharedPreferences.getInstance();
    var verifyToken = prefs.get("VerifyToken");
    
    // header'da gönderilen token ile Local DB de bulunan tokenin karşılaştırılması
    if (options.headers.value("verifyToken") == verifyToken) {
      return options;
    }
  }
// Token güncel değilse DioError ile yakalanması.
  return DioError(request: options.request, message: "User is no longer active");
}

``` 

- Repo'da bulunan uygulamada bir senaryo oluşturuldu. Senaryoya göre bir istek atılmadan önce geçerli token'in refresh edilmesi istendi ve bu token header'da gönderildi.

```
if(options.headers["refreshToken"] == null) {
      DioClient.dio.lock();
      Dio _tokenDio = Dio();
      _tokenDio.get('/token').then((value) {
        options.headers['refreshToken'] = value.data['data']['token'];
        handler.next(options);
      }).catchError((error, stackTrace){
        handler.reject(error,true);
      }).whenComplete(() {
        DioClient.dio.unlock();
      });
    }
```

- Token refresh edilirken Dio paketi sayesinde lock ve unlock işlemlerini kolayca yapabildik.
Yapılan isteğin sonucunda bir hata oluşursa Interceptor sınıfı ile birlikte bu hatanın çeşitini elde edip istenen işlemleri kolay bir şekilde ayırt edebiliriz.

```
switch(err.type){
    case DioErrorType.connectTimeout: {

    }
    break;
    case DioErrorType.receiveTimeout:{
        
    }
    break;
    case DioErrorType.sendTimeout:{

    }
    break;
    case DioErrorType.cancel:{

    }
    break;
    case DioErrorType.response:{

    }
    break;
    case DioErrorType.other:{

    }
    break;
  }
```
## Kaynakça

https://pub.dev/packages/dio

