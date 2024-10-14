# chargily-epay-springboot

Spring Boot Library for Chargily ePay Gateway

![Chargily ePay Gateway](https://raw.githubusercontent.com/Chargily/epay-gateway-php/main/assets/banner-1544x500.png "Chargily ePay Gateway")

# How to use

To use this library add the jar to your project libraries (it will be added to maven when possible)

There are two ways to configure keys and secrets:

- #### By providing your own configuration class like this

```java
import ChargilyEpayClientConfig;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import static ChargilyEpayConfigParams.*;

@Configuration
    public class ChargilyEpayConfiguration {

        @Bean
        public ChargilyEpayClientConfig configureChargily(){
        ChargilyEpayClientConfig chargilyEpayClientConfig = new ChargilyEpayClientConfig();
        chargilyEpayClientConfig.put(BASE_URL, "https://epay.chargily.com.dz");
        chargilyEpayClientConfig.put(API_KEY, "your_api_key");
        chargilyEpayClientConfig.put(SECRET, "your_secret");
        return chargilyEpayClientConfig;
    }
}
```

- #### or simply adding by these properties on application.properties file

```properties
chargily.epay.apikey=your_api_key
chargily.epay.url=https://epay.chargily.com.dz
chargily.epay.secret=your_secret
```

#### then to make a payment simply inject the ChargilyClient in your service

either by constructor or field injection like this (constructor injection is preferred, but I will use field injection
just for demo)

```java
public class MyService{
@Autowired
private ChargilyEpayClient client;

public void makePayment(){
    InvoiceModel invoice = new InvoiceModel(
                "someClient",
                "someEmail@mail.com",
                "1000",
                BigDecimal.valueOf(75.0),
                55d,
                "https://backurl.com/",
                "https://webhookurl.com/",
                Mode.CIB,
                "a comment"
                );
    //handle response after you get it as a call back
    client.makePayment(invoice, new Callback() {
            @Override
            public void onFailure(@NotNull Call call, @NotNull IOException e) {
                //in case of failure
            }

            @Override
            public void onResponse(@NotNull Call call, @NotNull Response response) throws IOException {
                //in case of success
            }
        });
    }
}
```
Here's a demo


https://user-images.githubusercontent.com/35870420/181354840-85d35365-4720-4ff8-8ce7-78f4cf5b3cda.mp4

