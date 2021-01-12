# Google Cloud PubSub

## Install Client Library

```xml
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>com.google.cloud</groupId>
                <artifactId>libraries-bom</artifactId>
                <version>16.1.0</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <dependency>
        <groupId>com.google.cloud</groupId>
        <artifactId>google-cloud-pubsub</artifactId>
        <exclusions>
            <exclusion>
                <groupId>com.google.guava</groupId>
                <artifactId>guava-jdk5</artifactId>
            </exclusion>
        </exclusions>

    </dependency>
```

## Publish

```java
    TopicName topicName = TopicName.of(projectId, topicId);

    Publisher publisher = null;

    try {

      publisher = Publisher.newBuilder(topicName)
          .setCredentialsProvider(FixedCredentialsProvider.create(credentials))
          .build();

      String message = "Hello World!";
      ByteString data = ByteString.copyFromUtf8(message);
      PubsubMessage pubsubMessage = PubsubMessage.newBuilder().setData(data).build();

      ApiFuture<String> messageIdFuture = publisher.publish(pubsubMessage);
      String messageId = messageIdFuture.get();
      System.out.println("Published message ID : " + messageId);

    } finally {

      if (publisher != null) {
        publisher.shutdown();
        publisher.awaitTermination(1, TimeUnit.MINUTES);
      }

    }
```

## Subscription

```java
   ProjectSubscriptionName subscriptionName =
        ProjectSubscriptionName.of(projectId, subscriptionId);

    // Instantiate an asynchronous message receiver.
    MessageReceiver receiver =
        (PubsubMessage message, AckReplyConsumer consumer) -> {
          // Handle incoming message, then ack the received message.
          System.out.println("Id: " + message.getMessageId());
          System.out.println("Data: " + message.getData().toStringUtf8());
          consumer.ack();
        };

    Subscriber subscriber = null;
    try {
      subscriber = Subscriber.newBuilder(subscriptionName, receiver)
          .setCredentialsProvider(FixedCredentialsProvider.create(credentials))
          .build();

      // Start the subscriber.
      subscriber.startAsync().awaitRunning();
      System.out.printf("Listening for messages on %s:\n", subscriptionName.toString());
      // Allow the subscriber to run for 30s unless an unrecoverable error occurs.
      subscriber.awaitTerminated(30, TimeUnit.SECONDS);
    } catch (TimeoutException timeoutException) {
      // Shut down the subscriber after 30s. Stop receiving messages.
      subscriber.stopAsync();
    }
```

## Error

```java
Exception in thread "main" java.lang.NoSuchMethodError: com.google.common.base.Preconditions.checkArgument(ZLjava/lang/String;I)V
	at com.google.api.gax.grpc.InstantiatingGrpcChannelProvider$Builder.setPoolSize(InstantiatingGrpcChannelProvider.java:538)
	at com.google.api.gax.grpc.InstantiatingGrpcChannelProvider$Builder.setChannelsPerCpu(InstantiatingGrpcChannelProvider.java:557)
	at com.google.api.gax.grpc.InstantiatingGrpcChannelProvider$Builder.setChannelsPerCpu(InstantiatingGrpcChannelProvider.java:546)
	at com.google.cloud.pubsub.v1.Publisher$Builder.<init>(Publisher.java:697)
	at com.google.cloud.pubsub.v1.Publisher$Builder.<init>(Publisher.java:648)
	at com.google.cloud.pubsub.v1.Publisher.newBuilder(Publisher.java:644)
	at com.google.cloud.pubsub.v1.Publisher.newBuilder(Publisher.java:623)
	at com.netmarble.push.statistics.GooglePublisherTest.publisherExample(GooglePublisherTest.java:78)
	at com.netmarble.push.statistics.GooglePublisherTest.main(GooglePublisherTest.java:33)
```
- Google Pubsub Client에서 guava-jdk5를 사용하기 때문에 발생한 충돌
- 다음의 dependency 추가

```xml
<dependency>
    <groupId>com.google.guava</groupId>
    <artifactId>guava-jdk5</artifactId>
    <version>17.0</version>
</dependency>
```