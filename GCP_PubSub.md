# GCP PubSub

## Using GCP console

### 1. Login GCP console

Choose the right GCP project

Go to Pub/Sub -> Topics

### 2. Create a Topic and a Subscription

![1686398149227](image/GCP_PubSub/1686398149227.png)

![1686398275859](image/GCP_PubSub/1686398275859.png)

![1686398302832](image/GCP_PubSub/1686398302832.png)

Topic: activity-log-test

Subscription: activity-log-test-sub

Details:

![1686398347407](image/GCP_PubSub/1686398347407.png)

### 3. Verify with a test message

![1686398429582](image/GCP_PubSub/1686398429582.png)

![1686398477810](image/GCP_PubSub/1686398477810.png)

## Using GCP CLI

### 1. gcloud config

```dos
gcloud auth login
gcloud config set project YOUR_PROJECT_ID
gcloud pubsub topics describe YOUR_TOPIC_ID
gcloud pubsub subscriptions describe YOUR_SUBSCRIPTION_ID
```

### 2. Publish messages

```dos
C:\tmp>gcloud pubsub topics publish activity-log-test --message="Hello World!" --attribute=KEY1=VAL1,KEY2=VAL2
messageIds:
- '8353411737264052'

C:\tmp>gcloud pubsub topics publish activity-log-test --message="{'id': 123}"   
messageIds:
- '8353411737292502'

C:\tmp>gcloud pubsub topics publish activity-log-test --message="{'id': 123, 'name': 'John Doe'}"
messageIds:
- '8353411737556015'

C:\tmp>gcloud pubsub topics publish activity-log-test --message="{'id': 123, 'name': 'John Doe', \"email\": \"johndoe@example.com\"}"
messageIds:
- '6904971219155605'
```

### 3. Pull the messages