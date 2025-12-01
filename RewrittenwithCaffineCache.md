import com.github.benmanes.caffeine.cache.Caffeine;
import com.github.benmanes.caffeine.cache.Cache;

import java.util.concurrent.TimeUnit;

public class SimpleCache<K, V> {

    private final Cache<K, V> cache;

    // Equivalent to your TTL = 1 minute
    private static final long TTL_MINUTES = 1;

    public SimpleCache() {
        this.cache = Caffeine.newBuilder()
                .expireAfterWrite(TTL_MINUTES, TimeUnit.MINUTES)
                .maximumSize(10_000) // optional safety limit
                .build();
    }

    public void put(K key, V value) {
        cache.put(key, value);
    }

    public V get(K key) {
        return cache.getIfPresent(key);
    }

    public long size() {
        return cache.estimatedSize(); // fast, non-blocking
    }
}
