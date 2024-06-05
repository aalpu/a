//
CompletableFuture<SendResult<K, V>> completableFuture = gksKafkaTemplate.send(message);
ListenableFuture<SendResult<K, V>> listenableFuture = new ListenableFuture<SendResult<K, V>>() {
    @Override
    public void addCallback(ListenableFutureCallback<? super SendResult<K, V>> callback) {
        completableFuture.handleAsync((result, ex) -> {
            if (ex != null) {
                callback.onFailure(ex);
            } else {
                callback.onSuccess(result);
            }
            return null;
        });
    }
};

AtomicBoolean status = new AtomicBoolean(false);
listenableFuture.addCallback(createListenableFutureCallback(status));
return status.get();
//
