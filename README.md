import java.util.concurrent.CompletableFuture;
import java.util.concurrent.atomic.AtomicBoolean;
import org.springframework.kafka.support.SendResult;
import org.springframework.kafka.support.KafkaHeaders;
import org.springframework.messaging.Message;
import org.springframework.messaging.support.MessageBuilder;
import org.springframework.kafka.core.KafkaTemplate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class KafkaPublisher {

    private static final Logger Log = LoggerFactory.getLogger(KafkaPublisher.class);
    private KafkaTemplate<K, V> kafkaTemplate;

    public <K, V, T> boolean publish(K key, V value, T topic) {
        Message<V> message = MessageBuilder.withPayload(value)
                .setHeader(KafkaHeaders.KEY, key)
                .setHeader(KafkaHeaders.TOPIC, topic)
                .build();

        CompletableFuture<SendResult<K, V>> future = kafkaTemplate.send(message).completable();

        AtomicBoolean status = new AtomicBoolean(false);
        future.whenComplete((result, ex) -> {
            if (ex != null) {
                onFailure(ex, status);
            } else {
                onSuccess(result, status);
            }
        });

        // You may need to block until the future completes if you need a synchronous result.
        // CompletableFuture.allOf(future).join(); // Uncomment if blocking is needed.

        return status.get();
    }

    private <K, V> void onSuccess(SendResult<K, V> result, AtomicBoolean status) {
        Log.info("Successfully published message to topic {} with offset {} and partition {}",
                result.getRecordMetadata().topic(),
                result.getRecordMetadata().offset(),
                result.getRecordMetadata().partition());
        status.set(true);
    }

    private void onFailure(Throwable ex, AtomicBoolean status) {
        Log.error("Failed to publish message to topic", ex);
        status.set(false);
    }
}
