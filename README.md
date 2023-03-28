# server-sent-events

Hello, Java enthusiasts and professional developers. If you ever had to face the challenges of one-way server-client communication and are searching for a straightforward solution, you came to the right place. The technology we will go through in this short blog presents an easy and effective solution.

The basic layout of the problem is interesting. On the background (server) side we have scheduled/non-scheduled jobs that are running and completing their tasks at different time intervals. When a specific job is finished, the client side (browser) is notified. This, by itself, presents a whole number of problems to solve, and a great way to do it is by using Server-Sent Events technology.

The best way to showcase the problem and the solutions Server-Sent Events technology brings to the table is to create a simple example application. The application will be a Notification Mechanism with a number of background jobs. Client (browser) will be notified when the job is finished. Before we start creating our example, we will cover some basics of Server-Sent Events (SSE) technology so we have an easier time down the road.

Add the necessary dependencies:

```
<dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>
```
Create a controller that will handle SSE requests:
```
import org.springframework.http.MediaType;
import org.springframework.http.codec.ServerSentEvent;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import reactor.core.publisher.Flux;

@RestController
public class SSEController {

    @GetMapping(path = "/sse", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public Flux<ServerSentEvent<String>> streamSSE() {
        return Flux.interval(Duration.ofSeconds(1))
                .map(sequence -> ServerSentEvent.<String>builder()
                        .id(String.valueOf(sequence))
                        .event("message")
                        .data("SSE message #" + sequence)
                        .build());
    }
}
```
In the above example, we have used a Flux to stream SSE messages. The ```Flux.interval()``` method is used to emit messages at regular intervals of 1 second. You can customize the interval as per your requirement. The ServerSentEvent class represents an individual SSE message. We have used the ```ServerSentEvent.builder()``` method to create instances of this class. The ```@GetMapping``` annotation is used to map the ```/sse``` endpoint to the ```streamSSE()``` method. Finally, we have specified the ```MediaType.TEXT_EVENT_STREAM_VALUE``` value for the produces attribute of the ```@GetMapping``` annotation. This tells Spring Boot that the endpoint will stream SSE messages.

To test the SSE endpoint, you can use a web client like cURL or a web browser that supports SSE. For example, you can use the following cURL command to connect to the SSE endpoint:
```
curl -v http://localhost:8080/sse
```


<a href="https://www.facebook.com/Khair07"><img src="https://image.similarpng.com/very-thumbnail/2020/04/Beautiful-design-Facebook-logo-social-media-png.png" width="50px"></a>
<a href="https://www.instagram.com/khair_muhammad0/"><img src="https://image.similarpng.com/very-thumbnail/2020/04/Instagram-logo-modern-paint-splash-social-media-png.png" width="50px"></a>
<a href="https://www.linkedin.com/in/khair-muhammad-memon/"><img src="https://image.similarpng.com/very-thumbnail/2020/04/Linkedin-logo-scribble-social-media-icon-png.png" width="50px"></a>
<a href="https://github.com/khairmuhammad"><img src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png" width="50px"></a>
