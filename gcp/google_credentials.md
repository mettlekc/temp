# 서비스 계정으로 인증

```java
  // You can specify a credential file by providing a path to GoogleCredentials.
  // Otherwise credentials are read from the GOOGLE_APPLICATION_CREDENTIALS environment variable.
  GoogleCredentials credentials = GoogleCredentials.fromStream(new FileInputStream(jsonPath))
        .createScoped(Lists.newArrayList("https://www.googleapis.com/auth/cloud-platform"));
```