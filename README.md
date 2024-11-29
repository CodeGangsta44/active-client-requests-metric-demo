### Demo project for `http_client_requests_active_seconds_count` metric issue

#### Steps to reproduce:
1. Launch wiremock container with pre-configured long-responding endpoint:

```shell
docker run -it --rm \
    -p 8080:8080 \
    --name wiremock \
    -v ./wiremock:/home/wiremock \
    wiremock/wiremock:3.9.1
```

2. Build and launch the app:
```shell
./gradlew bootRun
```

3. Execute cURL request with timeout to simulate request cancellation:
```shell
curl --location --request POST 'http://localhost:8876/test' -m 1
```

4. Query metrics and observe http_client_requests_active_seconds_count having value > 0:
```shell
curl --location 'http://localhost:8876/management/prometheus' | grep 'http_client_requests_active_seconds_count'
```